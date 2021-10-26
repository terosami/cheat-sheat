# VLAN

## Trunktype

| Wert         | 802.1Q  | ISL      |
| ------------ | ------- | -------- |
| Header Size  | 4 bytes | 26 bytes |
| Trailer Size | -       | 4 bytes  |
| Standard     | IEEE    | Cisco    |
| Max. VLANs   | 4094    | 1000     |

## VLAN Nummer nach Cisco

| ID        | Bedeutung    |
| --------- | ------------ |
| 0         | Reseviert    |
| 1         | Default      |
| 1002      | fddi-default |
| 1003      | tr           |
| 1004      | fdnet        |
| 1005      | trnet        |
| 1006-4094 | Erweiterte   |
| 4095      | Reseviert    |

## Konfiguration

```
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
```

## VTP

```
SW1#conf t
SW1(config)#
SW1(config)#vtp mode {server | client | transparent}
SW1(config)#vtp domain <name>
SW1(config)#vtp password <passsword>
SW1(config)#vtp version {1 | 2}
SW1(config)#vtp pruning
```

{% hint style="danger" %}
Für die Verwendung von VLAN ab 1005 muss folgender Befehl eingetragen werden: `SW1(config)#vtp mode transparent`
{% endhint %}

## Troubelshooting

```
show vlan
show interface [status | switchport]
show interface trunk
show vtp status
show vtp password
```
