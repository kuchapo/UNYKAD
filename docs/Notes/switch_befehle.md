# Switch Befehle

## Switch

```cli
Interface fa0/1
```

```cli
switchport mode access
```

```cli
switchport access vlan 10
```

```cli
int vlan 10
```

```cli
int fa0/3
```

```cli
switchport mode trunk
```

## Multilayer-Switch

```cli
Ip routing
```

```cli
int vlan 10
```

```cli
Ip address <Default Gateway> <Subnetzmaske>
```

Um mit dem Router zu verbinden:

```cli
int g1/0/3
```

```cli
no switchport
```

```cli
ip address <Multilayer-Switch> 255.255.255.252
```
