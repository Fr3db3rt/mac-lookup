
> ### Direkt ausführen:
>
> ```bash
> curl -fsSL https://raw.githubusercontent.com/Fr3db3rt/mac-lookup/main/mac-lookup.sh | bash
> ```
> *(mit Copy-Button beim Hover)*
---
# mac-lookup
Bash-Skript für Ping und MAC-Abfrage

ein kompaktes Bash-Skript, das beides in einem Rutsch erledigt:
---

So verwendest du es:

chmod +x mac-lookup.sh
./mac-lookup.sh 192.168.1.1

Beispielausgabe:

IP:       192.168.1.1
MAC:      aa:bb:cc:dd:ee:ff
Hersteller: AVM GmbH

Optionale Erweiterung – wenn du auch das Gerät (Hostname) mit ausgeben willst, einfach diese Zeile vor echo "IP:" einfügen:

HOST=$(ping -c 1 -W 1 "$IP" 2>&1 | awk -F'(' '{print $2}' | awk -F')' '{print $1}')

und in die Ausgabe ergänzen:

echo "Host:     ${HOST:-unbekannt}"

Das Skript funktioniert ohne zusätzliche Installation – es nutzt nur ping, ip, curl und awk, die in Linux Mint standardmäßig vorhanden sind.

---

Um in Linux Mint die MAC-Adresse einer IP-Adresse zu ermitteln, ist die zuverlässigste Methode, zuerst den Zielrechner anzupingen und dann den lokalen ARP-Cache auszulesen. Dies funktioniert nur für Geräte im lokalen Netzwerk (LAN) oder Router.

---

Alternative Werkzeuge
Für das Scannen ganzer Subnetze oder wenn der Cache leer ist, können Sie folgende Tools nutzen:

- arping: Sendet direkt ARP-Anfragen an eine spezifische IP (arping -I <Interface> <Ziel-IP>).
- nmap: Erkennt Geräte und deren MACs im gesamten Netz (sudo nmap -sP <Subnetze>).
- arp-scan: Eine spezialisierte Tool-Lösung für schnelle MAC-Sweeps (sudo arp-scan --localnet). 

---

