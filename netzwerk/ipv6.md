# IPv6

## IPv6 Adressen

| Range             | Bedeutung                      | IPv4 Gegenstück                               |
| ----------------- | ------------------------------ | --------------------------------------------- |
| ::1               | Localhost                      | 127.0.0.1                                     |
| ::/27             | WAN                            | 0.0.0.0                                       |
| fe80:: bis febf:: | Link-Lokal                     | 10.0.0.0/8, 172.16.0.0/12 192.168.0.0/16      |
| 2001:db8::/32     | Dokumentation und Beispielcode | 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24 |
| fc00::/7          | Unique-Local Unicast           | -                                             |
| fc00::/8          | Multicast                      | 224.0.0.0/4                                   |

## IPv6 Notation

### Regeln

1. Alle führenden Nullen eines Blocks werden grundsätzlich weggelassen.
2. Einer oder mehrere aufeinanderfolgende 4er Nullerblöcke werden durch zwei Doppelpunkte ("::") gekürzt. 2b. Die Kürzung zu zwei Doppelpunkte ("::") darf nur einmal bei der längsten Folge von Nullerblöcken durchgeführt werden. Oder bei gleicher Länge, die erste von links.

### Beispiel

Lange Schreibweise: 2001:0db8:0000:0000:f054:00ff:0000:02eb führende Nunllen entfrenen: 2001:db8:0:0:f054:ff:0:2eb Null-Blöcke zusammenfassen: 2001:db8::f054:ff:0:2eb

## IPv6 Subnet

| Prefix | Beispiel                                 |
| ------ | ---------------------------------------- |
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
