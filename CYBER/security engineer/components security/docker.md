# host security 

whne running a docker. a docker socket on the hst listen for command . 
this socket is available in the netwrok so we can use it to create new docker command .
the default port is 2375.

and we can use the following command to use the docker : 

```bash
docker -H tcp://{dokcer}:2375 ps
```
this allow us to use the docker in the host .

## TLS 

like you saw we can use http to access to docker . 
it is vital to use a TLS to encrypt and ensure that there is no MIM attack .
```bash
dockerd --tlsverify --tlscacert=myca.pem --tlscert=myserver-cert.pem --tlskey=myserver-key.pem -H=0.0.0.0:2376
```

# context 

when use docker there is a contect . 
a context give a new host docker command .
we can use a context to switch between envirnment and ssh access

# Capability 

we can give to docker container some capabilities like :

- CAP_NET_BIND_SERVICE : allow to bind a port to the host
- CAP_SYS_ADMIN: ariety of administrative privileges
- CAP_SYS_RESOURCE: allow change needed ressource .

we can see the capability given to a container with : capsh --print

we add a capability by adding --cap-add={cap} to the docker command 


# Image Security

in the security of image there are 2 main framework that define the security of an image :

NIST SP 800-190	: 
ISO 27001:

these framework deploy CVE of each public image .

a lot of tools can detect if the docker image hav a problem or security breach

CIS Docker Benchmark	
OpenSCAP	
Docker Scout	: given by docker itself
Anchore
Grype: This tool is a modern and fast vulnerability scanner for Docker images	