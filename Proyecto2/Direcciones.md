<XX> : 2 ultimos del carnet
<Y> : ultimo digito del carnet
<Carnet> : numero de carnet completo

# Core

- 3 Multislayer Switch 3650-24PS

- Red: 192.168.<XX>.0/16

- Conexion con Fibra Optica

# Telecom Uno

- Red: 172.16.1<Y>.0/24

- Departamentos:
    - Administracion -> Vlan: 10, Red: 172.16.1<Y>.0/26 255.255.255.192, 62 hosts
    - Atencion al Cliente -> Vlan: 20, Red: 172.16.1<Y>.64/26 255.255.255.192, 62 hosts
    - ServidoresWeb -> Vlan: 30, Red: 172.16.1<Y>.128/28 255.255.255.240, 14 hosts

- Topologia: Arbol

- Protocolo de Enrutamiento: OSPF

- 5 routers, 5 hosts, 2 enlaces LACP

- Proporciona DNS y HTTP a toda la topologia

# Redes Nacionales

- Red: 172.16.2<Y>.0/24

- Departamentos:
    - Ventas -> Vlan: 40, Red: 172.16.2<Y>.0/26 255.255.255.192, 62 hosts
    - Facturacion -> Vlan: 50, Red: 172.16.2<Y>.64/26 255.255.255.192, 62 hosts
    - ServidoresDHCP -> Vlan: 60, Red: 172.16.2<Y>.128/28 255.255.255.240, 14 hosts

- Topologia: Jerarquica

- Protocolo de Enrutamiento: OSPF

- 5 routers, 5 hosts, 2 enlaces LACP

- Redundancia: HSRP

- Proporciona DHCP a toda la topologia

# Conexiones Futuras

- Red: 172.16.3<Y>.0/24

- Departamentos:
    - Soporte -> Vlan: 70 , Red: 172.16.3<Y>.0/25 255.255.255.128, 126 hosts
    - Seguridad -> Vlan: 80, Red: 172.16.3<Y>.128/25 255.255.255.128, 126 hosts

- Topologia: Hub and Spoke

- Protocolo de Enrutamiento: EIGRP

- 5 routers, 5 hosts, 2 enlaces LACP

- Al menos 1 Router Inalambrico

# Comunicacion

Seguridad -> **Salida** Todos, **Entrada** Ninguno
Soporte -> **Salida** Todos, **Entrada** Todos
Administracion -> **Salida** Todos, **Entrada** Todos
Atencion al Cliente -> **Salida** Ventas, **Entrada** Ventas
Facturacion -> **Salida** Ventas, **Entrada** Ventas
Ventas -> **Salida** Facturacion y Atencion al Cliente, **Entrada** Facturacion y Atencion al Cliente

ServidoresWeb -> **Salida** Todos, **Entrada** Todos
ServidoresDHCP -> **Salida** Todos, **Entrada** Todos

# Web

- Dominio: www.proyecto2.<Carnet>.com

- Desplegar datos del estudiante y el curso

# DHCP

- Todos los dispositivos finales tienen que ser por DHCP, Menos servidores