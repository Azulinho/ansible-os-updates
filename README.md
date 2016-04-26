
USAGE:
=======


```
vagrant up centos7 --no-provision
LIMIT=centos7 vagrant provision centos7
vagrant package centos7 --output centos7-os-updates.box
vagrant destroy -f
vagrant box add centos7-os-updates centos7-os-updates.box


vagrant up suse12 --no-provision
LIMIT=suse12 vagrant provision suse12
vagrant package suse12 --output suse12-os-updates.box
vagrant box add centos7-os-updates centos7-os-updates.box
vagrant destroy -f

```
