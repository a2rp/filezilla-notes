# 07. Local and remote file navigation

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Ports, connection modes, and network settings](./06-ports-connection-modes-and-network-settings.md) | [Notes index](../README.md) | [Next: Transfer queue and transfer lifecycle](./08-transfer-queue-and-transfer-lifecycle.md) |

## Read the two sides of the window

FileZilla Client shows your computer on the left and the connected server on the right by default. Each side has a path field, a folder tree, and a file list for the selected folder.

| Area | What it shows |
| --- | --- |
| Local path field | The folder currently open on your computer |
| Local tree and list | Drives, folders, and files available to your account |
| Remote path field | The current directory on the server |
| Remote tree and list | Directories and files the server account can access |
| Message log | Connection replies and errors, useful when a folder will not open |
| Transfer queue | Files waiting to move between the two sides |

The two sides are independent. Opening a folder on the left does not change the server path on the right unless synchronized browsing is enabled.

## Move through local folders

Use the local path field to enter a path, choose a folder in the tree, or double-click a folder in the file list. Select the parent entry to move up one directory. You can also use the operating system's folder picker to choose a local path.

Local path syntax belongs to your operating system. For example:

~~~text
Windows: C:\Users\Ashish\Documents\site-files
macOS or Linux: /home/ashish/site-files
~~~

A path must exist and be available to your account. If a folder is on a removable drive or network share, check that it is connected before starting a transfer.

## Move through remote folders

The remote path field identifies the directory currently open on the server. Enter a server path and press Enter, choose a directory from the remote tree, or double-click it in the remote file list. The `..` entry moves to the parent directory when the account is allowed to access it.

The server chooses its own path format and starting directory. An FTP or SFTP account may be restricted to a home folder, so `/` in the server view does not always mean the physical root of the server. Use the path shown after login or ask the administrator for the correct target directory.

A question mark on a directory means FileZilla has not listed that directory yet, so it cannot tell whether it contains subdirectories. Open it to load its contents. If a directory cannot be opened, read the message log for permission, path, or connection errors.

## Select files without transferring the wrong ones

Click a file to select it. Use the operating system's usual modifier keys to select a range or several individual files. Check the highlighted names and the current path before choosing Upload or Download.

Double-clicking a file starts its default transfer action. Dragging a file between the local and remote panes queues a transfer. Dragging within the same side can move an item between directories on that same system if the server and permissions allow it. These actions have different results, so check which pane contains the source and destination before dropping.

To transfer a whole directory, select the directory and use its context menu or drag it to the other side. FileZilla transfers the directory contents recursively. Large folders can add many entries to the queue.

## Refresh listings and handle hidden files

Refresh a listing when another user or process may have changed files since you opened the directory. Refreshing asks the server for a new listing; it does not change the files.

Hidden files can be omitted from a server listing or blocked by the server. The client has a setting to request hidden files on servers that support the relevant listing behavior. Showing hidden names does not grant access to them. Be careful with names such as `.env`, `.htaccess`, and other dotfiles because they may contain settings or credentials.

A remote listing is also limited by the account's permissions. If an expected file is missing, check the selected path, server-side visibility rules, and account access before trying another transfer method.

## Keep matching folder trees in sync

Synchronized browsing changes the local and remote directory together as you navigate. It is useful when both sides have corresponding folder structures, such as a local copy of a website and its server document root.

For example, these paths can correspond:

~~~text
Local:  C:\Users\Ashish\Projects\portfolio\public
Remote: /home/account/www
~~~

The names do not have to match, but each folder you visit on one side needs a meaningful counterpart on the other. Set the default local and remote directories in the Site Manager, then enable synchronized browsing for that saved site. If a corresponding path is missing, navigation may stop matching or land in the wrong place. Turn the setting off when browsing unrelated folders.

## Avoid accidental overwrites

Before uploading, confirm the local source folder and remote destination path. Before downloading, confirm the remote source and local destination. A file with the same name may already exist and the selected transfer action can replace it, skip it, or ask what to do.

Use the directory comparison feature to inspect differences before synchronizing a tree. A matching name does not prove that two files have matching contents. Chapter 10 covers comparison, filters, and synchronization choices.

For a website, identify the document root with the site owner or hosting control panel. Uploading into the account home folder may place files outside the public site, while uploading into the wrong public directory may replace a live page.

## Practice

1. Connect to a test server and identify the local path, remote path, file list, message log, and queue.
2. Navigate to a local project folder using the path field, then return to its parent.
3. Open a remote folder from the tree and from the file list. Note what the question mark means before opening it.
4. Compare Windows and server path syntax, and explain why a remote `/` may not be the server's physical root.
5. Select several test files and describe how the highlighted selection changes when you use modifier keys.
6. Set matching local and remote defaults for a test site, try synchronized browsing, then disable it before opening unrelated folders.
7. Refresh a remote listing and explain why visibility still depends on server permissions.
8. Before a test upload, state the source path, destination path, and what you will do if a same-named file exists.

## Main references

- [FileZilla Client quick guide](https://wiki.filezilla-project.org/Using)
- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
- [FileZilla filename filters](https://wiki.filezilla-project.org/Filename_Filters)
- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
