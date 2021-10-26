# ACL

## Action

* permit
* deny
* remark

## ACL Nummern

| Range     | Bedeutung           |
| --------- | ------------------- |
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

## Quelle und Ziel

| Ziel                    | Beschreibung            |
| ----------------------- | ----------------------- |
| `any`                   | alle                    |
| `host 192.168.1.1`      | einzelner Host          |
| `192.168.1.0 0.0.0.255` | Netz mit Wildcard-Maske |

## Standard Syntax

`SW1(config)#access-list <number> {permit | deny} <source> [log]`

## Erweiterte Syntax

`SW1(config)#access-list <number> {permit | deny} <protocol> <source> [<ports>] <destination> [<ports>] [<options>]`

## Konfiguration

```
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
```

## Troubelshooting

```
show access-lists [<number> | <name>]
show ip access-lists [<number> | <name>]
show ip access-lists interface <interface>
show ip access-lists dynamic
show ip interface [<interface>]
show time-range [<name>]
```
