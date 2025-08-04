# physical device :

## boot system 

the first layer of security is boot password . this allow to unaloow unauthorized access to gain from resseting root password or os . 
a command named : grub2-mkpasswd-pbkdf2 allow to define password to boot system .

## Filesystem .

## encryption 

there is a lot of encryption firware for data in a HD .
we can use LUKS there is a default command for this . 


# Firewall 

ufw is a default firewall for linux. 
we can avoid port to be used to create reverse connection or use a non authorized port .


# ssh connection secure


## password sniffing 
use a ssl certificate to ensure that you speak only to the correct host . the SSH protocol is secure but no the telnet one .


## password guessing 

you need t disable root login in ssh_config with pass or put a strong password . 
another option 

## disable not used account 

in the password file we can change the value of the shell to diable an account .
Enabled account: michael:x:1000:1000:Michael:/home/michael:/usr/bin/fish
Disabled account: michael:x:1000:1000:Michael:/home/michael:/sbin/nologin

# Log 
/var/log/messages - a general log for Linux systems
/var/log/auth.log - a log file that lists all authentication attempts (Debian-based systems)
/var/log/secure - a log file that lists all authentication attempts (Red Hat and Fedora-based systems)
/var/log/utmp - an access log that contains information regarding users that are currently logged into the system
/var/log/wtmp - an access log that contains information for all users that have logged in and out of the system
/var/log/kern.log - a log file containing messages from the kernel
/var/log/boot.log - a log file that contains start-up messages and boot information