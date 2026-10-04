# 09. Transfer types and overwrite behavior

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Transfer queue and transfer lifecycle](./08-transfer-queue-and-transfer-lifecycle.md) | [Notes index](../README.md) | [Next: Directory comparison, filters, and synchronization](./10-directory-comparison-filters-and-synchronization.md) |

## Upload and download are directions

An upload sends data from your computer to the server. A download receives data from the server to your computer. FileZilla's two panes help show direction, but confirm the source and destination paths before transferring.

| Action | Source | Destination |
| --- | --- | --- |
| Upload | Local pane | Remote pane |
| Download | Remote pane | Local pane |

When both sides contain a file with the same name, the name alone does not tell you which copy is current. Check location, size, modification time, and the intended direction.

## Choose a transfer type

FTP has ASCII and binary transfer types. ASCII mode can convert line endings for text files between systems. Binary mode preserves the bytes sent by the source. FileZilla also offers an automatic mode that selects a type based on its file type rules.

| Content | Safer starting choice | Reason |
| --- | --- | --- |
| Images, audio, video, archives, PDFs, fonts, and executables | Binary | These files must arrive byte for byte unchanged |
| Source code and plain text | Auto or binary in most modern workflows | Avoid unintended text conversion unless it is required |
| A legacy FTP workflow that requires newline conversion | ASCII for the known text files | The receiving system expects converted line endings |

Do not use ASCII for binary data. A conversion can change bytes and corrupt the file. If an old server specifically requires ASCII for text, limit that choice to known text files rather than applying it to every transfer.

SFTP runs over SSH and does not use FTP's separate ASCII and binary data connection negotiation. The client may still display transfer type controls, but FTP's wire-level conversion explanation applies to FTP transfers. Check the active protocol and client version when diagnosing an unexpected file change.

## Understand automatic selection

Automatic mode uses FileZilla's configured list of file types to decide which items are treated as text. The decision is based on names and configured rules, not a semantic inspection of the file contents. An unfamiliar extension can therefore receive a different type than expected.

If a transferred file appears damaged:

1. Confirm whether the connection used FTP, FTPS, or SFTP.
2. Check the selected transfer type and automatic file type rules.
3. Compare file sizes and, where possible, cryptographic hashes on both sides.
4. Transfer a known test file in binary mode.
5. Restore the intended automatic settings after the test.

A matching size does not prove that two files have identical contents. A checksum is stronger evidence when both systems can calculate one.

## What an overwrite prompt means

If a destination already has a file with the same name, FileZilla may ask how to handle the conflict. Available choices depend on the client version and the transfer situation. They commonly include replacing the destination, skipping the source, renaming, resuming a partial transfer, or applying a rule to later conflicts.

| Choice | Effect | Use it when |
| --- | --- | --- |
| Overwrite | Replace the destination with the source | You confirmed the source is the intended complete version |
| Skip | Leave the destination as it is | You do not want this entry to change the destination |
| Rename | Keep both copies under different names | You need to inspect both versions |
| Resume | Continue a compatible partial transfer | The existing destination is an incomplete copy of the same source |
| Apply to remaining conflicts | Reuse a selected rule for later queue items | You reviewed the scope and want the same result for every conflict |

Labels may vary. Read the actual prompt in the installed version before selecting an option for the remaining queue.

## Resume only a matching partial file

Resume can save time after a connection drops during a large file. It is safe only when the existing destination is a partial copy of the same unchanged source and the server supports restarting at an offset.

Do not resume over a different file just because its name matches. If the source changed after the partial transfer began, the result can combine bytes from two versions. When uncertain, preserve or rename the destination, then transfer the full source and verify the result.

## Compare before replacing

Before replacing files on a live server, confirm:

- the source and destination paths;
- which copy should be authoritative;
- whether the file is complete and unchanged;
- whether a backup or rollback copy exists;
- whether the selected rule applies to one item or the rest of the queue.

Modification times can be affected by time zones, server settings, or clock differences. Size comparisons cannot detect different contents of the same length. Use directory comparison for a useful first review, and checksums or a deployment system when exact content verification matters.

## Keep a test workflow

A safe first transfer uses a noncritical file in a test directory. Upload it, download it to a separate local folder, compare sizes, and use a checksum tool on both copies if available. Keep the original source until the destination has been checked.

For many website files, choose transfer behavior deliberately and avoid applying a broad overwrite rule without reviewing the queue. A release or backup process is more reliable than manually replacing unknown live files.

## Practice

1. Identify the source and destination for an upload and a download.
2. Explain why an image or archive should not be transferred as ASCII.
3. Describe how automatic mode decides which files are text and why an unfamiliar extension can be misclassified.
4. Create two files with the same name in a test directory and inspect the overwrite choices without applying a rule to the full queue.
5. Explain when resuming a partial file is safe and when it can combine two versions.
6. Compare two same-named files by path, size, modification time, and checksum.
7. Transfer a harmless test file, download a copy, and verify it before deleting the source.
8. Explain why a matching file size is not proof that the contents match.

## Main references

- [FileZilla Client quick guide](https://wiki.filezilla-project.org/Using)
- [FileZilla filename filters](https://wiki.filezilla-project.org/Filename_Filters)
- [RFC 959: File Transfer Protocol](https://www.rfc-editor.org/rfc/rfc959)
- [RFC 3659: Extensions to FTP](https://www.rfc-editor.org/rfc/rfc3659)
