# Interface

| Interface             | Bedeutung                           | Abkürzung |
| --------------------- | ----------------------------------- | --------- |
| FastEthernet 0/1      | erster Fastbit Anschluss ohne Modul | fa0/1     |
| FastEthernet 0/0/1    | erster Fastbit Anschluss mit Modul  | fa0/0/1   |
| GigabitEthernet 0/1   | erster Gigabit Anschluss ohne Modul | gi0/1     |
| GigabitEthernet 0/0/1 | erster Gigabit Anschluss mit Modul  | gi0/0/1   |
| port-channel l        | erster LACP                         | -         |

## Konfiguration IP-Adressen eines Router

```
R1#conf t
R1(config)#interface GigabitEthernet 0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown
```

## Konfiguration Port ausschalten bei einem Range

SW1#conf SW1(config)#interface range GigabitEthernet 0/1-12 SW1(config-if)#shutdown

## Konfiguration LACP

SW1#conf SW1(config)#interface range FastEthernet 0/1-2 SW1(config-if)#channel-groupe 1 mode activ SW1(config-if)#interface port-channel 1 SW1(config-if)#switchport mod trunk

## Konfiguration Portsecurity

SW1#conf SW1(config)#interface GigabitEthernet 0/4 SW1(config-if)#switchport mode access\
SW1(config-if)#switchport port-security\
SW1(config-if)#switchport port-security mac-address 1234.5678.9ABC.EF12 SW1(config-if)#interface GigabitEthernet 0/5 SW1(config-if)#switchport mode access\
SW1(config-if)#switchport port-security\
SW1(config-if)#switchport port-security maximum 3

## Konfiguration DHCP-Snooping

SW1#conf SW1(config)#ip dhcp snooping SW1(config)#interface GigabitEthernet 0/24 SW1(config-if)#ip dhcp snooping trust

## Troubelshooting

```
show arp
show ip interface
show ip route
show ip dhcp snooping
shwo etherchannel summary
show port-security interface gi0/4## Interface
```

| Interface             | Bedeutung                           | Abkürzung |
| --------------------- | ----------------------------------- | --------- |
| FastEthernet 0/1      | erster Fastbit Anschluss ohne Modul | fa0/1     |
| FastEthernet 0/0/1    | erster Fastbit Anschluss mit Modul  | fa0/0/1   |
| GigabitEthernet 0/1   | erster Gigabit Anschluss ohne Modul | gi0/1     |
| GigabitEthernet 0/0/1 | erster Gigabit Anschluss mit Modul  | gi0/0/1   |
| port-channel l        | erster LACP                         | -         |

## Konfiguration IP-Adressen eines Router

```
R1#conf t
R1(config)#interface GigabitEthernet 0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown
```

## Konfiguration Port ausschalten bei einem Range

```
SW1#conf
SW1(config)#interface range GigabitEthernet 0/1-12
SW1(config-if)#shutdown
```

## Konfiguration LACP

```
SW1#conf
SW1(config)#interface range FastEthernet 0/1-2
SW1(config-if)#channel-groupe 1 mode activ
SW1(config-if)#interface port-channel 1
SW1(config-if)#switchport mod trunk
```

## Konfiguration Portsecurity

```
SW1#conf
SW1(config)#interface GigabitEthernet 0/4
SW1(config-if)#switchport mode access   
SW1(config-if)#switchport port-security   
SW1(config-if)#switchport port-security mac-address 1234.5678.9ABC.EF12
SW1(config-if)#interface GigabitEthernet 0/5
SW1(config-if)#switchport mode access   
SW1(config-if)#switchport port-security   
SW1(config-if)#switchport port-security maximum 3
```

## Konfiguration DHCP-Snooping

SW1#conf SW1(config)#ip dhcp snooping SW1(config)#interface GigabitEthernet 0/24 SW1(config-if)#ip dhcp snooping trust

## Troubelshooting

```
show arp
show ip interface
show ip route
show ip dhcp snooping
shwo etherchannel summary
show port-security interface gi0/4
```
