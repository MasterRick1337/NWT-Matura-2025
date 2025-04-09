# Theorie
OpenVPN is eine Open-Source-Software für virtuelle private Netzwerke (VPN), die eine sichere Kommunikation über das Internet ermöglicht, indem sie eine private und verschlüsselte Verbindung zwischen zwei Punkten herstellt. Es wird weitgehend für die Einrichtung von sicheren Punkt-zu-Punkt- oder Standort-zu-Standort-Verbindungen in gerouteten oder gebrückten Konfigurationen sowie für Remote-Zugriffseinrichtungen verwendet.
## Funktionen und Aspekte von OpenVPN:
1. **Sicherheit:** OpenVPN verwendet ein benutzerdefiniertes Sicherheitsprotokoll, das OpenSSL zur Verschlüsselung nutzt. Es unterstützt verschiedene Verschlüsselungsalgorithmen und Authentifizierungsmethoden, was ein hohes Maß an Sicherheit gewährleistet.
2. **Plattformübergreifend:** OpenVPN ist mit (fast) allen Betriebssystemen kompatibel, zB.: Windows, macOS, Linux, iOS und Android.
3. **Skalierbarkeit:** OpenVPN kann für verschiedene Szenarien konfiguriert werden, von kleinen persönlichen VPNs bis hin zu unternehmensweiten Implementierungen. Es unterstützt sowohl Site-to-Site-VPNs als auch Remote-Zugriffs-VPNs.
4. **SSL/TLS für den Schlüsselaustausch:** OpenVPN verwendet SSL/TLS-Protokolle für den Schlüsselaustausch und fügt den VPN-Verbindungen eine zusätzliche Sicherheitsebene hinzu.

Insgesamt ist OpenVPN eine beliebte Wahl für Einzelpersonen, Unternehmen und Organisationen, die sichere und private Kommunikationskanäle über das Internet herstellen möchten. Es wird häufig verwendet, um VPNs für den Remote-Zugriff, sichere Kommunikation zwischen Zweigstellen oder den Zugriff auf Ressourcen über das Internet auf sichere Weise einzurichten.

![](../images/openvpn.png)

# Config
![](../images/ovpn_site2site.png)
## Server

**Create a certificate**
```
/certificate/add name=ca common-name=ca key-usage=crl-sign,key-cert-sign
/certificate/sign ca
/certificate/add name=myservercert
common-name=myservercert
/certificate/sign myservercert ca=ca
```

**Create a tunnel on ether5**
```
/ip/address/add interface=ether5 address=10.0.0.1/24
/interface/ovpn-server/add name=ovpn user=site1 disabled=no
/interface/ovpn-server/server/set enabled=yes certificate=myservercert
/ppp/secret/add local-address=192.168.30.1 name=site1 password=letmein123 remote-address=192.168.30.2
```
## Client

**Create a tunnel on ether 5**
```
/ip/address/add interface=ether5 address=10.0.0.2/24
/interface/ovpn-client/add connect-to=10.0.0.1 password=letmein123 user=site1 disabled=no
```