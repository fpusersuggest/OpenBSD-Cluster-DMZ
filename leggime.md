# OpenBSD-Cluster-DMZ
Cluster OpenBSD Firewall

# NET Interfaces


* WAN = 203.0.113.1
* LAN = 192.168.1.250
* DMZ = 10.1.0.1
* PFSYNC = 10.254.254.1


# FAILOVER ofw1 (firewall 1)

    ofw1 # vi /etc/hostname.carp0
    inet  203.0.113.1 255.255.255.240 xx.xx.xxx.xxx vhid 1 carpdev bnx0 pass password1 advbase 1 advskew 0

    ofw1 # vi /etc/hostname.carp1
    inet 192.168.1.250 255.255.254.0 192.168.1.255 vhid 2 carpdev bge0 pass password1 advbase 1 advskew 0

    ofw1 # vi /etc/hostname.carp2
    inet 10.1.0.1 255.255.255.0 10.1.0.255 vhid 3 carpdev bge1 pass password1 advbase 1 advskew 0

    ofw1 # vi /etc/sysctl.conf
    net.inet.carp.preempt=1
  
    ofw1 # vi /etc/hostname.pfsync0
    up syncpeer 10.254.254.2 syncdev bnx1

    # internet 
    # range da xxx a xxx
    # bcast xxx.xxx.xxx.xxx 
    # gw xxx.XXX.173.113
    ofw1 # vi /etc/hostname.bnx0
    inet xxx.XXX.xxx.xxx 255.255.255.240 xxx.xx.xxx.xxx description "INTERNET"  

    # pfsync
    ofw1 # vi /etc/hostname.bnx1
    inet 10.254.254.1 255.255.255.0 10.254.254.255 description "PFSYNC"

    # lan
    /etc/hostname.bge0
    inet 192.168.1.249 255.255.254.0 192.168.1.255 description "LAN"

    # dmz
    /etc/hostname.bge1
    inet 10.1.0.2 255.255.255.0 10.1.0.255 description "DMZ"

    # gateway 93.46.173.113
    /etc/mygate
    xxx.xxx.xxx.xxx


FAILOVER ofw2 (firewall 2)
^^^^^^^^^^^^^^^^^^^^^^^^^^
ofw2 /etc/hostname.carp0
inet  xxx.xxx.xxx.xxx 255.255.255.240 xxx.xxx.xxx.xxx vhid 1 carpdev bnx0 pass password1 advbase 1 advskew 100

ofw2 /etc/hostname.carp1
inet 192.168.1.250 255.255.254.0 192.168.1.255 vhid 2 carpdev bge0 pass password1 advbase 1 advskew 100

ofw2 /etc/hostname.carp2
inet 10.1.0.1 255.255.255.0 10.1.0.255 vhid 3 carpdev bge1 pass password1 advbase 1 advskew 100

/etc/sysctl.conf
net.inet.carp.preempt=1

/etc/hostname.pfsync0
up syncpeer 10.254.254.1 syncdev bnx1


