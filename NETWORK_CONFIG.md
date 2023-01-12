# switch 2950T-24
#create vlan 
config terminal 
do sho r

## ALL SWITCH

````
int vlan 10
vlan 10
int vlan 20
vlan 20 
int vlan 30
vlan 30
exit
interface range f0/1-10
switchport access vlan 10 
exit
interface range f0/11-20
switchport access vlan 20
exit
interface range f0/21-24
switchport access vlan 30
exit
int g0/1
switchport trunk allowed vlan 10,20,30
switchport mode trunk
exit
````
END ALL SWITCH

LAB2  

## switch3

````
int g0/2
switchport trunk allowed vlan 10,20,30,111
switchport mode trunk
````
## router2

````
interface f0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
interface f0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
interface f0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit
int f0/0 
no shut
````

-----------------

````
router rip
version 2
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 199.199.199.0
exit
int f0/1
no shut 
ip address 199.199.199.1 255.255.255.252
````

## router3

````
int f0/0
no shut
ip address 199.199.199.2 255.255.255.252
exit
router rip 
version 2
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 199.199.199.0
exit
int f0/1
ip address 199.199.199.129 255.255.255.128
no shut
````

## server1

````
ip = 199.199.199.199/255.255.255.128
gw = 199.199.199.129
````

END LAB2

LAB1

## switch1

````
interface f0/23
switchport access vlan 10

interface f0/24
switchport access vlan 20
````


## router 1

````
int g0/1
ip address 199.199.199.129 255.255.255.128
no shutdown 

int g0/0
ip address 199.199.199.2 255.255.255.252
no shutdown 

exit 
router ospf 1
network 192.168.10.0 0.0.0.255 area 1
network 192.168.20.0 0.0.0.255 area 1
network 192.168.30.0 0.0.0.255 area 1
network 199.199.199.0 0.0.0.3 area 1
network 199.199.199.128 0.0.0.127 area 1
````

## router 4

````
interface f0/0  => sw port (0/2)
ip address 192.168.10.1 255.255.255.0
no shut

interface f0/1  => sw port (0/22)
ip address 192.168.30.1 255.255.255.0
no shut

int e0/1/0
no shut
ip address 199.199.199.1 255.255.255.252

exit 
router ospf 1
network 192.168.10.0 0.0.0.255 area 1
network 192.168.30.0 0.0.0.255 area 1
network 199.199.199.0 0.0.0.3 area 1
````
END LAB1

LAB3

## router5

````
int g0/1
ip address 199.199.199.129 255.255.255.128
no shut

int g0/0
ip address 199.199.199.1 255.255.255.252
no shut

router ospf 1
network 199.199.199.0 0.0.0.3 area 1
network 192.168.10.0 0.0.0.255 area 1
network 192.168.20.0 0.0.0.255 area 1
network 192.168.30.0 0.0.0.255 area 1
````

## switch5

````
int vlan 10
vlan 10
int vlan 20
vlan 20 
int vlan 30
vlan 30
int vlan 111
vlan 111
exit

int range f0/1-10
switchport access vlan 10

int range f0/11-20
switchport access vlan 20

int range f0/21-24
switchport access vlan 30

int g0/1   (for switch to switch)
switchport trunk allowed vlan 10,20,30
switchport mode trunk
exit

int g0/2   (to L3)
switchport trunk allowed vlan 10,20,30,111
switchport native vlan 111
switchport mode trunk
````


## Multilayer Switch0 L3

````
vl 10
vl 20
vl 30

int vlan 10
int vlan 20 
int vlan 30
int vlan 111

int g0/2
switchport trunk allowed vlan 10,20,30,111
switchport mode trunk (มันใส่ให้ AUTO)

int vlan 10 
ip address 192.168.10.1 255.255.255.0

int vlan 20 
ip address 192.168.20.1 255.255.255.0

int vlan 30 
ip address 192.168.30.1 255.255.255.0
exit

ip routing
int g0/1
no shut

int vl 1001
ip address 199.199.199.1 255.255.255.252
exit

int g0/1
switchport access vlan 1001

router ospf 1
network 192.168.10.0 0.0.0.255 area 1
network 192.168.20.0 0.0.0.255 area 1
network 192.168.30.0 0.0.0.255 area 1
network 199.199.199.0 0.0.0.3 area 1
````

(router5)

````
int g0/0
ip address 199.199.199.2 255.255.255.252
no shut

int g0/1
ip address 199.199.199.129 255.255.255.128

router ospf 1
network 192.168.10.0 0.0.0.255 area 1
network 192.168.20.0 0.0.0.255 area 1
network 192.168.30.0 0.0.0.255 area 1
network 199.199.199.0 0.0.0.3 area 1
network 199.199.199.128 0.0.0.127 area 1
````
//========end LAB3===============

## utility command check

````
do sh ip  rou
do sh vlan
````

### show routing

````
do sho ip ro
````
