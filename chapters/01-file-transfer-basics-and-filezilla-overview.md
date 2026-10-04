# 01. File transfer basics and FileZilla overview

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Notes index](../README.md) | [Notes index](../README.md) | [Next: FTP connections and sessions](./02-ftp-connections-and-sessions.md) |

## What FileZilla helps you do

FileZilla provides separate applications for file transfer. FileZilla Client connects to a remote server so you can browse, upload, download, and manage files allowed by your account. FileZilla Server listens for incoming client connections and gives configured users access to selected directories.

File transfer means moving or copying data between two systems. In the most common FileZilla Client workflow, one side is a local computer and the other is a remote server.

~~~text
Your computer                         Remote server
FileZilla Client  -- connection -->   FTP or SSH file service
Local files      <-- data transfer -> Remote files
~~~

The connection direction does not change the words upload and download. Upload sends a file from your local computer to the remote server. Download copies a remote file to your local computer.

## FileZilla Client and FileZilla Server have different jobs

The client is the application you use to initiate a connection. The server is the service that listens for connections and decides which files an authenticated account can access.

FileZilla Client supports FTP, FTPS, and SFTP connections. FileZilla Server supports FTP and FTP over TLS. SFTP uses SSH and is a separate protocol. The FileZilla Project documentation currently lists SFTP support for FileZilla Pro Enterprise Server, not the standard FileZilla Server product.

~~~text
FileZilla Client  -> connects to -> FileZilla Server or another compatible server
FileZilla Server -> accepts       -> incoming FTP and FTPS connections
SSH server       -> accepts       -> SFTP connections
~~~

Installing FileZilla Client does not create a public server. Running a server requires a reachable host, an installed server service, configured accounts, network rules, and a secure connection policy.

## Understand the main connection values

Before connecting, get these values from the server administrator or hosting provider:

- **Protocol:** FTP, FTP over TLS, or SFTP.
- **Host:** a DNS name or IP address for the server.
- **Port:** the listening port configured by the server.
- **Authentication:** a username and password, or an SSH key where supported.
- **Access path:** the server directory your account may open.

The server may use non-default ports. Confirm the actual values instead of assuming a protocol from the port alone.

~~~text
Protocol:  SFTP
Host:      files.example.com
Port:      22
User:      ashish
Auth:      password or an approved SSH key
~~~

`example.com` is used here as a placeholder. Use the connection values issued for your own server.

## Read the FileZilla Client layout

The client window separates the local file system from the remote one:

- The local site pane shows folders on your computer.
- The remote site pane shows the current server directory.
- The message log shows connection commands, replies, and errors.
- The transfer queue lists files waiting, transferring, completed, or failed.
- The Site Manager stores connection profiles and related settings.

You can drag files between panes, but check the destination path and overwrite choice before confirming a large transfer.

## Follow a simple file transfer

1. Open Site Manager and select the correct saved profile.
2. Check that the protocol, host, port, and authentication method match the values from the server owner.
3. Connect and wait for the remote directory listing.
4. Confirm the remote path before changing or uploading anything.
5. Select one test file and add it to the transfer queue.
6. Check the queue result and confirm the file appears at the intended destination.

For a first test, use a temporary file and a directory where you are allowed to write. Do not test by overwriting a production file.

## Separate the protocol from the application

FTP, FTPS, and SFTP describe protocols. FileZilla Client and FileZilla Server are applications that implement some of those protocols.

FTP sends its credentials and data without encryption unless protected by another mechanism. FTPS is FTP protected with TLS. SFTP carries file operations over an SSH connection. FTPS and SFTP are not two names for the same protocol.

~~~text
FTP  = File Transfer Protocol
FTPS = FTP protected by TLS
SFTP = SSH File Transfer Protocol
~~~

Choose the protocol the server actually supports. Selecting SFTP in the client will not connect to an FTP-only server.

## Access is limited by the server account

Seeing a remote directory does not mean the account can read, write, rename, or delete every item. The server controls authentication and permissions. A server may also map a virtual path to a different location on its disk.

Use only an account and directory assigned to you. If a transfer fails with a permission error, ask the server owner to check the account access rules rather than changing unrelated local permissions.

## Common first-connection misunderstandings

- A successful login does not prove the data connection can transfer files. FTP can use a separate data connection for listings and transfers.
- A directory listing can fail even when the host and password are correct. Network mode, firewall, NAT, and server configuration may be involved.
- A certificate or host key prompt is a trust decision. Verify the expected server identity before accepting a new key or certificate.
- A saved site stores connection settings. It does not grant permissions that the server account does not have.
- FileZilla is a client. It cannot fix DNS, firewall, server availability, or account policy by itself.

## Practice

1. Label the client, server, local files, and remote files in a file transfer diagram.
2. Explain the difference between upload and download from the local computer point of view.
3. Write down the protocol, host, port, and authentication method for a test server without including a real password in your notes.
4. Identify the local pane, remote pane, message log, and transfer queue in FileZilla Client.
5. Explain why FTP, FTPS, and SFTP must not be treated as interchangeable settings.

## Main references

- [FileZilla Client overview](https://wiki.filezilla-project.org/FileZilla_FTP_Client)
- [FileZilla Server overview](https://wiki.filezilla-project.org/FileZilla_FTP_Server)
- [FileZilla Project Wiki](https://wiki.filezilla-project.org/Main_Page)
- [FTP fundamentals](https://wiki.filezilla-project.org/File_Transfer_Protocol)
- [SFTP specifications](https://wiki.filezilla-project.org/SFTP_specifications)
