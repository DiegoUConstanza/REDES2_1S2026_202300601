> Solo copiar lo dentro de los comandos, abrir el dispositivo indicado y darle en Pegar y Enter.

# MS4

```bash
enab
conf t

interface vlan 99
ip access-group 100 out
exit
```

# MS6

```bash
enab
conf t

interface vlan 10
ip access-group 100 in
exit

interface vlan 20
ip access-group 100 in
exit
```

# MS9

```bash
enab
conf t

interface vlan 30
ip access-group 100 in
exit

interface vlan 40
ip access-group 100 in
exit
```