# 📡 Drop Box — Network Intrusion Monitor

> Sistem automat de monitorizare a rețelei locale, rulat pe **Raspberry Pi 5**, care detectează dispozitive noi și trimite alerte în timp real pe **Telegram**.

---

## 🔍 Descriere

**Drop Box** este un tool de securitate de rețea care scanează periodic rețeaua locală (LAN) și alertează utilizatorul atunci când un dispozitiv necunoscut se conectează. La detectarea unui dispozitiv nou, sistemul realizează automat o scanare detaliată a porturilor deschise și trimite un raport complet pe Telegram.

---

## ✨ Funcționalități

- 🔎 **Ping Sweep** automat al rețelei locale la fiecare 5 minute
- 📋 **Whitelist** de dispozitive cunoscute (MAC + IP + Vendor)
- ⚠️ **Alertă Telegram** la detectarea unui dispozitiv nou
- 🔍 **Scanare detaliată de porturi** (nmap `-F -sV`) pentru fiecare dispozitiv necunoscut
- 📊 **Raport complet** cu porturi deschise, servicii și versiuni

---

## 🛠️ Tehnologii folosite

| Tehnologie | Rol |
|---|---|
| Python 3.13 | Limbaj principal |
| nmap / python-nmap | Scanare rețea și porturi |
| Telegram Bot API | Notificări în timp real |
| Raspberry Pi 5 | Hardware de rulare |
| JSON | Stocare whitelist dispozitive |

---

## 📦 Instalare

### Cerințe

- Raspberry Pi 5 cu Raspberry Pi OS
- Python 3.13+
- nmap instalat pe sistem

```bash
sudo apt update
sudo apt install nmap
```

### Setup

```bash
# Clonează repository-ul
git clone https://github.com/<username>/drop-box.git
cd drop-box

# Creează un virtual environment
python3 -m venv venv
source venv/bin/activate

# Instalează dependențele
pip install python-nmap requests
```

### Configurare

Editează `dropbox.py` și completează:

```python
TOKEN_TELEGRAM = "your_telegram_bot_token"
CHAT_ID = "your_chat_id"
SUBNET_RETEA = "192.168.1.0/24"  # rețeaua ta locală
```

> 💡 Creează un bot Telegram cu [@BotFather](https://t.me/BotFather) și obține token-ul.

---

## 🚀 Rulare

```bash
sudo python3 dropbox.py
```

> ⚠️ `sudo` este necesar pentru scanările nmap cu privilegii de rețea.

### Rulare automată la pornire (systemd)

```bash
sudo nano /etc/systemd/system/dropbox.service
```

```ini
[Unit]
Description=Drop Box Network Monitor
After=network.target

[Service]
ExecStart=/home/proiect/Documents/P1/venv/bin/python /home/proiect/Documents/P1/dropbox.py
WorkingDirectory=/home/proiect/Documents/P1
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable dropbox
sudo systemctl start dropbox
```

---

## 📱 Exemplu alertă Telegram

```
⚠️ DISPOZITIV NOU DETECTAT ÎN REȚEA!

📍 IP: 192.168.1.22
🛡️ MAC: AA:BB:CC:DD:EE:FF
🏭 Vendor: Apple, Inc.
⏱️ Se începe scanarea detaliată de porturi...

🔍 Raport Porturi active pentru 192.168.1.22:
  - Port 22/tcp: open (ssh OpenSSH 8.9)
  - Port 80/tcp: open (http nginx 1.18)
```

---

## 📁 Structură proiect

```
drop-box/
├── dropbox.py                  # Script principal
├── dispozitive_cunoscute.json  # Whitelist auto-generat
├── venv/                       # Virtual environment
├── .gitignore
└── README.md
```

---

## ⚠️ Disclaimer

Acest proiect este destinat exclusiv monitorizării propriei rețele locale. Scanarea rețelelor fără permisiune explicită este ilegală.

---

## 📄 Licență

MIT License — liber de utilizat și modificat.
