<XX> : 2 ultimos del carnet
<Y> : ultimo digito del carnet

# Core

## MS1

```bash
enab
conf t

no ip domain-lookup
```

## MS2

```bash
enab
conf t

no ip domain-lookup
```

## MS3

```bash
enab
conf t

no ip domain-lookup
```

# AS 100 - Telecomo Uno

## Vlans

### Vlan 10 - Administracion

Hosts: 62
Red: 172.16.1<Y>.0/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63

### Vlan 20 - Atencion al Cliente

Hosts: 62
Red: 172.16.1<Y>.64/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63

### Vlan 30 - ServidoresWeb

Hosts: 14
Red: 172.16.1<Y>.128/28
Mascara: 255.255.255.240
Wildcard: 0.0.0.15

## MS1

```bash
enab
conf t

no ip domain-lookup
```

## R1

```bash
enab
conf t

no ip domain-lookup
```

## R2

```bash
enab
conf t

no ip domain-lookup
```

## R3

```bash
```

## R4

```bash
enab
conf t

no ip domain-lookup
```

## R5

```bash
enab
conf t

no ip domain-lookup
```

## S1

```bash
enab
conf t

no ip domain-lookup
```

## S2

```bash
enab
conf t

no ip domain-lookup
```

# AS 200 - Redes Nacionales

## Vlans

### Vlan 40 - Ventas

Hosts: 62
Red: 172.16.2<Y>.0/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63

### Vlan 50 - Facturacion

Hosts: 62
Red: 172.16.2<Y>.64/26
Mascara: 255.255.255.192
Wildcard: 0.0.0.63

### Vlan 60 - ServidoresDHCP

Hosts: 14
Red: 172.16.2<Y>.128/28
Mascara: 255.255.255.240
Wildcard: 0.0.0.15

## MS2

```bash
enab
conf t

no ip domain-lookup
```

## R6

```bash
enab
conf t

no ip domain-lookup
```

## R7

```bash
enab
conf t

no ip domain-lookup
```

## R8

```bash
```

## R9

```bash
enab
conf t

no ip domain-lookup
```

## R10

```bash
enab
conf t

no ip domain-lookup
```

## S3

```bash
enab
conf t

no ip domain-lookup
```

## S4

```bash
enab
conf t

no ip domain-lookup
```

# AS 300 - Conexiones Futuras

## Vlans

### Vlan 70 - Soporte

Hosts: 126
Red: 172.16.3<Y>.0/26
Mascara: 255.255.255.128
Wildcard: 0.0.0.127

### Vlan 80 - Seguridad

Hosts: 126
Red: 172.16.3<Y>.128/26
Mascara: 255.255.255.128
Wildcard: 0.0.0.127

## MS3

```bash
enab
conf t

no ip domain-lookup
```

## R11

```bash
enab
conf t

no ip domain-lookup
```

## R12

```bash
enab
conf t

no ip domain-lookup
```

## R13

```bash
```

## R14

```bash
enab
conf t

no ip domain-lookup
```

## WR15

### Network:

## S5

```bash
enab
conf t

no ip domain-lookup
```

## S6

```bash
enab
conf t

no ip domain-lookup
```