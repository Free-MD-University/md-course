# netplan

netplan is a abstraction layer .
we can define the real netork manager (networkd / networkmanager).
you define all the abastraction in a yaml files.
all yaml files are merge from the folder /etc/netplan/*.yaml

here an exemple 

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    {etherenet Interface}:
      dhcp4: yes/no
      addresses: [IP/mask] # define the ip in the network
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      routes: # defines route
      - to: "default" # define the route can be default or IP/MASK
        via: "192.168.100.1" # define the router .

```
after that we can generate the route with : 

netplan generate (--debug) # the debug will display the change

and apply them with

netplan apply (--debug) # the debug will display the change

# netwokd


# NetworkManager