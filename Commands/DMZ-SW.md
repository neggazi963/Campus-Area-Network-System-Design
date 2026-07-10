## DMZ-SW

```
enable
configure terminal

service password-encryption
hostname DMZ-SW
enable password cisco

ip ssh version 2
no ip domain-lookup
ip domain-name cisco.com

username cisco privilege 1 password cisco

spanning-tree mode pvst
spanning-tree extend system-id

interface FastEthernet0/1
 exit

interface range FastEthernet0/2 - 24
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface GigabitEthernet0/1
 exit

interface GigabitEthernet0/2
 exit

interface Vlan1
 no ip address
 shutdown
 exit

banner motd ^CINHOTHORIZED ACCESS IS PROHIBITED !!!^C

end
write memory
```

*Note : ce switch n'a pas d'`access-list 2`, pas de `line vty`/`line con` configurées explicitement dans la running-config d'origine — il n'y a donc rien à ajouter pour ces parties (elles restent aux valeurs par défaut).*

---
