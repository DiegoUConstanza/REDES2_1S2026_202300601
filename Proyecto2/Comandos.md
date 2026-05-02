# Core

Red para enrutamiento: 192.168.60.0/24

## MS1

```bash
enab
conf t

no ip domain-lookup

ip routing

interface gig1/0/1
no switchport
ip address 192.168.60.13 255.255.255.252
no shutdown
exit

interface gig1/1/1
no switchport
ip address 192.168.60.5 255.255.255.252
no shutdown
exit

interface gig1/1/2
no switchport
ip address 192.168.60.1 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.12 0.0.0.3 area 0
redistribute bgp 100 subnets
exit

router bgp 100
neighbor 192.168.60.2 remote-as 200
neighbor 192.168.60.6 remote-as 300
network 172.16.11.0 mask 255.255.255.192
network 172.16.11.64 mask 255.255.255.192
network 172.16.11.128 mask 255.255.255.240
exit
```

## MS2

```bash
enab
conf t

no ip domain-lookup

ip routing

interface gig1/0/1
no switchport
ip address 192.168.60.33 255.255.255.252
no shutdown
exit

interface gig1/1/1
no switchport
ip address 192.168.60.9 255.255.255.252
no shutdown
exit

interface gig1/1/2
no switchport
ip address 192.168.60.2 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.32 0.0.0.3 area 0
redistribute bgp 200 subnets
exit

router bgp 200
neighbor 192.168.60.1 remote-as 100
neighbor 192.168.60.10 remote-as 300
network 172.16.21.0 mask 255.255.255.192
network 172.16.21.64 mask 255.255.255.192
network 172.16.21.128 mask 255.255.255.240
exit
```

## MS3

```bash
enab
conf t

no ip domain-lookup

ip routing

interface gig1/0/1
no switchport
ip address 192.168.60.53 255.255.255.252
no shutdown
exit

interface gig1/1/1
no switchport
ip address 192.168.60.6 255.255.255.252
no shutdown
exit

interface gig1/1/2
no switchport
ip address 192.168.60.10 255.255.255.252
no shutdown
exit

router eigrp 1
network 192.168.60.52 0.0.0.3
redistribute bgp 300 metric 10000 100 255 1 1500
no auto-summary
exit

router bgp 300
neighbor 192.168.60.5 remote-as 100
neighbor 192.168.60.9 remote-as 200
network 172.16.31.0 mask 255.255.255.192
network 172.16.31.64 mask 255.255.255.192
network 172.16.31.128 mask 255.255.255.192
exit
```

# AS 100 - Telecomo Uno

## Vlans

### Vlan 10 - Administracion

Hosts: 62
Red: 172.16.11.0/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Vlan 20 - Atencion al Cliente

Hosts: 62
Red: 172.16.11.64/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Vlan 30 - ServidoresWeb

Hosts: 14
Red: 172.16.11.128/28
Mascara: 255.255.255.240
Wildcard: 0.0.0.15

## R1

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.14 255.255.255.252
no shutdown
exit

interface gig0/1
ip address 192.168.60.17 255.255.255.252
no shutdown
exit

interface gig0/2
ip address 192.168.60.21 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.12 0.0.0.3 area 0
network 192.168.60.16 0.0.0.3 area 0
network 192.168.60.20 0.0.0.3 area 0
exit
```

## R2

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.18 255.255.255.252
no shutdown
exit

interface range gig0/1-2
channel-group 1
exit

interface Port-channel 1
ip address 192.168.60.25 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.16 0.0.0.3 area 0
network 192.168.60.24 0.0.0.3 area 0
exit
```

## R3

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.22 255.255.255.252
no shutdown
exit

interface range gig0/1-2
channel-group 1
exit

interface Port-channel 1
ip address 192.168.60.29 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.20 0.0.0.3 area 0
network 192.168.60.28 0.0.0.3 area 0
exit
```

## R4

```bash
enab
conf t

no ip domain-lookup

interface range gig0/1-2
channel-group 1
exit

interface Port-channel 1
ip address 192.168.60.26 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.24 0.0.0.3 area 0
network 172.16.11.0 0.0.0.63 area 0
network 172.16.11.64 0.0.0.63 area 0
network 172.16.11.128 0.0.0.15 area 0
exit

interface gig0/0
no shutdown
exit

interface gig0/0.10
encapsulation dot1Q 10
ip address 172.16.11.1 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit

