# Cisco Cheat Sheat
<!---
Author: Gregor Holzfeind
Version: 0.9.5
Licens: Creative Commons Namensnennung 4.0 International Lizenz.
-->


## Allgemeines

|Befehl                        | Beschreibung                                          | Beispiel                            | Ergebniss     |
|------------------------------|-------------------------------------------------------|-------------------------------------|---------------|
| `en`                         | Enable Modus                                          | `SW>en`                             | `SW#`         |
| `wr`                         | Schreibe Konfiguration                                | `SW#wr`                             | -             |
| `show run`                   | Anzeigen der Aktuellen Konfiguration                  | `SW#show run`                       | -             |
| `conf t`                     | Konfigurations Modus                                  | `SW#conf t`                         | `SW(confing)#`|
| `do show run`                | Anzeigen der Aktuellen Konfiguration im tieferen Modus| `sw(config)#do show run`            | -             | 
| `do wr`                      | Schreibe Konfiguration im Konfigurations Modus        | `SW(config)#do wr`                  | -             |
| `hostname`                   | Hostename                                             | `SW(config)#hostname SW1`           | `SW1(config)#`|
| `enabel`                     | Enable Password                                       | `SW(config)#enabel secret Passwort` | -             |
| `ctrl+z`                     | Rückkehr in den Enable Modus                          | `SW(config-if)#^Z     `             | `SW#`         |
| `show run | inc ip address`  | Anzeigen aller IP-Adressen                            | `SW#show run | inc ip address`      | -             |
| `show ip int brief`          | Anzeigen aller IP-Adressen bei Router                 | `SW#show ip int brief`              | -             |

***************************************

## Interface
| Interface             | Bedeutung                           | Abkürzung |
|-----------------------|-------------------------------------|-----------|
| FastEthernet 0/1      | erster Fastbit Anschluss ohne Modul | fa0/1     |
| FastEthernet 0/0/1    | erster Fastbit Anschluss mit Modul  | fa0/0/1   |
| GigabitEthernet 0/1   | erster Gigabit Anschluss ohne Modul | gi0/1     |
| GigabitEthernet 0/0/1 | erster Gigabit Anschluss mit Modul  | gi0/0/1   |
| port-channel l        | erster LACP                         | -         |

### Konfiguration IP-Adressen eines Router
    R1#conf t
    R1(config)#interface GigabitEthernet 0/0
    R1(config-if)#ip address 192.168.1.1 255.255.255.0
    R1(config-if)#no shutdown

### Konfiguration Port ausschalten bei einem Range
    SW1#conf
    SW1(config)#interface range GigabitEthernet 0/1-12
    SW1(config-if)#shutdown
   
### Konfiguration LACP
    SW1#conf
    SW1(config)#interface range FastEthernet 0/1-2
    SW1(config-if)#channel-groupe 1 mode activ
    SW1(config-if)#interface port-channel 1
    SW1(config-if)#switchport mod trunk

### Konfiguration Portsecurity
    SW1#conf
    SW1(config)#interface GigabitEthernet 0/4
    SW1(config-if)#switchport mode access   
    SW1(config-if)#switchport port-security   
    SW1(config-if)#switchport port-security mac-address 1234.5678.9ABC.EF12
    SW1(config-if)#interface GigabitEthernet 0/5
    SW1(config-if)#switchport mode access   
    SW1(config-if)#switchport port-security   
    SW1(config-if)#switchport port-security maximum 3
   
### Konfiguration DHCP-Snooping
   SW1#conf
   SW1(config)#ip dhcp snooping
   SW1(config)#interface GigabitEthernet 0/24
   SW1(config-if)#ip dhcp snooping trust
  
### Troubelshooting
    show arp
    show ip interface
    show ip route
    show ip dhcp snooping
    shwo etherchannel summary
    show port-security interface gi0/4

***************************************

## VLAN

### Trunktype
| Wert         | 802.1Q  | ISL      |
|--------------|---------|----------|
| Header Size  | 4 bytes | 26 bytes |
| Trailer Size | -       | 4 bytes  |
| Standard     | IEEE    | Cisco    |
| Max. VLANs   | 4094    | 1000     |

### VLAN Nummer nach Cisco
| ID        | Bedeutung   |
|-----------|-------------|
| 0         | Reseviert   |
| 1         | Default     |
| 1002      | fddi-default|
| 1003      | tr          |
| 1004      | fdnet       |
| 1005      | trnet       |
| 1006-4094 | Erweiterte  |
| 4095      | Reseviert   |

