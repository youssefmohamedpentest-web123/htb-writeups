# Screenshots — Lame

These are real captures from the authorized lab session (target `10.129.22.59`), embedded in the writeup.

| File | Shows |
|------|-------|
| `01-nmap-allports.png` | Full `-p-` scan: 21, 22, 139, 445, 3632 |
| `02-nmap-ftp.png` | vsftpd 2.3.4, anonymous login allowed |
| `03-ftp-anon.png` | Anonymous FTP login, empty listing |
| `04-nmap-ssh.png` | OpenSSH 4.7p1 |
| `05-nmap-smb.png` | Samba 3.0.20-Debian, host `lame` |
| `06-smbclient-anonymous-fail.png` | Wrong share name → BAD_NETWORK_NAME |
| `07-smbclient-shares.png` | Share list: print$, tmp, opt, IPC$, ADMIN$ |
| `08-nmap-distcc.png` | distccd v1 GNU 4.2.4 |
| `09-samba-root.png` | Path A: Samba usermap → `whoami` root |
| `10-nc-listener.png` | netcat listener for distcc path |
| `11-distcc-exploit.png` | distcc exploit command (CVE-2004-2687) |
| `12-distcc-daemon-shell.png` | Path B shell as `daemon` |
| `13-suid-find.png` | SUID hunt — `/usr/bin/nmap` present |
| `14-nmap-suid-root.png` | `nmap --interactive` → root |
