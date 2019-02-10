## 1.  SCANNING   
Nmap ("Network Mapper") is a free and open source (license) utility for network discovery and security auditing. Many systems and network administrators also find it useful for tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. 

HOST DISCOVERY:

Quick TCP scan:

    nmap -sC -sV -oA quick 10.10.10.10
    
Full TCP scan:
    
    nmap -sC -sV -p- -oA full 10.10.10.10
    
 Quick UDP scan:
 
    nmap -sU -sV -oA quick_udp 10.10.10.10
    
     
Full UDP scan:

    nmap -sU -sV -p- -oA full_udp 10.10.10.10
    
How to scan with NSE scripts. You can check scripts with this command:
 
    locate *.nse

    nmap 192.168.1.1 --script default,safe -v -O -sV
    nmap -sC example.com -v


## Website scanning

Nikto is an Open Source (GPL) web server scanner which performs comprehensive tests against web servers for multiple items, including over 6700 potentially dangerous files/programs, checks for outdated versions of over 1250 servers, and version specific problems on over 270 servers. It also checks for server configuration items such as the presence of multiple index files, HTTP server options, and will attempt to identify installed web servers and software. Scan items and plugins are frequently updated and can be automatically updated.

    nikto -h 192.168.1.0
    
dirsearch is a simple command line tool designed to brute force directories and files in websites.
    
    ./dirsearch.py -u 192.168.1.0 -e asd -t50
    
## More nmap scripts
    
Checking for cross-site scripting vulnabilities:

    nmap -Pn --script=http-xssed 192.168.2.1
    nmap -p80 --script http-unsafe-output-escaping scanme.nmap.org
    
checking for SQLI vulnabilities:

    nmap -p80 --script http-sql-injection scanme.nmap.org
    nmap =p33-6 --script mysql-databases --script-args mysqluser=user,mysqlpass=password scanme.nmap.org

Checking geo-location/whois of a target:

    nmap --script ip-geolocation-* facebook.com
    nmap --script whois* facebook.com

Adding vulnabilities scripts:

    wget https://www.computec.ch/projekte/vulscan/?s=download
    cd /usr/share/nmap/scripts/
    tar xvfz ....

Checking for http vulnabilities:

    nmap -n -Pn -p 80 --open -sV -v --script http-method-tamper example.com
    nmap --script "vuln,exploit" -vv 192.168.2.14 -sV -O
    
Scanning for open smtp open relay:

    nmap -sV --script smtp-open-relay -v scanme.nmap.org
    nmap -sV --script smtp-open-relay -v -iR 10000 -p 25 -n -Pn
    
Checking random for vulnable web servers

    nmap -n -Pn -p 80 --open -sV -vvv --script banner,http-title -iR 100
    
Bruteforce attacks:

    nmap -p80 --script http-brute --script-args http-brute.path=/admin/ scanme.nmap.org
    nmap -p80 --script http-brute --script-args userdb=/var/usernames.txt,passdb=/var/password.txt scanme.nmap.org
    nmap -p80 --script http-wordpress-brute --script-args http-wordpress-brute.threads=5 scanme.nmap.org
    nmap -p25 --script smtp-brute scanme.nmap.org
    
Scan if webserver using a WAF:

    nmap -p80 --script http-waf-detect scanme.nmap.org
    
Ping scan to make it harder to get detected by IDS,WAFS:

    nmap -f 192.168.2.1
    nmap --mtu 192.168.2.1
    
Try to not get detected by IDS (change maccaddress):

    macchanger command
    nmap -f -T 0 -n -Pn --data-length 200 -D 192.168.2.101,192.168.2.102,192.168.103,192.168.2.14,192.168.2.15
    
Using decoys to cause confusion (last one is target also inlcude your own IP address:

    nmap -n -D 192.168.2.101 192.168.2.102 192.168.2.103 192.168.2.14 192.168.2.12
    
Use the spoof option to use someone elses IP address:

    nmap -S www.mexample.com www.example.com -e -eth0 -Pn
    
Using trusted ports so it looks like a normal scan. Examples of common overtrusted ports are DNS 53/TCP/UDP, FTP 20/TCP, Kerberos 88 /TCP/UDP, DHCP 67/UDP :

    nmap -n -T4 -g 53 192.168.2.1
 
Looking for DNS hostnames of a website:

    nmap -Pn --script=dns-brute example.com
    
Scanning for open http proxy"

    nmap --script http-open-proxy -p8080 192.168.2.1-254
    
Scanning for broadcast:

    nmap --script broadcast -v
    nmap --script "broadcast and not targets*" -v
    
Realtime broadcast tracing:

    nmap --script "broadcast and not targets*" -v --script-trace
    nmap --script "not intrusive" -v --script-trace
    
Discovery scan to see if the host is active (use this if you think the standard scan is not good enough:
 
     nmap 192.168.2.1-5 -PS22-25,80,113,1050,35000 -v
    
Random DNS port scans:
     
     nmap -iR 20 -PU53 -sn -vv
     
Scanning for SMB services:
 
     nmap --script smb-os-discovery -vv 192.168.2.1
     nmap -n -Pn -vv -O -sV --script smb-enum*,smb-ls,smb-mbenum,smb-os-discovery,smb-s*,smb-vuln*,smbv2* -vv 192.168.2.1
     
Scan a ICMP echo, timestamp, and netmask request discovery probes:

    nmap 192.168.2.1-255 -PE -sn -v
    
Scan IP protocol list:

    nmap 192.168.2.1-255 -PO1,2,4 -sn
    
ARP scan (local network much faster)

    nmap 192.168.2.1/24 -PR -sn -vv
    
Reverse dns lookup:

    nmap 192.168.2.1-50 -sL --dns-server 192.168.2.0
    
 UDP and TCP scan:
  
    nmap 192.168.2.254 -sU -v -sS
    
Include a message for example the system administrator:

    nmap --data-string "send a message!" 192.168.2.1
    
How to change your macaddress?

    macchanger command
    


