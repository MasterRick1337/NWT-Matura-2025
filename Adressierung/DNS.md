Das **Domain Name System (DNS)** ist eine der grundlegenden Technologien des Internets. Es sorgt dafür, dass **Domainnamen** (wie `www.google.com`) in **IP-Adressen** (wie `142.250.184.196`) übersetzt werden, die von Computern zur Kommunikation genutzt werden. DNS arbeitet auf der obersten Ebene des OSI-Modells und verwendet die Protokolle TCP und UDP über den Port 53, um Daten zu übertragen. Dadurch müssen wir uns keine komplexen IP-Adressen merken, sondern nur die für uns verständlicheren Domainnamen.

Vor der Einführung von DNS gab es das **Hosts-Datei-System**. In diesem System wurden alle Domainnamen und ihre zugehörigen IP-Adressen in einer Datei namens **hosts.txt** gespeichert.

## 1. Struktur und Hierarchie des DNS

Ein Domainname besteht aus mehreren Ebenen, die eine hierarchische Struktur bilden. 

**Beispiel:** `www.google.at`

- **at** → _Top-Level-Domain (TLD)_
	- Sie werden in zwei Hauptkategorien unterteilt:
	    - **Organisatorische TLDs**: `net`, `org`, `gov`
	    - **Länderspezifische TLDs**: `at`, `de`, `uk` 
        
- **google** → _Second-Level-Domain (SLD)_
    - Meist Name der Organisation/Webseite 
    - Feste SLDs: `ac` (akademisch), `co` (kommerziell)
        
- **www** → _Hostname / Subdomain_
    - Kennzeichnet Dienste innerhalb der Domain  
    - Typische Subdomains zur Kennzeichnung von Diensten sind:
        - **www** → für den Webserver (z.B. `www.google.com`)
        - **mail** → für den Mailserver (z.B. `mail.example.com`)
        - **ftp** → für den FTP-Server (z.B. `ftp.example.com`)
---

## 2. Funktionsweise des DNS

Wenn man eine Website wie `www.google.com` aufruft, erfolgt eine mehrstufige DNS-Anfrage:

### 1. DNS-Resolver (lokaler Client)

- Der Resolver prüft zunächst den lokalen DNS-Cache.
    - Ist der Eintrag vorhanden, wird die IP-Adresse direkt zurückgegeben.
    - Ist der Eintrag nicht im Cache, wird die Anfrage an die DNS-Hierarchie weitergeleitet.
### 2. Rekursive Anfrage an den DNS-Server

- Falls der DNS-Resolver den Eintrag nicht im lokalen Cache hat, sendet er eine **rekursive Anfrage** an einen DNS-Server. Der Server übernimmt die vollständige Suche nach der IP-Adresse der Domain.
### 3. Abfrage an die Root-Nameserver

- Da der DNS-Server nicht direkt weiß, welche IP-Adresse zu `google.at` gehört, sendet er eine Anfrage an einen der 13 **Root-DNS-Server** weltweit. 13 IPs aber >> 13 Server.
### 4. Abfrage an den TLD-Nameserver

- Der DNS-Server fragt nun den **TLD-Nameserver** (z.B. für `.com`), um den zuständigen **Authoritativen Nameserver** für die Domain zu finden (z.B. für `google`).
    
### 5. Abfrage an den Authoritative Nameserver

- Der DNS-Server fragt den **authoritativen Nameserver** der Domain (z.B. den Server, der für `google.com` zuständig ist), um die endgültige IP-Adresse zu erhalten.

### 6. Antwort an den Client
Der DNS-Server gibt schließlich die IP-Adresse an den DNS-Resolver zurück, der sie an den ursprünglichen Client (deinen Computer) weitergibt.

---

## 3. DNS Resource Records (RR)

Ein **Resource Record** ist wie ein kleiner „Eintrag“ in einem DNS-System. Jeder Eintrag etwas wichtiges über eine Domain, zum Beispiel:

