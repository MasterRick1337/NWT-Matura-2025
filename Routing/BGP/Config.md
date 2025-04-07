![[Images/Untitled-1.png]]
```
/system identity
set name=G1

/interface bridge
add name=br

/ip pool
add name=pool1 ranges=192.168.10.10-192.168.10.200

/ip dhcp-server
add address-pool=pool1 interface=br name=dhcp1

/interface bridge port
add bridge=br interface=ether4
add bridge=br interface=ether5

// BGP mit OSPF - ab hier

/ip address
add address=10.0.0.2/30 interface=ether1 network=10.0.0.0
add address=10.0.0.14/30 interface=ether2 network=10.0.0.12
add address=10.0.0.18/30 interface=ether3 network=10.0.0.16
add address=192.168.10.1/24 interface=br network=192.168.10.0

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1

/routing bgp connection
add as=1000 local.address=10.0.0.2 .role=ebgp name=bgpConnM1 output.redistribute=\
    connected remote.address=10.0.0.1 router-id=10.0.0.2
add as=1000 local.address=10.0.0.18 .role=ebgp name=bgpConnM2 output.redistribute=\
    connected remote.address=10.0.0.17 router-id=10.0.0.2
add as=1000 local.address=10.0.0.14 .role=ebgp name=bgpConnG2 output.redistribute=\
    connected remote.address=10.0.0.13 router-id=10.0.0.2
```
