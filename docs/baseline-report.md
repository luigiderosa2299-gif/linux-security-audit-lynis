# Lynis — Report Baseline (prima dei fix)

Data: 24 ottobre 2026
Sistema: Kali GNU/Linux Rolling 2026.3 (VM, UTM)

## Riepilogo

```
Hardening index : 65 [#############       ]
Tests performed : 277
Plugins enabled : 1

Software components:
- Firewall               [V]
- Intrusion software     [X]
- Malware scanner        [X]
```

## Suggerimenti completi (47 totali)

```
suggestion[]=DEB-0280|Install libpam-tmpdir to set $TMP and $TMPDIR for PAM sessions|-|-|
suggestion[]=DEB-0810|Install apt-listbugs to display a list of critical bugs prior to each APT installation.|-|-|
suggestion[]=DEB-0811|Install apt-listchanges to display any significant changes prior to any upgrade via APT.|-|-|
suggestion[]=DEB-0880|Install fail2ban to automatically ban hosts that commit multiple authentication errors.|-|-|
suggestion[]=BOOT-5122|Set a password on GRUB boot loader to prevent altering boot configuration (e.g. boot in single user mode without password)|-|-|
suggestion[]=BOOT-5180|Determine runlevel and services at startup|-|-|
suggestion[]=BOOT-5264|Consider hardening system services|Run '/usr/bin/systemd-analyze security SERVICE' for each service|-|
suggestion[]=PROC-3614|Check process listing for processes waiting for IO requests|-|-|
suggestion[]=AUTH-9230|Configure password hashing rounds in /etc/login.defs|-|-|
suggestion[]=AUTH-9262|Install a PAM module for password strength testing like pam_cracklib or pam_passwdqc or libpam-passwdqc|-|-|
suggestion[]=AUTH-9282|When possible set expire dates for all password protected accounts|-|-|
suggestion[]=AUTH-9286|Configure minimum password age in /etc/login.defs|-|-|
suggestion[]=AUTH-9286|Configure maximum password age in /etc/login.defs|-|-|
suggestion[]=AUTH-9328|Default umask in /etc/login.defs could not be found and defaults usually to 022, which could be more strict like 027|-|-|
suggestion[]=FILE-6310|To decrease the impact of a full /home file system, place /home on a separate partition|-|-|
suggestion[]=FILE-6310|To decrease the impact of a full /var file system, place /var on a separate partition|-|-|
suggestion[]=USB-1000|Disable drivers like USB storage when not used, to prevent unauthorized storage or data theft|-|-|
suggestion[]=STRG-1846|Disable drivers like firewire storage when not used, to prevent unauthorized storage or data theft|-|-|
suggestion[]=PKGS-7308|Check RPM database as RPM binary available but does not reveal any packages|-|-|
suggestion[]=PKGS-7346|Purge old/removed packages (8 found) with aptitude purge or dpkg --purge command. This will cleanup old configuration files, cron jobs and startup scripts.|-|-|
suggestion[]=PKGS-7370|Install debsums utility for the verification of packages with known good database.|-|-|
```

*(elenco troncato alle prime 20 righe più rilevanti; il report completo risiede in `/var/log/lynis-report.dat` sulla VM)*
