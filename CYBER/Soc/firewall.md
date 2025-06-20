# Firewall

# Type

there are different type of firewall :

-   stateless : just filter according to the binary of the packet not based on old data snet by the same host

-   stateful : filter according to binary and previous packet connection.

-   proxy : go to layer 7 of the osi can see the data of the packet and the content according to what want the app.

-   NGFW: next gen firewall go from level 3 to level 7 layer

# firewall connection and action :

a firewall have 2 connection : outbound for exit data and inbound for income data .

## RULE :

each connection type have rule to define what to filter. each rule have a rule type (allow or deny) to define what to do with filtered packet .
