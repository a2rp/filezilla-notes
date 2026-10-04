# 98. All code samples

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Secure transfer workflows and maintenance](./16-secure-transfer-workflows-and-maintenance.md) | [Notes index](../README.md) | [Next: Complete questions and answers](./99-complete-q-and-a.md) |

This appendix gathers every fenced example from the 16 core chapters. Names, addresses, and paths are examples. Replace them with values assigned for your own test system, and never place real passwords or private keys in notes.

## 01. File transfer basics and FileZilla overview

### Example 1

~~~text
Your computer                         Remote server
FileZilla Client  -- connection -->   FTP or SSH file service
Local files      <-- data transfer -> Remote files
~~~

### Example 2

~~~text
FileZilla Client  -> connects to -> FileZilla Server or another compatible server
FileZilla Server -> accepts       -> incoming FTP and FTPS connections
SSH server       -> accepts       -> SFTP connections
~~~

### Example 3

~~~text
Protocol:  SFTP
Host:      files.example.com
Port:      22
User:      ashish
Auth:      password or an approved SSH key
~~~

### Example 4

~~~text
FTP  = File Transfer Protocol
FTPS = FTP protected by TLS
SFTP = SSH File Transfer Protocol
~~~

## 02. FTP connections and sessions

### Example 1

~~~text
Client                                      Server
  |---- control connection --------------->|  commands and replies
  |<--- control connection stays open -----|
  |---- data connection for LIST/RETR ---->|  listing or file bytes
  |<--- transfer result on control --------|
~~~

### Example 2

~~~text
Client opens TCP connection to server port 21
Server replies: 220 Service ready
Client sends USER account-name
Server requests a password: 331
Client sends PASS value
Server confirms login: 230
~~~

### Example 3

~~~text
Client -> Server control connection: PASV or EPSV
Server -> Client control connection: passive address and port
Client -> Server passive port: open data connection
Client -> Server data connection: LIST, RETR, or STOR data
~~~

### Example 4

~~~text
Client -> Server control connection: PORT or EPRT with client endpoint
Server -> Client announced endpoint: open data connection
Client <-> Server data connection: directory listing or file data
~~~

### Example 5

~~~text
227 Entering Passive Mode (203,0,113,25,195,80)
port = 195 * 256 + 80
port = 50000
~~~

## 03. FTPS and SFTP security

### Example 1

~~~text
Explicit FTPS, commonly TCP 21:
  connect -> FTP greeting -> AUTH TLS -> certificate check -> protected FTP session

Implicit FTPS, commonly TCP 990:
  connect -> TLS handshake -> certificate check -> protected FTP session
~~~

### Example 2

~~~text
FileZilla Client -> SSH connection -> SFTP subsystem -> remote file operations
~~~

## 04. Installation and first connection

### Example 1

~~~text
FTP host:   ftp.example.com
SFTP host:  sftp://files.example.com
~~~

### Example 2

~~~text
Local test file:  filezilla-check.txt
Remote test path: /incoming/filezilla-check.txt
~~~

## 05. Site Manager and saved profiles

### Example 1

~~~text
Example local directory:  D:\projects\client-site
Example remote directory: /public_html
~~~

## 06. Ports, connection modes, and network settings

### Example 1

~~~text
External client
    |
    | TCP control port and one negotiated passive port
    v
Public firewall or NAT router
    | port forwarding for the configured range
    v
FileZilla Server on the private network
~~~

## 07. Local and remote file navigation

### Example 1

~~~text
Windows: C:\Users\Ashish\Documents\site-files
macOS or Linux: /home/ashish/site-files
~~~

### Example 2

~~~text
Local:  C:\Users\Ashish\Projects\portfolio\public
Remote: /home/account/www
~~~

## 08. Transfer queue and transfer lifecycle

### Example 1

~~~text
Direction: local to remote
Local source:  C:\Projects\portfolio\dist
Remote target: /home/account/www
Planned items: index.html, assets/, styles/
~~~

## 11. File permissions, timestamps, and filenames

### Example 1

~~~text
-rw-r--r--  regular file
drwxr-xr-x  directory
~~~

### Example 2

~~~text
644  owner can read and write; group and others can read
755  owner can read, write, and traverse; group and others can read and traverse
~~~

### Example 3

~~~text
Good:  project-report-2026.pdf
Good:  assets/logo-dark.svg
Risky: names with trailing spaces, reserved device names, or characters rejected by the destination
~~~

## 13. FileZilla Server users, groups, and access

### Example 1

~~~text
Native server folder: D:\Shared\Incoming
Virtual path shown to user: /
~~~
