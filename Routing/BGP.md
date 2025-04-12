# Theorie
BGP ist das **wichtigste Routing-Protokoll für das Internet** und gehört zur **Klasse der Path-Vector-Protokolle**. Es unterscheidet sich stark von OSPF oder RIP, da es nicht für interne Netzwerke (IGPs), sondern für das **Routing zwischen autonomen Systemen (AS)** genutzt wird.

Ein **Autonomes System (AS)** ist ein unabhängig verwaltetes Netzwerk oder eine Gruppe von Netzwerken mit einer eigenen Routing-Politik, das über eine eindeutige AS-Nummer (ASN) identifiziert wird und mithilfe von BGP mit anderen AS kommuniziert, um den globalen Internetverkehr zu steuern.
## BGP-Typen: iBGP & BGP
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
## Router Reflector (RR)
**Problem bei iBGP:**  
iBGP benötigt eine **vollständige Vermaschung**, da iBGP-Router untereinander keine Routen weitergeben dürfen (kein Split-Horizon für iBGP).

> split horizon erklären 

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

# Config (eBGP)
![](../images/Untitled-1.png)

## G1
```
/system identity
set name=G1

/interface bridge
add name=br

/interface wireless
set [ find default-name=wlan1 ] ssid=MikroTik
set [ find default-name=wlan2 ] ssid=MikroTik

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip hotspot profile
set [ find default=yes ] html-directory=hotspot

/interface bridge port
add bridge=br interface=ether4
add bridge=br interface=ether5

/ip address
add address=10.0.0.2/30 interface=ether1 network=10.0.0.0
add address=10.0.0.14/30 interface=ether2 network=10.0.0.12
add address=10.0.0.18/30 interface=ether3 network=10.0.0.16
add address=192.168.10.1/24 interface=br network=192.168.10.0

/routing bgp connection
add as=1000 local.role=ebgp name=toM1 output.redistribute=connected remote.address=10.0.0.1
add as=1000 local.role=ebgp name=toM2 output.redistribute=connected remote.address=10.0.0.17
add as=1000 local.role=ebgp name=toG2 output.redistribute=connected remote.address=10.0.0.13

/routing filter rule
add chain=outG2 disabled=no rule="if ( bgp-output-local-addr == 10.0.0.14) { s\
    et bgp-local-pref 105; accept }"
add chain=inG2 disabled=no rule="if ( bgp-input-local-addr == 10.0.0.14) { set\
    \_bgp-local-pref 105; accept }"
```

## G2
```
/system identity
set name=G2

/interface bridge
add name=brt

/interface wireless
set [ find default-name=wlan1 ] ssid=MikroTik
set [ find default-name=wlan2 ] ssid=MikroTik

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/interface bridge port
add bridge=brt interface=ether3
add bridge=brt interface=ether4
add bridge=brt interface=ether5

/ip address
add address=10.0.0.10/30 interface=ether1 network=10.0.0.8
add address=10.0.0.13/30 interface=ether2 network=10.0.0.12
add address=192.168.40.1/24 interface=brt network=192.168.40.0

/routing bgp connection
add as=4000 disabled=no input.filter=inM2 local.role=ebgp name=toM2 \
    output.filter-chain=outM2 .redistribute=connected remote.address=\
    10.0.0.9/32 routing-table=main
add as=4000 disabled=no input.filter=inG1 local.role=ebgp name=toG1 \
    output.filter-chain=outG1 .redistribute=connected remote.address=\
    10.0.0.14/32 routing-table=main
    
/routing filter rule
add chain=outM2 disabled=no rule="if ( bgp-output-local-addr == 10.0.0.10 ) { \
    set bgp-local-pref 105; accept }"
add chain=inM2 disabled=no rule="if ( bgp-input-local-addr == 10.0.0.10 ) { se\
    t bgp-local-pref 105; accept }"
add chain=outG1 disabled=no rule="if ( bgp-output-local-addr == 10.0.0.13) { s\
    et bgp-local-pref 105; accept }"
add chain=inG1 disabled=no rule="if ( bgp-input-local-addr == 10.0.0.13) { set\
    \_bgp-local-pref 105; accept }"
```

## M1

```
/system identity
set name=M1

/interface bridge
add name=br

/interface wireless
set [ find default-name=wlan1 ] ssid=MikroTik
set [ find default-name=wlan2 ] ssid=MikroTik

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/interface bridge port
add bridge=br interface=ether3
add bridge=br interface=ether4
add bridge=br interface=ether5

/ip address
add address=10.0.0.6/30 interface=ether2 network=10.0.0.4
add address=10.0.0.1/30 interface=ether1 network=10.0.0.0
add address=192.168.20.1/24 interface=br network=192.168.20.0

/routing bgp connection
add as=2000 local.role=ebgp name=toG1 output.redistribute=connected \
    remote.address=10.0.0.2
add as=2000 local.role=ebgp name=toM2 output.redistribute=connected \
    remote.address=10.0.0.5
    
/system logging
add topics=bgp
add topics=route
```

## M2

```
/system identity
set name=M2

/interface bridge
add name=br

/interface wireless
set [ find default-name=wlan1 ] ssid=MikroTik
set [ find default-name=wlan2 ] ssid=MikroTik

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/interface bridge port
add bridge=br interface=ether4
add bridge=br interface=ether5

/ip address
add address=10.0.0.5/30 interface=ether1 network=10.0.0.4
add address=10.0.0.17/30 interface=ether3 network=10.0.0.16
add address=10.0.0.9/30 interface=ether2 network=10.0.0.8
add address=192.168.30.1/24 interface=br network=192.168.30.0

/routing bgp connection
add as=3000 local.role=ebgp name=toM1 output.redistribute=connected \
    remote.address=10.0.0.6
add as=3000 disabled=no input.filter=inG2 local.role=ebgp name=toG2 \
    output.filter-chain=mychain .redistribute=connected remote.address=\
    10.0.0.10/32 routing-table=main
add as=3000 local.role=ebgp name=toG1 output.redistribute=connected \
    remote.address=10.0.0.18
    
/routing filter rule
add chain=mychain disabled=no rule="if ( bgp-output-local-addr == 10.0.0.9 ) {\
    \_set bgp-local-pref 105; accept }"
add chain=inG2 disabled=no rule="if ( bgp-input-local-addr == 10.0.0.9 ) { set\
    \_bgp-local-pref 105; accept }"
```