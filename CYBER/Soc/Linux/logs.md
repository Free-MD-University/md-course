# Logs
most logs are in /var/log
# syslog 

# other logs 
/var/log/kern.log: Kernel messages and errors, useful for more advanced investigations
/var/log/syslog (or /var/log/messages): A consolidated stream of various Linux events
/var/log/dpkg.log (or /var/log/apt): Package manager logs on Debian-based systems
/var/log/dnf.log (or /var/log/yum.log): Package manager logs on RHEL-based systems

# auth.d
There are many ways users authenticate into a Linux machine: locally, via SSH, using "sudo" or "su" commands, or automatically to run a cron job. Each successful logon and logoff is logged, and you can see them by filtering the events containing the "session opened" or "session closed" keywords:


# audit.d
# rules
present in /etc/audit/rules.d/ you can add file where you rules which logs to put ? 

# logs 
logs are in /var/log/audit/audit.log or use ausearch command 

ausearch is used with auseach -i -k {rules_key} --> display all search linked to this key

uf you want to search by command ausearch -i -x {commend}
 you will find a PID (process ID) and a parent PID. 

 ausearch -i --pid {PID} //FILTER BY PID
 ausearch -i --ppid {PPID} //FILTER all syscall where parent id is the parent id
# see sudo history 

/var/log/auth | grep sudo --> give all coamdn make with sdo 

# sudoers file 
when changinging this file the command ```visudo``` is called  

# see all crontab 
for user in $(cut -f1 -d: /etc/passwd); do crontab -u $user -l; done
or 
/etc/crontab


# importqant logs to check 

whoami --> 
sudo --> 
wget curl or scp
systemd-detect-virt
ps aux
pwd, ls /, env, uname -a, lsb_release -a, hostname
User and Groups Discovery	id, whoami, w, last, cat /etc/sudoers, cat /etc/passwd
Process and Network Discovery	ps aux, top, ip a, ip r, arp -a, ss -tnlp, netstat -tnlp


# check for persistance in 
Monitor changes in cron job files	/etc/crontab, /etc/cron.d*, /var/spool/cron/*, /var/spool/crontab/*
Monitor changes in systemd folders	/lib/systemd/system/*, /etc/systemd/system/*, and less common(opens in new tab) locations
Monitor related processes such as	nano /etc/crontab, crontab -e, systemctl start|enable <service>

## account persistance 
the goal is to let a account to recconet, the user can create a new user or group or maybe a new hey ssh in authoruzedkey   
search for : 
cat /var/log/auth.log | grep -E 'useradd|usermod'
ausearch -i -f /.ssh/authorized_keys