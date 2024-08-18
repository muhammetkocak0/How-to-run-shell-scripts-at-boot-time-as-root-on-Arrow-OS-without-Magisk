# How to run shell scripts at boot time as root on Arrow OS without Magisk

## Step 1 
go to /system/etc/init and create a file named myscript.rc with following content:
```c
on boot
    exec u:r:su:s0 root root -- /system/etc/myscript.sh
```

## Step 2 
create /system/etc/myscript.sh, write your script and make it executable.

### Note: the first line of the script must be #!/bin/sh.
### Note: If you want to use iptables in this script (or some other external binary), use full path (/system/bin/iptables) instead of just iptables. The /system/bin directory seems to be missing from the $PATH when the script is launched.

## Step 3  
open the /system/addon.d/50-arrow.sh file, find the list_files function and add paths to files you've just created 
It should look like this:
```c
list_files() {
cat <<EOF
etc/hosts
etc/myscript.sh
etc/init/myscript.rc
EOF
}
```
