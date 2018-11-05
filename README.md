# bbhttpd
Tiny web server with one requirement - busybox.

Notice:
-----------
It's not approach for production. This tool was made for quick file transferring from platforms busybox based platforms like as OpenWRT routers, android phones, small alpine containers and others. It also wasn't properly tested for security vulnerabilities, so, use it for your own risk.

Requirements:
------------
It needs only busybox binary!

Usage:
------------
```bash
$ ./bbhttpd help

BBHTTPd - smallest web server
(busybox: busybox)
Usage: ./bbhttpd start
       ./bbhttpd start <port number>
       ./bbhttpd start <ip address> <port number>
```
Examples:
------------
```bash
./bbhttpd start
# web server will starts at localhost (127.0.0.1) and port 8080
./bbhttpd start 9000
# web server will starts at localhost (127.0.0.1) and port 9000
./bbhttpd start 192.168.1.2 8000
# web server will starts at address 192.168.1.2 and port 8000
```
Logging:
------------
By default log file will be written to the /tmp/bbhttpd.log. Nothing will be printed to the stdout, cause it used by netcat (nc) command. It's normal.
Also you can set logfile path by using LOG variable like this:
```bash
LOG=./mylogfile.log ./bbhttpd start 8000
# log file will be placed in current directory with name mylogile.log
```
Logging also has few levels: debug, info, warning. Default is info. You can change it in the beginning of script in variable LOGLEVEL, or you can set this variable like LOG variable:
```bash
LOGLEVEL='debug' ./bbhttpd start
```
Example of the debug output:
```bash
2018.11.05-21:28:53   DEBUG: New session started

2018.11.05-21:28:53   DEBUG:  GET / HTTP/1.1
2018.11.05-21:28:53   DEBUG:  Host: localhost:8000
2018.11.05-21:28:53   DEBUG:  Connection: keep-alive
2018.11.05-21:28:53   DEBUG:  Upgrade-Insecure-Requests: 1
2018.11.05-21:28:53   DEBUG:  User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.119 Safari/537.36
2018.11.05-21:28:53   DEBUG:  Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
2018.11.05-21:28:53   DEBUG:  Accept-Encoding: gzip, deflate, br
2018.11.05-21:28:53   DEBUG:  Accept-Language: ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7
2018.11.05-21:28:54    INFO: HOST: localhost:8000 URL: / -- 200 OK
```
With LOGLEVEL='info':
```bash
2018.11.05-21:51:50    INFO: HOST: localhost:8000 URL: /mylogfile.log -- 200 OK
```

Other variables (also can be set as previous):
----------
```bash
INDEX='index.html'
```
Default index filename.

```bash
DIRINDEX=1
```
If it's equal 1, instead of index.html you will get list of files and directories with ability of downloading as arcives or as is.

```bash 
ERRPATH=`dirname "$0"`'/errors'
```
Path to the custom html files for the errors messages like ./errors/404.html, 503.html, etc. If file for the error code is not exists, the common.html error file will be used (next variable).
```bash
ERRORHTML=`dirname "$0"`'/errors/common.html'
```
Path to the custom html file for the errors, that hasn't dedicated html file like 404.html. If this file not exists, internal error message will be shown. If you place **%ERROR%** code to the common.html file, it will be replaced by server to the error code and short message like this: **404 Not Found**.
