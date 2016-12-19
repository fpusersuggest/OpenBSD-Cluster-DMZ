# OpenBSD-Cluster-DMZ
Cluster OpenBSD Firewall

FAILOVER ofw1 (firewall 1)
^^^^^^^^^^^^^^^^^^^^^^^^^^
ofw1 /etc/hostname.carp0
inet  93.46.173.116 255.255.255.240 93.46.173.127 vhid 1 carpdev bnx0 pass password1 advbase 1 advskew 0

ofw1 /etc/hostname.carp1
inet 192.168.1.250 255.255.254.0 192.168.1.255 vhid 2 carpdev bge0 pass password1 advbase 1 advskew 0

ofw1 /etc/hostname.carp2
inet 10.1.0.1 255.255.255.0 10.1.0.255 vhid 3 carpdev bge1 pass password1 advbase 1 advskew 0

/etc/sysctl.conf
net.inet.carp.preempt=1

/etc/hostname.pfsync0
up syncpeer 10.254.254.2 syncdev bnx1

# internet 
# range da 112 a 127
# bcast 93.46.173.127 
# gw 93.46.173.113
/etc/hostname.bnx0
inet 93.46.173.114 255.255.255.240 93.46.173.127 description "INTERNET"  

# pfsync
/etc/hostname.bnx1
inet 10.254.254.1 255.255.255.0 10.254.254.255 description "PFSYNC"

# lan
/etc/hostname.bge0
inet 192.168.1.249 255.255.254.0 192.168.1.255 description "LAN"

# dmz
/etc/hostname.bge1
inet 10.1.0.2 255.255.255.0 10.1.0.255 description "DMZ"

# gateway 93.46.173.113
/etc/mygate
93.46.173.113


FAILOVER ofw2 (firewall 2)
^^^^^^^^^^^^^^^^^^^^^^^^^^
ofw2 /etc/hostname.carp0
inet  93.46.173.116 255.255.255.240 93.46.173.127 vhid 1 carpdev bnx0 pass password1 advbase 1 advskew 100

ofw2 /etc/hostname.carp1
inet 192.168.1.250 255.255.254.0 192.168.1.255 vhid 2 carpdev bge0 pass password1 advbase 1 advskew 100

ofw2 /etc/hostname.carp2
inet 10.1.0.1 255.255.255.0 10.1.0.255 vhid 3 carpdev bge1 pass password1 advbase 1 advskew 100

/etc/sysctl.conf
net.inet.carp.preempt=1

/etc/hostname.pfsync0
up syncpeer 10.254.254.1 syncdev bnx1


