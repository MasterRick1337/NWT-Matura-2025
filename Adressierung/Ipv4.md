# IPv4, Subnetting & MikroTik IP-Vergabe

## Was ist IPv4?
- IPv4 = Internet Protocol Version 4  
- Standard zur Adressierung von Geräten im Netzwerk  
- 32-bit-Adresse: z. B. `192.168.1.1`  
- Besteht aus vier Oktetten/Byte (8 Bit) → Werte: 0–255  
- ~4,3 Mrd. Adressen (2³²)

---

##  Aufbau einer IPv4-Adresse
- Beispiel: `192.168.1.1`  
  → Binär: `11000000.10101000.00000001.00000001`
- Besteht aus:
  - **Netzwerkanteil** (Network Prefix)
  - **Hostanteil** (Geräte im Netzwerk)
- Aufteilung wird durch **Subnetzmaske** definiert

---

## Private vs Öffentliche IPv4-Adressen
- Öffentliche IPs: im Internet eindeutig  
- Private IPs: nur intern gültig (NAT erforderlich)

---
## Subnetting:
- Ziel: Aufteilen eines Netzwerks in kleinere Subnetze  
- Subnetzmaske bestimmt die Größe:
  - `/24` → 255.255.255.0 → 256 Adressen  
- Berechnung:
  - `/25` → 128 Adressen
  - `/26` → 64 Adressen
- Vorteil:
  - Bessere Netzsegmentierung
  - Erschwert Cyberattacken (ARP Spoofing, DHCP Starvation, Smurf-Attacke)

---

## Beispiel: Subnetting
- Gegeben: `192.168.0.0/24`  
- Unterteilung in 4 Subnetze:
  - `/26` → 4 Subnetze mit 64 Adressen:
    - `192.168.0.0/26`
    - `192.168.0.64/26`
    - `192.168.0.128/26`
    - `192.168.0.192/26`

---

##  MikroTik: Automatische IP-Vergabe (DHCP)
- MikroTik bietet DHCP-Server  
- IP-Adresse auf dem Interface
- IP-Pool definieren  
- DHCP Netzwerk und DHCP-Server

---

## MikroTik Skript: Automatische IP-Vergabe

```bash
/ip pool add name=dhcp_pool ranges=192.168.88.10-192.168.88.100

/ip address add address=192.168.88.1/24 interface=ether2

/ip dhcp-server network add address=192.168.88.0/24 gateway=192.168.88.1

/ip dhcp-server add name=dhcp1 interface=ether2 address-pool=dhcp_pool disabled=no
```

---

## Aufbau IPv4 Header
Der **IPv4-Header** ist ein zentraler Bestandteil jedes IP-Pakets – quasi der "Briefumschlag", auf dem steht, **woher** das Paket kommt, **wohin** es geht und **wie** es behandelt werden soll.


Der **IPv4-Header** enthält **Meta-Informationen** über das Paket. Er steht ganz am Anfang des Pakets, **vor** den eigentlichen Nutzdaten (wie z. B. TCP, UDP, HTTP-Inhalt usw.).

| Feld                       | Größe    | Beschreibung                                                                                                                                               |
| -------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Version**                | 4 Bit    | Protokollversion → `4` für IPv4                                                                                                                            |
| **IHL** (Header Length)    | 4 Bit    | Zeigt wie lang der Header ist, trennt Header von der eigentlichen Nachricht                                                                                |
| **Type of Service (TOS)**  | 8 Bit    | Diensttyp, Prioritäten, QoS                                                                                                                                |
| **Total Length**           | 16 Bit   | Gesamtlänge des IP-Pakets (Header + Daten)                                                                                                                 |
| **Identification**         | 16 Bit   | Wird zur Identifikation von Fragmenten genutzt                                                                                                             |
| **Flags**                  | 3 Bit    | Steuerung der Fragmentierung:<br>0 -> Reserviert<br>1 -> Don't Fragment - darf nicht fragmentiert werden<br>2 -> More Fragments - weitere Fragmente folgen |
| **Fragment Offset**        | 13 Bit   | Position eines Fragmentes im Originalpaket                                                                                                                 |
| **Time to Live (TTL)**     | 8 Bit    | Wie viele Hops (Router) das Paket überlebt                                                                                                                 |
| **Protocol**               | 8 Bit    | Welches Protokoll in den Daten steckt (z. B. TCP = 6, UDP = 17)                                                                                            |
| **Header Checksum**        | 16 Bit   | Fehlerprüfung für den Header:<br>- Erkennt Bitfehler im Header auf dem Weg    <br>- Wird bei jedem Router **neu berechnet**                                |
| **Source IP Address**      | 32 Bit   | Absender-IP                                                                                                                                                |
| **Destination IP Address** | 32 Bit   | Empfänger-IP                                                                                                                                               |
| **Options (optional)**     | variabel | Zusätzliche Steuerinfos (z. B. Security, Routing)                                                                                                          |


```
| Ver | IHL | TOS |       Total Length        |
|          Identification          |Flags|Fragment Offset|
|   TTL   | Protocol | Header Checksum        |
|            Source IP Address              |
|         Destination IP Address            |
|           (Optional: Options)             |
```

---
## Wichtigste Parameter:

| Feld                   | Zweck                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------ |
| **TTL**                | Verhindert Endlosschleifen im Netz. Jeder Router zieht 1 ab – bei 0 wird's gelöscht. |
| **Checksum**           | Prüft, ob der Header auf dem Weg beschädigt wurde                                    |
| **Fragmentierung**     | Große Pakete können aufgeteilt und später wieder zusammengesetzt werden              |
| **Protocol**           | Sagt dem Empfänger, welcher "Inhalt" drin ist (z. B. TCP, UDP)                       |
| **Source/Destination** | Damit das Paket weiß, woher und wohin                                                |