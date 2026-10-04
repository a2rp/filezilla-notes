# 99. Complete questions and answers

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: All code samples](./98-all-code-samples.md) | [Notes index](../README.md) | End of notes |

This chapter answers the review questions from all 16 core chapters. Examples use fictional hosts and paths. Check your own server's documentation before changing a live connection.

## Chapter 1: File transfer basics and FileZilla overview

**1. Which parts are the client, server, local files, and remote files?**

The client is FileZilla running on your computer. The server is the remote service. Local files are on your computer, and remote files are on storage exposed by the server.

**2. What is upload and download from your computer's point of view?**

Upload sends a local file to the remote server. Download copies a remote file to your local computer.

**3. Which connection details can be recorded safely?**

Record the protocol, host, port, and authentication method. Use a placeholder for passwords and never put a real secret in shared notes.

**4. Where are the main areas in FileZilla Client?**

The local pane is on the left and the remote pane is on the right by default. The message log shows connection replies, and the transfer queue shows pending and completed file operations.

**5. Why are FTP, FTPS, and SFTP different settings?**

FTP is the original file transfer protocol. FTPS adds TLS protection to FTP, while SFTP transfers files through SSH. They use different connection setup and server support.

## Chapter 2: FTP connections and sessions

**1. Why does FTP use separate control and data connections?**

The control connection carries commands and replies. A separate data connection carries directory listings and file contents.

**2. Which side opens the data connection in active and passive mode?**

In active mode, the server connects back to a port announced by the client. In passive mode, the client connects to the port announced by the server.

**3. What port is encoded by 195,80 in a PASV reply?**

Calculate 195 multiplied by 256, then add 80. The result is TCP port 50000.

**4. Why can login work while a directory listing fails?**

Authentication uses the control connection, but a listing needs a data connection. A blocked passive port, wrong advertised address, or directory permission can stop the listing after login succeeds.

**5. What must allow passive FTP through a server router?**

The configured control listener and the configured passive range must be allowed by the host firewall. The router must forward those TCP ports to the server, and the server must advertise its public address or DNS name.

## Chapter 3: FTPS and SFTP security

**1. How do FTPS and SFTP differ?**

FTPS is FTP protected with TLS. SFTP is a file transfer protocol carried over SSH. A server that supports one does not necessarily support the other.

**2. How do explicit and implicit FTPS begin?**

Explicit FTPS begins with an FTP connection and then negotiates TLS, commonly on port 21. Implicit FTPS starts TLS immediately, commonly on port 990. Ports can be customized.

**3. What do a certificate and a host key identify?**

A TLS certificate identifies an FTPS server and can be checked against its host name and trust chain. An SSH host key identifies an SFTP server and is checked using its fingerprint.

**4. How should a certificate or fingerprint be verified?**

Get the expected identity from the server administrator through a separate trusted channel, then compare the certificate details or complete fingerprint with FileZilla's prompt.

**5. Which Site Manager protocol should be selected?**

Choose FTP with the assigned encryption mode for plain FTP or either FTPS mode. Choose SFTP for an SSH file transfer service. Use the exact protocol and port the server owner provides.

## Chapter 4: Installation and first connection

**1. Where should FileZilla Client be downloaded?**

Use the official FileZilla project download page. Check the product name and operating system before installing.

**2. How can a temporary SFTP profile be prepared safely?**

Create a Site Manager entry with a fictional label and the assigned host, user, and port. Select SFTP and the approved credential method. Do not save a real password in shared notes.

**3. How can the message log show whether a connection succeeded?**

Read it in order. Successful name resolution and connection are followed by host identity verification, authentication, and a directory listing. The first error identifies the earliest failed stage.

**4. What should happen before trusting a new host fingerprint?**

Compare it with the fingerprint supplied by the server administrator through an independent trusted channel. Do not accept an unknown key just to proceed.

**5. How can a first test transfer be kept safe?**

Use a harmless small file and a test directory assigned by the server owner. Upload it, download a copy, confirm both paths and results, and remove test data only when authorized.

## Chapter 5: Site Manager and saved profiles

**1. How should test and production profiles be named?**

Use names that identify the environment and purpose, such as Test upload and Production read only. The name is only a reminder and does not change server permissions.

**2. How do Interactive and Ask for password differ?**

Ask for password lets you enter a password when connecting and reuse it during that FileZilla session. Interactive asks for the password on each new connection. Use the option that matches the shared-computer policy.

**3. What should starting directories contain?**

Set a known local test folder and the remote path provided by the administrator. A starting directory changes where the panes open, not what the account is allowed to access.

**4. What must be checked after duplicating a profile?**

Verify the host, protocol, encryption, port, account, authentication method, and both starting directories. Confirm the certificate or host key before connecting to a changed environment.

