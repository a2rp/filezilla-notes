# 14. Server passive mode and network configuration

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: FileZilla Server users, groups, and access](./13-filezilla-server-users-groups-and-access.md) | [Notes index](../README.md) | [Next: Logs, errors, and connection troubleshooting](./15-logs-errors-and-connection-troubleshooting.md) |

## Why FTP needs more than one port

FTP uses a control connection for commands and replies, plus a separate data connection for directory listings and file contents. A user may sign in successfully over the control connection while a listing or transfer fails because the data connection cannot be opened.

FTPS still uses FTP's control and data connection model. TLS protects the connections according to the client and server settings, but it does not remove the need to configure the data path. SFTP uses SSH and does not use FTP active or passive mode.

## Prefer passive mode for a server used by remote clients

In passive FTP, the server announces a data port and waits for the client to connect. The client initiates both the control connection and the data connection. This is usually easier for clients behind home or office firewalls.

In active FTP, the client announces a listening port and the server connects back to it. That can require incoming rules on each client's firewall or NAT router. Passive mode usually lets the server owner configure the network once for many clients.

## Configure the server and network as one path

For an FTP or FTPS server behind NAT, the passive setup must agree across the server, host firewall, and router:

1. Choose a limited passive port range supported by the installed server version.
2. Configure the server to announce that same range.
3. Allow the control listener and passive range through the server's host firewall.
4. Forward those TCP ports from the router's public address to the server's private address.
5. Configure the public IP address or DNS name that the server should advertise.
6. Connect from a separate external network and test both directory listing and file transfer.

If any layer uses a different port range, the control login may work while the data connection fails. Do not forward all ports or disable the firewall as a general fix. Use only the rules required for the configured listener and passive range.

The public address can change on residential connections. If it does, use a stable DNS name or a supported method to keep the advertised address current. Check that the DNS name resolves to the current public address from outside the network.

## Distinguish local and public tests

A client on the same LAN should normally use the server's private address. A client outside the LAN uses the public address or DNS name. Some routers do not support connecting to their own public address from inside, so an internal test can fail even though an external connection works.

A successful connection from the same computer proves little about router forwarding or public reachability. Use a separate network, such as a trusted mobile data connection, for the external test. Keep the test account restricted and remove temporary access when finished.

## Treat encryption as part of the listener design

Use the secure protocol and encryption mode required by the server owner. For FTPS, configure and renew the TLS certificate, verify the host name, and require protected data connections where the service policy calls for them. A certificate change should be verified before clients trust it.

Do not publish credentials in connection examples or logs. Use a dedicated account with only the folders and actions required. The server control interface should remain restricted to administrators and should not be exposed as a public file transfer listener.

## Understand IPv4, IPv6, and multiple firewalls

A service may listen on IPv4, IPv6, or both. A router rule for IPv4 does not automatically create an IPv6 firewall rule, and IPv6 generally uses firewall policy rather than IPv4-style address translation. Check the address family used by the client when diagnosing reachability.

The connection may cross several filters: a cloud security group, edge firewall, NAT router, host firewall, and server listener. Record the actual destination address and port, then check each layer in order. Avoid broad changes until the first blocking layer is identified.

## Read the failure in context

| Symptom | Likely area to inspect |
| --- | --- |
| Connection times out before login | DNS, public route, listener, or control-port rules |
| Connection is refused | Wrong address or port, stopped listener, or local firewall |
| Login works but directory listing fails | Passive range, advertised public address, or data-port forwarding |
| Listing works but a transfer fails | Data-port policy, TLS data protection, account permissions, or storage |
| Works on LAN but fails externally | Public DNS/IP, NAT forwarding, ISP policy, or external firewall |
| Large transfer ends with a timeout | Idle control connection handling, router timeout, or unstable network |

Check both the client message log and the server log. Record which command or operation failed and whether failure affects listings, downloads, uploads, or all of them.

## Version note for server configuration

The FileZilla Wiki network configuration page explains the FTP control and data paths, passive mode, NAT, and external testing. Its detailed server setup steps explicitly say they are outdated for FileZilla Server 1.x. Use the current documentation for the installed edition and version when changing server settings.

## Practice

1. Draw the control and data paths for passive FTP through a NAT router.
2. Explain why a successful login does not prove that file transfers will work.
3. List the server, host firewall, and router settings that must agree on a passive range.
4. Test a server from its LAN and then from an external network. Explain the difference.
5. Diagnose an internal success with an external directory-listing failure.
6. Explain why SFTP does not need FTP passive ports.
7. Identify every firewall or network layer between a remote client and the server.
8. Find the version-specific server instructions and verify that they match the installed interface before applying them.

## Main references

- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
- [FileZilla Server documentation](https://filezillapro.com/docs/server/)
- [FileZilla Server overview and downloads](https://filezilla-project.org/)
- [RFC 4217: Securing FTP with TLS](https://www.rfc-editor.org/rfc/rfc4217)