### Konfiguration
    SW1#conf t
    SW1(config)#vlan 100
    SW1(config-vlan)#name Server
    SW1(config-vlan)#vlan 101
    SW1(config-vlan)#name Client
    SW1(config-vlan)#exit
    SW1(config)#interface range fast 0/1-20
    SW1(config-if)#switchport mode access
    SW1(config-if)#switchport nonegotiate
    SW1(config-if)#switchport access vlan 101
    SW1(config-if)#interface range fast 0/21-24
    SW1(config-if)#switchport mode trunk
    SW1(config-if)#switchport trunk encapsulation dot1q
    SW1(config-if)#switchport trunk allowed vlan 101
    SW1(config-if)#switchport trunk native vlan 100
    SW1(config-if)#interface vlan100
    SW1(config-if)#ip address 192.168.100.2 255.255.255.0
    
### VTP
Für die Verwendung von VLAN ab 1005 muss folgender Befehl eingetragen werden:
`SW1(config)#vtp mode transparent`

    SW1#conf t
    SW1(config)#
    SW1(config)#vtp mode {server | client | transparent}
    SW1(config)#vtp domain <name>
    SW1(config)#vtp password <passsword>
    SW1(config)#vtp version {1 | 2}
    SW1(config)#vtp pruning
    
### Troubelshooting
    show vlan
    show interface [status | switchport]
    show interface trunk
    show vtp status
    show vtp password

***************************************

## Spanning Tree
| Attribut    |      STP    |      PVST  | PVST+       | RSTP                | RPVST+      | MST                 |
|-------------|-------------|------------|-------------|---------------------|-------------|---------------------|
| Algorithmus | Legacy ST   | Legacy ST  | Legac ST    | Rapid ST            | Rapid ST    | Rapid ST            |
| Standard    | 802.1D-1998 | Cisco      | Cisco       | 802.1w, 802.1D-2004 | Cisco       | 802.1s, 802.1Q-2003 |
| Instanzen   | 1           | 1 pro VLAN | 1 pro VLAN  | 1                   | 1 pro VLAN  | 1-mehre             |
| Trunk       | -           | ISL        | 802.1Q, ISL | -                   | 802.1Q, ISL | 802.1Q, ISL         |

### Link-Kosten
| Speed    | Kosten|
|----------|-------|
| 4 Mbps   | 250   |
| 10 Mbps  | 100   |
| 16 Mbps  | 62    |
| 45 Mbps  | 39    |
| 100 Mbps | 19    |
| 155 Mbps | 14    |
| 622 Mbps | 6     |
| 1 Gbps   | 4     |
| 10 Gbps  | 2     |
| 20+ Gbps | 1     |

### Priorität
1. 0 (Notfall Wert für neu Root-Bridge)
2. 4096 (tiefester Wert bei Normaler Konfiguration)
3. 8192
4. 12288
5. 20480
6. 24576
7. 28672
8. 32768 (Standard Wert)
9. 36864
10. 40960
11. 45056
12. 49152
13. 53248
14. 57344
15. 61440 (Maximal Wert)

### Bridge-ID
Die Bridge-ID ist wie folgt Zusammengesetz:
4 Bit Priorität + 12 System ID (VLAN) + 48 Bit MAC-Adresse

### Pfadentscheidung
1. Bridge mit der tiefesten ID wird Root-Bridge.
2. Switche mit den tieferen Pfadkosten zur Root-Bridge.
3. Switche mit der tieferen ID.
4. Tiefste Portnummer.

### Konfiguration RSTP, RPVST+
<!--Beispiel Switch
 _____    _____
| SW1 |--| SW2 |
|_____| ||_____|
        |   |
       _|___|_
      | SW 3  |
      |_______|
-->
#### Switch 1
    SW1#conf t
    SW1(config)spanning-tree mode rapid-pvst
    SW1(config)spanning-tree vlan 1 priority 4096
    SW1(config)interface range FastEthernet0/1-24
    SW1(config-if)spanning-tree portfast
    SW1(config-if)spanning-tree guard loop
    SW1(config-if)spanning-tree guard root
    SW1(config-if)spanning-tree bpduguard enable
    SW1(config-if)spanning-tree bpdufilter enable
    SW1(config-if)description Client
    SW1(config-if)interface range GigabitEthernet0/1-2
    SW1(config-if)no spanning-tree portfast
    SW1(config-if)spanning-tree guard root
    SW1(config-if)description Uplink
    SW1(config-if)do wr
