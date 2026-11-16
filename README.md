# mac-lookup
Bash-Skript für Ping und MAC-Abfrage



ein kompaktes Bash-Skript, das beides in einem Rutsch erledigt:

#!/usr/bin/env bash

# Verwendung: ./mac-lookup.sh <IP-Adresse>

if [[ -z "$1" ]]; then
    echo "Verwendung: $0 <IP-Adresse>"
    echo "Beispiel:    $0 192.168.1.1"
    exit 1
fi

IP="$1"

# 1. Ziel anpingen (damit der ARP-Eintrag existiert)
ping -c 1 -W 1 "$IP" > /dev/null 2>&1

# 2. MAC-Adresse aus dem ARP-Cache lesen
MAC=$(ip neigh show "$IP" 2>/dev/null | awk '{print $5}')

if [[ -z "$MAC" ]]; then
    echo "Kein ARP-Eintrag für $IP gefunden."
    echo "→ Gerät ist offline oder nicht im selben Subnetz."
    exit 1
fi

# 3. Hersteller via API abfragen
VENDOR=$(curl -s --max-time 5 "https://api.macvendors.com/$MAC")

if [[ -z "$VENDOR" || "$VENDOR" == "Not found" ]]; then
    VENDOR="Unbekannt"
fi

# 4. Ausgabe
echo "IP:       $IP"
echo "MAC:      $MAC"
echo "Hersteller: $VENDOR"

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

