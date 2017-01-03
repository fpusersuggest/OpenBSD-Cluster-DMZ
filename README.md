# OpenBSD-Cluster-DMZ
Cluster OpenBSD Firewall

# pf notes
## abilitare/disabilitare servizi:
    rcctl enable squid
    rcctl disable squid
    rcctl enable pf
    rcctl disable pf
## oppure
    pfctl -e
    pfctl -d
## liste
    { 192.168.0.1, 10.5.32.6 }
    # annidate
    trusted = "{ 192.168.1.2 192.168.5.36 }"
    pass in inet proto tcp from { 10.10.0.0/24 $trusted } to port 22
    
## macro
    
