# HTB Writeups — DarkMatter

A portfolio of Hack The Box machine writeups and penetration-test reports, written for learning and to demonstrate methodology. Each box is documented end-to-end across the standard phases: reconnaissance, enumeration, vulnerability assessment, exploitation, post-exploitation, reporting, and remediation.

> All boxes covered here are **retired / Starting Point** machines, for which HTB permits public writeups. Everything is for authorized lab use only.

## Machines

| Machine | OS | Difficulty | Key techniques | Writeup |
|---------|----|-----------|----------------|---------|
| **Cap** | Linux | Easy | IDOR, PCAP credential recovery, `cap_setuid` privesc | [cap/](cap/README.md) · [report](cap/Cap_Penetration_Test_Report.md) |
| **Lame** | Linux | Easy | SMB enum, Samba CVE-2007-2447 (usermap) → root | [lame/](lame/README.md) |
| **Archetype** | Windows | Easy | SMB, MSSQL `xp_cmdshell`, PowerShell-history privesc | [archetype/](archetype/README.md) |

## How these are written

- **Human-voice walkthroughs** — the real thought process, dead ends included, not just a command dump.
- **Accurate command output** inline, with clearly-marked `[SCREENSHOT]` slots to fill with your own session captures.
- **A formal report** per box (full template shown for Cap) with CVSS v3.1 severity, impact, and remediation.

## Toolkit

`Nmap` · `WhatWeb` · `Gobuster`/`ffuf` · `Nuclei` · `Burp Suite` · `smbclient`/`enum4linux` · `Wireshark`/`tshark` · `Metasploit` · `Impacket` · `Evil-WinRM` · `netcat` · `LinPEAS`/`WinPEAS`

---

*Author: DarkMatter · For educational and authorized testing only.*
