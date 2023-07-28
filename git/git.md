# GIT

## Definitaion

    Working tree.
    Index.
    HEAD.

    Working Tree.
    Staging are.
    Git directory.

## Git Commands

### Adding files

- stage all:

    `git add -a`

- stage the child files of 2 subfolders in a Git repository:

    `git add path/to/subfolder1/* path/to/subfolder2/*`

### Removing Files

    git rm 
    [-f | --force]
    [-r] allow recursive removal
    [--cached] only remove from the index

>Remove files and  stages the file’s removal.

### Git Restore

Let’s say you’ve changed two files and want to commit them as two separate changes, but you accidentally type **git add *** and stage them both. How can you unstage one of the two?

    git restore --staged <file>...

> Warning: dangerous command.

### Git reset

    git reset
    [--mixed | --soft | --hard | --merge | --keep] [<commit>]

Example:

    git reset --soft HEAD~
    git reset --mixed HEAD~
    git reset --hard HEAD~

>Warning: **Flag (--hard)** is dangerous command.

### Ignoring Files

- Start `#` are ignored.
- Standard glob patterns work.
- Start `/` to avoid recursivity.
- End `/` to specify a directory.
- `!` negate a pattern.
- `a/**/z` match nested directories;

example for .gitignore file:

    # ignore all .a files:
    *.a
    # but do track lib.a, even though you're ignoring .a files above
    !lib.a
    # only ignore the TODO file in the current directory, not subdir/TODO
    /TODO
    # ignore all files in any directory named build
    build/
    # ignore doc/notes.txt, but not doc/server/arch.txt
    doc/*.txt
    # ignore all .pdf files in the doc/ directory and any of its subdirectories
    doc/**/*.pdf


> Tip: nested .gitignore files apply only to the files under the directory where they are located.

---

## lower-level commands

- information about files in index/working directory:

    `git ls-files`
