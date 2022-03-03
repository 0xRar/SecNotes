# Ping Sweeping & Footprinting:

## Ping Sweeping:
Ping Sweeping is used to determine which ip addresses in scope are assigned to a host

Ping Sweeping tools perform the same operation to every host in a subnet or IP range

tools: 
- ping
```
ping 192.168.0.0/24
```
- fping
```
fping -a -g IPRANGE

fping -a -g 192.168.0.0 192.168.0.255
fping -a -r 0 -g 192.168.0.0/24

# to suppress warnings about offline hosts
fping -a -g 192.168.0.0 192.168.0.255 2>/dev/null
```
- nmap 
```
nmap -sn 200.200.0.0/16
nmap -sn 200.200.123.1-12
nmap -sn 172.16.12.*
nmap -sn 200.200.12-13.*

nmap -sn -iL hostslist.txt
```

## OS Fingerprinting

offline fingerprinting can be done with [p0f](https://lcamtuf.coredump.cx/p0f3/).

while active fingerprinting can be done using [nmap](https://nmap.org).

- nmap fingerprinting commands
```
nmap -Pn -O <target>

# limit os detection
nmap --osscan-limit <target>

# guess aggressively
nmap --osscan-guess <target>

# os detection and port connect scan
nmap -sT <target range>
```

**By 0xRar**