#### Switch 2
    SW2#conf t
    SW2(config)spanning-tree mode rapid-pvst
    SW2(config)spanning-tree vlan 1 priority 32768
    SW2(config)interface range FastEthernet0/1-24
    SW2(config-if)spanning-tree portfast
    SW2(config-if)spanning-tree guard loop
    SW2(config-if)spanning-tree guard root
    SW2(config-if)spanning-tree bpduguard enable
    SW2(config-if)spanning-tree bpdufilter enable
    SW2(config-if)description Client
    SW2(config-if)interface range GigabitEthernet0/1-2
    SW2(config-if)no spanning-tree portfast
    SW2(config-if)description Uplink
    SW2(config-if)do wr
#### Switch 3
    SW3#conf t
    SW3(config)spanning-tree mode rapid-pvst
    SW3(config)spanning-tree vlan 1 priority 32768
    SW3(config)interface range FastEthernet0/1-24
    SW3(config-if)spanning-tree portfast
    SW3(config-if)spanning-tree guard loop
    SW3(config-if)spanning-tree guard root
    SW3(config-if)spanning-tree bpduguard enable
    SW3(config-if)spanning-tree bpdufilter enable
    SW3(config-if)description Client
    SW3(config-if)interface range GigabitEthernet0/1-2
    SW3(config-if)no spanning-tree portfast
    SW3(config-if)description Uplink
    SW3(config-if)do wr

### Troubelshooting
    show spanning-tree [summary | detail | root]
    show spanning-tree [interface | vlan]
    show spanning-tree mst [...]
***************************************

## PPP

### Konfiguration
<!--
 _____                   _____ 
| R1  |-198.51.100.0/24-| R2  |
|_____|                 |_____|

-->

#### Router 1
    R1#conf t
    R1(config)#username R2 password ppppwd123
    R1(config)#interface GigabitEthernet 0/1
    R1(config-if)#encapsulation ppp
    R1(config-if)#ppp authentication chap
    R1(config-if)#ip address 198.51.100.1 255.255.255.252
    R1(config-if)#no shutdown
	
#### Router 1
    R2#conf t
    R2(config)#username R1 password ppppwd123
    R2(config)#interface GigabitEthernet 0/1
    R2(config-if)#encapsulation ppp
    R2(config-if)#ppp authentication chap
    R2(config-if)#ip address 198.51.100.2 255.255.255.252
    R2(config-if)#no shutdown
	
***************************************
## RIP
| Attribut         | Wert              |
|------------------|-------------------|
| Type             | Distanzbasiert    |
| Algorithmus      | Bellman-Ford      |
| Standard         | RFC 2080, 2453    |
| Protokoll        | IPv4, IPv6        |
| Port             | 520,521           |
| Authenifizierung | Klartext, MD5     |
| Multicast IP     | 224.0.0.9/FF02::9 |
| Update-Time      | 30 sek.           |
| Invalid-Time     | 180 sek.          |
| Flush-Time       | 240 sek.          |
| Hold-down-Time   | 180 sek.          |

### Konfiguration
<!--
 _____     _____     _____     _____
| WAN |---| R1  |---| R2  |---| R3  |
|_____|   |_____|   |_____|   |_____|
             |         |         |
           1.1/24    2.1/25    3.1/24
           1.2/23    20.2/23   3.2/24
                     200.3/24
-->

#### Router 1
    R1#conf t
    R1(config)#interface GigabitEthernet 0/0
    R1(config-if)#description WAN Link
    R1(config-if)#ip addresse 192.0.2.41 255.255.255.0
    R1(config-if)#interface GigabitEthernet 0/1
    R1(config-if)#description Transfernet1
    R1(config-if)#ip address 172.16.10.2 255.255.255.252
    R1(config-if)#interface GigabitEthernet 0/2.10
    R1(config-if)#encapsulation dot1q 10
    R1(config-if)#ip address 10.1.1.1 255.255.254.0
    R1(config-if)#interface GigabitEthernet 0/2.20
    R1(config-if)#encapsulation dot1q 20
    R1(config-if)#ip address 10.1.2.1 255.255.254.0
    R1(config-if)#exit
    R1(config)#router rip
    R1(config-router)#network 10.1.1.0
    R1(config-router)#passive-interface GigabitEthernet 0/0
    R1(config-router)#default-information originate
    R1(config-router)#exit
    R1(config)#ip route 0.0.0.0 0.0.0.0 192.0.2.1
    R1(config)#do wr

