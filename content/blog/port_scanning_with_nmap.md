+++
showdate = true
title = "Port Scanning with Nmap - Part 1"
date = 2023-03-15
url = "/blog/port_scanning_with_nmap/"
+++

# [Target Details]()
URL: http://scanme.nmap.org/

Host: scanme.nmap.org

# [Starting out with basic TCP port scanning]()
The TCP scans use the Transmission Control Protocol as the transport layer protocol for the probes. The different TCP scans are:
## Connect Scan
To scan for all open TCP open ports on a target, we use:

```bash
nmap -v scanme.nmap.org
``` 
The -v flag turns on verbose mode. This increases the verbosity of the details provided by the scan. This is useful for debugging and understanding what is going on. For this case we are going for level 1 verbosity. But higher levels can be achieved using `-vv` or `-vvv`.

The output of the above command is as follows:

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 16:39 IST
Initiating Ping Scan at 16:39
Scanning scanme.nmap.org (45.33.32.156) [2 ports]
Completed Ping Scan at 16:39, 0.27s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 16:39
Completed Parallel DNS resolution of 1 host. at 16:39, 0.24s elapsed
Initiating Connect Scan at 16:39
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Discovered open port 80/tcp on 45.33.32.156
Discovered open port 22/tcp on 45.33.32.156
Discovered open port 31337/tcp on 45.33.32.156
Discovered open port 9929/tcp on 45.33.32.156
Completed Connect Scan at 16:40, 11.89s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.28s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 996 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
9929/tcp  open  nping-echo
31337/tcp open  Elite

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 12.45 seconds
```

The above output shows that the target has 4 open ports. The ports are 22, 80, 9929 and 31337. The services running on these ports are ssh, http, nping-echo and Elite respectively.

Since we ran nmap without root privileges, nmap defaulted to a TCP connect scan. The equivalent of which can be achieved using
```bash
nmap -sT -v scanme.nmap.org
```

In a TCP connect scan, nmap opens up ports like a browser or a client would, making this scan a bit slower and 'noisy' than a TCP SYN scan. This is because the TCP connect scan requires the full TCP handshake to be completed, and the target system can log this activity.


## Stealth scanning 
This is the default scan type used by Nmap under root privileges. By default, Nmap performs a SYN Scan, though it substitutes a connect scan if the user does not have proper privileges to send raw packets (requires root access on Unix). Unprivileged users can only execute connect and FTP bounce scans. 

For stealth scanning, we use the `-sS` flag to launch a TCP SYN scan. This scan type requires root privileges.
```bash
sudo nmap -sS -v scanme.nmap.org
```

The output of the above command is as follows:

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 17:55 IST
Initiating Ping Scan at 17:55
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 17:55, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:55
Completed Parallel DNS resolution of 1 host. at 17:55, 0.00s elapsed
Initiating SYN Stealth Scan at 17:55
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Discovered open port 80/tcp on 45.33.32.156
Discovered open port 22/tcp on 45.33.32.156
Discovered open port 9929/tcp on 45.33.32.156
Discovered open port 31337/tcp on 45.33.32.156
Completed SYN Stealth Scan at 17:55, 4.55s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.34s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 817 closed ports, 179 filtered ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
9929/tcp  open  nping-echo
31337/tcp open  Elite

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 5.03 seconds
           Raw packets sent: 1183 (52.028KB) | Rcvd: 823 (32.940KB)
```

This is a much faster scan than the TCP connect scan. This is because the TCP SYN scan does not require the full TCP handshake to be completed, making it a stealthier scan as the target system will not log the activity of this scan.

## ACK scanning
The ACK scan is fundamentally different from the TCP SYN and Connect scans. It does not show an open or filtered ports. By default, the ACK scan probe packet has only the ACK flag set. When scanning systems, ports that respond to nmap's ACK probes are labelled as "unfiltered", as both open and closed probes return responses with RST flags.  

For a TCP ACK scan we can use the `-sA` flag. This scan type requires root privileges.
```bash
sudo nmap -sA -v scanme.nmap.org
```

The output of the above command is as follows:

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 18:09 IST
Initiating Ping Scan at 18:09
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 18:09, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 18:09
Completed Parallel DNS resolution of 1 host. at 18:09, 0.00s elapsed
Initiating ACK Scan at 18:09
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Completed ACK Scan at 18:10, 17.35s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.28s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 1000 scanned ports on scanme.nmap.org (45.33.32.156) are filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 17.79 seconds
           Raw packets sent: 2014 (80.592KB) | Rcvd: 11 (440B)
