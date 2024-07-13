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

  ```bash
  git add -a
  ```

- stage the child files of 2 subfolders in a Git repository:

  ```bash
  git add path/to/subfolder1/* path/to/subfolder2/*
  ```

### Removing Files

```bash
git rm
    [-f | --force]
    [-r] allow recursive removal
    [--cached] only remove from the index
```

> Remove files and stages the file’s removal.

### `Git restore`

Let’s say you’ve changed two files and want to commit them as two separate changes, but you accidentally type **git add \*** and stage them both. How can you unstage one of the two?

```bash
git restore --staged <file>...
```

> Warning: dangerous command.

### `Git reset`

```bash
git reset
[--mixed | --soft | --hard | --merge | --keep] [<commit>]
```

Example:

```bash
git reset --soft HEAD~
git reset --mixed HEAD~
git reset --hard HEAD~
```

> Warning: **Flag (--hard)** is dangerous command.

Remove last commit from remote Git repository

    A->B->C->D[HEAD, ORIGIN]

how can I go to

    A->B->C[HEAD,ORIGIN]

Command:

```bash
git reset HEAD^ # remove commit locally
git push origin +HEAD # force-push the new HEAD commit
```

Remove 3 last commits from remote Git repository

    A->B->C->D->E[HEAD, ORIGIN]

how can I go to

    A->B[HEAD,ORIGIN]

Command:

```bash
git reset HEAD~3 # remove commit locally
git push origin +HEAD # force-push the new HEAD commit
```

### Undo `Git reset`

```bash
git reflog
git reset --hard <commit>
```

### Ignoring Files

- Start `#` are ignored.
- Standard glob patterns work.
- Start `/` to avoid recursivity.
- End `/` to specify a directory.
- `!` negate a pattern.
- `a/**/z` match nested directories;

example for .gitignore file:

```bash
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
```

> Tip: nested .gitignore files apply only to the files under the directory where they are located.

---

## Rewriting History

### Rebasing

Modify your last commit message:

```bash
git commit --amend
```

Modify **n<sup>th</sup> commit** from HEAD:

```bash
git rebase -i HEAD~n

git commit --amend

git rebase --continue
```

> Note: This can lead to unavoiable conflicts when try to modify commits which are assosiated to another commits.

> Question: How to unstage text.txt file, which was contained in **n<sup>th</sup> commit** without conflict to **n+1<sup>th</sup> commit** (this commit contains text.txt as well.)?

### Splitting

...

### Squashing Commits

If you want to make a single commit from these three commits

```bash
git reset --soft HEAD~3
git commit
```

or

```bash
git rebase -i HEAD~3
```

> after that replace following before targeted commits:
>
> `pick`: is for the commit you want to keep.
>
> `squash`: is for the commit you want to merge into the previous commit.
>
> `drop`: is for the commit you want to remove.
>
> `edit`: is for the commit you want to modify.
>
> `reword`: is for the commit you want to modify the commit message.
>
> `skip`: is for the commit you want to skip.
>
> `fixup`: is for the commit you want to merge into the previous commit without modifying the commit message.

```bash
pick f7f3f6d Change my name a bit
squash 310154e Update README formatting and add blame
squash a5f4a0d Add cat-file
git rebase --continue
```

---

## lower-level commands

- information about files in index/working directory:

  ```bash
  git ls-files
  ```

## Contributing to a Project

Changes you submit may be rendered obsolete or severely broken by work that is merged in while you were working or while your changes were waiting to be approved or applied. How can you keep your code consistently up to date and your commits valid?

```

```
