# Screenshots — Archetype

Real captures from the authorized lab session (target `10.10.10.27`), embedded in the writeup.

| File | Shows |
|------|-------|
| `01-nmap.png` | Full scan: 135/139/445, MSSQL 1433, WinRM 5985; host ARCHETYPE |
| `02-smb-shares.png` | Null-session share list incl. `backups` |
| `03-backups-dtsconfig.png` | `backups` share → `get prod.dtsConfig` |
| `04-query-script.png` | Custom `pymssql` enumeration script |
| `05-query-output.png` | Output: SQL 2017, `ARCHETYPE\sql_svc`, syslogins dump |
| `06-permissions.png` | `fn_my_permissions` server-level check |
| `07-hash-dump.png` | SQL login password-hash dump |
| `08-hashcat-failed.png` | hashcat on `sa` hash — Exhausted, 0 recovered (dead end) |
| `09-user-flag.png` | user.txt located on sql_svc Desktop |
| `10-admin-root.png` | `whoami` archetype\administrator + root.txt |
