---
description: ( TCP 161 )
---

# SNMP

## :information\_source: Introduction

[Simple Network Management Protocol](https://datatracker.ietf.org/doc/html/rfc1157/)

SNMP (Simple Network Management Protocol) is an application layer protocol which uses UDP protocol to maintain and manage routers, hubs and switches other network devices on an IP network. SNMP is a very common protocol found enabled on a variety of operating systems like Windows Server, Linux & UNIX servers as well as network devices like routers, switches etc. SNMP enumeration is used to enumerate user accounts, passwords, groups, system names, devices on a target system. SNMP contains two passwords that for configuring and accessing the SNMP agent from the management station. The two SNMP passwords are:

#### 1) Read Community String : Configuring of the device or system can be viewed with help of this password. These strings are public.   2) Read/Write Community String : Configuration on the device can be changed or edited using this password. These strings are private.

When administrators leave the community strings at the default setting, attacker can use these default community strings (passwords) for changing or viewing the configuration of the device or system. Attackers enumerate SNMP to extract information about network resources such as hosts, routers, devices, shares, etc, and network information such as ARP tables, routing tables, device specific, and traffic stastitics.

SNMP uses a disturbed architechture comprising SNMP managers, SNMP agents, and Several related components. Commands associated with SNMP include:

#### 1) GetRequest : Used by the SNMP manager to request information from the SNMP Agent   2) GetNextRequest : Used by the SNMP manager continuosly to retrieve all the data stored in the array or table.   3) GetResponse : Used by the SNMP agent to satisfy a request made by the SNMP manager.   4) SetRequest : used by the SNMP manager to motify the value of a parameter within the SNMP agent’s Management Information Base (MIB).   5) Trap : Used by the SNMP agent to inform the pre-configured SNMP manager of a certain event.

### MIB

MIB stands for Management Information Base and is a collection of information organized hierarchically. These are accessed using a protocol such as SNMP. There are two types of MIBs: scalar and tabular.

Scalar objects define a single object instance whereas tabular objects define multiple related object instances grouped in MIB tables.

### OIDs

OIDs stands for Object Identifiers. OIDs uniquely identify managed objects in a MIB hierarchy. This can be depicted as a tree, the levels of which are assigned by different organizations. Top level MIB object IDs (OIDs) belong to different standard organizations.

These OIDs are not random addresses, rather they are highly structured and follow a hierarchical tree pattern, similar to the folder system in your computer. One difference is that all SNMP objects in the tree hierarchy are numbered. For example, the root object can be number “1” while the next one can be number “3”, followed by “6”, and so on. This is why an OID will be a string of numbers, starting from the root, so you can trace it down to the exact device. Let us take the example of an OID here

#### 1 . 3 . 6 . 1 . 4 . 1 . 1452 . 1 . 2 . 5 . 1 . 3. 21 . 1 . 4 . 7

```
1 – this is called the ISO and it establishes that this is an OID. This is why all OIDs start with “1”

3 – this is called ORG and it is used to specify the organization that built the device.

6 – this is the dod or the Department of Defense which is the organization that established the Internet first.

1 – this is the value of the internet to denote that all communications will happen through the Internet.

4 – this value determines that this device is made by a private organization and not a government one.

1 – this value denotes that the device is made by an enterprise or a business entity. 

These first six values tend to be the same for all devices and they give you the basic information about them. This sequence of numbers will be the same for all OIDs, except when the device is made by the government.
Moving on to the next set of numbers.

1452 – gives the name of the organization that manufactured this device.

1 – explains the type of device. In this case, it is an alarm clock.

2 – determines that this device is a remote terminal unit.

The rest of the values give specific information about the device.

5 – denotes a discrete alarm point. 1 – specific point in the device

3 – port

21 – address of the port

1 – display for the port

4 – point number

7 – state of the point
```

![](<../.gitbook/assets/image (37).png>)

### SNMP Versions

There are 2 important versions of SNMP:

#### SNMPv1: Main one, it is still the most frequent, the authentication is based on a string (community string) that travels in plain-text (all the information travels in plain text). Version 2 and 2c send the traffic in plain text also and uses a community string as authentication.   SNMPv3: Uses a better authentication form and the information travels encrypted using (dictionary attack could be performed but would be much harder to find the correct creds that inn SNMPv1 and v2).

### Community Strings

As mentioned before, in order to access the information saved on the MIB you need to know the community string on versions 1 and 2/2c and the credentials on version 3. The are 2 types of community strings:

public mainly read only functions private Read/Write in general

Note that the writability of an OID depends on the community string used, so even if you find that "public" is being used, you could be able to write some values. Also, there may exist objects which are always "Read Only". If you try to write an object a noSuchName or readOnly error is received.

In versions 1 and 2/2c if you to use a bad community string the server wont respond. So, if it responds, a valid community strings was used.

## :ballot\_box\_with\_check: Checklist

* [ ] Enumeration with community strings
* [ ] Brute force community string
* [ ] Modifying configurations and MIB values
* [ ] Eavesdropping
* [ ] Agent discovery

## SNMP Enumeration

### nmap & metasploit

```
use auxiliary/scanner/snmp/snmp_enum
use auxiliary/scanner/snmp/snmp_enumshares
auxiliary/scanner/snmp/snmp_enumusers

nmap -sU -p 161 10.10.10.10
# if found do: snmp-check -t 10.10.10.10

# important MIBs to look for
Names
Services
Listening ports

nmap -sU -sV -p 161 --script snmp-interfaces --script-args snmpcommunity=[community]
nmap -sU -sV -p 161 --script snmp-netstat --script-args snmpcommunity=[community]
nmap -sU -sV -p 161 --script snmp-processes --script-args snmpcommunity=[community]
nmap -sU -sV -p 161 --script snmp-sysdescr --script-args snmpcommunity=[community] 
```

### onesixtyone

A standalone SNMP community string scanner

does not stop scanning once a community string is found and will enumerate all community string guesses for each host. it is designed to scan at LAN speeds so you can increase per-packet delay with `-w` if you are scanning remote networks. it can scan 256 hosts in \~6 minutes.

```
onesixtyone -c snmp.txt -i hosts.txt
```

### Braa

it is able to query dozens or hundreds of hosts simultaneously, and in a single process.

```
braa ignite123@192.168.1.125:.1.3.6.*
```

### snmpenum

```
snmpenum 192.168.56.102 private linux.txt
snmpenum [ip] [community string (public/private/whatever)] [config file]
```

### snmpcheck

```
snmpcheck -t 10.10.10.4 -c public
```

### snmpget

```
snmpget -v 1 -c public [IP] [version]
```

### snmpbulkwalk

```
snmpbulkwalk -v2c -c public -Cn0 -Cr10 IP
snmpbulkwalk -v 2c -c public [ip]
```

## Changing MIB Values

### snmpwalk & snmpset

change an MIB variable using `snmpset` (included with Net-SNMP tools)

validate change using `snmpwalk`

```
snmpwalk -v 1 -c public [IP]

# enumerate MIB
snmpwalk -v2c -c public 192.168.31.1 system.sysLocation.0

# set MIB value
snmpset -v2c -c public 192.168.31.1 system.sysLocation.0 s "foo"
```

### metasploit

```
use auxiliary/scanner/snmp/snmp_set
```

## Brute Force Community Strings

```
use auxiliary/scanner/snmp/snmp_login
nmap -sU -p 161 -n --script snmp-brute [ip] --script-args snmp-brute.communitiesdb=wordlist
```

## SNMP Eavesdropping

### Ettercap

If SNMPv1 or SNMPv2c is in use and the attacker can observe the traffic, the community string can be obtained through password-sniffing techniques. If SNMP activity is present on the LAN, an attacker who launches a MitM attack with Ettercap will see output similar to that shown on this screen, identifying the SNMP agent sending or receiving SNMP traffic with the community string and the observed SNMP version.

```
ettercap -TqM arp:remote /10.144.246.1-254// /10.144.246.1-254//
```

{% hint style="info" %}
Note that Ettercap does not identify the address of the NMS interacting with the SNMP agent. To collect this level of detail, configure Ettercap to log observed traffic with the "-p pcapfile" argument and then inspect the address information with Wireshark to identify the NMS.
{% endhint %}

## SNMP Agent Discovery

Scanning for SNMP agent devices has some difficulty due to limitations with UDP port scanning (when ICMP is filtered, UDP port scanning is ineffective, and very slow) and variations in how different SNMP agents respond to requests when configured with the wrong SNMP community string. Fortunately, Nmap's version- scanning feature will probe a target system on UDP/161 to identify behavior that is characteristic of an SNMP agent. This technique has a high degree of success when targeting common embedded SNMP implementations (such as routers and other networking devices) as well as any agents using a default community string of "public". Nmap will also identify any system with SNMPv3 support, due to changes in the protocol where the SNMP agent will respond to failed authentication requests.

{% hint style="info" %}
Nmap's version scan will not identify the presence of Windows hosts running the Microsoft SNMP agent with a non-default community string. We'll look at alternate techniques for identifying these SNMP agents in this module.
{% endhint %}

As a secondary technique to narrow down a list of devices that are likely SNMP managed devices, DNS interrogation can also be used. Hostnames such as "cacti," "mrtg," "hpov," "Tivoli," and "nagios" are likely SNMP NMS devices, possibly accepting SNMP traps from managed devices. Further, devices with common acronyms or abbreviations indicating their functionality on the network as a router or bridge of some sort (such as "rtr," "router," "gig," "atm," and so on) in the hostname are also worthwhile targets to further evaluate as possible SNMP managed devices.