```

For the above scan we see all ports are filtered. This happens as nmap ACK scan cannot determine whether the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software.

Only the ACK scan labels ports as unfiltered. This information can be used to map firewall rule sets, classify ports into this state. Scanning unfiltered ports with other scan types such as Window scan, SYN scan, or FIN scan, may help resolve whether the port is open.

## Window scanning
This works exactly like the TCP ACK scan, except it makes use of the implementations on certain systems to differentiate between open and filtered ports, rather than always printing unfiltered when an RST is returned. It does this by examining the TCP Window value of the RST packets returned. 

According to the nmap [documentation](https://nmap.org/book/scan-methods-window-scan.html), the windows scan sends the same bare ACK probe as ACK scan but interprets the results as follows:
| Probe Response |	Assigned State |
|----------|:-------------:|
| TCP RST response with non-zero window field |	open |
| TCP RST response with zero window field | closed |
| No response received (even after retransmissions) | filtered |
| ICMP unreachable error (type 3, code 1, 2, 3, 9, 10, or 13) |	filtered |

For a TCP Window scan we can use the `-sW` flag. This scan type requires root privileges.
```bash
sudo nmap -sW -v scanme.nmap.org
```

The output of the above command is as follows:

```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 19:09 IST
Initiating Ping Scan at 19:09
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 19:09, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 19:09
Completed Parallel DNS resolution of 1 host. at 19:09, 0.00s elapsed
Initiating Window Scan at 19:09
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Window Scan Timing: About 10.60% done; ETC: 19:14 (0:04:21 remaining)
Window Scan Timing: About 21.60% done; ETC: 19:14 (0:03:45 remaining)
Window Scan Timing: About 32.60% done; ETC: 19:14 (0:03:12 remaining)
Window Scan Timing: About 43.60% done; ETC: 19:14 (0:02:40 remaining)
Window Scan Timing: About 54.55% done; ETC: 19:14 (0:02:09 remaining)
Window Scan Timing: About 65.55% done; ETC: 19:14 (0:01:38 remaining)
Window Scan Timing: About 76.55% done; ETC: 19:14 (0:01:06 remaining)
Window Scan Timing: About 87.55% done; ETC: 19:14 (0:00:35 remaining)
Completed Window Scan at 19:14, 282.78s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.28s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 1000 scanned ports on scanme.nmap.org (45.33.32.156) are filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 283.52 seconds
           Raw packets sent: 2004 (80.152KB) | Rcvd: 1 (28B)
```
<br>
<br>

# [Special FLAG based TCP port scans]()
This can be used to differentiate between open and closed ports. The following three types of scans exactly the same in behavior except for the TCP flags set in probe packets. These FLAG based scans normally do not receive any response back from the target, other than an RST packet [Page 65 of [RFC 793](http://www.rfc-editor.org/rfc/rfc793.txt) says this in detail]. Responses are treated as shown in the Table below.

| Probe Response | Assigned State |
|----------|:-------------:|
| No response received (even after retransmissions)| open\|filtered |
| TCP RST packet | closed |
| ICMP unreachable error (type 3, code 1, 2, 3, 9, 10, or 13)| filtered |

In nmap filtered means the packets were dropped. But for `open|filtered`, nmap is unable to determine whether a port is open or filtered. This occurs for scan types in which open ports give no response.
The UDP, IP protocol, FLAG based scans are the only ones which classify ports this way.


## NULL scanning
The TCP NULL scan is a special flag based scan that does not set any bits (TCP flag header is 0). We use the `-sN` flag for this scan. This scan type requires root privileges.
```bash
 sudo nmap -sN -v scanme.nmap.org
```

The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 19:40 IST
Initiating Ping Scan at 19:40
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 19:40, 0.32s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 19:40
Completed Parallel DNS resolution of 1 host. at 19:40, 0.00s elapsed
Initiating NULL Scan at 19:40
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Completed NULL Scan at 19:40, 17.08s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 1000 scanned ports on scanme.nmap.org (45.33.32.156) are open|filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 17.50 seconds
           Raw packets sent: 2014 (80.592KB) | Rcvd: 11 (440B)
```

