# 10. Directory comparison, filters, and synchronization

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Transfer types and overwrite behavior](./09-transfer-types-and-overwrite-behavior.md) | [Notes index](../README.md) | [Next: File permissions, timestamps, and filenames](./11-file-permissions-timestamps-and-filenames.md) |

## Compare the local and remote folders

Directory Comparison helps inspect differences between the open local and remote directories. In the View menu, choose Directory Comparison and select a comparison basis:

- Compare file size to identify same-named files whose sizes differ.
- Compare modification time to identify same-named files whose times differ.
- Hide identical files to reduce visual clutter.
- Enable Directory Comparison to show the result.

FileZilla color-codes entries according to the selected comparison. The documented view highlights entries that exist on only one side, and marks same-named files that differ by the chosen size or time check. Colors and exact menu labels can vary by version, so read the active comparison mode before acting.

Comparison is a review aid. It does not prove that two files have identical contents. Equal sizes can contain different bytes, while timestamps can differ because of clocks, time zones, or server behavior. Use checksums when you need stronger verification.

## Interpret common differences

| Observation | Possible meaning | Next check |
| --- | --- | --- |
| Name appears only locally | It may need uploading, or it may be intentionally local | Confirm whether it belongs on the server |
| Name appears only remotely | It may need downloading, or it may be server-generated | Confirm who owns the remote file |
| Same name, different size | One copy may be newer or incomplete | Compare timestamps and inspect the file |
| Same name, different time | One copy may have changed, or clocks may disagree | Check size, content, and server time settings |
| Entry is not visible | A filter or listing permission may hide it | Review active filters and account access |

Do not treat every highlighted file as a transfer instruction. A server may contain uploads, caches, logs, or configuration that should not be overwritten by a local copy.

## Use filename filters with care

Filename filters can hide entries from directory listings and affect which files are included in transfers. Local and remote filters are configured separately. Depending on the filter, rules can match names, paths, sizes, dates, or platform-specific attributes and permissions.

A filter can use conditions that match any, all, or none of its criteria. It can also apply to files, directories, or both. Multiple active filters can combine, so a missing item may be hidden rather than deleted.

For example, a temporary-file filter might match names ending in `.tmp`. Before using it, check whether the rule applies to directories and whether the filter is active on the local side, remote side, or both.

When a file seems to have disappeared:

1. Open the filename filter dialog.
2. Review active filters on the relevant side.
3. Temporarily disable a filter if you are unsure what it hides.
4. Refresh the listing.
5. Confirm whether server permissions or hidden-file behavior also apply.

Keep a record of custom filter rules if they are part of a repeated workflow. A filter set controls visibility and selection, so review it before a large queued transfer.

## Synchronized browsing means linked navigation

Synchronized browsing keeps local and remote navigation in step when both directories have corresponding folder structures. Configure matching default locations in the Site Manager and enable the option for that saved site.

For example, opening a local `images` folder can open its remote counterpart. The two paths can have different names, but their directory structure must correspond for navigation to remain useful.

Synchronized browsing does not compare file contents, upload changes, remove remote files, or keep copies automatically synchronized. It links navigation only. Use Directory Comparison to inspect differences, then select and queue the transfers you actually intend to make.

## Plan a one-way update

For a controlled upload from a local build folder:

1. Set the local pane to the intended source.
2. Set the remote pane to the intended server destination.
3. Review active filters so needed files are visible.
4. Compare by size or time to find candidate differences.
5. Check the highlighted files and decide which should move.
6. Queue only the intended uploads.
7. Review overwrite prompts and transfer results.

For a download, reverse the source and destination. Avoid deleting or replacing remote-only content until its purpose is known.

## Limitations of comparison

A size comparison can miss changes when two files have equal length. A time comparison can be misleading if timestamps are rounded, converted, or out of sync. A directory with the same name on both sides can still contain different children.

Use a checksum or a deployment tool for exact content verification. For a website release, keep a known build artifact and a rollback plan rather than relying only on colored rows.

## Practice

1. Compare two test folders by size, then by modification time, and note how the view changes.
2. Create a file that exists only locally and another that exists only remotely. Explain why neither should be transferred automatically.
3. Set a temporary filename filter and confirm how it changes the listing.
4. Disable a filter to recover a hidden item and distinguish hiding from deleting.
5. Enable synchronized browsing for matching test folders and explain why it does not copy files.
6. Plan a one-way upload by checking paths, filters, differences, and overwrite behavior before starting.
7. Explain why equal size or matching time alone cannot prove identical file content.

## Main references

- [FileZilla directory comparison](https://wiki.filezilla-project.org/Other_Features#Directory_Comparison)
- [FileZilla filename filters](https://wiki.filezilla-project.org/Filename_Filters)
- [FileZilla Client quick guide](https://wiki.filezilla-project.org/Using)
- [FileZilla Site Manager](https://wiki.filezilla-project.org/Site_Manager)