**5. Does deleting a profile delete the server account?**

No. It removes a local saved connection entry. The account and remote files remain under server administration.

## Chapter 6: Ports, connection modes, and network settings

**1. What happens to the data connection in active and passive FTP?**

Active mode has the server connect to a client-announced endpoint. Passive mode has the client connect to a server-announced endpoint.

**2. Why is passive mode usually simpler for a client behind NAT?**

The client initiates outgoing connections for both control and data. It normally avoids configuring incoming port forwarding on every client network.

**3. What settings must match a passive range?**

The server's configured range, host firewall rules, and router forwarding must agree. The server must also advertise the public address clients can reach.

**4. Does SFTP need FTP passive ports?**

No. SFTP uses SSH and does not use FTP's separate control and passive data connections.

**5. What should be checked when login works but listing times out?**

Check the passive data address and port, server range, router forwarding, host firewall, and account permission to list the target directory.

**6. Why test a public server from outside the LAN?**

A router may not support connecting to its own public address from inside. Only an external test checks public DNS, routing, firewall rules, and forwarding as remote clients see them.

## Chapter 7: Local and remote file navigation

**1. Where are the local path, remote path, file list, log, and queue?**

The local pane is on the left by default and the remote pane on the right. Each has a path and file listing. The message log records protocol activity, and the queue lists transfer work.

**2. How do you open a local folder and return to its parent?**

Enter its path, choose it in the tree, or double-click it in the listing. Use the parent directory entry or navigate to the parent path to go back.

**3. What does a question mark on a remote folder mean?**

FileZilla has not opened that directory and does not yet know whether it contains subdirectories. Open it to load the listing.

**4. Why may remote root not be the server's physical root?**

The account can be jailed or mapped to a virtual root. It may show only the directory assigned to the account.

**5. How can several files be selected?**

Use the operating system's selection modifier keys to select a range or individual entries. Verify the highlighted names before choosing a transfer action.

**6. What is required for synchronized browsing?**

The local and remote starting directories need corresponding folder structures. The option links navigation; disable it before moving through unrelated folders.

**7. Why might refresh not show an expected file?**

The file may be outside the current path, hidden by a filter, excluded by server listing behavior, or blocked by account permissions. Refresh only requests a new listing.

**8. What should be checked before upload when a same-named file exists?**

Confirm the local source and remote destination, decide which copy should be authoritative, and choose a deliberate overwrite, skip, rename, or resume action.

## Chapter 8: Transfer queue and transfer lifecycle

**1. What should be checked on queued test files?**

Check that each source is on the intended side, that the direction is correct, and that the destination path matches the test target.

**2. How can a small transfer be monitored?**

Watch its progress in the queue, then confirm it appears in the successful or failed result list. Read the message log if the result is not successful.

**3. What happens when a directory is queued?**

FileZilla expands it into transfer work for its files and subdirectories. Review the destination and queued entries before starting a large directory.

**4. What should be checked after interrupting a transfer?**

Inspect the source and destination for completeness and compare their sizes. Preserve or rename an uncertain partial destination before retrying.

**5. How does an unavailable path help diagnose a failure?**

A controlled test can show the exact path or permission response in the log. Use a harmless test environment, then correct the source or destination rather than repeatedly retrying it.

**6. Does cancelling a transfer roll back completed files?**

No. It stops pending or active work, but entries that completed remain changed. An interrupted active file can be partial.

**7. What happens with a low connection limit?**

The queue may run more slowly. A limit of one can also prevent browsing during an active transfer on some servers.

**8. How do FTP transfer sessions relate to browsing?**

FTP can use a browsing control session and separate data connections for listings and transfers. This can let browsing continue during transfers if server limits allow it.

## Chapter 9: Transfer types and overwrite behavior

**1. What are the directions of upload and download?**

Upload reads from the local pane and writes to the remote pane. Download reads from the remote pane and writes locally.

**2. Why should an image or archive not use ASCII mode?**

ASCII can convert text line endings. Such conversion changes binary bytes and can corrupt an image or archive. Binary mode preserves bytes.

**3. How does automatic transfer mode choose text files?**

It uses configured file type rules, often based on the name or extension. It does not understand the file's meaning, so an unfamiliar extension may be classified unexpectedly.

**4. How should overwrite choices be reviewed?**

Read the available choices in the prompt, decide for the current conflict, and do not apply the rule to the rest of the queue until the full scope is understood.

**5. When is resuming a partial transfer safe?**

Only when the destination is a partial copy of the same unchanged source and the server supports restarting at an offset. If the source changed, resuming can combine different versions.

**6. How should same-named files be compared?**

First confirm both paths and direction. Compare size and time, then use checksums or inspect the contents when exact equality matters.

