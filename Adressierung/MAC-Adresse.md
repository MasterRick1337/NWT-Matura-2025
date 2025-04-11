# MAC-Adresse: Alles, was du wissen musst

## 1. Was ist eine MAC-Adresse?

Eine MAC-Adresse (Media Access Control Address) ist eine eindeutige Hardware-Adresse, die jedem Netzwerkgerät (z. B. Netzwerkkarte, Router, Switch) zugewiesen wird. Sie dient zur Identifikation im lokalen Netzwerk (LAN) auf der Schicht 2 (Data Link Layer) des OSI-Modells.

### Beispiel einer MAC-Adresse:

```
00:1A:2B:3C:4D:5E  
(oder 00-1A-2B-3C-4D-5E)
```

- **48 Bit lang** (6 Byte, hexadezimal dargestellt).
- **Erstes Halbbyte (OUI):** Identifiziert den Hersteller (z. B. 00:1A:2B = Intel).
- **Zweites Halbbyte:** Vom Hersteller vergebene Geräte-ID.

## 2. Wofür wird eine MAC-Adresse verwendet?

- **Lokale Kommunikation:** Im LAN werden Pakete über MAC-Adressen (nicht IPs!) weitergeleitet.
- **ARP (Address Resolution Protocol):** Übersetzt IP-Adressen in MAC-Adressen.
- **Switch-Kommunikation:** Switches lernen MAC-Adressen, um Pakete zielgerichtet weiterzuleiten.
- **Sicherheit:** MAC-Filterung erlaubt/blockiert bestimmte Geräte im Netzwerk.

## 3. Aufbau einer MAC-Adresse

| Teil          | Beispiel  | Beschreibung |
|--------------|----------|--------------|
| **OUI (24 Bit)** | 00:1A:2B | Herstellerkennung (IEEE-registriert) |
| **NIC (24 Bit)** | 3C:4D:5E | Vom Hersteller vergebene Geräte-ID |
| **Bit 1 (U/L)** | 0 (00:1A:2B…) | 0 = Universell (vom Hersteller vergeben), 1 = Lokal verwaltet |
| **Bit 2 (I/G)** | 0 (00:1A:2B…) | 0 = Unicast, 1 = Multicast/Broadcast |

## 4. Arten von MAC-Adressen

| Typ        | Beispiel            | Verwendung |
|-----------|--------------------|------------|
| **Unicast**   | 00:1A:2B:3C:4D:5E | Kommunikation mit einem bestimmten Gerät |
| **Multicast** | 01:00:5E:…        | Gruppe von Geräten (z. B. für Streaming) |
| **Broadcast** | FF:FF:FF:FF:FF:FF | An alle Geräte im LAN (z. B. ARP-Requests) |
Bitte IP Multicast Adressenabbildung auf MAC Adressen dokumentieren!
## 5. MAC vs. IP-Adresse

| Eigenschaft     | MAC-Adresse | IP-Adresse |
|---------------|------------|-----------|
| **Schicht**  | Data Link Layer (Schicht 2) | Network Layer (Schicht 3) |
| **Zuständigkeit** | Lokales Netz (LAN) | Globale/logische Adressierung |
| **Vergabe** | Vom Hersteller festgelegt | Dynamisch (DHCP) oder statisch |
| **Änderbar?** | Theoretisch ja (Spoofing), aber normalerweise fest | Ja (je nach Konfiguration) |

## 6. Wie findet man die MAC-Adresse?

### Windows:
```sh
ipconfig /all  
# oder
getmac
```

### Linux/macOS:
```sh
ifconfig  
# oder
ip link show
```

### Router-Interface:
- Meist unter **„Connected Devices“** oder **„DHCP-Leases“** einsehbar.

## 7. Wichtige Protokolle mit MAC-Adressen

- **ARP (Address Resolution Protocol):**
  - Fragt: „Welche MAC-Adresse hat die IP 192.168.1.20?“
  - Antwort: „Hier ist 00:1A:2B:3C:4D:5E“

- **DHCP (Dynamic Host Configuration Protocol):**
  - Weist IPs basierend auf MAC-Adressen zu.

- **Switch-Learning:**
  - Switches merken sich, welche MAC-Adresse an welchem Port hängt.

## 8. MAC-Spoofing (Manipulation)

- **Was?** Temporäre Änderung der MAC-Adresse (z. B. für Privatsphäre oder Netzwerkumgehung).
- **Beispiel (Linux):**

```sh
sudo ifconfig eth0 down
sudo ifconfig eth0 hw ether 00:11:22:33:44:55
sudo ifconfig eth0 up
```

- **Risiko:** Kann für unerlaubten Netzwerkzugriff genutzt werden.

## 9. Zusammenfassung

✅ **MAC-Adresse = Hardware-Adresse** (48 Bit, hexadezimal).  
✅ Wird im **LAN für die direkte Kommunikation** verwendet (Schicht 2).  
✅ Bestimmt durch **OUI (Hersteller) + Geräte-ID**.  
✅ **ARP übersetzt IP → MAC**, DHCP nutzt MAC für IP-Vergabe.  
✅ Kann **gespooft** werden, ist aber normalerweise fest.  

### Beispiel aus der Praxis:

1. Du gibst `google.de` ein → DNS liefert die IP `142.250.185.3`.
2. Dein PC prüft: **Ist die IP im lokalen Netz?** → Nein, also geht das Paket an den Router (Gateway).
3. Der Router nutzt **ARP**, um die MAC-Adresse des nächsten Hops zu finden.
4. Das Paket wird über **Switches (die MAC-Adressen lernen)** weitergeleitet.

