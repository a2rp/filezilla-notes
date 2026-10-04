# 11. File permissions, timestamps, and filenames

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Directory comparison, filters, and synchronization](./10-directory-comparison-filters-and-synchronization.md) | [Notes index](../README.md) | [Next: TLS certificates, SSH host keys, and trust](./12-tls-certificates-ssh-host-keys-and-trust.md) |

## Read remote permissions

A remote file listing may show permissions or attributes, depending on the protocol and server. On Unix-like systems, permissions are commonly described for the owner, group, and everyone else, with read, write, and execute bits for each.

~~~text
-rw-r--r--  regular file
drwxr-xr-x  directory
~~~

The first character indicates a file type in listings that use this convention. The remaining characters describe read, write, and execute access in three groups. A directory's execute permission allows a user to enter or traverse it; directory write permission controls whether entries can be created, renamed, or removed, subject to the server's rules.

Windows servers and other storage systems can use permission models that do not map directly to Unix mode bits. The permissions displayed by the client are the server's view, not a complete explanation of every access rule.

## Change permissions only when needed

On supported servers, FileZilla can expose a file attributes or permissions action from the remote context menu. The server must support the request and the account must have permission to make the change.

A common Unix-style numeric mode is:

~~~text
644  owner can read and write; group and others can read
755  owner can read, write, and traverse; group and others can read and traverse
~~~

These examples describe permission bits only. They do not change who owns a file, the parent directory rules, access control lists, or hosting policy. Avoid setting every file to 777. Broad write access can allow other users or processes to change files and can create a security problem.

When a website file cannot be read, first check the path, account, owner, parent directory access, and server policy. Change a mode only when the administrator or hosting guidance identifies the required value. Directory permissions and file permissions often need different settings.

## Understand modification times

A modification time is metadata reported by the local file system or server. FTP extensions can provide machine-readable modification times, but server support and precision vary. FileZilla's directory comparison can use modification time as one comparison signal.

A timestamp difference does not always mean the contents differ. Possible causes include:

- the file was saved again without a content change;
- the server rounds timestamps to a coarser precision;
- the local and remote clocks or time zones differ;
- the server reports a timestamp using a different listing format;
- the file was copied or restored and received a new time.

RFC 3659 defines FTP modification-time values in UTC. Other listings or protocols may have different behavior. When a timestamp is unexpected, compare the file size and inspect its contents or checksum before replacing it.

## Use filenames that travel well

A filename accepted on one system may be invalid or ambiguous on another. Operating systems and server storage can differ in character support, case sensitivity, reserved names, trailing spaces, and maximum path length.

Use portable names for shared project files:

~~~text
Good:  project-report-2026.pdf
Good:  assets/logo-dark.svg
Risky: names with trailing spaces, reserved device names, or characters rejected by the destination
~~~

Avoid names that differ only by letter case when files must move between systems with different case behavior. For example, `Logo.png` and `logo.png` may be distinct on one file system and collide on another.

Unicode names can be transferred when the client and server support a compatible encoding. If accented or non-Latin characters display incorrectly, check the server's encoding support and the site's Charset setting before renaming files. A display problem does not by itself prove that the stored name is wrong. Preserve a copy before changing a production filename.

## Keep path and filename identity together

A file is identified by its directory and name. Renaming a file or moving it to another directory can break links, application configuration, or references in a website. Before a remote rename, search the project for references and confirm the target directory.

When names appear duplicated or missing, check active filters, case differences, Unicode display, and the current remote path. Do not delete a name that looks unfamiliar until you confirm its role.

## Practice

1. Read the sample Unix-style permission strings and explain the difference between a file and directory entry.
2. Describe why directory execute permission differs from a file's execute permission.
3. Explain why changing a file to 777 is not a general fix for a permission error.
4. Compare two files with different timestamps and list other evidence you would inspect before overwriting one.
5. Rename a test file using a portable name and explain which names can collide across file systems.
6. Inspect a filename with non-Latin characters and identify the Charset setting for the saved site.
7. Explain how renaming a remote file can break a website even if its contents are unchanged.
8. Identify which permission change requires server support and account authorization.

## Main references

- [FileZilla file attributes](https://wiki.filezilla-project.org/Other_Features#Chmod)
- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
- [RFC 3659: Extensions to FTP](https://www.rfc-editor.org/rfc/rfc3659)
- [RFC 2640: Internationalization of the File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc2640)
