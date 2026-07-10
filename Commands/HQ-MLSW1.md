## HQ-MLSW1

```
enable
configure terminal

service password-encryption
hostname HQ-MLSW1
enable password cisco

no ip cef
ip routing
no ipv6 cef

username cisco password cisco

ip ssh version 2
no ip domain-lookup
ip domain-name cisco.com

spanning-tree mode pvst

interface Port-channel1
 switchport mode trunk
 exit

interface Port-channel2
 exit

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.20.20.33 255.255.255.252
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

interface GigabitEthernet1/0/6
 switchport mode trunk
 exit

interface GigabitEthernet1/0/10
 switchport access vlan 50
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface GigabitEthernet1/0/21
 switchport mode trunk
 channel-group 1 mode active
 exit

interface GigabitEthernet1/0/22
 switchport mode trunk
 channel-group 1 mode active
 exit

interface GigabitEthernet1/0/23
 switchport mode trunk
 channel-group 1 mode active
 exit

interface Vlan1
 no ip address
 shutdown
 exit

interface Vlan10
 mac-address 00e0.b0a3.7301
 ip address 192.168.10.3 255.255.255.0
 ip helper-address 10.20.20.5
 ip helper-address 10.20.20.6
 standby 10 ip 192.168.10.1
 exit

interface Vlan20
 mac-address 00e0.b0a3.7302
 ip address 172.16.0.3 255.255.255.0
 ip helper-address 10.20.20.6
 ip helper-address 10.20.20.5
 standby 20 ip 172.16.0.1
 exit

interface Vlan50
 mac-address 00e0.b0a3.7303
 ip address 10.10.0.3 255.255.0.0
 ip helper-address 10.20.20.6
 ip helper-address 10.20.20.5
 standby 50 ip 10.10.0.1
 exit

router ospf 15
 router-id 2.1.2.1
 log-adjacency-changes
 network 10.20.20.32 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 172.16.0.0 0.0.255.255 area 0
 network 10.10.0.0 0.0.255.255 area 0
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