#### Router 2
    R2#conf t
    R2(config)#interface GigabitEthernet 0/0
    R2(config-if)#description Transfernet1
    R2(config-if)#ip addresse 172.16.10.3 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/1
    R2(config-if)#description Transfernet2
    R2(config-if)#ip address 172.16.10.5 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/2.10
    R2(config-if)#encapsulation dot1q 10
    R2(config-if)#ip address 10.2.1.1 255.255.255.128
    R2(config-if)#interface GigabitEthernet 0/2.20
    R2(config-if)#encapsulation dot1q 20
    R2(config-if)#ip address 10.2.20.1 255.255.254.0
    R2(config-if)#interface GigabitEthernet 0/2.30
    R2(config-if)#encapsulation dot1q 30
    R2(config-if)#ip address 10.2.200.1 255.255.255.0
    R2(config-if)#exit
    R2(config)#router rip
    R2(config-router)#network 10.2.1.0
    R2(config-router)#network 10.2.20.0
    R2(config-router)#network 10.2.200.0
    R2(config-router)#exit
    R2(config)#do wr
    
#### Router 3
    R3#conf t
    R3(config)#interface GigabitEthernet 0/0
    R3(config-if)#description Transfernet2
    R3(config-if)#ip addresse 172.16.10.6 255.255.255.252
    R3(config-if)#interface GigabitEthernet 0/2.10
    R3(config-if)#encapsulation dot1q 10
    R3(config-if)#ip address 10.3.1.1 255.255.255.0
    R3(config-if)#interface GigabitEthernet 0/2.20
    R3(config-if)#encapsulation dot1q 20
    R3(config-if)#ip address 10.3.2.1 255.255.255.0
    R3(config-if)#exit
    R3(config)#router rip
    R3(config-router)#network 10.3.1.0
    R3(config-router)#exit
    R3(config)#do wr

### Troubelshooting
    show ip[v6] protocols
    show ip[v6] rip database
    debug ip rip { database | events }
    debug ipv6 rip [interface]
***************************************

## EIGRP
| Attribut         | Wert               |
|------------------|--------------------|
| Type             | Distanzbasiert     |
| Algorithmus      | DUAL               |
| Standard         | Cisco, Proprietär  |
| Protokoll        | IP, IPX, Appletalk |
| Port             | 88                 |
| Authenifizierung | MD5                |
| Multicast IP     | 224.0.0.10         |
| Hello Timmers    | 5/60               |
| Hold Timers      | 15/180             |

### Konfiguration
<!--
 _____     _____     _____     _____
| WAN |---| R1  |---| R2  |---| R3  |
|_____|   |_____|   |_____|   |_____|
             |         |         |
           1.1/24    2.1/25    3.1/24
           1.2/23    20.2/23   3.2/24
                     200.3/24
-->

#### Router 1
    R1#conf t
    R1(config)#interface GigabitEthernet 0/0
    R1(config-if)#description WAN Link
    R1(config-if)#ip addresse 192.0.2.41 255.255.255.0
    R1(config-if)#interface GigabitEthernet 0/1
    R1(config-if)#description Transfernet1
    R1(config-if)#ip address 172.16.10.2 255.255.255.252
    R1(config-if)#interface GigabitEthernet 0/2.10
    R1(config-if)#encapsulation dot1q 10
    R1(config-if)#ip address 10.1.1.1 255.255.254.0
    R1(config-if)#interface GigabitEthernet 0/2.20
    R1(config-if)#encapsulation dot1q 20
    R1(config-if)#ip address 10.1.2.1 255.255.254.0
    R1(config-if)#exit
    R1(config)#router eigrp 100
    R1(config-router)#network 10.1.1.0
    R1(config-router)#passive-interface GigabitEthernet 0/0
    R1(config-router)#exit
    R1(config)#ip route 0.0.0.0 0.0.0.0 192.0.2.1
    R1(config)#do wr

#### Router 2
    R2#conf t
    R2(config)#interface GigabitEthernet 0/0
    R2(config-if)#description Transfernet1
    R2(config-if)#ip addresse 172.16.10.3 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/1
    R2(config-if)#description Transfernet2
    R2(config-if)#ip address 172.16.10.5 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/2.10
    R2(config-if)#encapsulation dot1q 10
    R2(config-if)#ip address 10.2.1.1 255.255.255.128
    R2(config-if)#interface GigabitEthernet 0/2.20
    R2(config-if)#encapsulation dot1q 20
    R2(config-if)#ip address 10.2.20.1 255.255.254.0
    R2(config-if)#interface GigabitEthernet 0/2.30
    R2(config-if)#encapsulation dot1q 30
    R2(config-if)#ip address 10.2.200.1 255.255.255.0
    R2(config-if)#exit
    R2(config)#router eigrp 100
    R2(config-router)#network 10.2.1.0
    R2(config-router)#network 10.2.20.0
    R2(config-router)#network 10.2.200.0
    R2(config-router)#exit
    R2(config)#do wr
    
