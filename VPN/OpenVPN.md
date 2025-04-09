# Theorie
**OpenVPN** ist eine Open-Source-Software zur Einrichtung von **virtuellen privaten Netzwerken (VPNs)**. Sie ermöglicht eine **sichere, verschlüsselte Verbindung** zwischen zwei oder mehreren Netzwerkknoten – ideal für den privaten Gebrauch, Unternehmen oder Organisationen, die Daten über das Internet sicher übertragen möchten.
## Was ist ein VPN überhaupt?
Ein VPN (Virtual Private Network) ist eine geschützte Netzwerkverbindung über ein öffentliches Netzwerk wie das Internet. Dabei wird ein "Tunnel" erstellt, durch den Daten verschlüsselt übertragen werden – so können z. B. Außendienstmitarbeiter sicher auf das Firmennetz zugreifen oder geografische Einschränkungen umgangen werden.
## Funktionen und Aspekte von OpenVPN:
1. **Sicherheit:**
   OpenVPN verwendet das **OpenSSL-Toolkit** für starke **Verschlüsselung (z. B. AES-256)** und **Authentifizierung** (z. B. mit Zertifikaten oder Benutzernamen/Passwort). Es gilt als eines der sichersten VPN-Protokolle überhaupt.
2. **Plattformunabhängig:**  
   Verfügbar für **Windows, macOS, Linux, iOS, Android** – auch Router (z. B. MikroTik) können OpenVPN einsetzen.
3. **Flexible Konfiguration:**
   - **Site-to-Site:** Verbindet z. B. zwei Standorte (Filialen) miteinander.
   - **Remote Access:** Einzelne Clients (z. B. Mitarbeiter) verbinden sich von unterwegs aus mit dem Firmennetzwerk.
4. **SSL/TLS-Verschlüsselung:**  
   Der Schlüsselaustausch erfolgt über **SSL/TLS**, wodurch Abhörsicherheit und Integrität gewährleistet werden.
5. **Firewall-freundlich:**  
   OpenVPN nutzt wahlweise **UDP oder TCP** und läuft auf Port **1194** (Standard), lässt sich aber auch auf Port 443 umstellen, um Firewalls zu umgehen.
### So funktioniert OpenVPN
![](../images/openvpn.png)
1. **Dein Gerät** verbindet sich mit einem **VPN-Client**.
2. Der VPN-Client verschlüsselt die Daten mithilfe sicherer Protokolle.
3. Diese verschlüsselten Daten werden durch einen **VPN-Tunnel** gesendet – dadurch entsteht **mehr Privatsphäre und Sicherheit**.
4. Der **VPN-Server** empfängt die Daten, entschlüsselt sie und leitet sie ins Internet weiter. Dabei wird deine IP-Adresse verborgen.
5. **Wer wird ausgesperrt?** Werbeunternehmen, Hacker und staatliche Überwachung sehen nur verschlüsselte Daten.
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