![[Untitled.png]]
### Router 1:
```
/system 
identity set name=Router1

/ip address
add address=10.0.0.1/30 interface=ether1 network=10.0.0.0
add address=10.0.0.6/30 interface=ether2 network=10.0.0.4
add address=192.168.10.1/24 interface=ether5 network=192.168.10.0

/routing ospf instance 
add disabled=no name=OSPFInst1 redistribute=connected router-id=10.0.0.1

/routing ospf area 
add disabled=no instance=OSPFInst1 name=backbone

/routing ospf interface-template 
add area=backbone cost=15 disabled=no interfaces=ether1 
add area=backbone cost=15 disabled=no interfaces=ether2
```
Hier sind alle Router in der Backbone-Area (Area-0) da sie alle wichtig sind.