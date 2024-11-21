---
title: 'TryHackMe: test'
author: c3l1kd
categories: [hackthebox]
tags: [web, vhost, subdomain, ufw, firewall, ftp, sudo, apt]
render_with_liquid: false
img_path: /images/tryhackme_dodge/
image:
  path: room_image.webp
---

**test** was another purple team room where we started on the attacker side and exploited a remote code execution **(RCE)** vulnerability to gain a foothold. After that, we exploited an unquoted service path to escalate our privileges and established basic persistence.

![Tryhackme Room Link](room_card.webp){: width="600" height="150" .shadow }
_<https://tryhackme.com/room/dodge>_

## Initial enumeration

### Nmap Scan

```console
$ nmap -T4 -n -sC -sV -Pn -p- 10.10.64.72
Nmap scan report for 10.10.64.72
Host is up (0.085s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 12:22:46:5a:8c:3a:53:16:60:78:3e:79:e1:66:ba:7d (RSA)
|   256 af:3a:3b:76:01:69:67:42:4a:53:ee:48:60:7c:6e:15 (ECDSA)
|_  256 36:87:fb:1f:3f:3c:30:cc:b5:61:23:9d:db:01:13:d9 (ED25519)
80/tcp  open  http     Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: 403 Forbidden
443/tcp open  ssl/http Apache httpd 2.4.41
|_http-title: 403 Forbidden
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dodge.thm/organizationName=Dodge Company, Inc./stateOrProvinceName=Tokyo/countryName=JP
| Subject Alternative Name: DNS:dodge.thm, DNS:www.dodge.thm, DNS:blog.dodge.thm, DNS:dev.dodge.thm, DNS:touch-me-not.dodge.thm, DNS:netops-dev.dodge.thm, DNS:ball.dodge.thm
| Not valid before: 2023-06-29T11:46:51
|_Not valid after:  2123-06-05T11:46:51
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: Hosts: default, ip-10-10-64-72.eu-west-1.compute.internal; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
With a nmap scan, there are three ports open:
- 22/SSH
- 80/HTTP
- 443/HTTPS