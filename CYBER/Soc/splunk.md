# Splunk 
splunk se compose de 3 main componants 
## Forwarder :
a very few ressource process, take logs and send them to splunk 

## Indexer
act as a processing tunneling of the data . 

## Head Search 

the discover of ELK allow to search and find 

# seach query

that can be a | rules . we can write some text to found with some specific fileds :
index=botsv1 imreallynotbatman.com sourcetype=stream*

we can pupe to do some logic 

 | stats count(src_ip) as Requests by src_ip | sort - Requests

 in the discovery we can change the visualization like directly on the visualisation tab ,

# Incident Handling 

## reconnaisance 

find if someine try to get reconnaisance . 