**7. How can a test transfer be verified?**

Upload a harmless file to a test directory, download it to a different local folder, compare sizes and checksums if available, and open it before removing the original.

**8. Does a matching size prove that files match?**

No. Different byte sequences can have the same length. A checksum or direct content comparison gives stronger evidence.

## Chapter 10: Directory comparison, filters, and synchronization

**1. What changes when comparing by size or modification time?**

The comparison highlights files that differ under the selected rule. A size check catches different lengths; a time check catches differing reported modification times.

**2. Should local-only or remote-only files be moved automatically?**

No. A local-only file may not belong on the server, and a remote-only file may be generated or maintained there. Confirm its purpose first.

**3. What does a filename filter do?**

It can hide matching items from a listing or affect which items are selected for transfer. Filters can apply separately to local and remote entries.

**4. Does disabling a filter restore a hidden item?**

It may make the item visible again after refreshing. Hiding changes the view or transfer selection, not the stored file itself.

**5. Does synchronized browsing copy files?**

No. It changes the local and remote directories together when you navigate. Use comparison and queue explicit transfers to move files.

**6. What should a one-way upload plan include?**

Check the source, destination, active filters, comparison result, intended files, overwrite policy, and post-transfer results before starting.

**7. Why are size and time comparisons imperfect?**

Equal sizes can contain different bytes, and timestamps can differ because of clock, precision, or time-zone behavior. Use checksums for exact verification.

## Chapter 11: File permissions, timestamps, and filenames

**1. What do the sample permission strings show?**

The leading character identifies a regular file or directory in this listing style. The remaining nine characters show read, write, and execute bits for owner, group, and others.

**2. What does directory execute permission allow?**

It allows traversing or entering a directory. Directory write access controls changes to entries, subject to parent permissions and server policy.

**3. Why is 777 not a general permission fix?**

It grants broad access and can let unintended users change files or directory contents. Find the specific blocked operation and set the narrow permission required.

**4. What evidence matters when timestamps differ?**

Check paths, sizes, content, checksums, server precision, and clock or time-zone behavior. A timestamp alone does not prove a content change.

**5. Which names are more portable?**

Use simple descriptive names with letters, numbers, hyphens, and common extensions. Avoid trailing spaces, destination-reserved names, invalid characters, and names that differ only by case.

**6. Where can a filename encoding be checked?**

Review the saved site's Charset setting and the server's advertised or documented encoding. Change it only when names display incorrectly and preserve a copy before renaming.

**7. How can a remote rename break a website?**

Pages, scripts, stylesheets, and application settings may refer to the old path. Search references and plan redirects or updates before renaming a live asset.

**8. Can FileZilla always change a permission?**

No. The server must support the request, the account must be authorized, and the underlying operating system must also permit the change.

## Chapter 12: TLS certificates, SSH host keys, and trust

**1. Why does a password not identify the server?**

A password authenticates an account to whichever endpoint answered. Certificate or host-key validation helps confirm that endpoint is the intended server.

**2. What should be checked on an FTPS certificate?**

Check the requested host name, validity dates, issuer or trust chain, and the server the connection reached. Ask the administrator to confirm a warning.

**3. How should an SFTP fingerprint be verified?**

Obtain the expected fingerprint through a separate trusted channel and compare it fully with FileZilla's first-connection prompt.

**4. What could a changed host key mean?**

The server may have been rebuilt or its key rotated, or the connection may have reached an unexpected machine. Confirm the change and fingerprint with the administrator before trusting it.

**5. What is the difference between SSH key terms?**

The host key identifies the server. Your public key is installed for account authentication. Your private key stays secret and proves possession. Its passphrase protects the private file; the account password is a separate credential.

**6. What should happen if a certificate name does not match?**

Stop and check the configured host name and certificate with the server administrator. Do not bypass the mismatch just to connect.

**7. What can happen if an unknown identity is trusted?**

A connection could be intercepted or could reach the wrong server, exposing credentials and files to that endpoint.

**8. What belongs in a redacted report?**

Include product version, protocol, host and port as appropriate, time, exact warning, and operation. Remove passwords, private keys, tokens, and any sensitive identifying values.

## Chapter 13: FileZilla Server users, groups, and access

**1. What are Client and Server responsible for?**

FileZilla Client connects to file services. FileZilla Server accepts incoming connections and controls account access to server folders.

**2. How does a mount point map a folder?**

It maps a native path on the server to a virtual path shown to the user. If a native folder is mapped to virtual root, its contents appear at the user's remote root.

**3. Which access choices fit common roles?**

A report reader usually needs read-only access. An uploader may need a restricted write-only drop path. A maintainer may need read and write access to a specific working path.

**4. Can a mount point override operating-system permissions?**

