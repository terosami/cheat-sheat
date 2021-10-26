# SNMP

| Attribut           | Wert v1       | Wert v2          | Wert v3              |
| ------------------ | ------------- | ---------------- | -------------------- |
| Einführung         | 1988          | 1993             | 1999                 |
| Standard           | RFC 1155-1157 | RFC 1901-8, 2578 | RFC 1905-06, 3411-18 |
| Protokoll          | UDP           | UDP              | UDP                  |
| Port               | 161           | 161              | 161                  |
| Authenifizierung   | Community     | Community        | Username, MD5, SHA   |
| Encryption         | Keine         | Keine            | DES, AES             |
| 64-Bit Zähler      | Ja            | Nein             | Ja                   |
| Standard Community | public        | public           | Keine                |

## Konfiguration v2

```
SW1#conf t
SW1(config)#snmp-server community cisco-snmp ro
SW1(config)#snmp-server location Rack 10, 1 UG
SW1(config)#snmp-server contact network@holzfeind.ch
SW1(config)#snmp-server host 192.168.10.20 version 2c cisco-snmp
SW1(config)#snmp-server host 192.168.10.20 informs version 2c cisco-snmp alarms
SW1(config)#do wr
```

## Konfiguration v3

```
SW1#conf t
SW1(config)#snmp-server location Rack 10, 1 UG
SW1(config)#snmp-server contact network@holzfeind.ch
SW1(config)#snmp-server group monitor-group v3 priv
SW1(config)#snmp-server user monitor-user monitor-group v3 priv auth sha 12345 priv ases 128 54321
SW1(config)#snmp-server host 192.168.10.20 version 3 monitor-user
SW1(config)#snmp-server host 192.168.10.20 informs version 3 monitor-user alarms
SW1(config)#do wr
```

## Troubelshooting

```
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
```
