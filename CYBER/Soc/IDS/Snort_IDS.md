Snort IDS is the most used open-source IDS host-based .
it use hybrid threat analasys to detect threat .
snort come with multiple mode :

-   Packet sniffer mode : log all packet in the network
-   Packet logging mode : detect treat and send them as alert
-   Network Intrusion Detection System mode: detect threat and send alert in real-time, check with malware analysis .

# installation and conifg

snort config are stored in :
/etc/snort

## RULE:

in the local.rules file we can define custom rules in this exemple :
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
{action} {protocol} {source IP} {SOURCE PORT} -> {DEST IP} {dest port} (msg:"{MESSAGE}"; sid:{signatureID}; rev:{rev_number};)

# SNORT COMMAND

the snort command allow to analyze pcap file by giving the -r {path_to_file}.
