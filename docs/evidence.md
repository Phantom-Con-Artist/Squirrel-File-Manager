# Evidence

Every number in the README comes from this page. Measurements were taken on 8 November 2026.

## Test machine

| | |
|---|---|
| CPU | AMD Ryzen 5 3450U (4 cores, 8 threads), a 2020 laptop chip |
| Memory | 13 GB |
| Storage | Micron 2210S NVMe SSD, 512 GB |
| OS | Fedora Linux 44 |
| Build | Release build, benchmarks capped at 5 GB RAM and 6 CPU cores |

## Real drive benchmark

The whole system drive (`/`) was mapped and queried with Squirrel's benchmark tool.

| Measure | Result |
|---|---|
| Files | 937,553 |
| Folders | 126,266 |
| Projects detected | 2,666 |
| System paths skipped (`/proc`, `/sys`, caches, build folders) | 114,611 |
| Time to map | 11.8 s |
| Index size on disk | 806 MB |
| Search index in memory | 199 MB |
| App start, index ready | 568 ms |
| App start, index rebuilt from disk | 1,950 ms |

### Searches

| Query | Matches | Time |
|---|---|---|
| `readme` | 11,818 | 11.0 ms |
| `cargo toml` | 5,474 | 6.7 ms |
| `main.rs` | 4,898 | 8.1 ms |
| `pubspec` | 781 | 3.8 ms |
| `x` | 176,104 | 6.6 ms |
| Recent files, newest first | 937,553 | 2.4 ms |
| Everything sorted by size | 937,553 | 8.5 ms |
| Open a folder (146 items) | 146 | 0.017 ms |

### Typing, one keystroke at a time

| Typed | Time per keystroke |
|---|---|
| r, re, rea, read, readm, readme | 14.9, 9.4, 12.7, 14.0, 11.3, 7.6 ms |
| i, in, inv, invo, invoi, invoic, invoice | 14.1, 11.3, 12.5, 6.6, 5.0, 3.5, 2.6 ms |
| s, sc, scr, … screenshot | 13.1 ms at first, falling to 0.9 ms |

### Known slow case

Path searches that match a large part of the drive are slower. `src/lib` matches 301,030 paths and takes **164 ms**.

### How the speed is achieved

Squirrel keeps a compact in-memory index and uses established techniques:

- **Block-Max WAND** (from search engines such as Lucene) skips blocks of files that cannot make the top results.
- **Character masks** reject files that lack a letter of the query before any scoring.
- **Refinement cache** (the approach used by fzf) checks only the previous matches when you add a letter.
- **Pre-sorted orders** answer "newest first" and "largest first" without sorting at query time.
- **Saved index snapshot** loads in one read at start-up and is checked against the main index before use.

Fuzzy scoring uses the `nucleo` matcher, the same engine as the Helix editor.

## Scale test

A synthetic index of **10 million** files answered typical searches in 45 to 55 ms, using 2.8 GB of RAM. A 50 million file test was refused by the test's memory guard on this 13 GB machine (it needs about 21 GB), so that size is not claimed.

## Tests

68 automated tests pass: 42 for the engine (Rust) and 26 for the interface (Flutter). Three further interface tests only render screenshots and are not counted.

| Area | What is checked |
|---|---|
| Nothing changes on disk | A deliberately hostile folder (broken links, link loops, a named pipe, unreadable files) is mapped, searched and browsed. Every byte, timestamp and permission is compared before and after. |
| Explicit actions only | Creating never overwrites; bad names are refused; deleting goes to the system Trash and is recoverable. |
| Attack tests | Swapping files for links during reads, swapping folders mid-scan, tampering with the saved index, 3,000-tag floods, absurd queries, repeated and partial Trash requests. |
| Search correctness | Fast paths are compared with brute force on random data. The score bound used to skip work is checked against 60,000 real scores. |
| Interface | Text contrast in both themes, and all four views at 100% and 200% text size with no clipped layouts. |

The code has also been through three rounds of adversarial code review by a separate AI model working read-only. Confirmed findings were fixed, each with a test that reproduces it. One finding was kept by design: a full scan pauses other actions until it finishes, because answers from a half-built index would be wrong.

## Screenshots

All screenshots are of the real app running on the real engine. To avoid showing anyone's personal files, they were captured against a demo home folder of sample files.
