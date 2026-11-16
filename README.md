# mac-lookup
Bash-Skript für Ping und MAC-Abfrage



ein kompaktes Bash-Skript, das beides in einem Rutsch erledigt:


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

