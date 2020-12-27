---
title: /etc/snmp/snmpd.conf
description: /etc/snmp/snmpd.conf SNMP Agent Configuration File
first-published: 2015-06-18
---

Sometimes, `snmpd` will report the wrong speed of a network interface. For 
example, the following output shows a speed of 10Mbps, whereas the ethernet 
port on the server is actually working at 100Mbps:

        IF-MIB::ifIndex.2 = INTEGER: 2
        IF-MIB::ifDescr.2 = STRING: eth0
        IF-MIB::ifType.2 = INTEGER: ethernetCsmacd(6)
        IF-MIB::ifMtu.2 = INTEGER: 1500
        IF-MIB::ifSpeed.2 = Gauge32: 10000000

The [Net-SNMP documentation mentions this][1]. This can be manually fixed with 
the `interface` directive in `snmpd.conf`, for example:

        interface eth0 6 100000000

<!-- Links -->
[1]: http://www.net-snmp.org/wiki/index.php/FAQ:Agent_36 "Wrong speed/type of network interfaces"
