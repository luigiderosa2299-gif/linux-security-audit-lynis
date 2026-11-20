# Lynis — Final Report (after fixes)

Date: 24 October 2026
System: Kali GNU/Linux Rolling 2026.3 (VM, UTM)

## Fixes Applied Before This Scan

1. `sudo apt install -y fail2ban && sudo systemctl enable --now fail2ban`
2. `sudo apt install -y libpam-passwdqc`
3. `/etc/login.defs`: `PASS_MAX_DAYS 90`, `PASS_MIN_DAYS 1`

## Summary

```
Hardening index : 68 [#############       ]
Tests performed : 278
Plugins enabled : 1

Software components:
- Firewall               [V]
- Intrusion software     [V]
- Malware scanner        [X]
```

## Direct Comparison

| | Baseline | Final |
|---|---|---|
| Hardening Index | 65 | **68** |
| Tests performed | 277 | 278 |
| Open suggestions | 47 | **43** |
| Intrusion software | ❌ | ✅ |

## fail2ban — Active Service Confirmation

```
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; enabled; preset: disabled)
     Active: active (running)
       Docs: man:fail2ban(1)
   Main PID: 164617 (fail2ban-server)
      Tasks: 5 (limit: 4666)
     Memory: 27.6M (peak: 28.6M)
     CGroup: /system.slice/fail2ban.service
             └─164617 /usr/bin/python3 /usr/bin/fail2ban-server -xf start

Oct 24 13:04:09 kali systemd[1]: Started fail2ban.service - Fail2Ban Service.
Oct 24 13:04:09 kali fail2ban-server[164617]: Server ready
```
