# 02. FTP connections and sessions

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: File transfer basics and FileZilla overview](./01-file-transfer-basics-and-filezilla-overview.md) | [Notes index](../README.md) | [Next: FTPS and SFTP security](./03-ftps-and-sftp-security.md) |

## FTP uses a control connection and a data connection

FTP uses a control connection for commands and replies. A separate data connection carries a directory listing or file contents. The control connection usually remains open while a client browses and transfers files.

This two-connection design explains a common situation: FileZilla can log in successfully, but the remote listing or file transfer fails because the data connection cannot be established.

~~~text
Client                                      Server
  |---- control connection --------------->|  commands and replies
  |<--- control connection stays open -----|
  |---- data connection for LIST/RETR ---->|  listing or file bytes
  |<--- transfer result on control --------|
~~~

The control and data connections are part of one FTP session, but they have different network behavior.

## Follow the opening of a session

A simplified plain FTP session on the default control port looks like this:

~~~text
Client opens TCP connection to server port 21
Server replies: 220 Service ready
Client sends USER account-name
Server requests a password: 331
Client sends PASS value
Server confirms login: 230
~~~

The exact messages vary with server configuration. The important idea is that the client and server exchange short commands and numeric replies on the control connection.

Plain FTP does not encrypt that connection. A network observer may be able to read the username, password, commands, and file names. Use FTPS or SFTP for connections across networks you do not control.

## Use active or passive mode for the data connection

FTP has two ways to establish the data connection. They differ in which side opens the data socket:

| Mode | Who initiates the data connection | Usual network effect |
| --- | --- | --- |
| Active | The server connects back to an address and port announced by the client | The client side must accept an incoming connection |
| Passive | The client connects to an address and port announced by the server | The server side must accept a data connection on its configured passive range |

Both modes can carry uploads, downloads, and directory listings. The names describe how the data socket is opened, not the direction of the file transfer.

## Passive mode is usually easier for clients

In passive mode the client asks the server for a data endpoint, then opens an outgoing connection to it. Outgoing connections are commonly allowed through client firewalls and home routers.

~~~text
Client -> Server control connection: PASV or EPSV
Server -> Client control connection: passive address and port
Client -> Server passive port: open data connection
Client -> Server data connection: LIST, RETR, or STOR data
~~~

The server must still be configured to listen on its passive port range. If it sits behind NAT, the server may need its public address configured and the passive range forwarded through the router.

## Active mode can require inbound client access

In active mode the client opens a local listening socket and tells the server where to connect. The server then initiates the data connection back to the client.

~~~text
Client -> Server control connection: PORT or EPRT with client endpoint
Server -> Client announced endpoint: open data connection
Client <-> Server data connection: directory listing or file data
~~~

When the client is behind NAT or a restrictive firewall, the address announced by the client may be private or unreachable from the server. Active mode then needs correctly configured client-side forwarding and firewall rules.

Use passive mode unless the network or server requires active mode. FileZilla can try another transfer mode when a server is misconfigured, but repeat failures should be diagnosed with the logs and network owner.

## Read a passive port from an FTP reply

An older `PASV` response includes an IPv4 address and two numbers for the port. The port is calculated as `first number * 256 + second number`:

~~~text
227 Entering Passive Mode (203,0,113,25,195,80)
port = 195 * 256 + 80
port = 50000
~~~

Modern clients may use `EPSV`, which returns only a port and avoids embedding an IPv4 address in the control reply. The server still needs to accept the connection on that port.

## Understand common default ports

Ports are defaults, not guarantees. A server administrator can choose different listener ports:

| Service | Common default port | Notes |
| --- | --- | --- |
| FTP control | TCP 21 | Plain FTP and commonly explicit FTPS start here |
| Implicit FTPS | TCP 990 | TLS begins immediately on the control connection |
| SFTP | TCP 22 | SSH service, unrelated to FTP data ports |

FTP data connections use additional ports selected through active or passive negotiation. Opening only TCP 21 on an FTP server may allow login but still leave directory listings and transfers blocked.

## Diagnose login success followed by listing failure

Check these items in order when credentials work but the remote pane stays empty:

1. Read the FileZilla message log to identify whether the server returned a passive endpoint or requested an active connection.
2. Confirm the chosen transfer mode and whether the server supports it.
3. Check the server passive range and firewall forwarding when using passive mode.
4. Confirm that the client can make outgoing connections to the selected passive port.
5. If the server is behind NAT, confirm it advertises a reachable public address.
6. Try the other transfer mode only to isolate the cause, then correct the server or network configuration.

Do not disable a firewall broadly to test FTP. Allow only the service and port range required by the intended setup.

## Remember the security boundary

Active and passive mode solve connection establishment. They do not encrypt credentials or file contents. Use FTPS or SFTP to protect traffic and verify the server identity.

An FTP-aware router may try to inspect or rewrite commands in the control connection. Such helpers can fail with encrypted sessions or nonstandard ports. Prefer clear server and firewall configuration over automatic packet rewriting.

## Practice

1. Explain why FTP has separate control and data connections.
2. Identify which side initiates the data socket in active and passive mode.
3. Calculate the port represented by a `PASV` reply of `227 Entering Passive Mode (203,0,113,25,195,80)`.
4. Diagnose why an FTP login can succeed while the remote directory listing fails.
5. List the firewall rules needed for a passive server behind a router.

## Main references

- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
- [FileZilla FTP protocol notes](https://wiki.filezilla-project.org/File_Transfer_Protocol)
- [RFC 959: File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)
- [RFC 2428: FTP extensions for IPv6 and NAT](https://www.rfc-editor.org/rfc/rfc2428)
