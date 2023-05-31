---
id: 474
title: 'svn and git notes'
date: '2016-11-12T21:08:52+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2016/11/182-revision-v1/'
permalink: '/?p=474'
---

SVN  
Create a branch:

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">export url=$(svn info | grep URL | awk '{print $2}') #get url
echo $url
export newurl=$url/../branches/path
svn mkdir --parents $newurl -m 'mkdir of branch'
svn copy $url  $newurl -m 'create branch' # add "-r466" option to branch from specific branch
cd your/local/branch/path
svn co $newurl

# remove branch
svn delete http://svn.example.com/repos/calc/branches/my-calc-branch -m "Removing obsolete branch of calc project."


```

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">svn log # list all commit records
svn diff -r 455:462 # diff two versions
```

- - - - - -

Git

[Tutorial](https://www.atlassian.com/git/tutorials/)

- - - - - -

[Git flow:](http://blog.osteele.com/posts/2008/05/my-git-workflow/)  
[![git-flow](http://www.pittnuts.com/wp-content/uploads/2015/09/git-flow-300x284.png)](http://www.pittnuts.com/wp-content/uploads/2015/09/git-flow.png)

- - - - - -

[Git Branching – Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)  
[Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell"># https://help.github.com/articles/merging-an-upstream-repository-into-your-fork/

# Merging an upstream repository into your fork
git checkout master # Check out the branch you wish to merge to
git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git BRANCH_NAME # This will pull remote files to local and may conflicts with your modification [ See how to solve conflicts: https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/]
git merge other-branch # Instead of remote filtes, this will merge the other branch in the same repo to current branch 

# Use tool to merge conflict
sudo apt-get install kdiff3
git mergetool --tool=kdiff3

git status; git commit -m 'merge' # Check and commit the merge
git remote -v # check what is origin URL
git push origin master # Push the merge to your GitHub repository.

# diff working copy and a branch
git diff --name-only master . > diff_files.txt
#!/bin/bash
files=$(cat diff_files.txt)
for file in $files; do
 git diff master:$file $file >> master_scnn.diff
done

git reset HEAD <file> # unstage a file


# track other's (e.g. wenwei202) fork
git remote add wenwei202 https://github.com/wenwei202/caffe.git
git fetch wenwei202 
git checkout --track wenwei202/sfm 
git branch -b sfm wenwei202/sfm
git checkout -b sfm wenwei202/sfm 
git push -u origin sfm 
git pull wenwei202 sfm # merge other's commits
```

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">
```

- - - - - -

[Git diff](http://stackoverflow.com/questions/1587846/how-do-i-show-the-changes-which-have-been-staged):  
[![git-diff](http://www.pittnuts.com/wp-content/uploads/2015/09/git-diff.png)](http://www.pittnuts.com/wp-content/uploads/2015/09/git-diff.png)