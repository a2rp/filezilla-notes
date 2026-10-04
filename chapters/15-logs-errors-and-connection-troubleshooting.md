# 15. Logs, errors, and connection troubleshooting

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Server passive mode and network configuration](./14-server-passive-mode-and-network-configuration.md) | [Notes index](../README.md) | [Next: Secure transfer workflows and maintenance](./16-secure-transfer-workflows-and-maintenance.md) |

## Start with the first failure

The message log shows connection steps and server replies. The transfer queue shows which file failed and whether other items completed. The server log can show why a login, path, or transfer was rejected.

Find the first unexpected response in time order. Later errors can be consequences of the first one. Record the operation that failed: name lookup, connection, TLS setup, login, directory listing, download, upload, rename, or delete.

## Read FTP reply classes

FTP replies begin with a three-digit code. The first digit gives the broad result:

| First digit | General meaning |
| --- | --- |
| 1 | Preliminary reply; more information will follow |
| 2 | Positive completion |
| 3 | More information or credentials are required |
| 4 | Temporary negative completion; retry may succeed after the cause clears |
| 5 | Permanent negative completion for the current request |

The full code and text matter. A 5xx response does not mean that the client should repeat the same request. It can indicate a missing path, insufficient rights, unsupported operation, or policy restriction. Ask the administrator when the message does not identify which rule blocked it.

## Diagnose by the operation that failed

| Symptom | Check first |
| --- | --- |
| Host name cannot be resolved | Spelling, DNS, VPN, and network availability |
| Connection times out | Address, listener, route, control port, and firewall |
| Connection is refused | Server process, listening address, and selected port |
| TLS negotiation or certificate warning | Protocol mode, certificate dates and host name, and trust chain |
| SFTP host key changed | Verify the new fingerprint with the administrator before accepting it |
| Login rejected | Protocol, user name, password or key, account state, and authentication method |
| Login works but listing fails | Remote path, account access, passive data ports, advertised address, and firewall |
| Listing works but upload fails | Destination path, write permission, storage space, overwrite choice, and data connection |
| Download fails partway through | Network stability, server read access, data connection, and partial destination |
| Several small files fail intermittently | Passive port range, concurrent connection limit, or firewall behavior |
| File name displays incorrectly | Server character encoding and the site's Charset setting |

Use the client log to determine whether failure occurs before or after login. A successful login only proves that the control connection and credentials worked.

## Use a repeatable check sequence

1. Confirm the saved site uses the intended host, protocol, port, and encryption mode.
2. Check whether the failure happens before connection, during login, during listing, or during a file operation.
3. Read the exact response around the first failure.
4. Confirm the current remote path and account permissions.
5. For FTP or FTPS, check active or passive mode and the data connection path.
6. For FTPS or SFTP, verify certificate or host-key warnings rather than bypassing them.
7. Check available storage and the destination's underlying permissions.
8. Change one relevant setting at a time and repeat the same small test.

This sequence keeps unrelated settings from changing and helps identify which layer caused the failure.

## Treat listing errors separately from login errors

A directory listing is an FTP data operation. It can fail even after the server accepts the credentials. Check the passive address and port range, network forwarding, host firewall, path, and account permission to list the directory.

SFTP uses a different connection design, so an FTP passive mode adjustment does not fix an SFTP problem. For SFTP, check the SSH host, port, host key, authentication, path, and server account policy.

## Collect a useful support report

Include:

- FileZilla Client or Server product and version;
- operating system and network location;
- protocol, host name, and port, with private host details redacted when needed;
- the operation that failed and its time;
- the first relevant client and server log lines;
- whether the problem affects listing, upload, download, or all operations;
- the last known successful test.

Remove passwords, private key content, tokens, and other secrets before sharing logs. User names and public server addresses may also be sensitive in some environments.

## Avoid risky troubleshooting shortcuts

Do not disable a firewall, accept an unverified server identity, or broaden account access just to see whether an error disappears. Ask the network or server owner to make a narrow, temporary test rule if a firewall change is necessary, then remove it after the test.

Do not delete FileZilla settings as a first step. The client stores saved sites and preferences there, and deleting the folder can remove them. Export or back up settings before any reset.

Older instructions can mention menu paths and configuration steps that no longer match the installed version. Check the documentation for the current edition and confirm the program version before following a product-specific procedure.

## Practice

1. Read a connection log and mark the first unexpected response.
2. Explain the difference between a failed login and a failed directory listing.
3. Diagnose a 550 response using its operation and full server text rather than repeating the request.
4. Compare likely causes for a timeout, a refused connection, and rejected credentials.
5. List the extra checks needed for an FTPS certificate warning and an SFTP host-key change.
6. Prepare a redacted support report that contains enough information to reproduce the failure.
7. Explain why changing a firewall or permission broadly can hide the actual cause.
8. Find a current product-specific document and compare its version with the installed server.

## Main references

- [FileZilla FAQ](https://wiki.filezilla-project.org/FAQ)
- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
- [FileZilla Client quick guide](https://wiki.filezilla-project.org/Using)
- [RFC 959: File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)