interface gig0/0.30
encapsulation dot1Q 30
ip address 172.16.11.129 255.255.255.240
no shutdown
exit
```

## R5

```bash
enab
conf t

no ip domain-lookup

interface range gig0/1-2
channel-group 1
exit

interface Port-channel 1
ip address 192.168.60.30 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.28 0.0.0.3 area 0
network 172.16.11.0 0.0.0.63 area 0
network 172.16.11.64 0.0.0.63 area 0
network 172.16.11.128 0.0.0.15 area 0
exit

interface gig0/0
no shutdown
exit

interface gig0/0.20
encapsulation dot1Q 20
ip address 172.16.11.65 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit
```

## S1

```bash
enab
conf t

no ip domain-lookup

vlan 10
name Administracion
exit

vlan 20
name AtencionCliente
exit

vlan 30
name ServidoresWeb
exit

interface fa0/5
switchport mode access
switchport access vlan 30
exit

interface range fa0/6-7
switchport mode access
switchport access vlan 10
exit

interface range fa0/2-4
channel-group 1 mode active
exit

interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit

interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
```

## S2

```bash
enab
conf t

no ip domain-lookup

vlan 10
name Administracion
exit

vlan 20
name AtencionCliente
exit

vlan 30
name ServidoresWeb
exit

interface range fa0/5-7
switchport mode access
switchport access vlan 20
exit

interface range fa0/2-4
channel-group 1 mode active
exit

interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit

interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
exit
```

## Configuracion ServerWeb

### IP:

Ip: 172.16.11.130
Mascara: 255.255.255.240
Gateway: 172.16.11.129

HTTP: 172.16.11.130
DNS: 172.16.11.130

### DNS:

A record: www.proyecto2.202300601.com -> 172.16.11.130

### HTTP:

Cambiar index.html por nombre y carnet

# AS 200 - Redes Nacionales

## Vlans

### Vlan 40 - Ventas

Hosts: 62
Red: 172.16.21.0/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Vlan 50 - Facturacion

Hosts: 62
Red: 172.16.21.64/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Vlan 60 - ServidoresDHCP

Hosts: 14
Red: 172.16.21.128/28
Mascara: 255.255.255.240
Wildcard: 0.0.0.15
DNS: 172.16.11.130

## R6

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.34 255.255.255.252
no shutdown
exit

interface gig0/1
ip address 192.168.60.37 255.255.255.252
no shutdown
exit

interface gig0/2
ip address 192.168.60.41 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.32 0.0.0.3 area 0
network 192.168.60.36 0.0.0.3 area 0
network 192.168.60.40 0.0.0.3 area 0
exit
```

## R7

```bash
enab
conf t

no ip domain-lookup

ip routing

interface gig1/0/1
no switchport
ip address 192.168.60.38 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.36 0.0.0.3 area 0
network 172.16.21.0 0.0.0.63 area 0
network 172.16.21.64 0.0.0.63 area 0
network 172.16.21.128 0.0.0.15 area 0
exit

interface range gig1/0/2-3
channel-group 1 mode active
exit

interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 40,50,60
no shutdown
exit

interface vlan 40
ip address 172.16.21.1 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit
```

## R8

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.42 255.255.255.252
no shutdown
exit

interface gig0/1
ip address 192.168.60.45 255.255.255.252
no shutdown
exit

interface gig0/2
ip address 192.168.60.49 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.40 0.0.0.3 area 0
network 192.168.60.44 0.0.0.3 area 0
network 192.168.60.48 0.0.0.3 area 0
exit
```

## R9

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.46 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.44 0.0.0.3 area 0
network 172.16.21.64 0.0.0.63 area 0
network 172.16.21.128 0.0.0.15 area 0
exit

interface gig0/1
no shutdown
exit

interface gig0/1.50
encapsulation dot1Q 50
ip address 172.16.21.66 255.255.255.192
standby version 2
standby 50 ip 172.16.21.65
standby 50 priority 110
standby 50 preempt
ip helper-address 172.16.21.135
exit

interface gig0/1.60
encapsulation dot1Q 60
ip address 172.16.21.130 255.255.255.240
standby version 2
standby 60 ip 172.16.21.129
standby 60 priority 110
standby 60 preempt
exit
```

## R10

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.50 255.255.255.252
no shutdown
exit

router ospf 1
network 192.168.60.48 0.0.0.3 area 0
network 172.16.21.64 0.0.0.63 area 0
network 172.16.21.128 0.0.0.15 area 0
exit

