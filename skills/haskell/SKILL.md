---
name: haskell
description: Getting exact answers about Haskell code from the compiler's own index, the .hie files that ghc writes and the hiedb database built from them. Use when writing .hie files with ghc or cabal, when indexing them with hiedb, when a hiedb query prints nothing, or when deciding whether an index of Haskell code can be trusted or has run the code.
---

# Haskell compiler indexes

Run `vyd recipe run how-do-i "<question>"` in the folder of the work, and read the entries it returns. Each entry is a note of this domain's register, with the evidence that was run and the versions it holds for. The points below are the ones the notes state, in the order work meets them. Write here whatever about Haskell no entry holds.

## Writing `.hie` files

- A `.hie` file starts with `HIE` and four digits of the GHC version that wrote it, as `HIE9103` for 9.10.3 and `HIE9124` for 9.12.4 (NOTE-1). Read those bytes to learn which GHC wrote a file and whether a tool can read it.
- A Hackage package built into the cabal store with no flag leaves no `.hie` file, so a dependency has none until it is built again with `-fwrite-ide-info` (NOTE-3).
- Setting `ghc-options: -fwrite-ide-info -hiedir=<dir>` for a package in `cabal.project.local` builds it again as a new store entry with a new unit id, and the old entry stays (NOTE-4). An earlier index then names the old unit id.
- Writing `.hie` files means compiling, so a module's Template Haskell splices run (NOTE-10).

## Reading them with hiedb

- hiedb reads only a file written by the GHC it was built with. A file from another version stops it with a panic and leaves an empty database (NOTE-2). A `.hie` file holds the source of its module, so one moved to another machine or kept after a GHC upgrade is stale in two ways.
- Reading `.hie` files with hiedb runs no splice (NOTE-10).
- `hiedb -D <path>` on a path that holds no database creates an empty one and exits 0, so a mistyped path reads as no results (NOTE-6).
- With no `-D`, hiedb opens `default_99999103.hiedb` in `$XDG_DATA_HOME`, else `~/.local/share`. It creates the database empty there when the folder exists and fails when it does not (NOTE-7).
- A name hiedb does not know gives no output and exit 0 from `name-refs`, and a message and exit 1 from `name-def` given a module (NOTE-8). A misspelt name is not a name with no callers.
- In `refs`, a reference names the module that defines the name, not the facade module the referencing module imported it through (NOTE-5).

## Reading the database without hiedb

- An hiedb database is in WAL mode, so a read-only open of it in a folder that cannot be written fails, and `file:<path>?immutable=1` reads it but does not see what its `-wal` file holds (NOTE-9).
