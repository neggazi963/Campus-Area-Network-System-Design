## HQ-ISP

```
enable
configure terminal

no ip cef
no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet0/0
 ip address 105.100.50.1 255.255.255.252
 duplex auto
 speed auto
 exit

interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
 exit

interface Serial0/3/0
 ip address 20.20.20.2 255.255.255.252
 clock rate 2000000
 exit

interface Serial0/3/1
 no ip address
 clock rate 2000000
 shutdown
 exit

interface Vlan1
 no ip address
 shutdown
 exit

interface Vlan10
 mac-address 0002.1646.cc01
 no ip address
 ip helper-address 10.20.20.5
 exit

router ospf 15
 router-id 7.1.7.1
 log-adjacency-changes
 network 105.100.50.0 0.0.0.3 area 0
 network 20.20.20.0 0.0.0.3 area 0
 exit

ip classless
ip flow-export version 9

line con 0
 exit

line vty 0 4
 login
 exit

end
write memory
```

---
