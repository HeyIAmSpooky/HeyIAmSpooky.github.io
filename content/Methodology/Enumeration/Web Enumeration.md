---
title: Web Enumeration
type:
source:
dg-publish: true
---

# Web Enumeration

> [!info] Navigation
> [[Home]] | [[Master Table of Contents]] | [[Exam Cram Guide]] | [[Command Dashboard]] | [[Curated External Sources]] | [[Main/03 - Study Materials/eJPT/10 - Visual Diagrams/Index]]



## Sections in This Note
- [[#Web Server Enumeration|Web Server Enumeration]]
- [[#Web Server Enumeration|Web Server Enumeration]]

---

## Web Server Enumeration

A web server serves website data on the web using HTTP (an application layer protocol using TCP port 80). We can use auxiliary modules to enumerate web server version, HTTP headers, brute-force directories, and more. Popular web servers: Apache, Nginx, Microsoft IIS.

```
auxiliary/scanner/http/apache_userdir_enum
auxiliary/scanner/http/brute_dirs
auxiliary/scanner/http/dir_scanner
auxiliary/scanner/http/dir_listing
auxiliary/scanner/http/http_put
auxiliary/scanner/http/files_dir
auxiliary/scanner/http/http_login
auxiliary/scanner/http/http_header
auxiliary/scanner/http/http_version
auxiliary/scanner/http/robots_txt
```

## Web Server Enumeration

From the Nmap results, port 80 is open and running **Microsoft IIS 7.5**. We can access the target IP in a browser to identify whether a web app is running.

The target system is also running a web server with an SSL certificate on port 4848:
```
URL: https://10.0.22.85:4848
```
This is running **GlassFish** (GlassFish Server Open Source Edition, Administration Console login).

Another interesting web server runs on port **8484** — a **Jenkins** dashboard ("Welcome to Jenkins!").

The target system also has a **WAMP server** configured to run on port **8585**. Accessing it reveals a WAMP homepage hosting web apps like **WordPress** and **phpMyAdmin**, with:
- Server Configuration: Apache 2.2.21, PHP 5.3.10, MySQL 5.5.20
- Tools: phpinfo(), phpmyadmin
- Your Projects: uploads, wordpress
- Your Aliases: httpd-dav, phpmyadmin, sqlbuddy, webgrind

## External Sources
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)


## Related
- [[Exam Cram Guide]]
- [[Command Dashboard]]
