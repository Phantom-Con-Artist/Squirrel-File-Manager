# Safety

Squirrel's promise is simple: **organizing is virtual, and your files stay where they are.**

## What Squirrel writes

| Where | What | When |
|---|---|---|
| Its own data folder | The index, settings and an action log | Always |
| Your folders | An empty new file or folder | Only when you choose New file or New folder |
| The system Trash | Items you delete | Only after you confirm |

Nothing else. Tags, collections, versions, duplicates and sorting exist only in Squirrel's index.

## How each action is protected

**Mapping and searching.** Files are opened read-only. Squirrel never follows a link when reading a file, never waits on special files such as named pipes, and on Linux and macOS checks after opening that the file is still inside a mapped location.

**Creating.** A new item is never written over an existing one. Names that could escape the folder, are reserved on Windows, or contain invisible or text-direction characters are refused.

**Deleting.** Items go to the system Trash, where you can restore them. The confirmation lists every item. Squirrel refuses to delete a mapped location, its own data folder, or anything that changed in the index after you chose it. If one item in a batch cannot be moved, the others still are, and you are told which one failed.

**Squirrel's own data.** The index, settings and log are never mapped, shown, opened or deleted through the app, however they are reached.

**Your tags.** Tags are kept even if a drive is unplugged during a rescan, or the app stops halfway through one. They return when the drive does.

## Action log

Every file created or moved to the Trash is recorded, with a timestamp, in `actions.log` inside Squirrel's data folder. Names are escaped so a file name cannot forge a log entry.

| System | Data folder |
|---|---|
| Linux | `~/.local/share/squirrel` |
| macOS | `~/Library/Application Support/Squirrel` |
| Windows | `%LOCALAPPDATA%\Squirrel` |

## How this is tested

See the [tests section of the evidence page](evidence.md#tests).
