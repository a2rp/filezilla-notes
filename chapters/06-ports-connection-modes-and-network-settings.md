# 06. Ports, connection modes, and network settings

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Site Manager and saved profiles](./05-site-manager-and-saved-profiles.md) | [Notes index](../README.md) | [Next: Local and remote file navigation](./07-local-and-remote-file-navigation.md) |

## Match the firewall rule to the protocol

Common default ports help identify a service, but the server owner can choose another port:

| Service | Common default | Network behavior |
| --- | --- | --- |
| FTP control and explicit FTPS | TCP 21 | Control connection; FTP data uses separately negotiated connections |
| Implicit FTPS | TCP 990 | TLS starts immediately; FTP still negotiates a data connection |
| SFTP over SSH | TCP 22 | One SSH transport carries the file session |

Do not open a port based only on the number. Confirm which service is listening and use the port given by the administrator.

FTP and FTPS need rules for data connections as well as the control port. SFTP does not use FTP active or passive mode.

## Choose the FTP data mode that fits the network

In passive mode the client opens the data connection to a port announced by the server. This is usually simpler for a client behind a home or company NAT because its firewall can allow outgoing connections.

In active mode the server connects back to an address and port announced by the client. The client network must accept that incoming connection, which can be difficult behind NAT.

Use passive mode by default when connecting to a remote FTP server. Change it only when the server is known to require another mode and the network rules are understood.

## Configure a passive server behind NAT

A passive FTP server behind a router needs a fixed, known data port range and a public address that external clients can reach. The server and router must agree on the same range.

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

The usual configuration work is:

1. Select a passive port range in the current FileZilla Server network settings.
2. Allow the control listener and that passive range through the host firewall.
3. Forward those TCP ports from the public router to the server host.
4. Configure the public IP address or DNS name that the server should advertise.
5. Test from outside the local network.

Use the current FileZilla Server manual and network wizard for the installed version. Older FileZilla wiki steps may describe the previous server interface.

## Understand why a login can work while transfers fail

A client may connect to the FTP control port and authenticate successfully, then fail when it tries to list a directory or transfer a file. Those later actions need the negotiated data connection.

In the message log, find the last successful control reply and the next data connection attempt. Confirm that the selected mode, passive port range, public address, firewall, and router forwarding all agree.

Changing from passive to active can help diagnose which side has a blocked connection, but it does not correct a server passive-range or NAT configuration error.

## Configure the client network

For a normal passive client connection, allow FileZilla to make outbound connections to the host and ports provided by the server. A restrictive company firewall may require the administrator to allow a specific range.

If active mode is required, the client must be reachable on the port it announces. A client behind NAT may need an external address, a limited active port range, firewall access, and router forwarding. This is why active mode often needs more configuration on each client network.

FileZilla Client includes a network configuration wizard in supported versions. Use it to inspect client settings, but do not apply broad firewall changes without understanding the requested ports.

## Use a proxy when the network requires one

An organization may require FTP, HTTP, or SOCKS proxy settings for outgoing connections. Ask the network administrator which proxy type, host, port, and authentication method to use.

A proxy changes how a connection is routed. It does not replace FTPS or SFTP encryption, certificate checks, or server authorization.

FTP-aware NAT helpers may rewrite addresses or ports inside FTP commands. These helpers can cause confusing results, particularly on nonstandard ports or encrypted control connections. Prefer explicit and documented firewall rules over automatic protocol rewriting.

## Keep the open port range narrow

Do not forward every port to the FTP server. Select a limited passive range large enough for the expected concurrent transfers, allow that range through the host firewall, and forward only those ports.

A range that is too small can prevent several parallel data connections from opening. The appropriate size depends on how many clients and transfers the server must support.

## Test from the right network

A server inside a home or office network may be reachable through its private address from that same LAN. Testing the public address from inside can fail when the router does not support NAT loopback, even though an external connection works.

Test a public server from a separate external network. A successful local test alone does not prove that external DNS, public routing, firewall rules, and passive ports are correct.

For an internal service, test with the intended private host name and network path rather than exposing it publicly just to simplify a test.

## Troubleshoot by symptom

| Symptom | First place to check |
| --- | --- |
| Host does not resolve | Host name, DNS, and network access |
| Connection refused | Listener address, protocol, port, and host firewall |
| Login works but listing times out | FTP mode, passive ports, NAT address, and data-channel rules |
| SFTP connects but authentication fails | SSH user, key, key passphrase, and server authorization |
| TLS connection fails before login | Encryption mode, certificate validity, host name, and trust chain |

Use the first failing message rather than changing unrelated settings. Record the time, host, protocol, port, and redacted error when asking an administrator for help.

## Practice

1. Draw the control and data paths for active FTP and passive FTP.
2. Explain why passive mode usually works better for a client behind NAT.
3. Design a limited passive port range and describe the server firewall and router rules it needs.
4. Explain why SFTP does not require an FTP passive range.
5. Diagnose a connection that authenticates but cannot list a directory.
6. Test an externally hosted FTP service from an external network rather than relying only on local access.

## Main references

- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
- [FileZilla Server overview](https://wiki.filezilla-project.org/FileZilla_FTP_Server)
- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
- [RFC 2428: FTP extensions for IPv6 and NAT](https://www.rfc-editor.org/rfc/rfc2428)
