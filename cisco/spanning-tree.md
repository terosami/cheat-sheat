# Spanning Tree

| Attribut    | STP         | PVST       | PVST+       | RSTP                | RPVST+      | MST                 |
| ----------- | ----------- | ---------- | ----------- | ------------------- | ----------- | ------------------- |
| Algorithmus | Legacy ST   | Legacy ST  | Legac ST    | Rapid ST            | Rapid ST    | Rapid ST            |
| Standard    | 802.1D-1998 | Cisco      | Cisco       | 802.1w, 802.1D-2004 | Cisco       | 802.1s, 802.1Q-2003 |
| Instanzen   | 1           | 1 pro VLAN | 1 pro VLAN  | 1                   | 1 pro VLAN  | 1-mehre             |
| Trunk       | -           | ISL        | 802.1Q, ISL | -                   | 802.1Q, ISL | 802.1Q, ISL         |

## Link-Kosten

| Speed    | Kosten |
| -------- | ------ |
| 4 Mbps   | 250    |
| 10 Mbps  | 100    |
| 16 Mbps  | 62     |
| 45 Mbps  | 39     |
| 100 Mbps | 19     |
| 155 Mbps | 14     |
| 622 Mbps | 6      |
| 1 Gbps   | 4      |
| 10 Gbps  | 2      |
| 20+ Gbps | 1      |

## Priorität

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

## Bridge-ID

Die Bridge-ID ist wie folgt Zusammengesetz: 4 Bit Priorität + 12 System ID (VLAN) + 48 Bit MAC-Adresse

## Pfadentscheidung

1. Bridge mit der tiefesten ID wird Root-Bridge.
2. Switche mit den tieferen Pfadkosten zur Root-Bridge.
3. Switche mit der tieferen ID.
4. Tiefste Portnummer.

## Konfiguration RSTP, RPVST+

### Switch 1

```
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
```

### Switch 2

```
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
```

### Switch 3

```
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
```

## Troubelshooting

```
show spanning-tree [summary | detail | root]
show spanning-tree [interface | vlan]
show spanning-tree mst [...]
```