#### Router 3
    R3#conf t
    R3(config)#interface GigabitEthernet 0/0
    R3(config-if)#description Transfernet2
    R3(config-if)#ip addresse 172.16.10.6 255.255.255.252
    R3(config-if)#interface GigabitEthernet 0/2.10
    R3(config-if)#encapsulation dot1q 10
    R3(config-if)#ip address 10.3.1.1 255.255.255.0
    R3(config-if)#interface GigabitEthernet 0/2.20
    R3(config-if)#encapsulation dot1q 20
    R3(config-if)#ip address 10.3.2.1 255.255.255.0
    R3(config-if)#exit
    R3(config)#router eigrp 100
    R3(config-router)#network 10.3.1.0
    R3(config-router)#exit
    R3(config)#do wr

### Troubelshooting
    show ip eigrp interfaces
    show ip eigrp neighbors
    show ip eigrp topology
    show ip eigrp traffic
    clear ip eigrp neighbors
    debug ip eigrp [packet | neighbors]

***************************************

## OSPF
| Attribut         | Wert             |
|------------------|------------------|
| Type             | Link-State       |
| Algorithmus      | Dijkstra         |
| Metric           | Cost (Bandbreite)|
| Standard         | RFC 3228, 2740   |
| Protokoll        | IP               |
| Port             | 89               |
| Authenifizierung | Klartext, MD5    |
| AllSPF Adresse   | 224.0.0.5        |
| AllDR Adresse    | 224.0.0.6        |
| Hello Timers     | 30               |
| Dead Timers      | 120              |


### Konfiguration

<!--Beispiel OSPF
 _____     _____     _____     _____
| WAN |---| R1  |---| R2  |---| R3  |
|_____|   |_____|   |_____|   |_____|
             |         |         |
           1.1/24    2.1/25    3.1/24
           1.2/23    20.2/23   3.2/24
                     200.3/24
-->

#### Router 1
    R1#conf t
    R1(config)#interface GigabitEthernet 0/0
    R1(config-if)#description WAN Link
    R1(config-if)#ip addresse 192.0.2.41 255.255.255.0
    R1(config-if)#interface GigabitEthernet 0/1
    R1(config-if)#description Transfernet1
    R1(config-if)#ip address 172.16.10.2 255.255.255.252
    R1(config-if)#interface GigabitEthernet 0/2.10
    R1(config-if)#encapsulation dot1q 10
    R1(config-if)#ip address 10.1.1.1 255.255.254.0
    R1(config-if)#interface GigabitEthernet 0/2.20
    R1(config-if)#encapsulation dot1q 20
    R1(config-if)#ip address 10.1.2.1 255.255.254.0
    R1(config-if)#exit
    R1(config)#router ospf 100
    R1(config-router)#network 10.1.1.0 0.0.3.255 area 0
    R1(config-router)#router-id 1.1.1.1
    R1(config-router)#default-information originate
    R1(config-router)#passive-interface GigabitEthernet 0/0
    R1(config-router)#exit
    R1(config)#ip route 0.0.0.0 0.0.0.0 192.0.2.1
    R1(config)#do wr

#### Router 2
    R2#conf t
    R2(config)#interface GigabitEthernet 0/0
    R2(config-if)#description Transfernet1
    R2(config-if)#ip addresse 172.16.10.3 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/1
    R2(config-if)#description Transfernet2
    R2(config-if)#ip address 172.16.10.5 255.255.255.252
    R2(config-if)#interface GigabitEthernet 0/2.10
    R2(config-if)#encapsulation dot1q 10
    R2(config-if)#ip address 10.2.1.1 255.255.255.128
    R2(config-if)#interface GigabitEthernet 0/2.20
    R2(config-if)#encapsulation dot1q 20
    R2(config-if)#ip address 10.2.20.1 255.255.254.0
    R2(config-if)#interface GigabitEthernet 0/2.30
    R2(config-if)#encapsulation dot1q 30
    R2(config-if)#ip address 10.2.200.1 255.255.255.0
    R2(config-if)#exit
    R2(config)#router ospf 100
    R2(config-router)#network 10.2.1.0 0.0.0.127 area 0
    R2(config-router)#network 10.2.20.0 0.0.1.255 area 0
    R2(config-router)#network 10.2.200.0 0.0.0.255 area 0
    R2(config-router)#router-id 1.1.1.2
    R2(config-router)#exit
    R2(config)#do wr
    
