# see sudo history 

/var/log/auth | grep sudo --> give all coamdn make with sdo 

# sudoers file 
when changinging this file the command ```visudo``` is called  

# see all crontab 
for user in $(cut -f1 -d: /etc/passwd); do crontab -u $user -l; done
or 
/etc/crontab