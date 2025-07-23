to prevent physhing there is 3 way : 

# SPF

allow to filter mail according to the sender .

## SYNTAX

### EXEMPLE
v=spf1 ip4:127.0.0.1 include:_spf.google.com -all

v=spf1 -> This is the start of the SPF record
ip4:127.0.0.1 -> This specifies which IP (in this case version IP4 & not IP6) can send mail
include:_spf.google.com -> This specifies which domain can send mail
-all -> non-authorized emails will be rejected


# DKIM

DomainKeys Identified Mail and is used for the authentication of an email that’s being sent.

## SYNTAX

### EXEMPLE

v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxTQIC7vZAHHZ7WVv/5x/qH1RAgMQI+y6Xtsn73rWOgeBQjHKbmIEIlgrebyWWFCXjmzIP0NYJrGehenmPWK5bF/TRDstbM8uVQCUWpoRAHzuhIxPSYW6k/w2+HdCECF2gnGmmw1cT6nHjfCyKGsM0On0HDvxP8I5YQIIlzNigP32n1hVnQP+UuInj0wLIdOBIWkHdnFewzGK2+qjF2wmEjx+vqHDnxdUTay5DfTGaqgA9AKjgXNjLEbKlEWvy0tj7UzQRHd24a5+2x/R4Pc7PF/y6OxAwYBZnEPO0sJwio4uqL9CYZcvaHGCLOIMwQmNTPMKGC9nt3PSjujfHUBX3wIDAQAB

# DMARC

If not already deployed, putting a DMARC record into place for your domain will give you feedback that will allow you to troubleshoot your SPF and DKIM configurations if needed. 

## SYNTAX 

### EXEMPLE
v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com 


# S/MIME 

(Secure/Multipurpose internet Mail Extensions) is a widely accepted protocol for sending digitally signed and encrypted messages."