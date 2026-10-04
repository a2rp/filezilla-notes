# 04. Installation and first connection

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: FTPS and SFTP security](./03-ftps-and-sftp-security.md) | [Notes index](../README.md) | [Next: Site Manager and saved profiles](./05-site-manager-and-saved-profiles.md) |

## Install FileZilla Client from the project source

Download FileZilla Client from the official FileZilla Project site for your operating system. Avoid third-party download mirrors and repackaged installers. Install only FileZilla Client when your goal is to connect to another server.

FileZilla Client and FileZilla Server are different applications. The client initiates file transfers. Install and configure FileZilla Server only when you are responsible for hosting a service that other clients will connect to.

Use the official download page:

- [Download FileZilla Client](https://filezilla-project.org/download.php?type=client)

Follow the installer prompts for your operating system. If you use an organization-managed device, follow its software installation policy.

## Choose a first connection method

For a quick one-time test, the Quickconnect bar is enough. For a repeat connection, create a named profile in Site Manager so the protocol, encryption, host, and other settings stay together.

Use the protocol prefix for a Quickconnect host when needed:

~~~text
FTP host:   ftp.example.com
SFTP host:  sftp://files.example.com
~~~

The administrator may provide a port other than the default. Enter the assigned port rather than assuming a default.

## Connect with Quickconnect

1. Open FileZilla Client.
2. Enter the host, user name, and assigned port in the Quickconnect bar.
3. Leave the password field empty if the server or your policy requires an interactive password prompt.
4. Select Quickconnect and wait for the message log.
5. If a certificate or SSH host key is presented, verify it with the server administrator before trusting it.
6. Confirm that the remote pane shows the directory assigned to your account.

Do not use an account you are not authorized to access. For a shared computer, avoid storing passwords in the application.

## Create a Site Manager profile

Open Site Manager and create a new site entry. Fill in only the values supplied by the server owner:

- **Protocol:** choose FTP, explicit or implicit FTPS, or SFTP as specified.
- **Host:** enter the host name or address.
- **Port:** enter the configured service port.
- **Logon type:** choose the approved password or SSH key method.
- **User:** enter the assigned account name.
- **Default local directory:** choose a local working folder if needed.
- **Default remote directory:** use a path only when the server owner provided it.

For an interactive password prompt, choose a logon option that asks when connecting. On a personal trusted computer, saved credentials may be convenient, but protect the operating system account and avoid saving secrets on shared devices.

## Check the first connection result

When a connection begins, the message log records the server greeting, selected protocol, authentication result, and directory listing. Read the final messages before interpreting an empty remote pane.

Common results include:

- A welcome or ready response means the server accepted the control connection.
- A login success response means the credentials were accepted.
- A remote listing means the client can read that directory.
- A certificate or host-key warning means the server identity still needs verification.
- A data-connection timeout after login usually points to transfer mode, firewall, NAT, or passive-port configuration.

Do not paste a full log into a public issue if it contains a user name, host, path, or other private information. Redact details first.

## Confirm upload and download paths with a harmless file

Before transferring project or production data, use a small temporary text file and a test directory where your account may write:

~~~text
Local test file:  filezilla-check.txt
Remote test path: /incoming/filezilla-check.txt
~~~

Upload the file, confirm it appears at the expected remote path, then download it to a different local folder and compare its contents. Remove the test copy only if you have permission to do so.

Double-clicking a file can start a transfer immediately. Check the selected pane, current path, and transfer direction before confirming a destructive overwrite.

## Keep a clean first-connection workflow

- Save the site under a clear name that identifies its purpose and environment.
- Record the protocol and port provided by the server owner.
- Verify the certificate or host key before accepting a first connection.
- Start with a test directory and a harmless file.
- Check the transfer queue and confirm the result.
- Avoid screenshots or logs that reveal credentials, access tokens, or private paths.

## Troubleshoot a connection that does not finish

If Quickconnect fails, copy only the safe error text and check the settings one at a time:

1. Confirm the host name resolves and the server is reachable.
2. Confirm the protocol matches the service running on the server.
3. Confirm the port and username match the issued connection details.
4. Verify the server identity prompt rather than accepting an unknown identity.
5. If login succeeds but the remote listing fails, review the FTP data mode and network settings in chapter 6.

For SFTP, a normal FTP or FTPS server address will not work unless that host also runs an SSH service.

## Practice

1. Download FileZilla Client only from the official project page.
2. Create a temporary SFTP Site Manager profile using example values, without saving a real password.
3. Read the message log and identify the point where the connection succeeds or fails.
4. Verify a server fingerprint before accepting a connection.
5. Upload and download a harmless test file in an assigned test directory.

## Main references

- [Official FileZilla Client download](https://filezilla-project.org/download.php?type=client)
- [FileZilla quick connection and navigation notes](https://wiki.filezilla-project.org/Using)
- [FileZilla Client overview](https://wiki.filezilla-project.org/FileZilla_FTP_Client)
- [FileZilla Project Wiki](https://wiki.filezilla-project.org/Main_Page)
