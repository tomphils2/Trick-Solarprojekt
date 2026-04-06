# Proxmox und damit ein pve installieren (pve steht für Proxmox Virtual Environment)
* ein spezielles Betriebssystem für Server auf Debian Basis

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
* 