No. The FileZilla Server process and the operating system must both permit the operation.

**5. How should a test account be checked?**

Use the account in a separate client session. Verify allowed listing, download, or upload actions and confirm that prohibited paths or changes are rejected.

**6. How do groups help and what can conflict?**

Groups can share mount points, filters, and limits among users when supported. Overlapping mappings can be resolved by user settings or group order, so inspect the installed version's rules and test the effective access.

**7. Why can login succeed while listing or upload fails?**

Authentication may work while the mount point is missing, the operation is disallowed, the native path is inaccessible to the service, or a network data connection is blocked.

**8. Why record edition and version?**

Server interfaces and features differ across releases and editions. The correct documentation depends on what is installed.

## Chapter 14: Server passive mode and network configuration

**1. What is the passive FTP path through NAT?**

The client opens the control connection to the server listener. For a listing or transfer, the server announces a passive port, and the client connects through the router's forwarded range to that port.

**2. Why does login not prove transfers will work?**

Login tests the control connection. Listings and file contents also require a working data connection.

**3. Which passive range settings must agree?**

The range configured on the server, the allowed host firewall ports, and the router's forwarded destination range must match. The server must advertise a public address clients can reach.

**4. Why test on LAN and externally?**

A local test checks the private route. An external test checks public DNS or IP, routing, firewall policy, and forwarding. The router may not support using its own public address from inside.

**5. What explains LAN success with external listing failure?**

The public address or DNS may be wrong, the router may not forward the passive range, a firewall may block it, or the server may advertise a private address.

**6. Does SFTP need passive settings?**

No. SFTP uses an SSH connection and does not negotiate FTP passive ports.

**7. Which network layers should be reviewed?**

Check public DNS or address, cloud security rules if used, edge firewall, NAT router, host firewall, server listener, and the service's own policy.

**8. How can server instructions be checked?**

Read the product edition and version from the installed server, then use the matching current documentation. The older wiki network page warns that its detailed server steps are outdated for version 1.x.

## Chapter 15: Logs, errors, and connection troubleshooting

**1. How do you find the first unexpected response?**

Read the log in time order and locate the earliest unexpected code or message. Later errors may be consequences of that first failure.

**2. How does failed login differ from failed listing?**

A failed login means authentication was rejected on the control connection. A failed listing occurs after or during an authenticated session and can involve path rights or the FTP data connection.

**3. How should a 550 response be diagnosed?**

Read the operation and complete server text. Check whether the target exists, the account can access it, and the server permits that action. Do not repeat the same request without correcting a cause.

**4. How do timeout, refusal, and rejected credentials differ?**

A timeout often points to a blocked or unreachable route. A refusal means a reachable endpoint rejected the connection or has no listener. Rejected credentials indicate authentication or account policy.

**5. What should be checked for identity warnings?**

For FTPS, inspect the certificate name, dates, and trust chain. For SFTP, verify a changed host-key fingerprint independently before accepting it.

**6. What should a useful support report include?**

Product and version, operating system, protocol, host and port as appropriate, time, failed operation, first relevant log lines, and whether listing, upload, or download fails. Redact secrets.

**7. Why avoid broad firewall or permission changes?**

They can expose unrelated services or data and obscure the actual failing layer. Identify the blocked operation and change only the required rule.

**8. How do you check that a server instruction is current?**

Compare the document's target edition and version with the installed one. Prefer the current product documentation when an old menu path does not match.

## Chapter 16: Secure transfer workflows and maintenance

**1. What checks belong in a small test upload?**

Confirm the protocol and server identity, source and destination paths, filters, intended files, overwrite behavior, and result before treating the transfer as complete.

**2. Why is plain FTP unsuitable for confidential transfers?**

Plain FTP does not encrypt credentials or file data. Use the secure protocol configured by the server owner, usually FTPS or SFTP.

**3. How should a Site Manager export be handled?**

Export the selected settings through FileZilla's supported export feature, review the export for private details, and store it in a protected location with limited access.

**4. What should a website rollback plan contain?**

Keep the exact local release, back up the current live files, identify the destination, upload to staging when supported, verify the result, and retain a known previous version for recovery.

**5. How can a downloaded backup be verified?**

Compare expected sizes, compare checksums when available, and open or restore the data with the application that owns it.

**6. Does a successful transfer prove a backup works?**

No. It proves the transfer finished, not that the files are complete, consistent, or restorable. Test the restore process.

**7. What should be backed up before server maintenance?**

Back up the server configuration, underlying shared data, certificate or key material according to policy, and account records needed by that product. Store the backup securely.

**8. What should a server review include?**

Review active users, mount points, file-system access, secure protocols, certificates and host keys, firewall and passive rules, logs, storage, and a recent restore test.
