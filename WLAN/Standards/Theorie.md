Das IEEE 802.11-Standardwerk beschreibt verschiedene Versionen von WLAN (Wireless Local Area Network), die sich hinsichtlich Geschwindigkeit, Frequenzbereich und Technologien unterscheiden.

| Standard | Frequenzbereich | Max. Datenrate | Besonderheiten                                                                              |
| -------- | --------------- | -------------- | ------------------------------------------------------------------------------------------- |
| 802.11g  | 2.4 GHz         | 54 Mbit/s      | Rückwärtskompatibel zu 802.11b, aber störanfällig durch andere Geräte im 2,4-GHz-Band       |
| 802.11n  | 2.4 & 5 GHz     | 600 Mbit/s     | Einführung von MiMo (Multiple Input Multiple Output) zur Verbesserung der Datenrate         |
| 802.11ac | 5 GHz           | 6.93 Gbit/s    | Unterstützt breitere Kanäle (80 MHz, 160 MHz) und Mehrfachantennentechnologien              |
| 802.11ax | 2.4 & 5 GHz     | 9.6 Gbit/s     | OFDMA (Orthogonal Frequency-Division Multiple Access), bessere Effizienz, niedrigere Latenz |
### Kanäle und Bandbreiten
WLAN-Netzwerke nutzen Kanäle innerhalb ihrer Frequenzbänder. In Europa sind für 2,4 GHz und 5 GHz unterschiedliche Kanäle verfügbar:
- **2,4 GHz-Band:** 13 Kanäle (je 5 MHz breit), nur 3 überlappungsfreie (1, 6, 11)
- **5 GHz-Band:** Mehr Kanäle verfügbar (36, 40, 44, 48, usw.), weniger Störungen
- **6 GHz-Band (Wi-Fi 6E):** Noch mehr Kanäle mit geringerer Interferenz

**Bandbreiten:**
- 20 MHz (Standard für 2,4 GHz)
- 40 MHz (802.11n)
- 80 MHz (802.11ac)
- 160 MHz (802.11ac/ax für maximale Datenraten)

### MiMo (Multiple Input Multiple Output)
MiMo ermöglicht die gleichzeitige Nutzung mehrerer Antennen für Senden und Empfangen, was die Effizienz und Geschwindigkeit verbessert.
- **SU-MiMo (Single User MiMo):** Ein Client profitiert von mehreren Antennen
- **MU-MiMo (Multi User MiMo):** Mehrere Clients können gleichzeitig Daten senden und empfangen

### Kanalbündelung (Channel Bonding)
Durch das Bündeln von zwei oder mehr Kanälen kann die verfügbare Bandbreite erhöht werden, was höhere Datenraten ermöglicht. Dies wird vor allem im 5-GHz-Band genutzt (40 MHz, 80 MHz, 160 MHz).