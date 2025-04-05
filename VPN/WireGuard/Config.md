![[wireguardS2S.png]]

**Router West**
```
interface/wireguard/add name=West
ip/address add address=10.0.1.1/24 interface=West
interface/wireguard/peers add interface=West public-key="0tcvcpdup2e8MTJhPyzK7F/USrKjnJbzCfQKGI7VlwA=" endpoint-address=59.61.225.141 endpoint-port=13231 allowed-address=10.0.1.0/24
```

**Router East**
```
interface/wireguard/add name=East
ip/address add address=10.0.1.2/24 interface=East
interface/wireguard/peers add interface=East public-key="JXx83B44YTdZXgUyBgkoJHqGEt3ZO5Mra61C+EW1XXk=" endpoint-address=173.52.12.153 endpoint-port=13231 allowed-address=10.0.1.0/24
```

Es werden automatisch public und private Key zur Verschlüsselung der Verbindung zwischen Geräten generiert

West-Router:
```
[admin@ROS7West] > interface/wireguard/print
Flags: X - disabled; R - running 
 0  R name="West" mtu=1420 listen-port=13231 private-key="uEUUufex2/C36952pAj2+8Vq2/Z3Ovf+g59QoYTYbEg=" public-key="JXx83B44YTdZXgUyBgkoJHqGEt3ZO5Mra61C+EW1XXk="
```

East-Router:
```
 [admin@ROS7East] > interface/wireguard/print
Flags: X - disabled; R - running 
 0  R name="East" mtu=1420 listen-port=13231 private-key="SP5dLklX/7dRrfEmequW+bJVisAx4ajMF/VzdYHv30U=" public-key="0tcvcpdup2e8MTJhPyzK7F/USrKjnJbzCfQKGI7VlwA="
```