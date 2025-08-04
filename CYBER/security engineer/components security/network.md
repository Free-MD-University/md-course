there are multi way to secure a network . 

# Vlan :
 to avoid that a virus or a person that have access to our network can go inside the all network. we can define some VLAN .
 a VLAN is a virtual lan in a switch level . in the ETHERNET frame a VLAN ID is added and not let the switch to go in other lan . 
this ID is know as that TAG VLAN . only autohorized computers can go inside the vlan
a default vlan named nativeVlan can be created to allow guest user or other user to connect to the network .

## Vlan Routing 
to speak between vlan we need to use a router connected to our switch . this is a ROAS (Router on a stick) the connection is named a trunk .

## Vlan Zone :

each vlan need to define a zone.  we need to define what / who / and which traffic can in / out .
each zone can be define like : 

External : not trusted devices 
DMZ : separate unsecure and secure network : mail, server ...
Trusted : trusted componants 
Restricted : only trusted device with secure info (DB, hight risk server ...)

# ACL 
an acl describe which trafic is allowed and which one is not . 
we can define which porotocol, from which ip to which ip from a port to which port .
it is a good way to secure a network . 

# Firewall 
a firewall allow to define ACl to rule which traffic go where . 

a firewall allow to more easyly filtering traffic 

there is 2 type of firewall : 

## stateful 
can remember packet and allow some packet according to his history
## stateless
filter each packet as new one .

# DHCP Snoopping

it will validate and rate-limit DHCP traffic as necessary. If a host is untrusted, its traffic will be filtered and rate-limited.

# Dynamic ARP Inspection

ARP inspection will validate and rate-limit ARP packets as necessary