#### Router 3
    R3#conf t
    R3(config)#interface GigabitEthernet 0/0
    R3(config-if)#description Transfernet2
    R3(config-if)#ip addresse 172.16.10.6 255.255.255.252
    R3(config-if)#interface GigabitEthernet 0/2.10
    R3(config-if)#encapsulation dot1q 10
    R3(config-if)#ip address 10.3.1.1 255.255.255.0
    R3(config-if)#interface GigabitEthernet 0/2.20
    R3(config-if)#encapsulation dot1q 20
    R3(config-if)#ip address 10.3.2.1 255.255.255.0
    R3(config-if)#exit
    R3(config)#router ospf 100
    R3(config-router)#network 10.3.1.0 0.0.1.255 area 0
    R3(config-router)#router-id 1.1.1.3
    R3(config-router)#exit
    R3(config)#do wr
    
### Troubelshooting
    clear ip[v6] ospf process
    show ip[v6] ospf [process] interface
    show ip[v6] ospf [process] neighbor
    show ip[v6] ospf border-routers
    show ip[v6] ospf database [LSA-type]
    show ip[v6] ospf virtual-links
    debug ip[v6] ospf [...]

***************************************

## ACL

### Action
 * permit
 * deny
 * remark
 
### ACL Nummern
| Range     | Bedeutung           |
|-----------|---------------------|
| 1-99      | IP Standard         |
| 100-199   | IP Erweiterte       |
| 200-299   | Protokoll           |
| 300-399   | DECnet              |
| 400-499   | XNS                 |
| 500-599   | XNS Erweiterte      |
| 600-699   | Appletalk           |
| 700-799   | MAC                 |
| 800-899   | IPX Standard        |
| 900-999   | IPX Erweiterte      |
| 1000-1099 | IPX SAP             |
| 1100-1199 | MAC Erweiterte      |
| 1200-1299 | IPX Zusammenfassung |

### Quelle und Ziel
| Ziel                    | Beschreibung           |
|-------------------------|------------------------|
| `any`                   | alle                   |
| `host 192.168.1.1`      | einzelner Host         |
| `192.168.1.0 0.0.0.255` | Netz mit Wildcard-Maske|

### Standard Syntax
`SW1(config)#access-list <number> {permit | deny} <source> [log]`

### Erweiterte Syntax
`SW1(config)#access-list <number> {permit | deny} <protocol> <source> [<ports>] <destination> [<ports>] [<options>]`

### Konfiguration
    SW1#conf t
    SW1(config)#access-list 101 remark Diese ACL definiert den Aussgehenden Verkehr
    SW1(config)#access-list 101 permit permit tcp 192.168.1.0 0.0.0.255 host 198.51.100.187 eq www 
    SW1(config)#access-list 101 permit permit tcp 192.168.1.0 0.0.0.255 host 198.51.100.187 eq 443 
    SW1(config)#access-list 101 permit permit icmp 192.168.1.0 0.0.0.255 198.51.100.0 0.0.0.255
    SW1(config)#access-list 101 deny ip any any
    SW1(config)#ip access-list extended in_network
    SW1(config-ext-nacl)#remark Diese ACL definiert den Eingehenden Verkehr
    SW1(config-ext-nacl)#permit tcp host 192.168.10.20 192.168.1.0 0.0.0.255 eq 10000 log
    SW1(config-ext-nacl)#deny ip any any
    SW1(config-ext-nacl)#exit
    SW1(config)#interface range GigabitEthernet0/1-12
    SW1(config-if)#ip access-group 101 out
    SW1(config-if)#ip access-group in_network in
    SW1(config-if)#do wr
    

### Troubelshooting
    show access-lists [<number> | <name>]
    show ip access-lists [<number> | <name>]
    show ip access-lists interface <interface>
    show ip access-lists dynamic
    show ip interface [<interface>]
    show time-range [<name>]
	
	
***************************************