| Record    | Beschreibung                                | Beispiel                                                                                       |
| --------- | ------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **A**     | IPv4-Adresse für eine Domain                | `example.com. IN A 192.168.1.1`                                                                |
| **AAAA**  | IPv6-Adresse für eine Domain                | `example.com. IN AAAA 2001:0db8::`                                                             |
| **CNAME** | Alias für eine andere Domain                | `www.example.com. IN CNAME example.com.`                                                       |
| **MX**    | Zuständiger Mailserver für eine Domain      | `example.com. IN MX 10 mailserver.example.com.`                                                |
| **NS**    | Zuständiger Nameserver                      | `example.com. IN NS ns1.example.com.`                                                          |
| **SRV**   | Server + Port für einen Dienst (z. B. SIP)  | `_sip._tcp.example.com. IN SRV 10 5 5060 sipserver.example.com.`                               |
| **TXT**   | Textinformationen (z. B. SPF-Konfiguration) | `example.com. IN TXT "v=spf1 ip4:192.168.1.1 -all"`                                            |
| **SOA**   | Zoneninformationen (Start of Authority)     | `example.com. IN SOA ns1.example.com. admin.example.com. (2023050801 7200 3600 1209600 86400)` |

---

## 4. Öffentliche DNS-Resolver

Es gibt mehrere öffentliche DNS-Resolver, die du für schnelleren und sichereren Zugriff auf das Internet verwendet werden können:

| Anbieter       | IPv4-Adresse | Besonderheiten                 |
| -------------- | ------------ | ------------------------------ |
| **Google**     | `8.8.8.8`    | Schnell, weit verbreitet       |
| **Cloudflare** | `1.1.1.1`    | Datenschutzfokussiert          |
| **Quad9**      | `9.9.9.9`    | Blockiert bekannte Schadseiten |

---

## 5. DNS und Sicherheit

### 1. DNS-Spoofing / Cache Poisoning

**DNS-Spoofing** ist ein Angriff, bei dem **gefälschte DNS-Antworten** in den Cache eines DNS-Resolvers eingeschleust werden.

**Ziel:** Der Benutzer wird auf eine **gefälschte Website** umgeleitet, obwohl er die richtige Adresse eingegeben hat.

**Ablauf:**

- Ein Angreifer sendet manipulierte DNS-Antworten.
    
- Der DNS-Resolver speichert die gefälschte Antwort im Cache.
    
- Künftige Anfragen an z. B. `bank.com` werden zur **Phishing-Seite** umgeleitet.
    
**Gefahr:** Die URL im Browser bleibt unverändert, was den Angriff schwer erkennbar macht.

### 2. DNSSEC (Domain Name System Security Extensions)

**DNSSEC** ist eine Erweiterung des DNS, die zur **Sicherung** von DNS-Anfragen dient und Angriffe wie DNS-Spoofing verhindert.

**Ziel:** Sicherstellen, dass DNS-Antworten **authentisch und unverändert** sind.

**Ablauf**

- Jede DNS-Zone kann ihre Daten **kryptografisch signieren**.
    
- Der autoritative Nameserver signiert seine DNS-Einträge mit einem privaten Schlüssel.
    
- Clients können mit dem öffentlichen Schlüssel prüfen, ob die Antwort manipuliert wurde.



##  6. DNS-Konfiguration auf einem MikroTik-Router

**DNS-Server festlegen**
```bash
/ip dns set servers=8.8.8.8
```

**Remote-Anfragen zulassen**
```bash
/ip dns set allow-remote-requests=yes
```

 **DNS-Cache konfigurieren**
```bash
/ip dns set cache-size=2048KiB
/ip dns set cache-max-ttl=1d
```

**Statische DNS-Einträge hinzufügen**
```bash
/ip dns static add name=www.example.com address=192.168.1.100
```

**Erlaubt DNS-Anfragen von Geräten im Netzwerk**
```bash
/ip dns set allow-remote-requests=yes
```

**Leitet DNS-Anfragen an andere Server weiter**
```bash
/ip dns set forwarding-enabled=yes
```

**Aktuelle DNS-Konfiguration überprüfen**
```bash
/ip dns print
```

 **DNS über HTTPS (DoH) aktivieren**

**1. Zertifikat importieren**
```bash
/certificate import file-name=example.crt
```

**2. DoH-Server aktivieren und Zertifikat verifizieren**
```bash
/ip dns set use-doh-server="https://example" verify-doh-cert=yes
```