interface gig0/1
no shutdown
exit

interface gig0/1.50
encapsulation dot1Q 50
ip address 172.16.21.67 255.255.255.192
standby version 2
standby 50 ip 172.16.21.65
standby 50 priority 100
ip helper-address 172.16.21.135
exit

interface gig0/1.60
encapsulation dot1Q 60
ip address 172.16.21.131 255.255.255.240
standby version 2
standby 60 ip 172.16.21.129
standby 60 priority 100
exit
```

## S3

```bash
enab
conf t

no ip domain-lookup

vlan 40
name Ventas
exit

vlan 50
name Facturacion
exit

vlan 60
name ServidoresDHCP
exit

interface range fa0/1-2
channel-group 1 mode active
exit

interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 40,50,60
exit

interface range fa0/6-8
switchport mode access
switchport access vlan 40
exit

interface range fa0/3-5
channel-group 2 mode active
exit

interface Port-channel 2
switchport mode trunk
switchport trunk allowed vlan 40,50,60
exit
```

## S4

```bash
enab
conf t

no ip domain-lookup

vlan 40
name Ventas
exit

vlan 50
name Facturacion
exit

vlan 60
name ServidoresDHCP
exit

interface range fa0/1-2
switchport mode trunk
switchport trunk allowed vlan 40,50,60
exit

interface range fa0/3-5
channel-group 1 mode active
exit

interface Port-channel 1
switchport mode trunk
switchport trunk allowed vlan 40,50,60
exit

interface range fa0/6-7
switchport mode access
switchport access vlan 50
exit

interface fa0/8
switchport mode access
switchport access vlan 60
exit
```

## Configuracion ServerDHCP

### IP:
Ip: 172.16.21.135
Mascara: 255.255.255.240
Gateway: 172.16.21.129

DNS: Ip: 172.16.11.130


### Vlan 10 - Administracion

Pool Name: Vlan10_Administracion
Gateway: 172.16.11.1
DNS Server: 172.16.11.130
Start IP: 172.16.11.5
Subnet Mask: 255.255.255.192
Max Users: 50

### Vlan 20 - Atencion_al_Cliente

Pool Name: Vlan20_Atencion_al_Cliente
Gateway: 172.16.11.65
DNS Server: 172.16.11.130
Start IP: 172.16.11.69
Subnet Mask: 255.255.255.192
Max Users: 50

### Vlan 40 - Ventas

Pool Name: Vlan40_Ventas
Gateway: 172.16.21.1
DNS Server: 172.16.11.130
Start IP: 172.16.21.5
Subnet Mask: 255.255.255.192
Max Users: 62

### Vlan 50 - Facturacion

Pool Name: Vlan50_Facturacion
Gateway: 172.16.21.65
DNS Server: 172.16.11.130
Start IP: 172.16.21.70
Subnet Mask: 255.255.255.192
Max Users: 62

### Vlan 70 - Soporte

Pool Name: Vlan70_Soporte
Gateway: 172.16.31.1
DNS Server: 172.16.11.130
Start IP: 172.16.31.5
Subnet Mask: 255.255.255.192
Max Users: 62

### Vlan 80 - Seguridad

Pool Name: Vlan80_Seguridad
Gateway: 172.16.31.65
DNS Server: 172.16.11.130
Start IP: 172.16.31.69
Subnet Mask: 255.255.255.192
Max Users: 62

### Vlan 90 - Inalambrica

Pool Name: Vlan90_Red_Inalambrica
Gateway: 172.16.31.129
DNS Server: 172.16.11.130
Start IP: 172.16.31.130
Subnet Mask: 255.255.255.192
Max Users: 62

# AS 300 - Conexiones Futuras

62	172.16.10.0 /26	    255.255.255.192	172.16.10.1	172.16.10.62	172.16.10.63
62	172.16.10.64 /26	255.255.255.192	172.16.10.65	172.16.10.126	172.16.10.127
62	172.16.10.128 /26	255.255.255.192	172.16.10.129	172.16.10.190	172.16.10.191

## Vlans

### Vlan 70 - Soporte

Hosts: 62
Red: 172.16.31.0/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Vlan 80 - Seguridad

Hosts: 62
Red: 172.16.31.64/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

### Red Inalambrica

Hosts: 62
Red: 172.16.31.128/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63
DNS: 172.16.11.130

## R11

```bash
enab
conf t

no ip domain-lookup

ip routing