## SNMP
| Attribut           | Wert v1       | Wert v2          | Wert v3              |
|--------------------|---------------|------------------|----------------------|
| Einführung         | 1988          | 1993             | 1999                 |
| Standard           | RFC 1155-1157 | RFC 1901-8, 2578 | RFC 1905-06, 3411-18 |
| Protokoll          | UDP           | UDP              | UDP                  |
| Port               | 161           | 161              | 161                  |
| Authenifizierung   | Community     | Community        | Username, MD5, SHA   |
| Encryption         | Keine         | Keine            | DES, AES             |
| 64-Bit Zähler      | Ja            | Nein             | Ja                   |
| Standard Community | public        | public           | Keine                |

### Konfiguration v2
    SW1#conf t
    SW1(config)#snmp-server community cisco-snmp ro
    SW1(config)#snmp-server location Rack 10, 1 UG
    SW1(config)#snmp-server contact network@holzfeind.ch
    SW1(config)#snmp-server host 192.168.10.20 version 2c cisco-snmp
    SW1(config)#snmp-server host 192.168.10.20 informs version 2c cisco-snmp alarms
    SW1(config)#do wr
	
### Konfiguration v3
    SW1#conf t
	SW1(config)#snmp-server location Rack 10, 1 UG
    SW1(config)#snmp-server contact network@holzfeind.ch
    SW1(config)#snmp-server group monitor-group v3 priv
    SW1(config)#snmp-server user monitor-user monitor-group v3 priv auth sha 12345 priv ases 128 54321
    SW1(config)#snmp-server host 192.168.10.20 version 3 monitor-user
    SW1(config)#snmp-server host 192.168.10.20 informs version 3 monitor-user alarms
    SW1(config)#do wr

### Troubelshooting
    show snmp
    show snmp host
    show snmp community
    show snmp contact
    show snmp location
    show snmp view
    show snmp group 
    show snmp user [username ]
    show snmp engineID
    show snmp sessions
    show snmp pending
    show snmp mib ifmib traps

***************************************
## IPv4

### IPv4 Adressen
| Range           | Bedeutung                      |
|-----------------|--------------------------------|
| 10.0.0.0/8      | Privates Netzwerk              |
| 127.0.0.0/8     | Localnet                       |
| 169.254.0.0/16  | Zeroconf                       |
| 172.16.0.0/12   | Privates Netzwerk              |
| 192.0.2.0/24    | Dokumentation und Beispielcode |
| 192.168.0.0/16  | Privates Netzwerk              |
| 198.51.100.0/24 | Dokumentation und Beispielcode |
| 203.0.113.0/24  | Dokumentation und Beispielcode |
| 224.0.0.0/4     | Multicast                      |
    
### IPv4 Subnet
| CIDR | Subnet Mask     | Adresse       | Wildcard        |
|------|-----------------|---------------|-----------------|
| /32  | 255.255.255.255 | 1             | 0.0.0.0         |
| /31  | 255.255.255.254 | 2             | 0.0.0.1         |
| /30  | 255.255.255.252 | 4             | 0.0.0.3         |
| /29  | 255.255.255.248 | 8             | 0.0.0.7         |
| /28  | 255.255.255.240 | 16            | 0.0.0.15        |
| /27  | 255.255.255.224 | 32            | 0.0.0.31        |
| /26  | 255.255.255.192 | 64            | 0.0.0.63        |
| /25  | 255.255.255.128 | 128           | 0.0.0.127       |
| /24  | 255.255.255.0   | 256           | 0.0.0.255       |
| /23  | 255.255.254.0   | 512           | 0.0.1.255       |
| /22  | 255.255.252.0   | 1'024         | 0.0.3.255       |
| /21  | 255.255.248.0   | 2'048         | 0.0.7.255       |
| /20  | 255.255.240.0   | 4'096         | 0.0.15.255      |
| /19  | 255.255.224.0   | 8'192         | 0.0.31.255      |
| /18  | 255.255.192.0   | 16'384        | 0.0.63.255      |
| /17  | 255.255.128.0   | 32'768        | 0.0.127.255     |
| /16  | 255.255.0.0     | 65'536        | 0.0.255.255     |
| /15  | 255.254.0.0     | 131'072       | 0.1.255.255     |
| /14  | 255.252.0.0     | 262'144       | 0.3.255.255     |
| /13  | 255.248.0.0     | 524'288       | 0.7.255.255     |
| /12  | 255.240.0.0     | 1'048'576     | 0.15.255.255    |
| /11  | 255.224.0.0     | 2'097'152     | 0.31.255.255    |
| /10  | 255.192.0.0     | 4'194'304     | 0.63.255.255    |
| /9   | 255.128.0.0     | 8'388'608     | 0.127.255.255   |
| /8   | 255.0.0.0       | 16'777'216    | 0.255.255.255   |
| /7   | 254.0.0.0       | 33'554'432    | 1.255.255.255   |
| /6   | 252.0.0.0       | 67'108'864    | 3.255.255.255   |
| /5   | 248.0.0.0       | 134'217'728   | 7.255.255.255   |
| /4   | 240.0.0.0       | 268'435'456   | 15.255.255.255  |
| /3   | 224.0.0.0       | 536'870'912   | 31.255.255.255  |
| /2   | 192.0.0.0       | 1'073'741'824 | 63.255.255.255  |
| /1   | 128.0.0.0       | 2'147'483'648 | 127.255.255.255 |
| /0   | 0.0.0.0         | 4'294'967'296 | 255.255.255.255 |

