# Screenshots — Cap

Real captures from the authorized lab session (target `10.10.10.245`), embedded in the writeup.

| File | Shows |
|------|-------|
| `01-nmap.png` | `nmap -sC -sV -A`: ftp 21, ssh 22, http 80 (gunicorn / Security Dashboard) |
| `02-hosts.png` | Adding `cap.htb` to `/etc/hosts` |
| `03-dashboard.png` | Security Dashboard landing page |
| `04-capture-empty.png` | Own capture `/data/7` — 0 packets |
| `05-data-url.png` | The sequential `/data/<id>` URL |
| `06-data0-idor.png` | `/data/0` — 219 packets (IDOR, not my capture) |
| `07-wireshark-ftp.png` | Wireshark: FTP USER nathan / PASS in cleartext |
| `08-creds.png` | Recovered credentials |
| `09-ftp-login.png` | FTP login as nathan, home dir listing |
| `10-user-flag.png` | user.txt |
| `11-ssh-login.png` | SSH as nathan (Ubuntu 20.04) |
| `12-getcap.png` | `getcap` — cap_setuid on python3.8 |
| `13-root-flag.png` | setuid(0) root shell + root.txt |