vlan 70
name Soporte
exit

vlan 80
name Seguridad
exit

vlan 90
name Wireless_VLAN
exit

interface gig1/0/1
no switchport
ip address 192.168.60.54 255.255.255.252
no shutdown
exit

interface gig1/0/2
no switchport
ip address 192.168.60.57 255.255.255.252
no shutdown
exit

interface gig1/0/4
no switchport
ip address 192.168.60.65 255.255.255.252
no shutdown
exit

router eigrp 1
network 192.168.60.52 0.0.0.3
network 192.168.60.56 0.0.0.3
network 192.168.60.60 0.0.0.3
network 192.168.60.64 0.0.0.3
network 172.16.31.0 0.0.0.63
network 172.16.31.64 0.0.0.63
network 172.16.31.128 0.0.0.63
no auto-summary
exit

interface Vlan 90
ip address 172.16.31.129 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit

interface GigabitEthernet1/0/3
switchport
switchport mode access
switchport access vlan 90
exit
```

## R12

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.66 255.255.255.252
no shutdown
exit

interface gig0/1
ip address 192.168.60.69 255.255.255.252
no shutdown
exit

router eigrp 1
network 192.168.60.64 0.0.0.3
network 192.168.60.68 0.0.0.3
no auto-summary
exit
```

## R13

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.70 255.255.255.252
no shutdown
exit

router eigrp 1
network 192.168.60.68 0.0.0.3
network 172.16.31.0 0.0.0.63
network 172.16.31.64 0.0.0.63
network 172.16.31.128 0.0.0.63
no auto-summary
exit

interface gig0/1
no shutdown
exit

interface gig0/1.80
encapsulation dot1Q 80
ip address 172.16.31.65 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit
```

## R14

```bash
enab
conf t

no ip domain-lookup

interface gig0/0
ip address 192.168.60.58 255.255.255.252
no shutdown
exit

router eigrp 1
network 192.168.60.56 0.0.0.3
network 172.16.31.0 0.0.0.63
network 172.16.31.64 0.0.0.63
network 172.16.31.128 0.0.0.63
no auto-summary
exit

interface gig0/1
no shutdown
exit

interface gig0/1.70
encapsulation dot1Q 70
ip address 172.16.31.1 255.255.255.192
ip helper-address 172.16.21.135
no shutdown
exit
```

## WR15

### Network Setup

Ip: 172.16.31.130
Subnet Mask: 255.255.255.192

### Wireless Setup

SSID: 202300601_WR15
Channel: 6
Broadcast: Si

### Wireless Security

Security: WPA2 Personal
Password: 202300601

## S5

```bash
enab
conf t

no ip domain-lookup

vlan 70
name Soporte
exit

vlan 80
name Seguridad
exit

vlan 90
name Wireless_VLAN
exit

interface range fa0/2-3
switchport mode access
switchport access vlan 70
exit

interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 70,80,90
exit
```

## S6

```bash
enab
conf t

no ip domain-lookup

vlan 70
name Soporte
exit

vlan 80
name Seguridad
exit

vlan 90
name Wireless_VLAN
exit

interface range fa0/2-3
switchport mode access
switchport access vlan 80
exit

interface fa0/1
switchport mode trunk
switchport trunk allowed vlan 70,80,90
exit
```

# Comandos Calificacion

## R4

```bash
enab
conf t

ip access-list extended FILTRO_ENTRADA_ADMIN
permit icmp any 172.16.11.0 0.0.0.63 echo-reply
permit tcp any 172.16.11.0 0.0.0.63 established
 
permit ip 172.16.31.0 0.0.0.63 172.16.11.0 0.0.0.63
permit ip 172.16.11.128 0.0.0.15 172.16.11.0 0.0.0.63
 
deny ip 172.16.11.64 0.0.0.63 172.16.11.0 0.0.0.63 
deny ip 172.16.21.0 0.0.0.63 172.16.11.0 0.0.0.63  
deny ip 172.16.21.64 0.0.0.63 172.16.11.0 0.0.0.63 
 
permit ip any any
exit

interface gig0/0.10
ip access-group FILTRO_ENTRADA_ADMIN out
exit
```

## R5

```bash
enab
conf t

