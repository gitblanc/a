---
title: Tools I use for Pentesting 🌀
---
![](gitblanc.png)

## Information Gathering
### Domain name

- [whois](https://docs.microsoft.com/en-us/sysinternals/downloads/whois) - Windows Whois performs the registration record for the domain name or IP address that you specify.
- [DNSrecon-gui](https://github.com/micro-joan/DNSrecon-gui) - DNSrecon tool with GUI for Kali Linux
- [Dnsx](https://github.com/projectdiscovery/dnsx) - dnsx is a fast and multi-purpose DNS toolkit allow to run multiple DNS queries of your choice with a list of user-supplied resolvers.

### Subdomain

- [subDomainsBrute](https://github.com/lijiejie/subDomainsBrute) - A fast sub domain brute tool for pentesters 
- [ksubdomain](https://github.com/boy-hack/ksubdomain) - Subdomain enumeration tool, asynchronous dns packets, use pcap to scan 1600,000 subdomains in 1 second 
- [Sublist3r](https://github.com/aboul3la/Sublist3r) - Fast subdomains enumeration tool for penetration testers 
- [OneForAll](https://github.com/shmilylty/OneForAll) -  OneForAll is a powerful subdomain integration tool
- [LayerDomainFinder](https://github.com/euphrat1ca/LayerDomainFinder) - a subdomains enumeration tool by Layer
- [ct](https://github.com/knownsec/ct) - Collect information tools about the target domain.
- [Subfinder](https://github.com/projectdiscovery/subfinder) - Subfinder is a subdomain discovery tool that discovers valid subdomains for websites. Designed as a passive framework to be useful for bug bounties and safe for penetration testing.
- [Probable_subdomains](https://github.com/zzzteph/probable_subdomains) - Subdomains analysis and generation tool. Reveal the hidden!
    - [domains](https://weakpass.com/generate/domains) - Generate subdomains and wordlists Online.
- [MassDNS](https://github.com/blechschmidt/massdns) - High-performance DNS stub resolver targeting those who seek to resolve a massive amount of domain names in the order of millions or even billions.
- [altdns](https://github.com/infosec-au/altdns) - Altdns takes in words that could be present in subdomains under a domain (such as test, dev, staging) as well as takes in a list of subdomains that you know of.
- [dnscan](https://github.com/rbsec/dnscan) - Fast and lightweight dns bruteforcer with built-in wordlist and zone transfer checks.

### Google Hacking

- [GHDB](https://www.exploit-db.com/google-hacking-database/) - Google Hack Database 
- [SearchDiggity](http://www.bishopfox.com/resources/tools/google-hacking-diggity/attack-tools/) - SearchDiggity 3.1 is the primary attack tool of the Google Hacking Diggity Project
- [Katana](https://github.com/adnane-X-tebbaa/Katana) - A Python Tool For google Hacking
- [GooFuzz](https://github.com/m3n0sd0n4ld/GooFuzz) - GooFuzz is a tool to perform fuzzing with an OSINT approach, managing to enumerate directories, files, subdomains or parameters without leaving evidence on the target's server and by means of advanced Google searches (Google Dorking).
- [Pagodo](https://github.com/opsdisk/pagodo) - pagodo (Passive Google Dork) - Automate Google Hacking Database scraping and searching.
- [Google-Dorks](https://github.com/Proviesec/google-dorks) - Useful Google Dorks for WebSecurity and Bug Bounty.

### Port Scan

- [Nmap | Zenmap](https://nmap.org/) - Free and open source utility for network discovery and security auditing
- [Masscan](https://github.com/robertdavidgraham/masscan) - TCP port scanner, spews SYN packets asynchronously
- [Ports](https://github.com/nixawk/pentest-wiki/blob/master/3.Exploitation-Tools/Network-Exploitation/ports_number.md) - Common service ports and exploitations
- [Goby](https://gobies.org/) - Attack surface mapping
    - [Gobyu-POC](https://github.com/20142995/Goby) - The POC of Goby
- [Goscan](https://github.com/marco-lancini/goscan) - Interactive Network Scanner
- [NimScan](https://github.com/elddy/NimScan) - Fast Port Scanner
- [RustScan](https://github.com/RustScan/RustScan) -The Modern Port Scanner
- [TXPortMap](https://github.com/4dogs-cn/TXPortMap) - Port Scanner & Banner Identify From TianXiang
- [Scaninfo](https://github.com/redtoolskobe/scaninfo) - fast scan for redtools
- [SX](https://github.com/v-byte-cpu/sx) - Fast, modern, easy-to-use network scanner
- [Yujianportscan](https://github.com/foryujian/yujianportscan) A Fast Port Scanner GUI Tools Build by VB.NET + IOCP
- [Naabu](https://github.com/projectdiscovery/naabu) - A fast port scanner written in go with a focus on reliability and simplicity.
- [Shodan.io](https://www.shodan.io/) - Shodan is the world's first search engine for Internet-connected devices.

## Vulnerability Analysis

### Fuzzing

- [httpX](https://github.com/projectdiscovery/httpx) -httpx is a fast and multi-purpose HTTP toolkit that allows running multiple probes using the retryablehttp library.

### Vulnerability Scanner

- [Struts-Scan](https://github.com/Lucifer1993/struts-scan) - Struts2 vulnerability detection and utilization tools
- [Nikto](https://github.com/sullo/nikto) - Nikto is an Open Source (GPL) web server scanner which performs comprehensive tests against web servers for multiple items
- [W3af](https://github.com/andresriancho/w3af/) - Web application attack and audit framework, the open source web vulnerability scanner
- [Openvas](http://www.openvas.org/) - The world's most advanced Open Source vulnerability scanner and manager
    - [Openvas Docker](https://github.com/mikesplain/openvas-docker)
- [Archery](https://github.com/archerysec/archerysec) - Open Source Vulnerability Assessment and Management helps developers and pentesters to perform scans and manage vulnerabilities
- [Taipan](https://github.com/enkomio/Taipan) - Web application vulnerability scanner
- [Arachni](https://github.com/Arachni/arachni) - Web Application Security Scanner Framework
- [Nuclei](https://github.com/projectdiscovery/nuclei) - Fast and customizable vulnerability scanner based on simple YAML based DSL
- [Xray](https://github.com/chaitin/xray) - A passive-vulnerability-scanner Tool
- [Super-Xray](https://github.com/4ra1n/super-xray) - Web Vulnerability Scanner XRAY GUI Starter 
- [SiteScan](https://github.com/kracer127/SiteScan) - All in One Website Information Gathering Tools for pentest.
- [Banli](https://github.com/Goqi/Banli) - High-risk asset identification and high-risk vulnerability scanner. 
- [vscan](https://github.com/veo/vscan) - Open Source Vulnerability Scanner
- [Wapiti](https://github.com/wapiti-scanner/wapiti) - Web vulnerability scanner written in Python3.
- [Scaninfo](https://github.com/redtoolskobe/scaninfo) - fast scan for redtools
- [osv-scanner](https://github.com/google/osv-scanner) - Vulnerability scanner written in Go which uses the data provided by [https://osv.dev](https://osv.dev/)
- [Afrog](https://github.com/zan8in/afrog) - A Vulnerability Scanning Tools For Penetration Testing
- [OpalOPC](https://opalopc.com/) - A vulnerability and misconfiguration scanner for OPC UA applications

## Web applications

### CMS & Framework Identification

#### Offline

- [AngelSword](https://github.com/Lucifer1993/AngelSword) - CMS vulnerability detection framework 
- [WhatWeb](https://github.com/urbanadventurer/WhatWeb) - Next generation web scanner 
- [Wappalyzer](https://github.com/AliasIO/Wappalyzer) - Cross-platform utility that uncovers the technologies used on websites 
- [Whatruns](https://www.whatruns.com/) - A free browser extension that helps you identify technologies used on any website at the click of a button (Just for chrome)
- [WhatCMS](https://github.com/HA71/WhatCMS) - CMS Detection and Exploit Kit based on Whatcms.org API
- [CMSeeK](https://github.com/Tuhinshubhra/CMSeeK) - CMS Detection and Exploitation suite - Scan WordPress, Joomla, Drupal and over 180 other CMSs
- [EHole](https://github.com/EdgeSecurityTeam/EHole) - CMS Detection for RedTeam 
- [ObserverWard](https://github.com/0x727/ObserverWard) - Cross platform community web fingerprint identification tool
    - [FingerprintHub](https://github.com/0x727/FingerprintHub) - The Database of ObserverWard

#### Online

- [Yunsee](http://www.yunsee.cn/) - Online website for to find the CMS footprint 
- [Bugscaner](http://whatweb.bugscaner.com/look/) - A simple online fingerprint identification system that supports hundreds of cms source code recognition 
- [WhatCMS online](https://whatcms.org/) - CMS Detection and Exploit Kit website Whatcms.org
- [TideFinger](http://finger.tidesec.com/) - Fingerprinter Tool from TideSec Team 
- [360finger-p](https://fp.shuziguanxing.com/) - Fingerprinter Tool from 360 Team


### Web Application Proxies

- [Burpsuite](https://portswigger.net/) - Burpsuite is a graphical tool for testing Web application security
- [ZAP](https://github.com/zaproxy/zaproxy) One of the world’s most popular free security tools
- [Mitmproxy](https://github.com/mitmproxy/mitmproxy) - An interactive TLS-capable intercepting HTTP proxy for penetration testers and software developers.
- [Broxy](https://github.com/rhaidiz/broxy) - An HTTP/HTTPS intercept proxy written in Go.
- [Hetty](https://github.com/dstotijn/hetty) - An HTTP toolkit for security research.
- [Proxify](https://github.com/projectdiscovery/proxify) - Swiss Army knife Proxy tool for HTTP/HTTPS traffic capture, manipulation, and replay on the go.

### Web browser extensions

- [Hack-Tools](https://github.com/LasCC/Hack-Tools) - The all-in-one Red Team extension for Web Pentester
- [Wappalyzer](https://chromewebstore.google.com/detail/wappalyzer-technology-pro/gppongmhjkpfnbhagpmjfkannfbllamg?hl=es) - Wappalyzer is a browser extension that uncovers the technologies used on websites. It detects content management systems, web shops, web servers, JavaScript frameworks, analytics tools and many more.
- [FoxyProxy](https://chromewebstore.google.com/detail/foxyproxy/gcknhkkoolaabfmlnjonogaaifnjlfnp?hl=es) - Proxy changer tool

### Web Crawlers & Directory Brute Force

- [Dirbrute](https://github.com/Xyntax/DirBrute) - Multi-thread WEB directory blasting tool (with dics inside)
- [Dirb](https://dirb.sourceforge.net/) - DIRB is a Web Content Scanner. It looks for existing (and/or hidden) Web Objects. It basically works by launching a dictionary based attack against a web server and analyzing the responses.
- [ffuf](https://github.com/ffuf/ffuf) - Fast web fuzzer written in Go.
- [Dirbuster](https://sourceforge.net/projects/dirbuster/) - DirBuster is a multi threaded java application designed to brute force directories and files names on web/application servers.
- [Dirsearch](https://github.com/maurosoria/dirsearch) - Web path scanner.
- [Gobuster](https://github.com/OJ/gobuster) Directory/File, DNS and VHost busting tool written in Go. 
- [WebPathBrute](https://github.com/7kbstorm/7kbscan-WebPathBrute) - Web path Bruter.
- [wfuzz](https://github.com/xmendez/wfuzz) - Web application fuzzer
- [Dirmap](https://github.com/H4ckForJob/dirmap) - An advanced web directory & file scanning tool that will be more powerful than DirBuster, Dirsearch, cansina, and Yu Jian.
- [YJdirscan](https://github.com/foryujian/yjdirscan) - Yujian dirscan Gui Pro

## Password attacks

- [Hydra](https://github.com/vanhauser-thc/thc-hydra) - Hydra is a parallelized login cracker which supports numerous protocols to attack
- [Medusa](http://foofus.net/goons/jmk/medusa/medusa.html) - Medusa is intended to be a speedy, massively parallel, modular, login brute-forcer
- [Sparta](https://github.com/SECFORCE/sparta) - Network Infrastructure Penetration Testing Tool. 
- [Hashcat](https://github.com/hashcat/hashcat) - World's fastest and most advanced password recovery utility
- [Patator](https://github.com/lanjelot/patator) - Patator is a multi-purpose brute-forcer, with a modular design and a flexible usage.
- [HackBrowserDat](https://github.com/moonD4rk/HackBrowserData) - Decrypt passwords/cookies/history/bookmarks from the browser
- [John](https://github.com/openwall/john) - John the Ripper jumbo - advanced offline password cracker, which supports hundreds of hash and cipher types, and runs on many operating systems, CPUs, GPUs, and even some FPGAs.
- [crowbar](https://github.com/galkan/crowbar) - brute forcing tool that can be used during penetration tests. Supports OpenVPN, RDP (with NLA), ssh and VNC.

### Wordlists

- [wordlists](https://github.com/trickest/wordlists/) - Real-world infosec wordlists, updated regularly
- [psudohash](https://github.com/t3l3machus/psudohash) - Password list generator that focuses on keywords mutated by commonly used password creation patterns
- [wister](https://github.com/cycurity/wister) - A wordlist generator tool, that allows you to supply a set of words, giving you the possibility to craft multiple variations from the given words, creating a unique and ideal wordlist to use regarding a specific target.
- [Rockyou](https://gitlab.com/kalilinux/packages/wordlists) - wordlists packaging for Kali Linux.
- [Weakpass](https://weakpass.com/) - For any kind of bruteforce find wordlists.

## Exploitation Tools

### Vulnerability Search

- [SPLOITUS](https://sploitus.com/) - Sploitus is а convenient central place for identifying the newest exploits and finding attacks that exploit known vulnerabilities
- [SearchSploit](https://github.com/offensive-security/exploitdb) - The official Exploit Database repository
- [Getsploit](https://github.com/vulnersCom/getsploit) - Command line utility for searching and downloading exploits
- [Houndsploit](https://github.com/nicolas-carolo/houndsploit) - An advanced graphical search engine for Exploit-DB
- [OSV](https://osv.dev/) - Open source vulnerability DB and triage service.

### Cross-site Scripting (XSS)

- [BeeF](https://github.com/beefproject/beef) - The Browser Exploitation Framework Project
- [BlueLotus_XSSReceiver](https://github.com/firesunCN/BlueLotus_XSSReceiver) - XSS Receiver platform without SQL
- [XSStrike](https://github.com/s0md3v/XSStrike) - Most advanced XSS scanner.
- [xssor2](https://github.com/evilcos/xssor2) - XSS'OR - Hack with JavaScript.
- [Xsser-Varbaek](https://github.com/Varbaek/xsser) - From XSS to RCE 2.75 - Black Hat Europe Arsenal 2017 + Extras
- [Xsser-Epsylon](https://github.com/epsylon/xsser) - Cross Site "Scripter" (aka XSSer) is an automatic framework to detect, exploit and report XSS vulnerabilities in web-based applications.
- [Xenotix](https://github.com/ajinabraham/OWASP-Xenotix-XSS-Exploit-Framework) - An advanced Cross Site Scripting (XSS) vulnerability detection and exploitation framework
- [PwnXSS](https://github.com/pwn0sec/PwnXSS) - PwnXSS: Vulnerability (XSS) scanner exploit
- [dalfox](https://github.com/hahwul/dalfox) - DalFox is an powerful open source XSS scanning tool and parameter analyzer, utility
- [ezXSS](https://github.com/ssl/ezXSS) - ezXSS is an easy way for penetration testers and bug bounty hunters to test (blind) Cross Site Scripting.

### Sql Injection

- [Sqlmap](https://github.com/sqlmapproject/sqlmap) - Automatic SQL injection and database takeover tool
- [SSQLInjection](https://github.com/shack2/SuperSQLInjectionV1) - SSQLInjection is a SQL injection tool , support Access/MySQL/SQLServer/Oracle/PostgreSQL/DB2/SQLite/Informix Database.
- [Jsql-injection](https://github.com/ron190/jsql-injection) jSQL Injection is a Java application for automatic SQL database injection.
- [NoSQLMap](https://github.com/codingo/NoSQLMap) - Automated NoSQL database enumeration and web application exploitation tool.
- [Sqlmate](https://github.com/s0md3v/sqlmate) - A friend of SQLmap which will do what you always expected from SQLmap
- [SQLiScanner](https://github.com/0xbug/SQLiScanner) - Automatic SQL injection with Charles and sqlmap api
- [sql-injection-payload-list](https://github.com/payloadbox/sql-injection-payload-list) - SQL Injection Payload List
- [Advanced-SQL-Injection-Cheatsheet](https://github.com/kleiton0x00/Advanced-SQL-Injection-Cheatsheet) - A cheat sheet that contains advanced queries for SQL Injection of all types.

### Command Injection

- [Commix](https://github.com/commixproject/commix) - Automated All-in-One OS command injection and exploitation tool

### File Inclusion

- [LFIsuite](https://github.com/D35m0nd142/LFISuite) - Totally Automatic LFI Exploiter (+ Reverse Shell) and Scanner
- [Lfi-Space](https://github.com/capture0x/Lfi-Space) - Lfi Scan Tool
- [Kadimus](https://github.com/P0cL4bs/Kadimus) - Kadimus is a tool to check sites to lfi vulnerability , and also exploit it
- [Shellfire](https://github.com/unix-ninja/shellfire) - Exploitation shell for exploiting LFI, RFI, and command injection vulnerabilities
- [LFIter2](https://github.com/3mrgnc3/LFIter2) - LFIter2 Local File Include (LFI) Tool - Auto File Extractor & Username Bruteforcer
- [FDsploit](https://github.com/chrispetrou/FDsploit) - File Inclusion & Directory Traversal fuzzing, enumeration & exploitation tool.

### File Upload

- [Fuxploider](https://github.com/almandin/fuxploider) - File upload vulnerability scanner and exploitation tool

### XML External Entity Attack (XXE)

- [XXEinjector](https://github.com/enjoiz/XXEinjector) - Tool for automatic exploitation of XXE vulnerability using direct and different out of band methods
- [Oxml_xxe](https://github.com/BuffaloWill/oxml_xxe) - A tool for embedding XXE/XML exploits into different filetypes

### Cross-site request forgery (CSRF)

- [Deemon](https://github.com/tgianko/deemon/) - Deemon is a tool to detect CSRF in web application

### Deserialization exploit framework

- [Ysomap](https://github.com/wh1t3p1g/ysomap) - A helpful Java Deserialization exploit framework.

### Exploit framework

- [POC-T](https://github.com/Xyntax/POC-T) - Pentest Over Concurrent Toolkit
- [Pocsuite3](https://github.com/knownsec/pocsuite3) - pocsuite3 is an open-sourced remote vulnerability testing framework developed by the Knownsec 404 Team.
- [Metasploit](https://github.com/rapid7/metasploit-framework) - The world’s most used penetration testing framework
- [Venom](https://github.com/r00t-3xp10it/venom) - Shellcode generator/compiler/handler (metasploit)
- [Empire](https://github.com/BC-SECURITY/Empire) - Empire is a PowerShell and Python post-exploitation agent
- [Starkiller](https://github.com/BC-SECURITY/Starkiller) - Starkiller is a Frontend for PowerShell Empire.
- [Koadic](https://github.com/zerosum0x0/koadic) - Koadic C3 COM Command & Control - JScript RAT
- [Viper](https://github.com/FunnyWolf/Viper) - metasploit-framework UI manager Tools
- [MSFvenom-gui](https://github.com/ssooking/msfvenom-gui) - gui tool to create normal payload by msfvenom
- [MYExploit](https://github.com/achuna33/MYExploit) - A GUI Tools for Scanning OA vulnerabilities

## Maintaining Access

### Shell

- [Goshell](https://github.com/eze-kiel/goshell) - Generate reverse shells in command line with Go !
- [Print-My-Shell](https://github.com/sameera-madushan/Print-My-Shell) - Python script wrote to automate the process of generating various reverse shells.
- [Reverse-shell-generator](https://github.com/0dayCTF/reverse-shell-generator) - Hosted Reverse Shell generator with a ton of functionality. -- (Great for CTFs)
- [Girsh](https://github.com/nodauf/Girsh) - Automatically spawn a reverse shell fully interactive for Linux or Windows victim
- [Blueshell](https://github.com/whitehatnote/BlueShell) - Generate a reverse shells for RedTeam
- [Clink](http://mridgers.github.io/clink/) - Powerful Bash-style command line editing for cmd.exe
- [Natpass](https://github.com/jkstack/natpass) - A new RAT Tools, Support Web VNC and Webshell
- [Platypus](https://github.com/WangYihang/Platypus) 🔨 A modern multiple reverse shell sessions manager written in go
- [shells](https://github.com/4ndr34z/shells/) - Script for generating revshells
- [Reverse_ssh](https://github.com/NHAS/reverse_ssh) - SSH based reverse shell
- [Hoaxshell](https://github.com/t3l3machus/hoaxshell) - A Windows reverse shell payload generator and handler that abuses the http(s) protocol to establish a beacon-like reverse shell.

### Listener

- [Netcat](https://netcat.sourceforge.net/) - Netcat is a featured networking utility which reads and writes data across network connections, using the TCP/IP protocol.
- [Rustcat](https://github.com/robiot/rustcat) - Rustcat(rcat) - The modern Port listener and Reverse shell.
- [Rlwrap](https://github.com/hanslub42/rlwrap) - A readline wrapper.
- [Pwncat](https://github.com/calebstewart/pwncat) - Fancy reverse and bind shell handler.
- [Powercat](https://github.com/besimorhino/powercat) - netshell features all in version 2 powershell.
- [Socat](https://repo.or.cz/socat.git) - Socat is a flexible, multi-purpose relay tool.

### Privilege Escalation Auxiliary

- [windows-exploit-suggester](https://github.com/GDSSecurity/Windows-Exploit-Suggester) - This tool compares a targets patch levels against the Microsoft vulnerability database in order to detect potential missing patches on the target
- [Windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits) - windows-kernel-exploits
- [linux-exploit-suggester-2](https://github.com/jondonas/linux-exploit-suggester-2) - Next-Generation Linux Kernel Exploit Suggester
- [Linux-kernel-exploits](https://github.com/SecWiki/linux-kernel-exploits) - linux-kernel-exploits Linux
- [BeRoot](https://github.com/AlessandroZ/BeRoot) - Privilege Escalation Project - Windows / Linux / Mac
- [PE-Linux](https://github.com/WazeHell/PE-Linux) - Linux Privilege Escalation Tool By WazeHell
- [Portia](https://github.com/SpiderLabs/portia) - Portia aims to automate a number of techniques commonly performed on internal network penetration tests after a low privileged account has been compromised.
- [PEASS-ng](https://github.com/carlospolop/PEASS-ng) - PEASS - Privilege Escalation Awesome Scripts SUITE (with colors)
- [GTFOBins](https://gtfobins.github.io/) - GTFOBins is a curated list of Unix binaries that can be used to bypass local security restrictions in misconfigured systems.
- [LOLBAS](https://lolbas-project.github.io/) - Living Off The Land Binaries, Scripts and Libraries.
- [WADComs](https://wadcoms.github.io/) - WADComs is an interactive cheat sheet, containing a curated list of offensive security tools and their respective commands, to be used against Windows/AD environments.
- [HijackLibs](https://hijacklibs.net/) - DLL Hijacking is, in the broadest sense, tricking a legitimate/trusted application into loading an arbitrary DLL.
- [GTFOBLookup](https://github.com/nccgroup/GTFOBLookup) - Offline command line lookup utility for GTFOBins、LOLBAS and WADComs.
- [PrintNotifyPotato](https://github.com/BeichenDream/PrintNotifyPotato) - PrintNotifyPotato

### Command & Control (C2)

- [DeimosC2](https://github.com/DeimosC2/DeimosC2) - DeimosC2 is a Golang command and control framework for post-exploitation.
- [Sliver](https://github.com/BishopFox/sliver) - Implant framework
- [PHPSploit](https://github.com/nil0x42/phpsploit) - Full-featured C2 framework which silently persists on webserver via evil PHP oneliner
- [Shad0w](https://github.com/bats3c/shad0w) - A post exploitation framework designed to operate covertly on heavily monitored environments (Win8、Win10)
- [Covenant](https://github.com/cobbr/Covenant) - Covenant is a collaborative .NET C2 framework for red teamers.
- [Emp3r0r](https://github.com/jm33-m0/emp3r0r) - linux post-exploitation framework made by linux user
- [C3](https://github.com/FSecureLABS/C3) - Custom Command and Control (C3). A framework for rapid prototyping of custom C2 channels, while still providing integration with existing offensive toolkits.
- [byob](https://github.com/malwaredllc/byob) - An open-source post-exploitation framework for students, researchers and developers.
- [Havoc](https://github.com/HavocFramework/Havoc) - Havoc is a modern and malleable post-exploitation command and control framework.
- [Villain](https://github.com/t3l3machus/Villain) - Villain is a Windows & Linux backdoor generator and multi-session handler that allows users to connect with sibling servers (other machines running Villain) and share their backdoor sessions, handy for working as a team.

## Intranet penetration

### Service detection

- [Netspy](https://github.com/shmilylty/netspy) - A tool to quickly detect the reachable network segments of the intranet.
- [Cube](https://github.com/JKme/cube) - Intranet penetration testing tools, weak password blasting, information collection and vulnerability scanning.