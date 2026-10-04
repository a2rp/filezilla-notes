# 16. Secure transfer workflows and maintenance

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Logs, errors, and connection troubleshooting](./15-logs-errors-and-connection-troubleshooting.md) | [Notes index](../README.md) | [Next: All code samples](./98-all-code-samples.md) |

## Use a repeatable secure routine

A good transfer process checks identity, paths, permissions, and results before changing important files. Use this sequence for routine work:

1. Open the saved site and confirm the host, protocol, port, and encryption mode.
2. Verify the FTPS certificate or SFTP host key when prompted.
3. Navigate to the intended source and destination paths.
4. Review filters and compare the directories when updating an existing tree.
5. Queue only the files needed for the change.
6. Read overwrite choices before applying them to the remaining queue.
7. Check successful and failed entries after the transfer.
8. Verify important files by size and, where possible, checksum or application behavior.
9. Disconnect when the work is complete, especially on a shared computer.

Use FTPS or SFTP when credentials and file contents must be protected in transit. Plain FTP does not provide that protection. Which secure option is available depends on the server and its configuration.

## Protect accounts and saved sites

Use a dedicated account with access only to the required paths and operations. Do not share one personal account among people who need different permissions. Remove access when it is no longer needed and rotate credentials according to the organization's policy.

Avoid saving credentials on a shared or public computer. Keep private SSH keys outside shared folders, protect them with a passphrase, and never add private key content to a repository. Do not include passwords or tokens in connection screenshots, documentation, logs, or support reports.

The Site Manager stores connection details and preferences. Protect exported site settings as sensitive data, because they may reveal hosts, usernames, paths, or other private configuration.

## Deploy files with a rollback plan

A direct upload can change a live site file by file. If a transfer stops halfway through a release, visitors may see a mixture of old and new files. Plan important changes as a release:

- keep a local copy of the exact build being deployed;
- identify the live destination and files that must remain;
- back up the current remote files or use the hosting provider's release process;
- upload to a staging directory when the server supports it;
- verify the staged files and switch to them using the provider's supported method;
- keep the previous release available until the new one is checked.

Not every host supports a safe directory switch or rename. Do not assume that a staging workflow is atomic. Use the hosting platform's deployment method when it provides one.

## Verify downloads and backups

A successful queue status means the transfer completed according to the protocol. For important data, verify the downloaded result. Compare file sizes and use checksums when both sides provide them. Open or test the file with the application that owns it when appropriate.

A FileZilla transfer is not itself a complete backup plan. A useful backup has separate copies, a known retention period, and a restore test. Keep backups protected from the same account or failure that could affect the original files.

## Maintain FileZilla Client and Server

Install the client or server from the official FileZilla project source. Keep the installed product current under your normal change process. Before an upgrade on a server, check the current version notes, save configuration backups, and plan a maintenance window if active users may be affected.

FileZilla Client provides export and import options for site entries, settings, and the queue in supported versions. Store an export securely and test that it can be imported when moving to another machine. A settings export does not back up the files stored on the server.

For a server, separately back up the FileZilla Server configuration, the underlying shared data, certificate and key material according to policy, and any account records the product requires. Protect administrative access and verify that the service starts and users retain only the intended access after maintenance.

## Review the service over time

Periodically review:

- enabled accounts and their owners;
- mount points and native folder access;
- protocol and encryption requirements;
- certificates and SSH host keys;
- public listener and passive-port rules;
- failed transfers and repeated login attempts;
- storage, log growth, and backup restore results.

Remove temporary test accounts and firewall rules when testing ends. Record changes so another administrator can understand why a setting exists.

## Practice

1. Use the secure routine to plan a small test upload, including identity and destination checks.
2. Explain why plain FTP is unsuitable when credentials or contents need confidentiality.
3. Prepare a redacted Site Manager export process and identify where the export should be stored.
4. Draft a rollback plan for a website update that transfers many files.
5. Verify a downloaded backup with size, checksum, and an application-level check.
6. Explain why a successful transfer does not prove that a backup can be restored.
7. List the items to back up before changing a server configuration.
8. Review a test server's users, shared paths, encryption, firewall rules, and restore process.

## Main references

- [Official FileZilla Client and Server downloads](https://filezilla-project.org/)
- [FileZilla FAQ](https://wiki.filezilla-project.org/FAQ)
- [FileZilla other features and export options](https://wiki.filezilla-project.org/Other_Features)
- [FileZilla Server documentation](https://filezillapro.com/docs/server/)
- [FileZilla TLS specifications](https://wiki.filezilla-project.org/TLS_specifications)
- [FileZilla SFTP specifications](https://wiki.filezilla-project.org/SFTP_specifications)
