## IT-SW

```
enable
configure terminal

service password-encryption
hostname IT-SW
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
 switchport access vlan 20
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface range FastEthernet0/21 - 24
 switchport access vlan 50
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

interface GigabitEthernet0/1
 switchport access vlan 10
 switchport mode access
 exit

interface GigabitEthernet0/2
 switchport access vlan 10
 switchport mode access
 exit

interface Vlan1
 no ip address
 shutdown
 exit

banner motd ^CINHOTHORIZED ACCESS IS PROHIBITED !!^C

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

*Remarques sur IT-SW : contrairement aux autres switchs d'accès, les ports GigabitEthernet0/1 et 0/2 ne sont **pas** en `shutdown` (ils sont actifs), et le banner ne contient que deux points d'exclamation (`!!`) au lieu de trois (`!!!`) — différences reproduites fidèlement depuis la config d'origine.*

---

### Remarques générales

- Les mots de passe/enable password apparaissent chiffrés (`password 7 ...`) dans les running-config d'origine ; j'ai remis `cisco` en clair partout car c'est le mot de passe classique utilisé dans les labs NetAcad — à adapter si ce n'est pas le bon.
- Sur les ASA, `nat` et les ACL doivent être saisis dans cet ordre (objets NAT avant `access-group`, ACL avant `crypto map ... match address`).
- Pense à faire `write memory` (ou `copy run start`) à la fin sur chaque device pour sauvegarder.
- Le tunnel VPN L2L (HQ-FWL ↔ BRANCH-FWL) et les adjacences OSPF dépendent de la connectivité IP de base : configure d'abord les interfaces et le routage avant les ACL/VPN pour éviter des erreurs de dépendance en Packet Tracer/GNS3.
