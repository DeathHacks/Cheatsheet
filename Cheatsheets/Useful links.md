---
title: Useful links


---

Useful links

[Stabalise shell](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)

[Pentestmonkey Git](https://github.com/pentestmonkey)

[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)

[GTFOBINS](https://gtfobins.github.io/)

[Pentester-BufferOverflow](https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/exploits/buffer-overflows.rst)


[AD Exploit cheat-sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)

[Cheatsheet-God](https://github.com/OlivierLaflamme/Cheatsheet-God)

[PENTESTING-BIBLE](https://github.com/blaCCkHatHacEEkr/PENTESTING-BIBLE)

[hacker-roadmap](https://github.com/sundowndev/hacker-roadmap)

[Active-Directory-Exploitation-Cheat-Sheet](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)

[penetration-testing-cheat-sheet](https://github.com/ivan-sincek/penetration-testing-cheat-sheet)

[Awesome-RedTeam-Cheatsheet](https://github.com/RistBS/Awesome-RedTeam-Cheatsheet)

[wifi-penetration-testing-cheat-sheet](https://github.com/ivan-sincek/wifi-penetration-testing-cheat-sheet)

[recon Cheat-sheet](https://github.com/Knowledge-Wisdom-Understanding/recon)

[Cloakify](https://github.com/TryCatchHCF/Cloakify)

[JustTryHarder](https://github.com/sinfulz/JustTryHarder)

[enum4linux-ng](https://github.com/cddmp/enum4linux-ng)

[haiti](https://github.com/noraj/haiti)

[webstor](https://github.com/RossGeerlings/webstor)

[InfosecHouse](https://github.com/InfosecHouse/InfosecHouse)

[MobileApp-Pentest-Cheatsheet   (Mobile app pentest cheatsheat)](https://github.com/christofersimbar/MobileApp-Pentest-Cheatsheet)

[OSCPRepo](https://github.com/rewardone/OSCPRepo)

[awesome-hacking](https://github.com/jekil/awesome-hacking)

[pentest-guide](https://github.com/Voorivex/pentest-guide)

[One-Lin3r](https://github.com/D4Vinci/One-Lin3r)

[awesome-pentest-cheat-sheets](https://github.com/coreb1t/awesome-pentest-cheat-sheets)

[liodeus cheatsheets](https://liodeus.github.io/tags.html)


Enum users using enum4linux
```
(echo; enum4linux -U MSF2|grep user:|cut -d\[ -f2|cut -d\] -f1) >
/home/kali/Desktop/users.txt
```
```
?query=query%3Bstart-sleep+-s+15+%23
?query=query%26timeout+%2FT+15
```
```
wpscan --rua -e ap,at,tt,cb,dbe,u,m --url http://geek.com \[--plugins-detection aggressive\] --api-token Va7bjbU6BHmpRutUX49rvISh1A1ISgaWt7WFAfTJnWw --passwords /usr/share/wordlists/external/SecLists/Passwords/probable-v2-top1575.txt
```
```
while IFS=: read user shadow uid guid geco home sh; do if [[ $uid -le 100 ]]; then echo $user $home; fi; done < /etc/passwd 
```

Prints users and home directory from etc/passwd