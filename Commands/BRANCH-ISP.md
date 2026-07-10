## BRANCH-ISP

```
enable
configure terminal

no ip cef
no ipv6 cef

spanning-tree mode pvst

interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
 shutdown
 exit

interface GigabitEthernet0/1
 ip address 205.200.100.1 255.255.255.252
 duplex auto
 speed auto
 exit

interface GigabitEthernet0/2
 no ip address
 duplex auto
 speed auto
 shutdown
 exit

interface Serial0/3/0
 ip address 30.30.30.2 255.255.255.252
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

router ospf 15
 router-id 9.1.9.1
 log-adjacency-changes
 network 205.200.100.0 0.0.0.3 area 0
 network 30.30.30.0 0.0.0.3 area 0
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
