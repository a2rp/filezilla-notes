# 13. FileZilla Server users, groups, and access

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: TLS certificates, SSH host keys, and trust](./12-tls-certificates-ssh-host-keys-and-trust.md) | [Notes index](../README.md) | [Next: Server passive mode and network configuration](./14-server-passive-mode-and-network-configuration.md) |

## Client and server have different jobs

FileZilla Client connects to a remote service and transfers files. FileZilla Server accepts incoming connections and decides which accounts and folders are available. Installing the client alone does not create a server or an FTP account.

A server account is separate from a Windows, Linux, or hosting-panel account unless the server is specifically configured to use those system credentials. The client cannot grant itself server access.

## Create the account around a specific task

A server user controls whether a person can connect and what resources they can access. Depending on the installed product and version, user settings can include credentials, mount points, filters, and connection or transfer limits.

Create one account for a clear purpose, such as downloading reports or uploading to a drop folder. Give it access only to the directory and actions it needs. Disable unused accounts, and use a unique strong credential or the supported key-based method.

Test the account from a separate client connection. Confirm both that allowed operations work and that forbidden paths or actions are denied.

## Map a virtual path to a server folder

A mount point maps a native path on the server to a virtual path shown to the connected user.

~~~text
Native server folder: D:\Shared\Incoming
Virtual path shown to user: /
~~~

With this mapping, the user sees the contents of the selected native folder at the remote root. The client does not need to know the server's internal disk layout. A user-specific mapping can show a different folder at the same virtual path to another account.

Choose a dedicated folder instead of exposing an entire drive or broad home directory. Confirm that the native path exists, that the server process can access it, and that the virtual root is the intended starting location. A virtual mapping does not override the underlying operating system's access rules.

## Grant the smallest useful permissions

The current FileZilla Server documentation describes mount point access such as Read only, Read + Write, Write Only, and Disabled. Exact controls and labels depend on product edition and version.

| Access choice | General effect | Example use |
| --- | --- | --- |
| Read only | User can view and download permitted content | Report consumer |
| Read + Write | User can read and make permitted changes | Maintainer for a working directory |
| Write Only | User can send files without reading or listing existing content | Drop-off folder |
| Disabled | User cannot access that mapped path | Block a sensitive subfolder |

Read + Write can include changes to the directory structure unless the corresponding option restricts that behavior. A write-only drop folder can still need careful cleanup and quota rules. Consider whether the account may create folders, replace files, rename items, or delete existing content.

FileZilla Server permissions are also constrained by the server operating system. If the service account cannot read a native folder, a user mount point cannot make that folder readable. If the underlying system marks a file read-only, a broader server-side setting may still be unable to modify it.

## Use groups for repeated policy

Groups can apply shared mount points, filters, and limits to several users when the installed edition supports them. They reduce repeated setup, but user-specific settings and group membership order can affect the final result.

Before adding a user to multiple groups, check how the server resolves overlapping settings. Current FileZilla Server documentation gives mount points set directly on a user priority over group mount points. When multiple groups define the same virtual path, group order can determine which mapping is used. Limits and filters can also combine according to the server's rules.

Keep group names tied to a clear role, such as `read-only-reports` or `incoming-uploads`. Test a sample account after changing membership, especially if groups share paths or define different limits.

## Separate account access from file-system access

A login can succeed while listing, downloading, or uploading fails. These are different checks:

1. Is the account enabled and authenticated?
2. Does the account have a mount point for the requested virtual path?
3. Does the mount point allow the requested action?
4. Can the FileZilla Server process access the native path?
5. Does the operating system or storage layer allow that operation?
6. Does the network permit the transfer connection?

Read the server log and client message log to find which stage failed. A permission denied response does not always mean that the FileZilla user setting is the only problem.

## Keep access changes reviewable

Before changing a shared server:

- record the account, group, virtual path, and native path;
- note existing access rights and limits;
- change one rule at a time;
- test using the actual user account;
- confirm that unrelated users still have the expected access;
- keep a rollback record for important services.

Do not use a broad writable mapping as a quick way to make an upload succeed. First identify which path and operation is being denied.

## Version and edition note

FileZilla Server interfaces and available features have changed over time, and FileZilla Pro Enterprise Server has additional capabilities. Use the documentation for the installed edition and version before applying a menu-by-menu procedure. This chapter describes the access concepts that carry across those interfaces.

## Practice

1. Explain the difference between FileZilla Client and FileZilla Server.
2. Map a dedicated server folder to a virtual root and describe which path the client sees.
3. Choose an access level for a report reader, an uploader, and a maintainer.
4. Explain why server mount-point access cannot override operating-system permissions.
5. Create a test account in a permitted environment and verify both allowed and denied actions.
6. Describe how a group can simplify shared policy and why overlapping group mappings need review.
7. Diagnose a user who can log in but cannot list or upload to a mapped path.
8. Record the edition and version before following product-specific server instructions.

## Main references

- [FileZilla Server user settings](https://filezillapro.com/docs/server/advanced-options/filezilla-server-users-panel/)
- [FileZilla Server group settings](https://filezillapro.com/docs/server/advanced-options/filezilla-server-group-panel/)
- [FileZilla Server mount points and permissions](https://filezillapro.com/docs/server/advanced-options/how-to-edit-mount-points-virtual-native-paths/)
- [FileZilla Server rights management](https://filezillapro.com/docs/server/advanced-options/filezilla-server-rights-management/)
