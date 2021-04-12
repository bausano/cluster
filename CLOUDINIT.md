# cloudinit
I've tried to setup cloudinit with a custom user, but I could not get it
working with ssh. Importing id from Github nor setting initial ssh authorized
keys worked. With my next node, I'd like to try to fix the ssh issue and
actually use the script.

```
#cloud-config

# This is the user-data configuration file for cloud-init. By default this sets
# up an initial user called "ubuntu" with password "ubuntu", which must be
# changed at first login. However, many additional actions can be initiated on
# first boot from this file. The cloud-init documentation has more details:
#
# https://cloudinit.readthedocs.io/
#
# Some additional examples are provided in comments below the default
# configuration.

manage_etc_hosts: true
hostname: rasp1

# we don't want pwd auth over ssh, only cert
ssh_pwauth: false

groups:
- rasp1: [rasp1]

# create only one user, a member of the sudo group, with a test password
users:
- name: rasp1
  shell: /bin/bash
  groups: rasp1,adm,sudo
  sudo: ALL=(ALL) NOPASSWD:ALL
  lock_passwd: true

locale: "en_US.UTF-8"
```

## Resources
- https://cloudinit.readthedocs.io/en/latest/topics/examples.html
- https://www.digitalocean.com/community/tutorials/how-to-use-cloud-config-for-your-initial-server-setup
- https://gist.github.com/Darkflib/1cdea1305e41ecc28009be8884f7eaa8
- https://stackoverflow.com/questions/38113380/how-do-i-stop-cloud-init-from-overwriting-my-hostname-on-aws-centos
- https://pcocc.readthedocs.io/en/latest/manpages/man7/cloudconfig-tutorial.html
- https://serverfault.com/questions/549695/hostname-via-cloud-init-on-centos-running-on-ec2-do-not-work
- https://help.switch.ch/engines/faq/ubuntu-why-cant-i-persistently-change-the-hostname-of-my-vm/
- https://bugs.launchpad.net/cloud-init/+bug/1894839
- https://bugs.launchpad.net/subiquity/+bug/1766980
