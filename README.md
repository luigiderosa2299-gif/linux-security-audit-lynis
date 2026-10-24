# Linux Security Audit con Lynis — Hardening Score e Remediation

> ⚠️ **Contesto**: audit eseguito esclusivamente sulla mia VM Kali Linux personale (laboratorio isolato su UTM, rete privata domestica). Nessun sistema di terzi è stato toccato. Tutti i fix applicati sono reversibili e documentati.

## 📌 Obiettivo

Eseguire un audit di sicurezza reale su una VM Linux con [Lynis](https://cisofy.com/lynis/) (lo stesso tipo di tool usato in ambito professionale per verificare la conformità a baseline tipo CIS Benchmarks), identificare i punti deboli concreti, applicare correttivi mirati e misurare il miglioramento con un confronto prima/dopo basato sull'Hardening Index.

## 🧩 Contesto / Scenario

Laboratorio personale (vedi [home-lab-cybersecurity](https://github.com/luigiderosa2299-gif/home-lab-cybersecurity)): VM Kali Linux su UTM, rete privata domestica — la stessa VM usata nel progetto [ssh-brute-force-detection-lab](https://github.com/luigiderosa2299-gif/ssh-brute-force-detection-lab).

## 🛠 Tecnologie e Strumenti

- **Lynis 3.1.7** — security auditing tool open source per sistemi Unix/Linux, usato per compliance e hardening assessment
- **fail2ban** — intrusion prevention, banna automaticamente host dopo ripetuti fallimenti di autenticazione
- **libpam-passwdqc** — modulo PAM per la verifica della robustezza delle password
- Ambiente: VM Kali Linux (Kali GNU/Linux Rolling 2026.3) su UTM

## ⚙️ Cosa ho fatto

### 1. Baseline — audit iniziale

```bash
sudo lynis audit system
```

**Risultato baseline**: vedi [`docs/baseline-report.md`](docs/baseline-report.md)
- **Hardening Index: 65/100**
- 277 test eseguiti, 47 suggerimenti, 0 warning critici
- Software components: Firewall ✅, Intrusion software ❌, Malware scanner ❌

### 2. Analisi dei suggerimenti

Esaminata la lista completa dei 47 suggerimenti generati da Lynis (salvati in `/var/log/lynis-report.dat`), selezionati 3 fix concreti e a basso rischio, prioritizzati per impatto reale sulla postura di sicurezza:

| ID Lynis | Suggerimento | Priorità |
|---|---|---|
| DEB-0880 | Installare fail2ban per bannare automaticamente host con fallimenti di autenticazione ripetuti | Alta — si collega direttamente alla detection di brute force del progetto [ssh-brute-force-detection-lab](https://github.com/luigiderosa2299-gif/ssh-brute-force-detection-lab) |
| AUTH-9262 | Installare un modulo PAM per la verifica della robustezza password | Media |
| AUTH-9286 | Configurare età minima/massima delle password in `/etc/login.defs` | Media |

### 3. Applicazione dei fix

**Fix 1 — fail2ban**:
```bash
sudo apt install -y fail2ban
sudo systemctl enable --now fail2ban
```

**Fix 2 — verifica robustezza password**:
```bash
sudo apt install -y libpam-passwdqc
```

**Fix 3 — scadenza password** in `/etc/login.defs`:
```
PASS_MAX_DAYS   90
PASS_MIN_DAYS   1
```

### 4. Re-scan e confronto

```bash
sudo lynis audit system
```

**Risultato finale**: vedi [`docs/final-report.md`](docs/final-report.md)
- **Hardening Index: 68/100** (+3 punti)
- 278 test eseguiti, 43 suggerimenti (-4 rispetto alla baseline)
- Software components: Firewall ✅, **Intrusion software ✅** (fail2ban rilevato attivo), Malware scanner ❌

## 📊 Confronto Prima/Dopo

| Metrica | Baseline | Dopo i fix | Delta |
|---|---|---|---|
| Hardening Index | 65/100 | 68/100 | **+3** |
| Suggerimenti aperti | 47 | 43 | **-4** |
| Intrusion software | ❌ Assente | ✅ Attivo (fail2ban) | Risolto |

## 🧱 Osservazioni

Un incremento di 3 punti su un sistema già ragionevolmente curato (Kali Linux parte da una base più indurita di una distribuzione desktop generica) conferma un principio importante di security hardening: **i miglioramenti marginali richiedono sforzo crescente** — i primi fix (quelli ad alto impatto, come l'intrusion prevention) sono relativamente rapidi da applicare, mentre arrivare a un punteggio molto più alto richiederebbe interventi più invasivi (partizionamento disco separato per `/home` e `/var`, hardening GRUB con password, disabilitazione driver USB/firewire) che vanno valutati con attenzione rispetto all'usabilità del sistema in un contesto di laboratorio attivo.

I restanti 43 suggerimenti includono interventi a maggiore impatto sull'usabilità quotidiana della VM (es. disabilitare USB storage) che non sono stati applicati in questa iterazione per non compromettere l'operatività del lab.
