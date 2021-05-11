git_gerrit_command
-------------------
Revert to previous changes:
git reset --hard HEAD~1
git push -f origin master (option -f ==> force)

Note:
$ git log --oneline

3fad532  Last commit   (HEAD)
3bnaj03  Commit before HEAD   (HEAD~1)
vcn3ed5  Two commits before HEAD   (HEAD~2)

reset to jsut one commit
---------------------------
git reset --hard HEAD~1

reset to jsut second commit
---------------------------
git reset --hard HEAD~2

 
 “git reset” with the “–soft” option in order to undo the last commit and perform additional modifications.
 $ git reset --soft HEAD~1
 
 In order to undo the last commit and discard all changes in the working directory and index, execute the “git reset” command with the “–hard” option and specify the commit before HEAD (“HEAD~1”).
 
  git reset --hard HEAD~1
  
  Details
  -----------
  
  reset have 5 mode
  1. soft
  2. mixed
  3. hard
  4. merge
  5. keep
  
SOFT
--------
When using git reset --soft HEAD~1 you will remove the last commit from the current branch, but the file changes will stay in your working tree. Also the changes will stay on your index, so following with a git commit will create a commit with the exact same changes as the commit you "removed" before.

How would this look like in practice? Like this:

> git reset --soft HEAD^ # Assuming HEAD points at 7e05a95
> git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    modified:   a
As you see the changes in file a are on the index, and ready to be committed again.
  
MIXED
----------

This is the default mode and quite similar to soft. When "removing" a commit with git reset HEAD~1 you will still keep the changes in your working tree but not on the index; so if you want to "redo" the commit, you will have to add the changes (git add) before commiting.

In practice the result might look like this:

> git reset --mixed HEAD^ # Assuming HEAD points at 7e05a95
> git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   a

no changes added to commit (use "git add" and/or "git commit -a")
The changes of file a are still there but they're not on the index.

HARD
--------
When using git reset --hard HEAD~1 you will lose all uncommited changes in addition to the changes introduced in the last commit. The changes won't stay in your working tree so doing a git status command will tell you that you don't have any changes in your repository.

Tread carefully with this one. If you accidentally remove uncommited changes which were never tracked by git (speak: committed or at least added to the index), you have no way of getting them back using git.

A practical example might look like this:

> git reset --hard HEAD^ # Assuming HEAD points at 7e05a95
> git status
On branch main
nothing to commit, working tree clean
As you can see, no changes remain. Assuming you also had some uncommitted changes in the file b these would be lost too!

> echo 'some uncommitted changes' > b
> git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   b

no changes added to commit (use "git add" and/or "git commit -a")

> git reset --hard HEAD^ # Assuming HEAD points at 7e05a95
> git status
On branch main
nothing to commit, working tree clean

KEEP
-----
git reset --keep HEAD~1 is an interesting and useful one. It only resets the files which are different between the current HEAD and the given commit. It aborts the reset if one or more of these files has uncommited changes. It basically acts as a safer version of hard.

Let's revisit the example from before, where you had some uncommitted changes in b:

> echo 'some uncommitted changes' > b
> git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   b

no changes added to commit (use "git add" and/or "git commit -a")

> git reset --keep HEAD^ # Assuming HEAD points at 7e05a95
> git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   b

no changes added to commit (use "git add" and/or "git commit -a")
You removed the changes in file a but retained the uncommitted changes in file b!

Note : So to reiterate: "hard" will remove all changes while "keep" only removes changes from the reset commit(s).

When doing git reset to remove a commit the commit isn't really lost, there just is no reference pointing to it or any of it's children. You can still recover a commit which was "deleted" with git reset by finding it's SHA-1 key, for example with a command such as git reflog.

--------------------------------------------------------------------------------------------------------------
bcompare integration with
---------------------------
Serach bcompare in machine
 find / -name "bc*"

DEBIAN, UBUNTU:
wget https://www.scootersoftware.com/bcompare-4.3.7.25118_amd64.deb

sudo apt-get update

sudo apt-get install gdebi-core

sudo gdebi bcompare-4.3.7.25118_amd64.deb


Terminal Uninstall
sudo apt-get remove bcompare

REDHAT ENTERPRISE LINUX, FEDORA, CENTOS:

Graphical Install
->Double click on the .rpm package to install using the graphical package manager.
->wget https://www.scootersoftware.com/bcompare-4.3.7.25118.x86_64.rpm
->su
->rpm --import https://www.scootersoftware.com/RPM-GPG-KEY-scootersoftware
->yum install bcompare-4.3.7.25118.x86_64.rpm

Terminal Uninstall
->su
->yum remove bcompare

OPENSUSE
su
rpm --import https://www.scootersoftware.com/RPM-GPG-KEY-scootersoftware
zypper refresh
zypper install https://www.scootersoftware.com/bcompare-4.3.7.25118.x86_64.rpm

Terminal Uninstall
su
zypper remove bcompare


