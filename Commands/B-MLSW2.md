## B-MLSW2

```
enable
configure terminal

service password-encryption
hostname B-MLSW2
enable password cisco

no ip cef
ip routing
no ipv6 cef

username cisco password cisco

ip ssh version 2
no ip domain-lookup
ip domain-name cisco.com

spanning-tree mode pvst

interface Port-channel2
 switchport mode trunk
 exit

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.20.20.45 255.255.255.252
 duplex auto
 speed auto
 exit

interface GigabitEthernet1/0/2
 switchport mode trunk
 exit

interface GigabitEthernet1/0/3
 switchport mode trunk
 exit

interface GigabitEthernet1/0/4
 switchport mode trunk
 exit

interface GigabitEthernet1/0/5
 switchport mode trunk
 exit

interface GigabitEthernet1/0/21
 switchport mode trunk
 channel-group 2 mode passive
 exit

interface GigabitEthernet1/0/22
 switchport mode trunk
 channel-group 2 mode passive
 exit

interface GigabitEthernet1/0/23
 switchport mode trunk
 channel-group 2 mode passive
 exit

interface Vlan1
 no ip address
 shutdown
 exit

interface Vlan60
 mac-address 0001.c90a.7e01
 ip address 172.17.0.2 255.255.0.0
 ip helper-address 10.20.20.5
 ip helper-address 10.20.20.6
 standby 60 ip 172.17.0.1
 exit

interface Vlan90
 mac-address 0001.c90a.7e02
 ip address 10.11.0.2 255.255.0.0
 ip helper-address 10.20.20.5
 ip helper-address 10.20.20.6
 standby 90 ip 10.11.0.1
 exit

router ospf 15
 router-id 5.1.5.1
 log-adjacency-changes
 network 10.20.20.44 0.0.0.3 area 0
 network 172.17.0.0 0.0.255.255 area 0
 network 10.11.0.0 0.0.255.255 area 0
 exit

ip classless
ip flow-export version 9

access-list 2 permit 0.0.0.0 255.255.255.0
access-list 2 deny any

banner motd ^CINHOTHORIZED ACCESS IS PROHIBITED !!!^C

line con 0
 exec-timeout 3 0
 password cisco
 login
 exit

line vty 0 4
 access-class 2 in
 login local
 transport input ssh
 exit

line vty 5 15
 access-class 2 in
 login local
 transport input ssh
 exit

end
write memory
```

---
