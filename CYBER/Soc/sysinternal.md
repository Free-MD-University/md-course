The Sysinternals tools are a compilation of over 70+ Windows-based tools. 
# TCP View 
TCPView is a Windows program that will show you detailed listings of all TCP and UDP endpoints on your system,


# Process Explorer

read all process and handle

# Sysmon 
a tool used to monitor and log events on Windows, is commonly used by enterprises as part of their monitoring and logging solutions
https://tryhackme.com/room/sysmon


# PROCESS 
## system process
System process is a special process that have always PID of 4

##Windows Session Manager

responsible to create windows session is the first process create by the kernel 

## Client Server Runtime Process csrr
Win32 console window and process thread creation and deletion.

#  wininit.exe, 

is responsible for launching services.exe

# lsaiso.exe 

is a process associated with Credential Guard and KeyGuard. You will only see this process if Credential Guard is enabled. 

# Services
There is a key identifier in the binary path, and that identifier is -k . This is how a legitimate svchost.exe process is called. 

# winlogon.exe
 is responsible for handling the Secure Attention Sequence, This process is also responsible for loading the user profile. 

# explorer.exe



# WebDav

We can use the sysinternal tool from internet . to do so we need a software called webdav.
Install-WindowsFeature WebDAV-Redirector –Restart

# Sigcheck

Sigcheck is a command-line utility that shows file version number, timestamp information, and digital signature details, including certificate chains.

# Stream
this is a software utilities to read data stream of a files .
Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System). Every file has at least one data stream ($DATA) and ADS allows files to contain more than one stream of data.
we can get the stream data with :

Get-Content {file} -Stream StreamName 


# Autoruns

check exe that been in auto run mode . allow to find persistance backdoor.

# ProcDump

use for check cpu use can be remplaced by Process Explorer


# Process Explorer

see all process an detail

# Process Monitor(ProcMon)

Process Monitor is an advanced monitoring tool for Windows that shows real-time file system, Registry and process/thread activity

# BgInfo
It automatically displays relevant information about a Windows computer on the desktop's background, such as the computer name, IP address, service pack version, and more

# Strings
show all string of an app