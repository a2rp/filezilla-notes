# FileZilla Study Notes

These are my personal study notes from learning and working with FileZilla. They collect the concepts, settings, and troubleshooting steps I want to keep close when transferring files or managing an FTP server.

The notes focus on FileZilla Client and FileZilla Server, with clear explanations and practical examples. They cover FTP, FTPS, SFTP, connection settings, transfers, access control, network configuration, and secure operations.

## About this collection

This repository is a working record of what I study and practice with FileZilla. Each chapter builds on the connection and file transfer basics, then moves into client workflows, server administration, security, and troubleshooting.

FileZilla Client and FileZilla Server are separate products with different responsibilities. The chapter titles identify when a setting applies to the client or the server.

## Core topics

- File transfer fundamentals and FTP sessions
- FTP, explicit and implicit FTPS, and SFTP
- Site Manager, saved profiles, and connection options
- Local and remote navigation, queues, and overwrite rules
- Active and passive modes, ports, proxies, and firewalls
- Directory comparison, filters, permissions, and timestamps
- TLS certificates, SSH host keys, users, and access rules
- FileZilla Server configuration, logs, and troubleshooting

## Chapters


01. [File transfer basics and FileZilla overview](./chapters/01-file-transfer-basics-and-filezilla-overview.md)  
   Understand clients, servers, files, directories, and the parts FileZilla manages.

02. [FTP connections and sessions](./chapters/02-ftp-connections-and-sessions.md)  
   Learn control and data connections, credentials, ports, and the FTP request flow.

03. [FTPS and SFTP security](./chapters/03-ftps-and-sftp-security.md)  
   Compare explicit FTPS, implicit FTPS, and SSH-based SFTP.

04. [Installation and first connection](./chapters/04-installation-and-first-connection.md)  
   Install FileZilla Client and make a safe connection to a server.

05. [Site Manager and saved profiles](./chapters/05-site-manager-and-saved-profiles.md)  
   Store connection settings, credentials, protocol, and transfer preferences.

06. [Ports, connection modes, and network settings](./chapters/06-ports-connection-modes-and-network-settings.md)  
   Understand default ports, active and passive FTP, proxies, and firewall rules.

07. [Local and remote file navigation](./chapters/07-local-and-remote-file-navigation.md)  
   Navigate local and remote panes, paths, directory listings, and hidden files.

08. [Transfer queue and transfer lifecycle](./chapters/08-transfer-queue-and-transfer-lifecycle.md)  
   Add, pause, resume, prioritize, and review queued transfers.

09. [Transfer types and overwrite behavior](./chapters/09-transfer-types-and-overwrite-behavior.md)  
   Choose ASCII or binary transfer modes and handle files that already exist.

10. [Directory comparison, filters, and synchronization](./chapters/10-directory-comparison-filters-and-synchronization.md)  
   Compare local and remote trees, filter names, and use synchronized browsing carefully.

11. [File permissions, timestamps, and filenames](./chapters/11-file-permissions-timestamps-and-filenames.md)  
   Review remote permissions, modification times, character sets, and path conventions.

12. [TLS certificates, SSH host keys, and trust](./chapters/12-tls-certificates-ssh-host-keys-and-trust.md)  
   Inspect certificate and host key prompts and verify server identity.

13. [FileZilla Server users, groups, and access](./chapters/13-filezilla-server-users-groups-and-access.md)  
   Understand server users, shared directories, permissions, and listener settings.

14. [Server passive mode and network configuration](./chapters/14-server-passive-mode-and-network-configuration.md)  
   Configure listeners, passive port ranges, NAT, firewalls, and public addresses.

15. [Logs, errors, and connection troubleshooting](./chapters/15-logs-errors-and-connection-troubleshooting.md)  
   Read status messages and logs to isolate DNS, authentication, TLS, and transfer failures.

16. [Secure transfer workflows and maintenance](./chapters/16-secure-transfer-workflows-and-maintenance.md)  
   Apply safe file handling, backup, account, update, and operational practices.

## Reference chapters

- [All code samples](./chapters/98-all-code-samples.md) collects examples from the core chapters.
- [Complete questions and answers](./chapters/99-complete-q-and-a.md) gathers review questions across the notes.

## How to use these notes

Read the chapters in order when learning FileZilla, or open the section that answers a question from your current setup. Try each setting in a test connection before applying it to an important server or transfer.

## Main references

- [FileZilla Project](https://filezilla-project.org/)
- [FileZilla documentation](https://filezilla-project.org/documentation.php)
- [FileZilla Project Wiki](https://wiki.filezilla-project.org/)
- [FileZilla support forum](https://forum.filezilla-project.org/)

## License

These notes are available under the [MIT License](./LICENSE).
## Links

- Portfolio: https://www.ashishranjan.net
- GitHub: https://github.com/a2rp
- CodePen: https://codepen.io/ash1198
- LinkedIn: https://www.linkedin.com/in/aashishranjan
- Facebook: https://www.facebook.com/theash.ashish/
- YouTube: https://www.youtube.com/@ashishranjan-ashz?sub_confirmation=1
- Email: mailto:ash.ranjan09@gmail.com

## Support

- Support: https://a2rp-donation-page.netlify.app/
- Buy Me a Coffee: https://buymeacoffee.com/ashishranjan
- Patreon: https://www.patreon.com/ashishranjan
