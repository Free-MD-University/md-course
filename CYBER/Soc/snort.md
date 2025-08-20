# IDS 
active monitor solution for preventing possible malicious activities/patterns
nids monitor traffic flow from various point
hids monitor only one point
# IPS
active protecting solution for preventing possible malicious activities/patterns
NBA : allow to authentificate behavior 
Network Intrusion Prevention System (NIPS)
Host-based Intrusion Prevention System (HIPS)

SNORT is an open-source, rule-based Network Intrusion Detection and Prevention System (NIDS/NIPS)

# How it work 

snort -V : check the version 

snort -T -c {path to config } : check config validity 


# sniffer mode : 

-v : verbose 

-d : display packet data 

-i {interface}: interface

## sniff to a file : 

 -l: log file 
 -r  : read logged files
 -n X: read only X first packet

### filter 
we can add filter in  Berkeley Packet Filters  at the end of the command

# NIDS

- A {mode}: Alertmode possible value are FULL / FAST / CONSOLe / CMG / NONE
-D Background
-N disable logging


# create your own rule 

Snort Rule Header
The Snort rule header consists of the following parts:

1. Rule Action
When a rule is met, it specifies what action to take. When executing a standard Snort rule, there are five rule actions by default: Alert, Pass, Dynamic, Log, or/and Activate. The most common rule action is “alert,” which, as its name implies, sends an alert to the network administrator if a security threat is detected.

2. Protocol
This specifies the protocol the rule applies, such as TCP, UDP, ICMP, etc. The protocol is the unique address of a computer. While writing the rule, you can enter the Protocol address you should be wary of based on experience or past events.

3. Source IP Address
This specifies the source IP address that the rule applies to. For instance, assuming your threat is from Mr. Martin, the IP address, or in certain situations, his network ID identifies the computer making the connection. If you want alerts from any and every source, you can key in “any” in this part of the rule.

4. Destination IP Address
This specifies the destination address to which the rule applies. This is where network packets originate at a given Source IP address and are routed through a certain source port and destination port.

5. Source Port
This specifies the source port that the rule applies to. This functions similarly to the intermediary between two or more computer networks, relaying communications between them. The default number of TCP ports on a computer is 65,536; however, you can use “any” to specify all of these ports in the rule.

6. Destination Port
This specifies the destination port that the rule applies to. The destination port allows destination TCP/UDP to communicate with the Source ports in the same way as the Source ports allow for communication amongst each other.

7. Direction
This specifies the direction of traffic that the rule applies to, such as incoming or outgoing. Using this keyword/symbol, we may target just specified traffic segments. It is denoted with the arrow (</>) symbol. 

## EXEMPLE

alert udp anyany -> any 53 (msg: “Suspicious DNS Query detected”; content:”|00 01 00 00 00 01|”; depth:6; content: “example.com”; nocase; sid:1000004; rev:1;)

alert tcp anyany -> anyany (msg: “Possible SQL Injection attempt”; flow:to_server, established; content: “POST”; nocase; content:”/login.php”; nocase; content: “username=”; nocase; content: “password=”; nocase; content:”‘”; sid:1000003; rev:1;)

alert tcp $EXTERNAL_NET 80 -> $HOME_NET any
(
    msg:"Attack attempt!";
    flow:to_client,established;
    file_data;
    content:"1337 hackz 1337",fast_pattern,nocase;
    service:http;
    sid:1;
)

we can read more in https://docs.snort.org/rules/