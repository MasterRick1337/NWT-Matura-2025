BGP ist das **wichtigste Routing-Protokoll für das Internet** und gehört zur **Klasse der Path-Vector-Protokolle**. Es unterscheidet sich stark von OSPF oder RIP, da es nicht für interne Netzwerke (IGPs), sondern für das **Routing zwischen autonomen Systemen (AS)** genutzt wird.

Ein **Autonomes System (AS)** ist ein unabhängig verwaltetes Netzwerk oder eine Gruppe von Netzwerken mit einer eigenen Routing-Politik, das über eine eindeutige AS-Nummer (ASN) identifiziert wird und mithilfe von BGP mit anderen AS kommuniziert, um den globalen Internetverkehr zu steuern.

### BGP-Typen: iBGP & BGP
| BGP-Typ             | Einsatzbereich                                 | Hop-Limit           | Best Path Selection                     |
| ------------------- | ---------------------------------------------- | ------------------- | --------------------------------------- |
| iBGP (Internal BGP) | Routing innerhalb eines autonomen Systems (AS) | Kein Hop-Limit      | Setzt bevorzugte externe eBGP-Routen um |
| eBGP (External BGP) | Routing zwischen autonomen Systemen (AS)       | Hop-Limit = 1 (TTL) | Sucht den besten Weg ins Internet       |
### eBGP (External BGP)
- Wird zwischen **verschiedenen AS** verwendet.
- Nutzt **AS-Path** als wichtigstes Attribut für die Routenwahl.
- Hat standardmäßig ein **TTL (Time-to-Live) von 1**, was bedeutet, dass die Nachbarn **direkt verbunden** sein müssen (kann mit `ebgp-multihop` erweitert werden).
### iBGP (Internal BGP)
- Wird **innerhalb desselben AS** verwendet.
- Funktioniert **nicht automatisch wie OSPF**, weil iBGP **kein Routing innerhalb des AS übernimmt**.
- iBGP-Router müssen **vollständig vermascht (Full-Mesh)** sein oder Route Reflectors (RR) nutzen.

### Router Reflector (RR)
**Problem bei iBGP:**  
iBGP benötigt eine **vollständige Vermaschung**, da iBGP-Router untereinander keine Routen weitergeben dürfen (kein Split-Horizon für iBGP).

**Lösung:**  
Route Reflectors (RR) reduzieren die Anzahl der iBGP-Verbindungen und erlauben eine **Hierarchie**.
- **RR empfängt iBGP-Routen** und kann sie an andere iBGP-Router weitergeben.
- Spart **Konfigurationsaufwand** und reduziert **Netzwerkkomplexität**.
```
    AS 65000
      |
+-------------------+
| Route Reflector  |
+-------------------+
    |       |
  R1       R2

```
- R1 und R2 müssen nicht direkt miteinander sprechen, sondern nur mit dem RR.