ip access-list extended ACL_ATENCION_CLIENTE
permit udp any any eq bootps
permit ip 172.16.11.64 0.0.0.63 172.16.21.0 0.0.0.63
permit ip 172.16.11.64 0.0.0.63 172.16.31.64 0.0.0.63
permit ip 172.16.11.64 0.0.0.63 host 172.16.21.135
permit ip 172.16.11.64 0.0.0.63 host 172.16.11.130
permit ip 172.16.11.64 0.0.0.63 172.16.11.0 0.0.0.63
permit ip 172.16.11.64 0.0.0.63 172.16.31.0 0.0.0.63
deny ip any any
exit

interface gig0/0.20
ip access-group ACL_ATENCION_CLIENTE in
exit

ip access-list extended FILTRO_ADMIN
permit tcp any 172.16.11.0 0.0.0.63 established
permit icmp any 172.16.11.0 0.0.0.63 echo-reply
permit ip 172.16.31.0 0.0.0.63 172.16.11.0 0.0.0.63
permit ip 172.16.11.128 0.0.0.15 172.16.11.0 0.0.0.63
deny ip 172.16.11.64 0.0.0.63 172.16.11.0 0.0.0.63
deny ip 172.16.21.0 0.0.0.63 172.16.11.0 0.0.0.63
deny ip 172.16.21.64 0.0.0.63 172.16.11.0 0.0.0.63
permit ip any any
exit

interface gig0/0.10
 ip access-group FILTRO_ADMIN out
exit
```

## R7

```bash
enab
conf t

ip access-list extended ACL_VENTAS
permit udp any any eq bootps
permit ip 172.16.21.0 0.0.0.63 172.16.11.64 0.0.0.63
permit ip 172.16.21.0 0.0.0.63 172.16.21.64 0.0.0.63
permit ip 172.16.21.0 0.0.0.63 host 172.16.21.135
permit ip 172.16.21.0 0.0.0.63 host 172.16.11.130
permit ip 172.16.21.0 0.0.0.63 172.16.11.0 0.0.0.63
permit ip 172.16.21.0 0.0.0.63 172.16.31.0 0.0.0.63
permit ip 172.16.21.0 0.0.0.63 172.16.31.64 0.0.0.63
deny ip any any
exit

interface vlan 40
ip access-group ACL_VENTAS in
exit
```

## R9 y R10

```bash
enab
conf t

ip access-list extended ACL_FACTURACION

permit udp any any eq bootps
permit ip 172.16.21.64 0.0.0.63 172.16.21.0 0.0.0.63
permit ip 172.16.21.64 0.0.0.63 host 172.16.21.135
permit ip 172.16.21.64 0.0.0.63 host 172.16.11.130
permit ip 172.16.21.64 0.0.0.63 172.16.31.64 0.0.0.63
permit ip 172.16.21.64 0.0.0.63 172.16.11.0 0.0.0.63
permit ip 172.16.21.64 0.0.0.63 172.16.31.0 0.0.0.63
deny ip any any
exit

interface gig0/1.50
ip access-group ACL_FACTURACION in
exit
```

## R13

```bash
enab
conf t

ip access-list extended ACL_SEGURIDAD_PROTECT
permit ip host 172.16.11.130 172.16.31.64 0.0.0.63
permit ip host 172.16.21.135 172.16.31.64 0.0.0.63
 
permit udp any 172.16.31.64 0.0.0.63 eq bootpc
permit udp any 172.16.31.64 0.0.0.63 eq domain
 
permit tcp any 172.16.31.64 0.0.0.63 established
permit icmp any 172.16.31.64 0.0.0.63 echo-reply
permit ip 172.16.31.0 0.0.0.63 172.16.31.64 0.0.0.63

deny ip any 172.16.31.64 0.0.0.63 
permit ip any any
exit

interface gig0/1.80
 ip access-group ACL_SEGURIDAD_PROTECT out
exit
```

## R14

```bash
enab
conf t

ip access-list extended FILTRO_SOPORTE
permit tcp any 172.16.31.0 0.0.0.63 established
permit icmp any 172.16.31.0 0.0.0.63 echo-reply
permit ip 172.16.11.0 0.0.0.63 172.16.31.0 0.0.0.63
permit ip 172.16.11.128 0.0.0.15 172.16.31.0 0.0.0.63
deny ip 172.16.11.64 0.0.0.63 172.16.31.0 0.0.0.63
deny ip 172.16.21.0 0.0.0.63 172.16.31.0 0.0.0.63
deny ip 172.16.21.64 0.0.0.63 172.16.31.0 0.0.0.63
permit ip any any
exit

interface gig0/1.70
ip access-group FILTRO_SOPORTE out
exit
```