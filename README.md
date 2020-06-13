# Git Demo

* Let's create a new directory to initialize git

``` bash
$ mkdir gitDemo
$ cd gitDemo
```

* Initializing empty Git repository

```sh
$ git init
Initialized empty Git repository in /Users/tanul/Library/Mobile Documents/com~apple~CloudDocs/Github/gitDemo/.git/
$ ls -a
.	..	.git
```

* Let's look at the folder contents now. All the Git data will be stored in
.git folder.

```sh
$ ls .git
HEAD		description	info		refs
config		hooks		objects
```

* checking the current status of git

``` sh
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)

$ echo 'Hello world!' >> hello.txt
$ cat hello.txt
Hello World
```

* checking the status of git after changing the directory content

``` sh
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello.txt

nothing added to commit but untracked files present (use "git add" to track)
```

* **Staging**

```sh
$ git add hello.txt
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   hello.txt
```

* **Commit**

```sh
$ git commit -m "Hello file added."
[master (root-commit) 37052ff] Hello file added
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

* Let's see the git log. This is going to be the most used command
as it will give you the entire history of the git commits along with all the branches.

```sh
$ git log --all --graph --decorate --oneline
* 37052ff (HEAD -> master) Hello file added
$ git log --all --graph --decorate
* commit 37052ff89fced9f3d838b7e332dff08cf0d53939 (HEAD -> master)
  Author: Tanul <tanulgupta123@gmail.com>
  Date:   Sat Jun 13 00:42:10 2020 +0200
```

* Changing some of the contents of the __hello.txt__ file and exploring how git recognizes the difference between two files.

```sh
$ echo 'Bye Bye world!' >> hello.txt

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git diff
diff --git a/hello.txt b/hello.txt
index 557db03..7f03b50 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 Hello World
+Bye Bye world!

$ git add .
$ git commit -m 'Hello file updated to include Bye Bye!'
[master 2d52482] Hello file updated to include Bye Bye
 1 file changed, 1 insertion(+)

 $ git log --all --decorate --graph --oneline
* 2d52482 (HEAD -> master) Hello file updated to include Bye Bye
* 37052ff Hello file added

$ git diff
```

* Exploring git diff in details

```sh
$ git diff <Hash_1> <Hash_2> <filename>
$ git diff 37052f 2d52482
diff --git a/hello.txt b/hello.txt
index 557db03..7f03b50 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 Hello World
+Bye Bye world!
```

* Can I delete a commit? Yes, you can. But generally it is not advised to do so.

```sh
$ echo 'Ram Ram!' >> hello.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git add .
$ git commit -m "Ram Ram"

$ git reset --hard 2d524824
HEAD is now at 2d52482 Hello file updated to include Bye Bye
```

* Let's go deeper, and talk about __branches__ and __resolving merge conflicts.__

```sh
$ touch transport.py
```

``` python
# Add following content to transport.py
import sys

def default():
	print("I just try to move.")

if __name__ == '__main__':
	if sys.argv[1] == "train":
		print("I ply on railway tracks")
	else:
		default()
```

```sh
$ git add transport.py
$ git commit -m "Created python file transport.py"
[master 97ba455] Created python file transport.py
 1 file changed, 12 insertions(+)
 create mode 100644 transport.py

$ git branch --v
* master 97ba455 Created python file transport.py

# Let's create a new branch
$ git branch airways
$ git log --all --decorate --graph --oneline
* 97ba455 (HEAD -> master, airways) Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added

$ git checkout airways
```
```python
# Change the content of transport.py
import sys

def default():
	print("I just try to move.")