***************************************
## IPv6

### IPv6 Adressen
| Range             | Bedeutung                      | IPv4 Gegenstück                               |
|-------------------|--------------------------------|-----------------------------------------------|
| ::1               | Localhost                      | 127.0.0.1                                     |
| ::/27             | WAN                            | 0.0.0.0                                       |
| fe80:: bis febf:: | Link-Lokal                     | 10.0.0.0/8, 172.16.0.0/12 192.168.0.0/16      |
| 2001:db8::/32     | Dokumentation und Beispielcode | 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24 |
| fc00::/7          | Unique-Local Unicast           | -                                             |
| fc00::/8          | Multicast                      | 224.0.0.0/4                                   |

### IPv6 Notation

#### Regeln
 1. Alle führenden Nullen eines Blocks werden grundsätzlich weggelassen.
 2. Einer oder mehrere aufeinanderfolgende 4er Nullerblöcke werden durch zwei Doppelpunkte ("::") gekürzt.
 2b. Die Kürzung zu zwei Doppelpunkte ("::") darf nur einmal bei der längsten Folge von Nullerblöcken durchgeführt werden. Oder bei gleicher Länge, die erste von links.
 
#### Beispiel
Lange Schreibweise: 2001:0db8:0000:0000:f054:00ff:0000:02eb 
führende Nunllen entfrenen: 2001:db8:0:0:f054:ff:0:2eb 
Null-Blöcke zusammenfassen: 2001:db8::f054:ff:0:2eb 

### IPv6 Subnet
| Prefix | Beispiel                                 |
|--------|------------------------------------------|
| 4      | 1::                                      |
| 8      | 12::                                     |
| 12     | 123::                                    |
| 16     | 1234::                                   |
| 20     | 1234:5::                                 |
| 24     | 1234:56::                                |
| 28     | 1234:567::                               |
| 32     | 1234:5678::                              |
| 36     | 1234:5678:9::                            |
| 40     | 1234:5678:90::                           |
| 44     | 1234:5678:90a::                          |
| 48     | 1234:5678:90ab::                         |
| 52     | 1234:5678:90ab:c::                       |
| 56     | 1234:5678:90ab:cd::                      |
| 60     | 1234:5678:90ab:cde::                     |
| 64     | 1234:5678:90ab:cdef::                    |
| 68     | 1234:5678:90ab:cdef:1::                  |
| 72     | 1234:5678:90ab:cdef:12::                 |
| 76     | 1234:5678:90ab:cdef:123::                |
| 80     | 1234:5678:90ab:cdef:1234::               |
| 84     | 1234:5678:90ab:cdef:1234:5::             |
| 88     | 1234:5678:90ab:cdef:1234:56::            |
| 92     | 1234:5678:90ab:cdef:1234:567::           |
| 96     | 1234:5678:90ab:cdef:1234:5678::          |
| 100    | 1234:5678:90ab:cdef:1234:5678:9::        |
| 104    | 1234:5678:90ab:cdef:1234:5678:90::       |
| 108    | 1234:5678:90ab:cdef:1234:5678:90a::      |
| 112    | 1234:5678:90ab:cdef:1234:5678:90ab::     |
| 116    | 1234:5678:90ab:cdef:1234:5678:90ab:c::   |
| 120    | 1234:5678:90ab:cdef:1234:5678:90ab:cd::  |
| 124    | 1234:5678:90ab:cdef:1234:5678:90ab:cde:: |
| 128    | 1234:5678:90ab:cdef:1234:5678:90ab:cdef  |
