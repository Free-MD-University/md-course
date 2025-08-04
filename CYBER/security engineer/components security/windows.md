# LOGS 

# Service 
windows handle a lot of function like sounds, network and more in service (like systemctl in linux),
these service are code that run in the BG .
the Service Control Manager allow to check services that runs 

# Registry

the registry unified container database that stores configurational settings, essential keys and shared preferences for Windows or applications. 

# Event Viewer
allow to see all logs of the computer . 

# Telemetry 

a data collection system send to microsoft each 15 minutes . these logs are stored in %appdata%/microsoft/diagnosis 


# Security 

## UAC 
User Account Control (UAC) is a feature that enforces enhanced access control and ensures that all services and applications execute in non-administrator accounts.
to check it go in coltrol panel (not settings) > user account > change user account control settigngs

## group and local policy 

Group Policy Editor is a built-in interactive tool by Microsoft that allows to configure and implement local and group policies. We mainly use this feature when part of a network


## Password policy 
Security settings > Account Policies > Password policy

# App 

we can define to allow app only from the microsoft store . 

Setting > Select Apps and Features

# Data Encryption

## Bitlocker 
allow to encryt the data of the disk .
A trusted Platform Module chip TPM is needde to do this feature . 

![windows cheat shewt]("windows cheat sheet.png")