if __name__ == '__main__':
	if sys.argv[1] == "train":
		print("I ply on railway tracks")
	elif sys.argv[1] == "IC814":
		print("I was hijacked by the terrorists in the 90s."
	else:
		default()
```
```sh
$ git add transport.py
$ git commit -m "added info regarding IC814."
$ git log --all --decorate --graph --oneline
* 9874674 (HEAD -> airways) added info regarding IC814
* 97ba455 (master) Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added

# Let's go back to master and create another branch
$ git checkout master
$ git checkout -b waterways
```
``` python
# Change the content of tranport.py to the following.
import sys

def default():
	print("I just try to move.")

if __name__ == '__main__':
	if sys.argv[1] == "train":
		print("I ply on railway tracks")
	elif sys.argv[1] == "Titanic":
		print("An ice berg sank me.")
	else:
		default()
```
``` sh
$ git add transport.py
$ git commit -m "Added Titanic"

# Let's see the branching.
$ git log --all --decorate --graph --oneline
* 0cffd79 (HEAD -> waterways) Added Titanic
| * 9874674 (airways) added info regarding IC814
|/
* 97ba455 (master) Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added
```

* __Merging branches__ and __Resolving conflicts__.

```sh
$ git checkout master
$ git merge airways
Updating 97ba455..9874674
Fast-forward
 transport.py | 2 ++
 1 file changed, 2 insertions(+)
$ git merge waterways
Auto-merging transport.py
CONFLICT (content): Merge conflict in transport.py
Automatic merge failed; fix conflicts and then commit the result.

# After resolving the merge conflict in text editor of your choice.
$ git merge --continue
$ git log --all --decorate --graph --oneline
*   6bd4b31 (HEAD -> master) Merge branch 'waterways'
|\
| * 0cffd79 (waterways) Added Titanic
* | 9874674 (airways) added info regarding IC814
|/
* 97ba455 Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added
```

* Let's talk about __remote__.
	* Remote in Pendrive
	* Remote at Github

```sh
# Pendrive remote
$ mkdir ../remoteGit
$ cd ../remoteGit
$ git init --bare
$ git remote add pendrive ../remoteGit

# git push <remote name> <local branch>:<remote branch>
$ git push pendrive master:master
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Delta compression using up to 4 threads
Compressing objects: 100% (14/14), done.
Writing objects: 100% (18/18), 1.70 KiB | 581.00 KiB/s, done.
Total 18 (delta 4), reused 0 (delta 0)
To ../remoteGit
 * [new branch]      master -> master

$ git log --decorate --all --graph --oneline
*   6bd4b31 (HEAD -> master, pendrive/master) Merge branch 'waterways'
|\
| * 0cffd79 (waterways) Added Titanic
* | 9874674 (airways) added info regarding IC814
|/
* 97ba455 Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added

# Let's try and clone this and see what we get.
$ mkdir ../temp
$ cd ..
$ git clone remoteGit temp
Cloning into 'temp'...
done.
$ git log --all --decorate --graph --oneline
*   6bd4b31 (HEAD -> master, origin/master, origin/HEAD) Merge branch 'waterways'
|\
| * 0cffd79 Added Titanic
* | 9874674 added info regarding IC814
|/
* 97ba455 Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added

# [MACHINE 1] Let's make some changes to the original repo
$ echo 'Adding extra line' >> hello.txt
$ git add hello.txt
$ git commit -m "One extra line"
[master d3c6bfa] one extra line
 1 file changed, 1 insertion(+)
$ git log --all --decorate --oneline --graph
 * d3c6bfa (HEAD -> master) one extra line
 *   6bd4b31 (pendrive/master) Merge branch 'waterways'
 |\
 | * 0cffd79 (waterways) Added Titanic
 * | 9874674 (airways) added info regarding IC814
 |/
 * 97ba455 Created python file transport.py
 * 2d52482 Hello file updated to include Bye Bye
 * 37052ff Hello file added

$ git branch --set-upstream-to=pendrive/master
 Branch 'master' set up to track remote branch 'master' from 'pendrive'.
$ git push
 Enumerating objects: 5, done.
 Counting objects: 100% (5/5), done.
 Delta compression using up to 4 threads
 Compressing objects: 100% (2/2), done.
 Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
 Total 3 (delta 0), reused 0 (delta 0)
 To ../remoteGit
    6bd4b31..d3c6bfa  master -> master
# [MACHINE 2]
$ cd ../temp
$ git fetch
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From /Users/tanul/Library/Mobile Documents/com~apple~CloudDocs/Github/remoteGit
   6bd4b31..d3c6bfa  master     -> origin/master

$ git log --all --decorate --graph --oneline
* d3c6bfa (origin/master, origin/HEAD) one extra line
*   6bd4b31 (HEAD -> master) Merge branch 'waterways'
|\
| * 0cffd79 Added Titanic
* | 9874674 added info regarding IC814
|/
* 97ba455 Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added

$ git merge
$ git log --all --decorate --graph --oneline
* d3c6bfa (HEAD -> master, origin/master, origin/HEAD) one extra line
*   6bd4b31 Merge branch 'waterways'
|\
| * 0cffd79 Added Titanic
* | 9874674 added info regarding IC814
|/
* 97ba455 Created python file transport.py
* 2d52482 Hello file updated to include Bye Bye
* 37052ff Hello file added
# One could have used git pull here.
```
* Setting up **Github remote**
	* Create online repository on git
	* Add that as remote to your current git directory

```sh
$ git remote add github https://github.com/galvanixer/gitDemo.git
$ git push -u github master
```
