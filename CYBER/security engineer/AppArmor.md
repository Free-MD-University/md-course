# AppArmor

feature in Linux, that allow to secure an app directly in the operating system.

we can check if appArmor is activated with :
```bash
sudo aa-status
```


we can define json for apparmor like :
here we define what the binarie of /usr/sbin/httpd can and can't
```json
/usr/sbin/httpd {

  capability setgid,
  capability setuid,

  /var/www/** r,
  /var/log/apache2/** rw,
  /etc/apache2/mime.types r,

  /run/apache2/apache2.pid rw,
  /run/apache2/*.sock rw,

  # Network access
  network tcp,

  # System logging
  /dev/log w,

  # Allow CGI execution
  /usr/bin/perl ix,

  # Deny access to everything else
  /** ix,
  deny /bin/**,
  deny /lib/**,
  deny /usr/**,
  deny /sbin/**
}
```

use the profile in the os:
```bash
sudo apparmor_parser -r -W /home/cmnatic/container1/apparmor/profile.json
```

use the profile in docker 

```bash
--security-opt apparmor=/home/cmnatic/container1/apparmor/profile.json
```