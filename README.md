# cldemo-radius

This demo create a base line radius demo that shows user authentication working.

It has two roles:
1. Deploy FreeRadius package onto oob-mgmt-server
* Configure Radius server with user account to allow login.
2. Deploy FreeRadius client package onto all switches in infrastructure
* Configure Radius client to use Radius server for authentication

There is a client called **testuser** with password **cn321**. Test that tacacs is working by trying to log in with the user:
`ssh testuser@leaf01`

#Steps to run demo
```
git clone https://github.com/CumulusNetworks/cldemo-vagrant.git
cd cldemo-vagrant
vagrant up oob-mgmt-server oob-mgmt-switch
vagrant up leaf01 leaf02 leaf03 leaf04 spine01 spine02
```

```
vagrant ssh oob-mgmt-server
git clone https://github.com/CumulusNetworks/cldemo-radius.git
cd cldemo-radius
ansible-playbook run-demo.yml
```

```
cumulus@oob-mgmt-server:~$ ssh testuser@leaf01
testuser@leaf01's password:

Welcome to Cumulus VX (TM)

Cumulus VX (TM) is a community supported virtual appliance designed for
experiencing, testing and prototyping Cumulus Networks' latest technology.
For any questions or technical support, visit our community site at:
http://community.cumulusnetworks.com

The registered trademark Linux (R) is used pursuant to a sublicense from LMI,
the exclusive licensee of Linus Torvalds, owner of the mark on a world-wide
basis.
testuser@leaf01:~$
```

Look at logs:
```
testuser@leaf01:~$ sudo tail -f /var/log/syslog
2017-10-31T17:05:56.313150+00:00 leaf01 sshd[4326]: Accepted password for testuser from 192.168.0.254 port 59812 ssh2
2017-10-31T17:05:56.336837+00:00 leaf01 sshd[4326]: pam_unix(sshd:session): session opened for user testuser by (uid=0)
```

```
testuser@leaf01:~$ groups
radius_users
```
```
testuser@leaf01:~$ whoami
testuser
```

```
testuser@leaf01:~$ getent passwd testuser
testuser:x:1002:1002:testuser mapped user:/home/testuser:/bin/bash
```