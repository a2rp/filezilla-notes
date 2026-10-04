# 05. Site Manager and saved profiles

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Installation and first connection](./04-installation-and-first-connection.md) | [Notes index](../README.md) | [Next: Ports, connection modes, and network settings](./06-ports-connection-modes-and-network-settings.md) |

## Use Site Manager for named connections

Quickconnect is useful for a short test. Site Manager stores named connection profiles so you can reopen a site with the same protocol, host, encryption, authentication method, and starting directories.

Open Site Manager from the File menu or use the keyboard shortcut shown by your FileZilla version. Create a new entry, give it a name that explains its purpose, and group related entries in folders if that helps.

Use names such as `Production read-only` or `Test upload area` instead of a name that only identifies the host. A name is a reminder of the environment and intended use, not a permission check.

## Set the general connection values

The General tab contains the values needed to identify and log in to a service:

- **Protocol:** FTP or SFTP, as provided by the administrator.
- **Encryption:** the required plain, explicit TLS, or implicit TLS mode for FTP.
- **Host:** the DNS name or address of the server.
- **Port:** enter it when the service uses a non-default port.
- **Logon Type:** choose how the client obtains the username and credential.
- **User:** enter the assigned account name.

Changing only the port does not change the protocol. Choose the correct protocol and encryption mode as well.

## Choose a logon type deliberately

Common logon types include:

- **Anonymous:** use only when the service is intentionally configured for anonymous access.
- **Normal:** provide a user name and password in the profile.
- **Ask for password:** enter the password when connecting and reuse it during that FileZilla session.
- **Interactive:** ask for the password on each new connection.
- **Key file:** use an SSH private key for SFTP when the server has the matching public key.
- **Account:** provide an additional FTP account value when a server specifically requires it.

For an account on a shared computer, interactive prompting avoids putting the password in the profile. A saved password is still sensitive even when the profile has a harmless name.

An SSH key file is the private half of an SSH key pair. Keep it private and grant access only to the intended operating-system account. The server needs the corresponding public key.

## Set starting local and remote directories

The Advanced tab can set a local starting folder and a remote starting folder for a profile:

~~~text
Example local directory:  D:\projects\client-site
Example remote directory: /public_html
~~~

Use paths supplied or confirmed by the server owner. A remote path may be relative to the directory root assigned to your account, not the server filesystem root.

A starting directory controls where FileZilla opens the panes after connecting. It does not grant access to a directory that the server account cannot read.

## Keep per-site transfer settings with the profile

The Transfer Settings tab can set preferences for one site, such as transfer mode, a connection limit, or whether transfers use the default behavior. Leave inherited defaults alone unless the server owner or a troubleshooting step requires a change.

If a server limits simultaneous connections, a profile can restrict how many FileZilla connections it opens. FTP clients may use one connection for browsing and others for transfers, so a very low limit can make browsing unavailable during an active transfer.

Connection mode and firewall details are covered in the next chapter. Change one per-site setting at a time so the effect is easy to identify.

## Review character set and displayed timestamps

The Charset tab controls how an FTP server and client interpret file names when the server does not correctly advertise its encoding. Leave the automatic or server-provided setting unless file names appear with incorrect characters.

A time offset can adjust displayed modification times when a server reports times in a different zone or convention. It changes how a timestamp is displayed; it does not necessarily change the file contents or the time stored by the server.

## Edit, copy, rename, and remove entries

Site Manager lets you edit, copy, rename, move, or delete a profile. Copy a profile when two environments share most connection settings, then change and verify the host, account, encryption, and paths before connecting.

Before deleting a profile, confirm that no other environment or colleague depends on its settings. Deleting a local profile does not delete the remote account or files.

## Protect and share connection profiles carefully

- Do not paste saved passwords, private key paths, or private connection details into a public issue.
- Do not share a profile export without checking whether it contains credentials or internal paths.
- Use a separate profile for test and production systems.
- Make read-only access explicit in the profile name when the account is read-only.
- Remove stale profiles when their access is revoked.

Site Manager only stores the connection instructions that FileZilla uses. The server still authenticates the account and enforces its permissions.

## Troubleshoot a saved profile

When a profile stops connecting, compare it with the values issued by the server administrator:

1. Confirm the protocol and encryption policy.
2. Confirm the host name and port.
3. Confirm the logon type and account name.
4. Verify a changed certificate or SSH host key before accepting it.
5. Check that the local and remote starting directories still exist and are accessible.
6. Review the connection log for the first failing step.

## Practice

1. Create separate test and production profile names using example connection values.
2. Choose Interactive logon for a shared computer and compare it with Ask for password.
3. Set a default local test folder and a remote path assigned by an administrator.
4. Duplicate a profile, change the protocol and host, then inspect every field before connecting.
5. Explain why deleting a Site Manager entry does not delete a server account.

## Main references

- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
- [FileZilla quick connection and navigation notes](https://wiki.filezilla-project.org/Using)
- [FileZilla Client overview](https://wiki.filezilla-project.org/FileZilla_FTP_Client)
