## Nmap Overview and Demonstration

Sometimes the best way to understand something is to see it in action. This section includes examples of Nmap used in (mostly) fictional yet typical circumstances. Nmap newbies should not expect to understand everything at once. This is simply a broad overview of features that are described in depth in later chapters. The “solutions” included throughout this book demonstrate many other common Nmap tasks for security auditors and network administrators.

## Avatar Online

Felix dutifully arrives at work on December 15th, although he does not expect many structured tasks. The small San Francisco penetration-testing firm he works for has been quiet lately due to impending holidays. Felix spends business hours pursuing his latest hobby of building powerful Wi-Fi antennas for wireless assessments and war driving exploration. Nevertheless, Felix is hoping for more business. Hacking has been his hobby and fascination since a childhood spent learning everything he could about networking, security, Unix, and phone systems. Occasionally his curiosity took him too far, and Felix was almost swept up in the 1990 Operation Sundevil prosecutions. Fortunately Felix emerged from adolescence without a criminal record, while retaining his expert knowledge of security weaknesses. As a professional, he is able to perform the same types of network intrusions as before, but with the added benefit of contractual immunity from prosecution and even a paycheck! Rather than keeping his creative exploits secret, he can brag about them to client management when presenting his reports. So Felix was not disappointed when his boss interrupted his antenna soldering to announce that the sales department closed a pen-testing deal with the Avatar Online gaming company.

Avatar Online (AO) is a small company working to create the next generation of massive multi-player online role-playing games (MMORPGs). Their product, inspired by the Metaverse envisioned in Neil Stevenson's Snow Crash, is fascinating but still highly confidential. After witnessing the high-profile leak of Valve Software's upcoming game source code, AO quickly hired the security consultants. Felix's task is to initiate an external (from outside the firewall) vulnerability assessment while his partners work on physical security, source code auditing, social engineering, and so forth. Felix is permitted to exploit any vulnerabilities found.

The first step in a vulnerability assessment is network discovery. This reconnaissance stage determines what IP address ranges the target is using, what hosts are available, what services those hosts are offering, general network topology details, and what firewall/filtering policies are in effect.

Determining the IP ranges to scan would normally be an elaborate process involving ARIN (or another geographical registry) lookups, DNS queries and zone transfer attempts, various web sleuthing techniques, and more. But in this case, Avatar Online explicitly specified what networks they want tested: the corporate network on 6.209.24.0/24 and their production/DMZ systems residing on 6.207.0.0/22. Felix checks the IP whois records anyway and confirms that these IP ranges are allocated to AO[1]. Felix subconsciously decodes the CIDR notation[2] and recognizes this as 1,280 IP addresses. No problem.

Being the careful type, Felix first starts out with what is known as an Nmap list scan (-sL option). This feature simply enumerates every IP address in the given target netblock(s) and does a reverse-DNS lookup (unless -n was specified) on each. One reason to do this first is stealth. The names of the hosts can hint at potential vulnerabilities and allow for a better understanding of the target network, all without raising alarm bells[3]. Felix is doing this for another reason—to double-check that the IP ranges are correct. The systems administrator who provided the IPs might have made a mistake, and scanning the wrong company would be a disaster. The contract signed with Avatar Online may act as a get-out-of-jail-free card for penetrating their networks, but will not help if Felix accidentally compromises another company's server! The command he uses and an excerpt of the results are shown in Example 1.1.

Example 1.1. Nmap list scan against Avatar Online IP addresses

```
felix> nmap -sL 6.209.24.0/24 6.207.0.0/22

Starting Nmap ( https://nmap.org )
Nmap scan report for 6.209.24.0
Nmap scan report for fw.corp.avataronline.com (6.209.24.1)
Nmap scan report for dev2.corp.avataronline.com (6.209.24.2)
Nmap scan report for 6.209.24.3
Nmap scan report for 6.209.24.4
...
Nmap scan report for dhcp-21.corp.avataronline.com (6.209.24.21)
Nmap scan report for dhcp-22.corp.avataronline.com (6.209.24.22)
Nmap scan report for dhcp-23.corp.avataronline.com (6.209.24.23)
...
Nmap scan report for 6.207.0.0
Nmap scan report for gw.avataronline.com (6.207.0.1)
Nmap scan report for ns1.avataronline.com (6.207.0.2)
Nmap scan report for ns2.avataronline.com (6.207.0.3)
Nmap scan report for ftp.avataronline.com (6.207.0.4)
Nmap scan report for 6.207.0.5
Nmap scan report for 6.207.0.6
Nmap scan report for www.avataronline.com (6.207.0.7)
Nmap scan report for 6.207.0.8
...
Nmap scan report for cluster-c120.avataronline.com (6.207.2.120)
Nmap scan report for cluster-c121.avataronline.com (6.207.2.121)
Nmap scan report for cluster-c122.avataronline.com (6.207.2.122)
...
Nmap scan report for 6.207.3.255
Nmap done: 1280 IP addresses (0 hosts up) scanned in 331.49 seconds
felix>
```

