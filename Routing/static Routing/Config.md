![](SR1.png)
Router 1:
```
/ip address 
add address=10.1.1.2 interface=ether1
add address=172.16.1.1/30 interface=ether2
add address=192.168.1.1/24 interface=ether3

/ip route 
add gateway=10.1.1.1
add dst-address=192.168.2.0/24 gateway=172.16.1.2
```
Router 2:
```
/ip address
add address=172.16.1.2/30 interface=ether1
add address=192.168.2.1/24 interface=ether2

/ip route 
add gateway=172.16.1.1
```