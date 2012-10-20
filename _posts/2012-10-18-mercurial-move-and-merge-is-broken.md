---
layout: post
title: Mercurial Move and Merge is Broken
---

At work, we exclusively use [Mercurial][1] for all our version control
needs. For the most part, I am extremely pleased that a massively
large company has allowed our team to work with a distributed version
control system (DVCS), even if there is no evidence of better commits in
DVCS. See [here][2].

Overall, I am very happy with Mercurial and the implementation choices
they have made. I think of [Git][3] as the more powerful and more dangerous
sibling of Mercurial. While Mercurial strives to be simple and
maintain the integrity of the commit history, Git allows you craft
arbitrarily complex commands that can completely destroy the history
of commits. 

As much as I like Mercurial, I believe that the move command is
fundementally broken.

The `hg mv` command is an alias for the `hg rename` command, which
itself is shorthand for calling `hg copy` followed by an `hg rm` of
the original file. This command allows you to move files around in the
repository, while preserving the change history. This is a *good*
thing. Preserving the history of edits made to a files is the reason
for version control in the first place.

However, the problem with distributed version control systems is that
other developers can be making different changes to the file at the
same time as you are. When you go to reconcile the two different
commit histories, inevitably they must be merged. Most of the time
this merge is performed without any problems. But, if both developers
have modified the same line, the version control system has to ask for
help in the form of a merge conflict. So far, this behavior is exactly
what most sane people want from their DVCS.

The brokeness comes when you move the file, someone else modifies the
unmoved file then a merge is performed. Mercurial does *not* say this
is a merge conflict! By moving a source file, dependencies might have
changed, namespaces might be modifed to reflect the new location of
the file in the file system, the file may have been deprecated, the
list goes on. By allowing for a seemless merge, Mercurial has hidden
the fact that a major change has happened to the files you have
modified.

Let me belabor the point with this simple example. First, we will
create our test repository and add a single file. 

    ~/hgtest$ mkdir a; hg init a; cd a
    ~/hgtest/a[]$ echo "Hello" > file
    ~/hgtest/a[]$ hg add file
    ~/hgtest/a[]$ hg commit -m "Added file"
    ~/hgtest/a[0]$

Note: I am using the wonderful [hg prompt][4] plugin from [Steve
Losh][5].

Now, we can create another feature branch, where that file is modified. 

    ~/hgtest/a[0]$ hg branch feature
    marked working directory as branch feature
    (branches are permanent and global, did you want a bookmark?)
    ~/hgtest/a[feature:0]$ echo "Hi" > file
    ~/hgtest/a[feature:0]$ hg commit -m "Changed greeting"
    ~/hgtest/a[feature:1]$

We return to the default branch and move the file.

    ~/hgtest/a[feature:1]$ hg up default
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    ~/hgtest/a[0]$ hg mv file file_old
    ~/hgtest/a[0]$ ls
    file_old
    ~/hgtest/a[0]$ hg commit -m "Moved file"

So, now we have two branches, one with the file moved and one with the
file in its original location. The feature branch contains a change to
the original file in the original location. Now, when we go to merge
the branches.

    ~/hgtest/a[2]$ hg merge feature 
    merging file_old and file to file_old
    0 files updated, 1 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    ~/hgtest/a[2]$ hg commit -m "Merge"
    ~/hgtest/a[3]$ ls
    file_old
    ~/hgtest/a[3]$ cat file_old
    Hi
    ~/hgtest/a[3]$


The merge in this scenario seamlessly merges the change from the
feature branch into the moved file in the default branch. This is
possibly a good thing, but it feels very wrong. One developer is
moving a file for possible development in another package or another
program or tool, but he wants to keep the history of the file in
question. Then, another developer, not knowing that this refactoring
is going on, changes the file in place to add some functionality. But,
this functionality is not supported in the new location. When the
merge happens, there is absolutely no warning or error. This type of
behavior should cause a merge conflict.

[1]: http://mercurial.selenic.com/
[2]: http://tinytocs.org/vol1/papers/tinytocs-v1-wuttke.pdf
[3]: http://git-scm.com/
[4]: http://stevelosh.com/projects/hg-prompt/
[5]: http://stevelosh.com/blog/