## FIN scanning
The TCP FIN scan is a special flag based scan that sets the FIN bit. We can use the `-sF` flag to perform a FIN scan. This scan type requires root privileges.
```bash
nmap -sF -v scanme.nmap.org
```
The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 19:43 IST
Initiating Ping Scan at 19:43
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 19:43, 0.30s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 19:43
Completed Parallel DNS resolution of 1 host. at 19:43, 0.00s elapsed
Initiating FIN Scan at 19:43
Scanning scanme.nmap.org (45.33.32.156) [65535 ports]
FIN Scan Timing: About 4.69% done; ETC: 19:54 (0:10:30 remaining)
FIN Scan Timing: About 13.18% done; ETC: 19:51 (0:06:42 remaining)
FIN Scan Timing: About 23.70% done; ETC: 19:50 (0:04:53 remaining)
FIN Scan Timing: About 35.35% done; ETC: 19:49 (0:03:41 remaining)
FIN Scan Timing: About 48.29% done; ETC: 19:48 (0:02:42 remaining)
FIN Scan Timing: About 62.57% done; ETC: 19:48 (0:01:48 remaining)
FIN Scan Timing: About 77.65% done; ETC: 19:48 (0:01:01 remaining)
Completed FIN Scan at 19:47, 251.38s elapsed (65535 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 65535 scanned ports on scanme.nmap.org (45.33.32.156) are open|filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 251.78 seconds
```

## Xmas scanning
The TCP Xmas scan is a special flag based scan that sets the FIN, PSH, and URG bits. We can use the `-sX` flag to perform an Xmas scan. This scan type requires root privileges.
```bash
nmap -sX -v scanme.nmap.org
```
The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 20:03 IST
Initiating Ping Scan at 20:03
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 20:03, 0.31s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:03
Completed Parallel DNS resolution of 1 host. at 20:03, 0.00s elapsed
Initiating XMAS Scan at 20:03
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Completed XMAS Scan at 20:03, 16.56s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 1000 scanned ports on scanme.nmap.org (45.33.32.156) are open|filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 16.98 seconds
           Raw packets sent: 2014 (80.592KB) | Rcvd: 11 (440B)
```

## Maimon scanning
The Maimon scan is named after its discoverer, Uriel Maimon. He described the technique in Phrack Magazine issue #49 (November 1996). This is a similar scan type like the NULL, FIN and Xmas scans, except that the probe is FIN/ACK.

We can use the `-sM` flag to perform a Maimon scan. This scan type requires root privileges.
```bash
sudo nmap -sM -v scanme.nmap.org
```

The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 20:56 IST
Initiating Ping Scan at 20:56
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 20:56, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:56
Completed Parallel DNS resolution of 1 host. at 20:56, 0.00s elapsed
Initiating Maimon Scan at 20:56
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Completed Maimon Scan at 20:56, 16.71s elapsed (1000 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
All 1000 scanned ports on scanme.nmap.org (45.33.32.156) are open|filtered

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 17.41 seconds
           Raw packets sent: 2014 (80.592KB) | Rcvd: 11 (440B)
```

# [UDP Scanning]()
The UDP scanning is a type of port scanning that uses the User Datagram Protocol (UDP) to determine which ports are open on a target host. UDP scan is activated with the `-sU` option. It can be combined with a TCP scan type such as SYN scan (using `-sS`) to check both protocols during the same run.

```bash
nmap -sU -v scanme.nmap.org
```

For an easy example we can use the `--top-ports` flag to specify the number of "mostly" used ports to scan. Here we use 10 ports.
```bash
nmap -sU --top-ports 10 -v scanme.nmap.org
```

For the above command, the output is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 21:09 IST
Initiating Ping Scan at 21:09
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 21:09, 0.32s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:09
Completed Parallel DNS resolution of 1 host. at 21:09, 0.00s elapsed
Initiating UDP Scan at 21:09
Scanning scanme.nmap.org (45.33.32.156) [10 ports]
Discovered open port 123/udp on 45.33.32.156
Completed UDP Scan at 21:10, 4.64s elapsed (10 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.28s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f

PORT     STATE  SERVICE
53/udp   closed domain
67/udp   closed dhcps
123/udp  open   ntp
135/udp  closed msrpc
137/udp  closed netbios-ns
138/udp  closed netbios-dgm
161/udp  closed snmp
445/udp  closed microsoft-ds
631/udp  closed ipp
1434/udp closed ms-sql-m

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 5.06 seconds
           Raw packets sent: 20 (830B) | Rcvd: 11 (742B)
```

# [IP Protocol Scanning]() 
The IP protocol scanning can allow us to determine which IP protocols (TCP, ICMP, IGMP, etc.) are supported by target machines. This is not a proper port scanning technique as it cycles through IP protocol numbers rather than TCP or UDP port numbers.

To do this we can use the `-sO` flag. This scan type requires root privileges.
```bash
sudo nmap -sO -v scanme.nmap.org
```

The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 21:39 IST
Initiating Ping Scan at 21:39
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 21:39, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:39
Completed Parallel DNS resolution of 1 host. at 21:39, 0.00s elapsed
Initiating IPProto Scan at 21:39
Scanning scanme.nmap.org (45.33.32.156) [256 ports]
IPProto Scan Timing: About 44.92% done; ETC: 21:41 (0:00:39 remaining)
Discovered open port 1/ip on 45.33.32.156
Discovered open port 17/ip on 45.33.32.156
Completed IPProto Scan at 21:40, 55.68s elapsed (256 total ports)
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.28s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 254 open|filtered protocols
PROTOCOL STATE SERVICE
1        open  icmp
17       open  udp

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 56.13 seconds
           Raw packets sent: 515 (10.516KB) | Rcvd: 4 (152B)
```

# [Idle Scanning]()
The idle scanning option can mask the attacker's IP address by using a zombie host to send the packets. This is done by using the `-sI` flag. This scan type requires root privileges. For an example we can use the following command:
```bash
sudo nmap -sI -v www.example.org scanme.nmap.org
```

The first step however is to find an appropriate zombie host. The host has to be idle. Our zombie host in the above example is `www.example.org`. The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 23:17 IST
Initiating Parallel DNS resolution of 1 host. at 23:17
Completed Parallel DNS resolution of 1 host. at 23:17, 0.00s elapsed
Initiating idle scan against scanme.nmap.org (45.33.32.156) at 23:17
Idle scan zombie www.example.org (93.184.216.34) port 80 cannot be used because it has not returned any of our probes -- perhaps it is down or firewalled.
QUITTING!
```

# [SCTP Scanning]()
There are two primary types of SCTP scanning methods provided by nmap. The first is the `INIT` scan which is used to determine which SCTP ports are open. The second is the `COOKIE-ECHO` scan which is used to determine which SCTP ports are open and which are closed. The `INIT` scan is activated with the `-sY` flag and the `COOKIE-ECHO` scan is activated with the `-sZ` flag. Both of these scan types require root privileges.

## SCTP INIT Scanning
For an example we can use the following command:
```bash
sudo nmap -sY -v scanme.nmap.org
```
All packets that I scan were filtered but yes that's one way how its done.

## SCTP COOKIE-ECHO Scanning
For an example we can use the following command:
```bash
sudo nmap -sZ -v scanme.nmap.org
```
Same with this one.

# [Script Scanning]()
The script scanning option can be used to run a script against the target machine. This is done by using the `--script` flag. For an example we can use the following command:
```bash
nmap --script http-enum scanme.nmap.org -v
```

The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-16 23:33 IST
NSE: Loaded 1 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 23:33
Completed NSE at 23:33, 0.00s elapsed
Initiating Ping Scan at 23:33
Scanning scanme.nmap.org (45.33.32.156) [4 ports]
Completed Ping Scan at 23:33, 0.33s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 23:33
Completed Parallel DNS resolution of 1 host. at 23:33, 0.00s elapsed
Initiating SYN Stealth Scan at 23:33
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Discovered open port 22/tcp on 45.33.32.156
Discovered open port 80/tcp on 45.33.32.156
Discovered open port 31337/tcp on 45.33.32.156
Discovered open port 9929/tcp on 45.33.32.156
Completed SYN Stealth Scan at 23:33, 3.89s elapsed (1000 total ports)
NSE: Script scanning 45.33.32.156.
Initiating NSE at 23:33
Completed NSE at 23:34, 51.67s elapsed
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.35s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 733 closed ports, 263 filtered ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
9929/tcp  open  nping-echo
31337/tcp open  Elite

NSE: Script Post-scanning.
Initiating NSE at 23:34
Completed NSE at 23:34, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 56.14 seconds
           Raw packets sent: 1268 (55.768KB) | Rcvd: 739 (29.580KB)
```

Default reconnaissance scripts can be applied to scans using the `--script=default` flag or the '-sC' flag.


# [Version Scanning]()
The version scanning option can be used to determine the version of the target machine. This is done by using the `-sV` flag. For an example we can use the following command:
```bash
nmap -sV -v scanme.nmap.org
```

The output of the above command is as follows:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-17 00:40 IST
NSE: Loaded 45 scripts for scanning.
Initiating Ping Scan at 00:40
Scanning scanme.nmap.org (45.33.32.156) [2 ports]
Completed Ping Scan at 00:40, 0.28s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 00:40
Completed Parallel DNS resolution of 1 host. at 00:40, 0.00s elapsed
Initiating Connect Scan at 00:40
Scanning scanme.nmap.org (45.33.32.156) [1000 ports]
Discovered open port 22/tcp on 45.33.32.156
Discovered open port 80/tcp on 45.33.32.156
Discovered open port 31337/tcp on 45.33.32.156
Discovered open port 9929/tcp on 45.33.32.156
Completed Connect Scan at 00:41, 10.28s elapsed (1000 total ports)
Initiating Service scan at 00:41
Scanning 4 services on scanme.nmap.org (45.33.32.156)
Completed Service scan at 00:41, 31.86s elapsed (4 services on 1 host)
NSE: Script scanning 45.33.32.156.
Initiating NSE at 00:41
Completed NSE at 00:41, 1.17s elapsed
Initiating NSE at 00:41
Completed NSE at 00:41, 1.08s elapsed
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 996 closed ports
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http       Apache httpd 2.4.7
9929/tcp  open  nping-echo Nping echo
31337/tcp open  tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.92 seconds
```