Reading over the results, Felix finds that all of the machines with reverse-DNS entries resolve to Avatar Online. No other businesses seem to share the IP space. Moreover, these results give Felix a rough idea of how many machines are in use and a good idea of what many are used for. He is now ready to get a bit more intrusive and try a port scan. He uses Nmap features that try to determine the application and version number of each service listening on the network. He also requests that Nmap try to guess the remote operating system via a series of low-level TCP/IP probes known as OS fingerprinting. This sort of scan is not at all stealthy, but that does not concern Felix. He is interested in whether the administrators of AO even notice these blatant scans. After a bit of consideration, Felix settles on the following command:

nmap -sS -p- -PE -PP -PS80,443 -PA3389 -PU40125 -A -T4 -oA avatartcpscan-%D 6.209.24.0/24 6.207.0.0/22

These options are described in later chapters, but here is a quick summary of them.

-sS

Enables the efficient TCP port scanning technique known as SYN scan. Felix would have added a U at the end if he also wanted to do a UDP scan, but he is saving that for later. SYN scan is the default scan type, but stating it explicitly does not hurt.

-p-

Requests that Nmap scan every port from 1-65535. This is more comprehensive than the default, which is to scan only the 1,000 ports which we've found to be most commonly accessible in large-scale Internet testing. This option format is simply a short cut for -p1-65535. Felix could have specified -p0-65535 if he wanted to scan the rather illegitimate port zero as well. The -p option has a very flexible syntax, even allowing the specification of a differing set of UDP and TCP ports.

-PE -PP -PS80,443 -PA3389 -PU40125

These are all host discovery techniques (ping types) used in combination to determine which targets on a network are really available and avoid wasting a lot of time scanning IP addresses that are not in use. This particular incantation sends ICMP echo request and timestamp request packets; TCP SYN packet to ports 80 and 443; TCP ACK packets to port 3389; and a UDP packet to port 40,125. If Nmap receives a response from a target host to any of these probes, it considers the host to be up and available for scanning. This is the most effective six-probe combination that we've found in large-scale empirical testing for host discovery against targets over the Internet. It is more extensive than the Nmap default, which is -PE -PS443 -PA80 -PP. In a pen-testing situation, you often want to scan every host even if they do not seem to be up. After all, they could just be heavily filtered in such a way that the probes you selected are ignored but some other obscure port may be available. To scan every IP whether it shows an available host or not, specify the -Pn option instead of all of the above. Felix starts such a scan in the background, though it may take many hours to complete.

-A

This shortcut option turns on Advanced and Aggressive features such as OS and service detection. At the time of this writing it is equivalent to -sV -sC -O --traceroute (version detection, Nmap Scripting Engine with the default set of scripts, remote OS detection, and traceroute). More features may be added to -A later.

-T4

Adjusts timing to the aggressive level (#4 of 5). This is the same as specifying -T aggressive, but is easier to type and spell. In general, the -T4 option is recommended if the connection between you and the target networks is reasonably fast and reliable.

-oA avatartcpscan-%D

Outputs results in every format (normal, XML, grepable) to files named avatartcpscan-<date>.<extension> where the extensions are .nmap, .xml, and .gnmap respectively. The <date> gives the month, day, and year in a format like 073010. All of the output formats include the start date and time, but Felix likes to note the date explicitly in the filename. Normal output and errors are still sent to stdout[4] as well.

6.209.24.0/24 6.207.0.0/22

These are the Avatar Online netblocks discussed above. They are given in CIDR notation, but Nmap allows them to be specified in many other formats. For example, 6.209.24.0/24 could instead be specified as 6.209.24.0-255.

Since such a comprehensive scan against more than a thousand IP addresses could take a while, Felix simply starts it executing and resumes work on his Yagi antenna. An couple hours later he notices that it has finished and takes a peek at the results.

Example 1.2. Nmap results against an AO firewall

```
Nmap scan report for fw.corp.avataronline.com (6.209.24.1)
(The 65530 ports scanned but not shown below are in state: filtered)
PORT     STATE  SERVICE    VERSION
22/tcp   open   ssh        OpenSSH 3.7.1p2 (protocol 1.99)
| ssh-hostkey: 1024 7c:14:2f:92:ca:61:90:a4:11:3c:47:82:d5:8e:a9:6b (DSA)
|_2048 41:cf7d:839d:7f66:0ae1:8331:7fd4:5a97:5a (RSA)
|_sshv1: Server supports SSHv1
53/tcp   open   domain     ISC BIND 9.2.1
110/tcp  open   pop3       Courier pop3d
113/tcp  closed auth
143/tcp  open   imap       Courier Imap 1.6.X - 1.7.X
3128/tcp open   http-proxy Squid webproxy 2.2.STABLE5
Device type: general purpose
Running: Linux 2.4.X|2.5.X
OS details: Linux Kernel 2.4.0 - 2.5.20
Uptime 3.134 days

```

