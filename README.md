<div align="center">

<img src="docs/brand/squirrel-256.png" width="96" alt="Squirrel logo">

# Squirrel

**Find anything. Move nothing.**

A file manager for your whole computer that organizes everything virtually.<br>
Search, tag and sort files across every drive without moving a single one.

![Status](https://img.shields.io/badge/status-pre--release-E5602A)
![Platform](https://img.shields.io/badge/platform-Linux-555)
![Tests](https://img.shields.io/badge/tests-68%20passing-2E7D32)
![License](https://img.shields.io/badge/license-proprietary-555)

<img src="docs/screenshots/search.png" alt="Searching for 'invoice pdf' across the whole computer, with the details pane open" width="900">

</div>

## Contents

- [The problem](#the-problem)
- [What makes Squirrel different](#what-makes-squirrel-different)
- [How it works](#how-it-works)
- [Features](#features)
- [Screenshots](#screenshots)
- [Search examples](#search-examples)
- [Performance](#performance)
- [Safety](#safety)
- [Privacy](#privacy)
- [FAQ](#faq)
- [Roadmap](#roadmap)
- [Built with](#built-with)
- [Status and availability](#status-and-availability)
- [License](#license)

## The problem

Files end up everywhere: Downloads, project folders, old drives, three copies of the same PDF. Regular file managers show one folder at a time. Search tools find names but cannot organize. Organizing usually means moving and renaming files, which breaks projects, links and backups.

## What makes Squirrel different

| | |
|---|---|
| **Virtual** | Tags, collections, versions and duplicates live in Squirrel's own index. Your folders stay exactly as they are. |
| **Safe** | Nothing on disk changes unless you ask. Creating a file never overwrites, and deleting always goes to the system Trash. |
| **Fast** | Over a million files searched in about 10 ms on a laptop, as you type. |
| **Whole computer** | Maps your system drive, home folder and external drives together, not one folder at a time. |

## How it works

1. **Map.** Choose your drives and folders. Squirrel walks them once, read-only, and records a small pointer for each file: its path, name, size, type and date.
2. **Find.** Every search runs against those pointers in memory, so results appear as you type. Squirrel reads a file's contents only when you preview it, search inside it or check it for duplicates.
3. **Organize.** Tags, collections, versions and duplicates are stored as extra information on the pointers. Your files are never moved or renamed to organize them.

Squirrel suits anyone whose files are spread across many folders and drives: developers with dozens of projects, designers and photographers with large libraries, and people who keep everything in Downloads.

## Features

- **Search that understands you.** Type `report pdf last week` or `>1gb` and Squirrel turns it into filters. Partial names work too: `rdme` finds `README.md`.
- **Search inside files.** Find text inside notes, code and other text files.
- **Tags.** Tag any file, then filter with `#taxes`. The file itself is never changed.
- **Collections.** Images, videos, documents, downloads, screenshots and large files, gathered from every drive.
- **Duplicates.** Byte-for-byte identical copies, grouped by how much space you would get back.
- **Versions and source.** See other versions of a file, the project it belongs to, and the web address it was downloaded from when the browser recorded one.
- **Four views.** Details, list, tiles and large tiles with image thumbnails.
- **Keyboard first.** Every action has a shortcut. Press `Ctrl /` to see them all.
- **Accessible.** Light and dark themes that meet WCAG 2.2 AA contrast, screen reader labels on controls and results, and result views tested at 200% text size.

## Screenshots

| | |
|---|---|
| <img src="docs/screenshots/computer.png" alt="Computer view with mapped locations"> | <img src="docs/screenshots/large-tiles.png" alt="Images collection in large tiles view"> |
| **Computer.** Every mapped drive and folder in one place. | **Large tiles.** Thumbnails for images, from any folder. |
| <img src="docs/screenshots/duplicates.png" alt="Duplicate files grouped with the newest copy marked to keep"> | <img src="docs/screenshots/recent-light.png" alt="Recent files in light theme, list view"> |
| **Duplicates.** Identical copies, newest marked to keep. | **Recent.** Everything you changed lately, light theme. |

More in [docs/screenshots](docs/screenshots).

## Search examples

| You type | Squirrel finds |
|---|---|
| `invoice pdf` | PDFs with "invoice" in the name |
| `photos from 2024` | Images modified in 2024 |
| `#taxes pdf` | PDFs you tagged "taxes" |
| `in:Downloads >100mb` | Files over 100 MB in any Downloads folder |
| `report pdf last week` | PDFs named "report" changed in the last 7 days |
| `>1gb` | Every file over 1 GB |

## Performance

Measured on a real drive with **1,063,819 files and folders** (AMD Ryzen 5 3450U laptop, 13 GB RAM, NVMe SSD, Fedora 44).

| Task | Time |
|---|---|
| Search "readme" (11,818 matches) | 11 ms |
| Each keystroke while typing | 1 to 15 ms |
| Recent files, newest first | 2.4 ms |
| Sort everything by size | 8.5 ms |
| Open a folder | 0.02 ms |
| App start with the index ready | 0.57 s |
| Map the whole drive | 11.8 s |

Method, raw numbers and known slow cases are in [docs/evidence.md](docs/evidence.md).

## Safety

Squirrel reads your drives. It writes only to its own data folder, unless you explicitly create a file or move one to the Trash, and asks you to confirm first.

This is checked by automated tests, including one that maps a deliberately hostile folder and verifies that every byte, timestamp and permission is unchanged afterwards. Details in [docs/safety.md](docs/safety.md).

## Privacy

Squirrel works fully offline. It has no account, no cloud service, no analytics and no crash reporting, and it makes no network connections. The index stays on your computer in Squirrel's own data folder, and deleting that folder removes everything Squirrel knows.

## FAQ

**Does Squirrel replace my file manager?**
It can sit beside it. Squirrel is for finding and organizing across your whole computer. Any file opens in its usual app, or in your system file manager with one click.

**What happens to my tags if I move a file outside Squirrel?**
Tags are tied to the file's path, so after a rescan the file appears at its new path without its tags. Squirrel never needs you to move files to organize them, and tags are never lost to a crash or an unplugged drive.

**Do changes on disk show up instantly?**
Not yet. New, moved or deleted files appear after a rescan (`Ctrl R`). Live updates are on the roadmap.

**How much space does the index use?**
About 800 MB on disk per million files, and about 200 MB of memory for the search index. See [docs/evidence.md](docs/evidence.md).

## Roadmap

- Live updates when files change on disk
- Verified builds for macOS and Windows
- A smaller index on disk
- First public release

## Built with

- [Flutter](https://flutter.dev) for the interface
- [Rust](https://www.rust-lang.org) for the engine
- [LMDB](https://www.symas.com/lmdb) through [heed](https://github.com/meilisearch/heed) for the index on disk
- [nucleo](https://github.com/helix-editor/nucleo) for fuzzy matching

## Status and availability

Squirrel is in active development and not yet available for download. It runs on Linux today; macOS and Windows support is written but not yet verified.

This repository holds the public documentation and screenshots. The source code is private.

## License

Proprietary. Copyright © 2026 Subhradeep Sarkar. All rights reserved. See [LICENSE](LICENSE).
