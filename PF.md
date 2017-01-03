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
    # annidate, liste di liste.
    trusted = "{ 192.168.1.2 192.168.5.36 }"
    pass in inet proto tcp from { 10.10.0.0/24 $trusted } to port 22
    
## macro
    ext_if = "fxp0"
## tabelle
    table <bruttagente> { 192.168.66.1/24 }
    # attributo **const** rende la tabelle non modificabile
    table <rfc1918>  const { 192.168.0.0/16, 172.16.0.0/12, 10.0.0.0/8 }
    # attributo **persist** forza il caricamente della tabella in memoria.
    table <spammers> persist
    # questa è una lista di tabelle
    block in on fxp0 from { <rfc1918>, <spammers> } to any
    # eccetto
    table <goodguys> { 192.0.2.0/24, !192.0.2.5 }
    # include file
    table <spammers> persist file "/etc/spammers"
    # manipolare le tabelle
    pfctl -t spammers -T add 203.0.113.0/24
    pfctl -t spammers -T show
    pfctl -t spammers -T delete 203.0.113.0/24
    
## opzioni
    # imposta la policy di blocca a...drop (scarta il pacchetto) oppure return (ritorna un tcp/rst)
    set block-policy option
    * drop
    * return
    # imposta nomefile per il fingerprinting degli os
    set fingerprints nomefile
    # imposta il limite per *tipo* a *valore* 
    set limit option value
    # tipo puo' essere frags, src-nodes, states, tables, table-entries   
    set limit frags 1000
    set limit src-nodes 1000
    set limit states 100
    set limit tables 100
    set limit table-entries 100
    # setta interfaccia per le statistiche
    set loginterface interface
    # normal, high-latency, aggressive, conservative
    set optimization option 
