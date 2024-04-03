# Prepare Alpine

Hi, this is a quick write on how to create an Alpine template:


Create a new VM and use a high number identify (e. 9000) and a simple name (e. alpine-cloudinit)

**Hardware:**\
| OS | Alpine virtual ISO file downloaded early | |
| :-- | -- | -- |
| SYSTEM | VGA: Serial terminal 0 | Enable Qemu Agent |
| HDD | SCSI 4Gb   | This can be increased according to the needs |
| CPU | 1 core Type: Host  | Some as the above |
| RAM | 512 MiB RAM with 256 MiB minimum |   |
| NET | Add bridge 
 
\
\
This is the iso that was used,  https://dl-cdn.alpinelinux.org/alpine/v3.19/releases/x86_64/alpine-virt-3.19.1-x86_64.iso


Start the VM and access to the console:

Login as root and type setup-alpine to initialise the installation. Choose the standard option to the installation:

> **Don't configure a user besides root.**


| prompt | answer |
| :-- | -- |
| keyboard: | UK |
| variant: | UK |
| hostname: | alpine |
| interface: | eth0 |
| ip: | dhcp |
| manual: | n |
| root pw: | any (will be deleted later) |
| timezone: | Europe/London |
| proxy: | none|
| mirrors: | 1 |
| ssh: | openssh |
| disk: | avalilable disk |
| format: | set it up as lvmsys, use ? for more info |
| erase: | y |



Unmount the iso file from the machine and reboot.

# Install cloud-init

Login with user root
Cloud init needs some dependencies to work.
<i>These dependencies will be different based on Alpine Version.</i>

```bash
apk add gcc linux-headers py3-pip musl-dev python3-dev e2fsprogs e2fsprogs-extra cloud-init \
 libblockdev lsblk parted sfdisk sgdisk lvm2 device-mapper \
 doas eudev mount openssh-server-pam sudo py3-pyserial py3-netifaces
```


# Enable PAM on sshd config file

```bash
sed -i '/^.*?UsePAM *=.*?$/d' /etc/ssh/sshd_config  # Erase any lines in the config that match the pattern, just to make sure. It was commented out in my install by default. I didn't run this.
echo "UsePAM yes" >> /etc/ssh/sshd_config
```

## Enable cloud-init.

Simply running rc-update add cloud-init boot will NOT work. Instead run:

```bash
setup-cloud-init
```


# Next steps

## Add Qemu agent

Install qemu-guest-agent for a better integration with proxmox dashboard

```bash
apk add qemu-guest-agent
```

\
\
add the agent to start at the boot:

```sh
rc-update add qemu-guest-agent
```

As extra you can change the motd file:

```bash
nano /etc/motd
```

<b>BE CAREFUL NOW!</b>

Now you can lock down your template VM before converting it to a template. Lockout root access with ```passwd -l root```

If you had any public ssh keys you can remove them from /root/.ssh/authorized_keys if you want.

```bash
poweroff
```

- Add Cloudinitdrive on the Hardware tab.
- Go to Cloud-Init tab and finish the configuration
- Convert the VM to a template

# DONE! 

Enjoy the pleasure of right-click and full-clone the template to a VM. You can specify all the settings on the new VM before the first boot, like hardware, network, and cloud-init settings.
