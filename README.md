<!-- SPDX-License-Identifier: MIT -->

# Vyvid Haskell Domain

Вивід: домен Haskell

This domain holds notes on how to get exact answers about Haskell code from the compiler's own index, kept so that any project can use them. It has one register.

- `notes.md` holds one note `NOTE-n` per finding, on `ghc`, `cabal`, `hiedb` and `sqlite3` as they meet in `.hie` files. A note says what to do and why, and its Evidence cell says what was run and on which version.

A note holds for the versions and the platform it names and no others, until it is run again. A project keeps a register of the same type in its own documents and names a note of this domain in its Specialises column, so a question asked in that project is answered with the project's row first and the note beneath it.

[MIT](LICENSE)
