<img src="https://tryhackme-images.s3.amazonaws.com/room-icons/c583f9d6cc8a7f2a749fad911eb81eb3.png" width=25% height=25%>

# TryHackMe | Dig Dug | Writeup

[TryHackMe | DigDug](https://tryhackme.com/room/digdug "TryHackMe | Dig_Dug")

This simple box uses the tool `dig` to enumerate information from a DNS server.

# Dig

The `dig` command in Linux is used to gather DNS information. It stands for _Domain Information_ Groper, and it collects data about Domain Name Servers. The `dig` command is helpful for troubleshooting DNS problems, but is also used to display DNS information.


The `dig` command is used as follows;

`dig [server] [name] [type]`

`[server]` - The hostname or IP address the query is directed to

`[name]` - The DNS of the server to query

`[type]` - The type of DNS record to retrieve. By default (or if left blank), `dig` uses the A record type


# Enumeration

First lets run a simple `dig` scan.

```bash
# dig 10.10.106.50
```

```
; <<>> DiG 9.11.3-1ubuntu1.13-Ubuntu <<>> 10.10.106.50
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 14031
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;10.10.106.50.			IN	A

;; Query time: 6 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Sun Jul 10 21:55:31 BST 2022
;; MSG SIZE  rcvd: 41
```

Ok! so at this point you can add a `[type]` on to the end of the command such as `A` `MX` `TXT` or `ANY`

In this case we are unsure of what record we are looking for so we will leave it blank as by defualt `dig` uses the `ANY` argument.

In the descirption of the box it mentions the domain `givemetheflag.com`.

Lets include the domain name in to the `dig` scan.

```bash
# dig givemetheflag.com @10.10.106.50
```
```
; <<>> DiG 9.11.3-1ubuntu1.13-Ubuntu <<>> givemetheflag.com @10.10.106.50
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 17820
;; flags: qr aa; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;givemetheflag.com.		IN	A

;; ANSWER SECTION:
givemetheflag.com.	0	IN	TXT	"flag{********************************}"

;; Query time: 1 msec
;; SERVER: 10.10.106.50#53(10.10.106.50)
;; WHEN: Sun Jul 10 21:55:58 BST 2022
;; MSG SIZE  rcvd: 86
```

# Summary

This box demonstrates how you look for diffrent DNS records. The flag was hidden inside a `TXT` record and using `dig` we have been able to enumurate the DNS server to extract the flag. This technique can be very helpful when performing reconasance during engaments.
