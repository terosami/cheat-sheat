# OSPF

| Attribut         | Wert              |
| ---------------- | ----------------- |
| Type             | Link-State        |
| Algorithmus      | Dijkstra          |
| Metric           | Cost (Bandbreite) |
| Standard         | RFC 3228, 2740    |
| Protokoll        | IP                |
| Port             | 89                |
| Authenifizierung | Klartext, MD5     |
| AllSPF Adresse   | 224.0.0.5         |
| AllDR Adresse    | 224.0.0.6         |
| Hello Timers     | 30                |
| Dead Timers      | 120               |

## Konfiguration

![OSPF Beispiel](../.gitbook/assets/rip.svg)

### Router 1

```
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
```

### Router 2

```
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
```

### Router 3

```
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
```

## Troubelshooting

```
clear ip[v6] ospf process
show ip[v6] ospf [process] interface
show ip[v6] ospf [process] neighbor
show ip[v6] ospf border-routers
show ip[v6] ospf database [LSA-type]
show ip[v6] ospf virtual-links
debug ip[v6] ospf [...]
```
