# Proxmox und damit ein pve installieren (pve steht für Proxmox Virtual Environment)
* ein spezielles Betriebssystem für Server auf Debian Basis

2. Der Host (Der Gastgeber)
In deinem Netzwerk ist dieser Rechner der Host. Er stellt die physischen Ressourcen (CPU, RAM, Festplatte) bereit. Alles, was innerhalb von Proxmox läuft, nennt man dann Gäste (VMs oder Container).

3. Virtualisierungs-Plattform
Das ist die gebräuchlichste Bezeichnung. Proxmox kombiniert zwei Technologien:

KVM (Virtual Machines): Für "schwere" Systeme wie deinen Windows 11 VPS. Hier wird die komplette Hardware simuliert.

LXC (Linux Container): Für "leichte" Dienste wie Home Assistant oder Jellyfin. Diese teilen sich den Kernel mit Proxmox und verbrauchen fast keine Ressourcen.

## Kurze Zusammenfassung der Konfiguration:
* **Netzwerk-Modus:** Interner Hyper-V Switch (`Proxmox-Net`).
* **IP-Bereich:** Umgestellt auf `10.0.0.x` via Windows Registry, um Konflikte mit Hotel-WLANs oder Heimroutern zu vermeiden.
* **Internet:** Dein Laptop teilt sein WLAN-Internet (ICS) mit Proxmox. Proxmox "sieht" deinen Laptop als Gateway unter der `10.0.0.1`.
* **DNS:** Fest auf `8.8.8.8` gesetzt, damit die Namensauflösung stabil bleibt, auch wenn sich das äußere WLAN ändert.

### Wie geht es weiter?
Da das Fundament (Netzwerk & Internet) nun steht, können wir mit der **Phase 3 (Remote-Management)** weitermachen.

### vorher das Debian auf pve updaten
- apt update && apt dist-upgrade -y
- reboot //  da ggf. neuer Kernel installiert wurde

Mein Vorschlag für den nächsten Schritt: **Netbird Installation**.
* **Warum?** Damit bekommst du eine feste Adresse (z.B. `proxmox.netbird.cloud`).
* **Vorteil:** Du musst dann nie wieder IPs tippen, egal ob du gerade per LAN, WLAN oder Hotspot verbunden bist. Netbird kümmert sich im Hintergrund um den Tunnel.
 
### Netbird?
* Wir haben Netbird auf unserem pve Server (node) installiert um Fernzugriff auf alles zu erhalten.
- curl -fsSL https://pkgs.netbird.io/install.sh | sh
- netbird up --setup-key 6F65BF50-8768-4922-9BE9-F4D1780F7C39

* https://pve.netbird.cloud:8006

* Docker in einem LXC auf Proxmox installieren incl. Portainer und Netbird

### neue HDD einbinden
- lsblk -f
- partitionieren, formatieren, mounten und in Proxmox einhaengen
- 
 Was passiert jetzt?
Proxmox merkt sich: „Der neue Speicher namens docker liegt auf dem Ordner /mnt/docker – und dahinter steckt die neue HDD (/dev/sdc1)“.
Ab sofort kannst du diesen Speicher überall in Proxmox benutzen (für LXCs, VMs, Backups usw.).

