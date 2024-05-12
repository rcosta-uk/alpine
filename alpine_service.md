Alpine Linux comes with OpenRC init system. This tutorial shows how to use the various command on OpenRC to manage services.

# View status of all services

## Type the following command:
```
# rc-status
```

## View service list
Type the following command:
```
# rc-status --list
```

Sample outputs:

```
boot
nonetwork
default
sysinit
shutdown
```

You can change run level using the rc command:

```
# rc {runlevel}
# rc boot
# rc default
# rc shutdown
```

  1. **boot** – Generally the only services you should add to the boot runlevel are those which deal with the mounting of filesystems, set the initial state of attached peripherals and logging. Hotplugged services are added to the boot runlevel by the system. All services in the boot and sysinit runlevels are automatically included in all other runlevels except for those listed here.
  2. **single** – Stops all services except for those in the sysinit runlevel.
  3. **reboot** – Changes to the shutdown runlevel and then reboots the host.
  4. **shutdown** – Changes to the shutdown runlevel and then halts the host.
  5. **default** – Used if no runlevel is specified. (This is generally the runlevel you want to add services to.)

### To see manually started services, run:
```
# rc-status --manual
apache2
```

To see crashed services, run:
```
# rc-status --crashed
```

How to list all available services
Type the following command:
```
# rc-service --list
# rc-service --list | grep -i nginx
```
If apache2/nginx not installed, try the apk command to install it:
```
# apk add apache2
```

##How to add/enable service at boot time

The syntax is:
```
rc-update add {service-name} {run-level-name}
```

To add apache2 service at boot time, run:
```
# rc-update add apache2
```

OR
```
# rc-update add apache2 default
```

## Removing service at boot time on Alpine Linux

Pass the del as follows:
```
rc-update del {service-name-here}
```

If you wish to add that service again, simple run:
```
# rc-update add vnstatd
```

# How to start/stop/restart services on Alpine Linux

## How to start service

The syntax is:
```
# rc-service {service-name} start
```

## How to stop service

The syntax is:
```
# rc-service {service-name} stop
```

##How to restart service

The syntax is:
```
# rc-service {service-name} restart
```

For more info see [Alpine Linux](https://wiki.alpinelinux.org/wiki/OpenRC) project.
