

# Hersteller aus MAC-Adresse ermitteln
Die ersten drei Oktette (OUI) der MAC-Adresse identifizieren den Hersteller. In Linux Mint gibt es mehrere Wege:

---

1. mac-lookup (empfohlen, einfachste Methode)
sudo apt install mac-lookup
mac-lookup AA:BB:CC:DD:EE:FF

Gibt direkt den Herstellernamen aus. 

---

2. grep über lokale OUI-Datenbanken
Falls nmap installiert ist:

grep -i "^aabbcc" /usr/share/nmap/nmap-mac-prefixes

Oder über das Paket ieee-data:

sudo apt install ieee-data
grep -i "^aabbcc" /usr/share/ieee-data/oui.txt

Ersetze aabbcc durch die ersten 6 Hex-Zeichen deiner MAC (ohne Trennzeichen).

---

3. Ohne Installation (API)
curl -s https://api.macvendors.com/aa:bb:cc:dd:ee:ff

Gibt den Hersteller als reinen Text zurück – keine Installation nötig.

---

4. Python: mac-vendor-lookup
pip install mac-vendor-lookup
mac_vendor_lookup AA-BB-CC-DD-EE-FF

---

Tipp: Am schnellsten im Alltag ist Variante 3 (curl), da keine Installation nötig ist und die Ausgabe sofort lesbar ist.


