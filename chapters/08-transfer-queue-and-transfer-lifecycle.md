# 08. Transfer queue and transfer lifecycle

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Local and remote file navigation](./07-local-and-remote-file-navigation.md) | [Notes index](../README.md) | [Next: Transfer types and overwrite behavior](./09-transfer-types-and-overwrite-behavior.md) |

## What the queue does

The transfer queue is the work list for uploads and downloads. You can start a transfer immediately or add selected files to the queue and begin later. A queued entry records the source, destination, direction, and transfer result.

Queueing is useful when a job contains many files. It lets you review the planned work, start it at a convenient time, and inspect failures without losing them among successful transfers.

## Follow a transfer from selection to result

A typical transfer moves through these stages:

1. Select a local or remote source and choose Upload, Download, or Add to Queue.
2. FileZilla adds one or more entries to the queue.
3. When the queue runs, the client opens the required data connection and transfers each file.
4. The server and client close that data connection after the item completes.
5. FileZilla records the result in the successful or failed list.

The message log describes protocol activity. The queue shows file-level progress and outcomes. Use both when a transfer does not finish.

FileZilla uses separate FTP sessions for browsing and transfers. A directory listing can remain available while file transfers run, subject to the server's connection limits. SFTP uses its own SSH session behavior and does not use FTP's separate active or passive data connections.

## Add work and review it before starting

To build a queue, select the intended files or directories and choose Add to Queue from the context menu. You can also drag selected items into the queue. Check the direction and both paths before starting.

For a website upload, a useful review looks like this:

~~~text
Direction: local to remote
Local source:  C:\Projects\portfolio\dist
Remote target: /home/account/www
Planned items: index.html, assets/, styles/
~~~

The queue may contain many individual entries after a directory is expanded. Confirm the remote target before starting, especially if a live site already contains files with matching names.

## Start, pause, and cancel carefully

Start the queue when the destination and overwrite behavior are understood. Pause it when you need to stop sending more entries temporarily. Cancel a queued item to prevent it from starting. Cancelling an item that is already transferring interrupts that transfer and can leave an incomplete destination file, depending on the server and protocol.

After an interruption, inspect the destination before retrying. A partial file may have the same name as the source. Replacing it may be correct for a simple transfer, but a production workflow should confirm the expected file and use an appropriate backup or deployment process.

Stopping the queue does not undo files that already completed. Treat completed uploads and downloads as changes that have happened.

## Read queue results

Use the successful list to confirm which entries completed. Use the failed list to identify entries that need attention. A failed item can be caused by a missing source, an inaccessible path, a permission problem, a network interruption, an unavailable data port, a name conflict, or insufficient server storage.

Before retrying a failure:

1. Read the message log around the failure.
2. Confirm that the source still exists and the destination path is correct.
3. Check server permissions and available storage when relevant.
4. Decide whether a partial destination file should be replaced, renamed, or removed.
5. Retry only the affected item or a clearly understood group.

Repeatedly retrying a permission or path error does not fix the underlying cause.

## Control parallel connections

FileZilla can transfer more than one file at a time when the server permits it. A server may limit concurrent sessions and return an error such as 421 when the limit is exceeded. The Site Manager has a transfer setting to limit simultaneous connections for a saved site.

Reducing the connection limit can help with a restrictive server, but it can also make a large queue slower. A limit of one connection may prevent browsing while a transfer is active on some servers. Follow the server owner's limit rather than guessing.

## Understand direction and source safety

An upload reads from the local side and writes to the remote side. A download reads from the remote side and writes to the local side. The same filename can exist in both locations, so do not infer direction from the name alone.

For a reliable workflow, write down the source path, destination path, and expected result before starting a large queue. If only a subset should move, remove unrelated entries before starting. Use directory comparison in chapter 10 when the local and remote trees need review.

## Practice

1. Add two test files to the queue without starting it. Verify the direction and destination of both entries.
2. Start one small transfer and identify its progress and final result.
3. Queue a directory and observe how it becomes individual transfer work.
4. Interrupt a test transfer, inspect both source and destination, and decide how to retry safely.
5. Create a harmless failed transfer by selecting an unavailable test path, then use the message log to diagnose it.
6. Explain what cancelling an active transfer does and why it does not roll back entries already completed.
7. Set a low connection limit for a test site and observe the effect on a queue.
8. Describe how FTP's transfer sessions differ from its browsing session.

## Main references

- [FileZilla Client quick guide](https://wiki.filezilla-project.org/Using)
- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
- [FileZilla network configuration](https://wiki.filezilla-project.org/Network_Configuration)
- [RFC 959: File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)
