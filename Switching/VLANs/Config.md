![](vlanConf.png)


In diesem Beispiel werden wir zwei Netze anlegen:

| Schüler Netz              | Admin Netz                |
| ------------------------- | ------------------------- |
| VLAN-ID 20                | VLAN-ID 10                |
| Netzwerk  192.168.20.0/24 | Netzwerk  192.168.10.0/24 |
#### Konfiguration
**Router**
```
# VLAN Bridge erstellen
/interface bridge
add name=br vlan-filtering=yes

# Bridge-Ports definieren
/interface bridge port
add bridge=br interface=ether1

# VLANs auf der Bridge taggen
/interface bridge vlan
add bridge=br tagged=ether1 vlan-ids=10,20

# VLAN-Interfaces auf dem Router
/interface vlan
add interface=br name=vlan10 vlan-id=10
add interface=br name=vlan20 vlan-id=20

# IP-Adressen für VLANs
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

**Switch**
```
# VLAN Bridge erstellen
/interface bridge
add name=bridge_vlan protocol-mode=none vlan-filtering=yes

# Bridge-Ports definieren
/interface bridge port
add bridge=bridge_vlan interface=ether1
add bridge=bridge_vlan interface=ether2
add bridge=bridge_vlan interface=ether3
add bridge=bridge_vlan interface=ether4

# VLANs auf der Bridge taggen
/interface bridge vlan
add bridge=bridge_vlan tagged=ether1 untagged=ether2,ether3 vlan-ids=20
add bridge=bridge_vlan tagged=ether1 untagged=ether4 vlan-ids=10
```
