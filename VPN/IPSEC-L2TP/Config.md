![[Images/site2siteExample.drawio.png]]

**VPN-Server**
```
/interface l2tp-server server
set enabled=yes ipsec-secret=mySecret use-ipsec=required
/ppp secret
add local-address=10.0.0.2 name=user1 password=Gantner123 remote-address=10.0.0.1 service=l2tp
/ip route
add dst-address=192.168.20.0/24 gateway=10.0.0.1
```

**VPN-Client**
```
/interface l2tp-client
add connect-to=193.82.200.13 disabled=no name=l2tp-client use-ipsec=required user=user1 password=Gantner123
/ip route
add dst-address=192.168.10.0/24 gateway=l2tp-client
```
