# 12. TLS certificates, SSH host keys, and trust

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: File permissions, timestamps, and filenames](./11-file-permissions-timestamps-and-filenames.md) | [Notes index](../README.md) | [Next: FileZilla Server users, groups, and access](./13-filezilla-server-users-groups-and-access.md) |

## Identify which identity check applies

FTPS and SFTP protect file transfers in different ways. FTPS uses TLS with FTP. SFTP uses the SSH protocol. The connection setup therefore asks you to verify different server identity information.

| Connection | Server identity | What to check |
| --- | --- | --- |
| FTPS | TLS certificate | Host name, validity period, issuer or trust chain, and certificate details |
| SFTP | SSH host key | Host name, key type, and fingerprint supplied through a trusted channel |

A password proves something about the account. It does not prove that you connected to the intended server. Server identity checks help detect a wrong host or an intercepted connection.

## Validate an FTPS certificate

When an FTPS server presents a certificate, compare the certificate details with the host you intended to reach. A valid certificate should be current, cover the requested host name, and chain to a trusted certificate authority unless your organization uses a separately verified private certificate.

A certificate warning can have several causes:

- the certificate expired or is not yet valid;
- the certificate name does not match the host;
- the server sent an incomplete trust chain;
- the certificate is self-signed or issued by a private authority;
- the computer's clock is incorrect;
- the connection reached a different server than expected.

Do not accept a warning simply to make the connection continue. Ask the server administrator to confirm the expected certificate or fingerprint through a separate trusted channel. For a private certificate, follow the organization's certificate distribution process.

FTPS can protect the control connection and data connections. Server and client policy determine whether protected data connections are required. Confirm the saved site's encryption setting and the server's requirements.

## Verify the SFTP host key

On the first SFTP connection, FileZilla may show the server's host key and fingerprint. Obtain the expected fingerprint from the administrator using a separate trusted method, such as an authenticated support channel or published organization documentation. Compare every part of the fingerprint before trusting it.

Trusting a key for a site tells the client to recognize that server identity on later connections. If the key changes, stop and verify the change with the administrator. A legitimate server rebuild or key rotation can change it, but an unexpected change can also mean that the connection is reaching another machine.

Do not delete the old key or accept the new one until the reason and new fingerprint are confirmed. If your team has a documented key rotation process, use it to update the trusted identity.

## Understand account credentials and keys

A TLS certificate identifies the FTPS server. An SSH host key identifies the SFTP server. These are separate from your account password or user authentication key.

With SFTP public-key authentication:

- the server holds a copy of your public key;
- your client uses the matching private key to prove possession;
- a passphrase can protect the private key file at rest.

Keep the private key private. Do not email or upload it as a substitute for installing the public key. If a private key may have been exposed, tell the administrator so the corresponding public key can be removed or replaced.

A key passphrase protects the private key file. It is not the same as the server account password, and entering one does not change the server's trusted host key.

## Diagnose identity errors without weakening checks

| Message or symptom | Check |
| --- | --- |
| Certificate expired | Certificate dates, server clock, and renewal status |
| Certificate host name mismatch | Site Manager host name and certificate subject names |
| Untrusted certificate authority | Expected organization trust chain and server certificate configuration |
| New or changed SSH host key | Whether the server was rebuilt or its SSH keys were rotated; confirm fingerprint independently |
| Password rejected after identity check | Account name, password, authentication method, and account status |
| Connection works but secure transfer is refused | FTPS protection policy for the data connection and server configuration |

Record the host, protocol, port, time, and exact warning for the administrator. Do not send passwords, private keys, or unredacted secret connection details in a support message.

## Practice

1. Explain why an account password does not identify the server.
2. Read an FTPS certificate and check its host name, validity dates, and issuer.
3. Obtain an SFTP host key fingerprint through a separate channel and compare it with the first connection prompt.
4. Explain what a changed SFTP host key could mean and how to verify it safely.
5. Distinguish the SSH host key, your public key, your private key, its passphrase, and your account password.
6. Describe what you would do if an FTPS certificate name does not match the configured host.
7. Explain why trusting an unknown certificate or host key without verification can expose credentials and files.
8. Prepare a redacted connection report for an administrator without including secrets.

## Main references

- [FileZilla TLS specifications](https://wiki.filezilla-project.org/TLS_specifications)
- [FileZilla SFTP specifications](https://wiki.filezilla-project.org/SFTP_specifications)
- [RFC 4217: Securing FTP with TLS](https://www.rfc-editor.org/rfc/rfc4217)
- [RFC 4253: SSH Transport Layer Protocol](https://www.rfc-editor.org/rfc/rfc4253)
