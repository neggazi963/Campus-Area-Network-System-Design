## B-ENG-SW

```
enable
configure terminal

service password-encryption
hostname B-ENG-SW
enable password cisco

ip ssh version 2
no ip domain-lookup
ip domain-name cisco.com

username cisco privilege 1 password cisco

spanning-tree mode pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport mode trunk
 exit

interface FastEthernet0/2
 switchport mode trunk
 exit

interface range FastEthernet0/3 - 20
 switchport access vlan 60
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface range FastEthernet0/21 - 24
 switchport access vlan 90
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface GigabitEthernet0/1
 switchport access vlan 199
 switchport mode access
 shutdown
 exit

interface GigabitEthernet0/2
 switchport access vlan 199
 switchport mode access
 shutdown
 exit

interface Vlan1
 no ip address
 shutdown
 exit

banner motd ^CINHOTHORIZED ACCESS IS PROHIBITED !!!^C

access-list 2 permit 0.0.0.0 255.255.255.0
access-list 2 deny any

line con 0
 password cisco
 login
 exec-timeout 3 0
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
