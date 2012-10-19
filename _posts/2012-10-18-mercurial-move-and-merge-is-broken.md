---
layout: post
title: Mercurial Move and Merge is Broken
---

<p class="meta">{{ post.date | date_to_long_string }}</p>

{% highlight bash %}
~/hgtest$ mkdir a
~/hgtest$ hg init a
~/hgtest$ cd a
~/hgtest/a[]$ echo "Hello" > file
~/hgtest/a[]$ hg add file
~/hgtest/a[]$ hg commit -m "Added file"
~/hgtest/a[0]$ hg branch feature
marked working directory as branch feature
(branches are permanent and global, did you want a bookmark?)
~/hgtest/a[feature:0]$ echo "Hi" > file
~/hgtest/a[feature:0]$ hg commit -m "Changed greeting"
~/hgtest/a[feature:1]$ hg up default
1 files updated, 0 files merged, 0 files removed, 0 files unresolved
~/hgtest/a[0]$ hg mv file file_old
~/hgtest/a[0]$ ls
file_old
~/hgtest/a[0]$ hg commit -m "Moved file"
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
{% endhighlight %}


The merge in this scenario seamlessly merges the change in the feature
branch with the moved file in the default branch. This is possibly a
good thing, but it feels very wrong and leads to bad merging
results. One developer is moving a file for possible development in
another package or another program or tool, but he wants to keep the
history of the file in question. Then, another developer, not knowing
that this refactoring is going on, changes the file in place to add
some functionality. But, this functionality is not supported in the
new location. When the merge happens, there is absolutely no warning
or error. This type of behavior should cause a merge behavior.

