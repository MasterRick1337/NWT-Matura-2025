OSPF (Open Shortest Path First) ist ein dynamisches Routing-Protokoll, das das **Link-State-Routing-Verfahren** verwendet.

### Grundprinzip
- Es nutzt den **Dijkstra-Algorithmus** (Shortest Path First, SPF), um die kürzesten Pfade zu berechnen.
- Es nutzt **Multicast (224.0.0.5 und 224.0.0.6)** für die Kommunikation zwischen Routern.

### Netzwerkausbau
- OSPF organisiert Netzwerke in **Areas**, um die Routing-Tabelle zu optimieren.
- **Area 0 (Backbone-Area)** ist die Hauptarea, zu der alle anderen Areas verbunden sein müssen.

- Routing Tabelle enthält Regeln darüber, wie Datenpakete von einem Netzwerk zum anderen weitergeleitet werden. Jeder Router oder Computer mit mehreren Netzwerkschnittstellen nutzt eine Routing-Tabelle, um zu entscheiden, wohin ein Paket gesendet werden soll. Areas sind dafür da um die Routing Tabelle kleiner und Router-ressourcensparender zu halten.

### Pakettypen - Ablauf
1. **Hello-Paket** – Dient zur Nachbarschaftsbildung.
2. **Database Description (DBD)** – Überträgt eine Zusammenfassung der LSDB.
3. **Link-State Request (LSR)** – Fordert fehlende Informationen an.
4. **Link-State Update (LSU)** – Enthält die vollständigen Routing-Informationen.
5. **Link-State Acknowledgment (LSAck)** – Bestätigung für empfangene LSAs.

### OSPF-Typen (LSA-Typen)
OSPF verwendet verschiedene LSA-Typen, um Routing-Informationen im Netzwerk auszutauschen. Diese Typen definieren, welche Informationen verteilt werden und wie die Netzwerkstruktur aussieht.

- **LSA Type 1 (Router LSA)** – Enthält Link-Informationen eines Routers innerhalb einer Area.
- **LSA Type 2 (Network LSA)** – Erstellt von DRs für Multiaccess-Netzwerke.
- **LSA Type 3 (Summary LSA)** – Wird von Area Border Routers (ABRs) erstellt, um Informationen zwischen Areas auszutauschen.
- **LSA Type 4 (ASBR Summary LSA)** – Zeigt den Weg zu einem ASBR an.
- **LSA Type 5 (External LSA)** – Wird von ASBRs generiert, um externe Routen (z. B. aus BGP) weiterzuleiten.