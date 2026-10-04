# 03. FTPS and SFTP security

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: FTP connections and sessions](./02-ftp-connections-and-sessions.md) | [Notes index](../README.md) | [Next: Installation and first connection](./04-installation-and-first-connection.md) |

## Plain FTP does not protect the session

With plain FTP, the control connection is not encrypted. User names, passwords, commands, directory names, and file data may be visible to a network observer. A password-protected login is not the same as an encrypted connection.

Use an encrypted protocol when credentials or files cross a network you do not control. FileZilla Client supports both FTPS and SFTP, but they secure different protocols and use different server configuration.

## FTPS adds TLS to FTP

FTPS is FTP protected by Transport Layer Security. It keeps the FTP command model and its separate data connection, while TLS encrypts the negotiated control and data channels.

Explicit FTPS starts with an ordinary FTP control connection, then the client requests TLS with `AUTH TLS`. The server must support and require the protection policy selected in FileZilla.

Implicit FTPS starts TLS immediately when the client connects. It is a legacy connection style that commonly uses port 990. Use the mode specified by the server owner instead of guessing from the port.

~~~text
Explicit FTPS, commonly TCP 21:
  connect -> FTP greeting -> AUTH TLS -> certificate check -> protected FTP session

Implicit FTPS, commonly TCP 990:
  connect -> TLS handshake -> certificate check -> protected FTP session
~~~

The common ports are defaults. A server can use other ports, and a port number alone does not prove which protocol is listening.

## SFTP runs over SSH

SFTP means SSH File Transfer Protocol. It carries file operations over an SSH connection and usually uses TCP port 22 by default.

SFTP is not FTP with TLS, and it does not use FTP active or passive data connections. The SSH connection carries the SFTP session through a single protected transport connection.

~~~text
FileZilla Client -> SSH connection -> SFTP subsystem -> remote file operations
~~~

Choose **SFTP - SSH File Transfer Protocol** in the FileZilla Site Manager for an SSH file service. Do not choose plain FTP or FTPS for that server.

## Verify a certificate before trusting FTPS

During the TLS handshake, the server presents a certificate. Check that it is valid for the server name, within its validity period, and issued by a chain your client trusts.

For a private or self-signed certificate, ask the server owner to confirm the fingerprint through a separate trusted channel. An encrypted connection to the wrong server can still expose credentials and files.

Do not accept a warning simply to make the prompt disappear. If a certificate changes unexpectedly, verify whether the server was renewed, reconfigured, or replaced before trusting it.

## Verify the SSH host key before trusting SFTP

An SSH server presents a host key to prove its identity. On a first connection, compare the fingerprint shown by FileZilla with a value obtained from the administrator through a separate trusted channel.

A changed host key may reflect a legitimate server rebuild or key rotation, but it can also mean the connection reaches a different host. Confirm the change before replacing a stored key.

An SSH host key identifies the server. A user private key authenticates a person or client account. Keep the private key secret and protected with a passphrase when supported.

## Select the intended protocol in Site Manager

Create a Site Manager entry with the protocol and encryption mode specified by the server administrator:

| Server service | FileZilla protocol choice | Common default port |
| --- | --- | --- |
| Plain FTP | FTP | 21 |
| FTP with explicit TLS | FTP with explicit FTP over TLS | 21 |
| FTP with implicit TLS | FTP with implicit FTP over TLS | 990 |
| SSH file transfer | SFTP - SSH File Transfer Protocol | 22 |

The server may use a custom port. For production accounts, require encryption rather than silently falling back to plain FTP.

## Know which FileZilla Server product accepts SFTP

The standard FileZilla Server product supports FTP and FTP over TLS. The FileZilla Project documentation lists SFTP support under FileZilla Pro Enterprise Server. If the service is an SSH server, configure the SFTP client protocol and connect to that SSH service.

An FTP server cannot become an SFTP server by changing its port to 22. The server software must implement the SSH service.

## Read common security failures

- **Certificate name mismatch:** the certificate name does not match the host used to connect. Verify the correct hostname and certificate configuration.
- **Untrusted certificate authority:** the certificate chain cannot be validated. Confirm the server certificate or approved private CA.
- **Unknown SSH host key:** the client has not recorded the server key before. Verify its fingerprint with the administrator.
- **SSH key rejected:** confirm the matching public key is authorized for the account and that FileZilla has access to the intended private key.
- **Login succeeds over FTP but credentials must stay private:** switch to FTPS or SFTP and confirm the encrypted mode is active.

## Avoid protocol fallback that weakens the connection

If the server requires encryption, configure the client to require that protocol. A fallback to plain FTP can expose credentials and data without an obvious failure.

Do not send credentials until the certificate or host key identifies the expected server. Store saved credentials only on a trusted device with appropriate account access controls.

## Practice

1. Explain the difference between FTPS and SFTP without describing them as the same protocol.
2. Compare explicit and implicit FTPS connection startup.
3. Identify what a TLS certificate proves and what an SSH host key proves.
4. Verify an example certificate or host-key fingerprint through an independent source.
5. Select a Site Manager protocol for plain FTP, explicit FTPS, implicit FTPS, and SFTP.

## Main references

- [FileZilla Client supported protocols](https://wiki.filezilla-project.org/FileZilla_FTP_Client)
- [FileZilla Server supported protocols](https://wiki.filezilla-project.org/FileZilla_FTP_Server)
- [FileZilla TLS specifications](https://wiki.filezilla-project.org/TLS_specifications)
- [RFC 4217: Securing FTP with TLS](https://www.rfc-editor.org/rfc/rfc4217)
- [SFTP specifications](https://wiki.filezilla-project.org/SFTP_specifications)
- [RFC 4253: SSH transport layer](https://www.rfc-editor.org/rfc/rfc4253)
