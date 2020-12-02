# Git notes

## TODO
There are some improvements that can be done to these notes, but I don't have time for them now. These are:
- [ ] restructure for a cleaner view
- [ ] table of contents at the beginning
- [ ] $ git rebase
- [ ] info on detaching HEAD

---

## Commands

### $ git init
This command transforms the current folder to a git repository. It adds a hidden .git folder, that stores all commits.

---

### $ git clone
This command clones a particular repository (e.g. from GitHub) to the current repository.

---

### $ git status
This command shows the current status of the repository. It shows what files were changed from the last commit, where are already in the staging index or unlisted. It's usefull to run this command every time we start working to remind ourselves of some unfinished work that was left hanging.

---

### $ git log
This command allows you to see history of the current repository. You can navigate with arrows and quit with *q* button. You can also you *--oneline* flag for a short version of the report. Stats can be viewed by adding *--stat* flag in the command line. You can see details of what was changed in the file with *--patch* flag, or, shortened, *-p*.

Another usefull flag is *--oneline*. It only shows the first 7 characters of SHA and commit message.

We can also provide first 7 characters of the SHA of the commit as the last argument for *git log* command to start viewing history from that particular commit. 

Too see commits from all branches in a visual way we can use *git log --oneline --graph --all*.

#### --author
You can specify whose commits you want to see instead of all of them. The syntax is like that: `git log --author=Tester`, where `Tester`'s the name. If there are multiple authors that fall under that search criteria then both of them will be listed. Of course, we can specify it like that: `git log --author="First Tester"`. Notice the quotes.

#### --grep
This flag allows you to filter commits by some words. We can run the command differently, examples:
- `git log --grep bug`
- `git log --grep=bug`
- `git log --grep="bug"`
All of those are right and will work.

#### Tracking branch
When we use git log to view history of our repo, we can see which commits branches point to. And when we use remote repos, we see so-called __tracking branches__. Those are just representations of branches on the remote repos. Usually they are represented by their remote shortname followed by __/__ and then the name of the branch. They are not showing the current state of the branch on the remote repo though, only the state that was recordered when you last checked (for example, when you last pulled the remote repo).

---

### $ git show
This command does the same as *git log* but only show the most recent commit. We can pass first 7 characters of SHA of the particular commit to view it instead of the most recent one.

Also, the flag *-p* is applied by default to this command, so we don't need to pass it unless we use *--stat*. In this case, patch information will not be displayed until we pass *--patch* of *-p* as an argument.

---

### $ git add
This command adds every unlisted before or changed file to the staging index. You can list multiple files separated by spaces to add them all, or you can use *"."* to stage all files in the current folder.

---

### $ git rm
This command allows us to "unstage" files from staging index, as opposed to *git add*'s "staging" process. To do so we need to run *git rm --cached file*, where *file* is the name of the file we need to remove from staging index. It doesn't affect files in the working directory.

---

### $ git commit
This command creates a commit of all the files in the staing index. If we call it as is, it will open git's default code editor to write down the commit message and will show some info about current commit. We can use *-m* flag for a quick naming of the commit.

#### $ git commit --amend

*--amend* flag allows you to edit the last commit you've made. If there are no files in the working directory, you can just run this command and Git will open your default text editor with commit message for you to edit.

Alternatively, you can stage some new files or changes in them in the Staging index and if that's the case, *git commit --amend* will add them to the last commit without creating a new one.

---

### $ git revert
This command allows you to revert changes made in a commit. You just need to tell it SHA of the commit you want to revert and Git will create a new one with all the changes made by a specified commit, but reverted. Lines that were removed will be added, lines that were added will be removed.

---

### $ git reset
This command allows you to delete a commit that you specify with the last parameter. For example, we can run _git reset HEAD~1_ to delete the last commit and move the HEAD with a current branch to a parent of the last commit. What happens to files and changes that are stored in the deleted commit changes based on the flag that we'll use with this command. Let's see different cases:
- __git reset --mixed__ - this will delete the commit and move any files and changes in it to the working directory.
- __git reset --soft__ - this will delete the commit and move any files and changes in it to the staging index.
-- __git reset --hard__ - this will completely delete the commit and it's content.

If you ever need to do reseting on your repository it might be a good idea to create a backup branch by using _git branch backup_ just in case you delete something that you actually need. When you need to get back to the backed up branch, you can just merge it in back to the master or any other branch you'll need to fix.


---

### $ git reflog
This command is used to access data that was deleted from repository, Git stores that data for about 30 days.

---

### $ git config
This command allows you to set different settings according git's work.

#### $ git config --global core.editor "your-editor"
This one allows you to set the default editor that git will address. For example, it'll be used to type a commit message if it's not passed to *git commit* command. You need to specify editor passing it's cmd command as a last parameter (e.g. for VS Code it is "code").

#### $ git config --global user.name "my name"
Allows you to set your name.

#### $ git config --global user.email "myemail@example.com"
Allows you to set you email.

---

### $ git diff
This command allows you to view what changes were made to file since last commit.

To see difference between two particular commits we can use this command like this: _git diff 123123..123124_. 

#### $ git diftool
A graphical tool for viewing changes between commits.

---

### $ git tag
This command allows us to give current commit a tag, for example for versioning. The tag is tied to the last commit that was made for this repository. We can do this with *git tag -a v1.0*, for example. This will open your default editor to provide some information about the tag. We can later view the current (last) tag by running *git tag*. The *-a* flag is there to create an annotated tag, which store more information than a lightweight tag (this one is created by running the above command without *-a* flag).

You can also tag a certain commit by providing it's SHA as the last parameter for the command.

If you happen to make a mistake in the tag, you can delete it by running *git tag -d "tagname"*.


---

## Git Ignore

If we don't want some files in our repository we can tell git to ignore them. This is done with a special file called *.gitignore*. This file must be placed in the root of the repository.

In this file we can write the document's name directly or we can use "globbing", which means creating filters with special characters. These characters are:

- blank lines can be used fro spacing
- **#** - marks line as a comment
- __*__ - matches one or more characters
- **?** - matches 1 character
- **[abc]** - matches a, b, *or* c
- __**__ - matches nested directories

---

## Branching

This section lists all tools needed for work with branches in git.

### $ git branch

This command allows us to manipulate branches. If we run the command as is, it will show the current list of the branches in the repository.

To create a new branch, we just need to run the same command but provide a name for a new branch at the end, like *git branch sidebar*.

By default, when we create a new branch it will be pointed to the last commit of the current branch. But we can create a branch that is pointing to the commit that we want, like so: *git branch alt-sidebar 42a69f*. We just need to provide the SHA to the commit when creating new branch.

Also if we provide an existing branch name as the last parameter we can create a branch based not on the current branch but on a specific one.

#### Delete a branch

If the purpose of the branch is fulfilled, and the changes are merged to the target branch, we might no longer need a specific branch and we will want to delete it. It can be accomplished by running *git branch -d "branchname"*. 

It is important to know that Git won't allow us to delete a current branch, so firstly we need to move to another branch using *git checkout*. Also, Git won't allow branch delete action if we have unique commits in that branch (those that are not already merged to other branches). If we by some reason still need to forcibly delete a branch we can use a capital D flag like *git branch -D sidebar*.

### $ git checkout

This command will allows us to switch what branch HEAD is pointed to (name of the branch is provided as the last argument). This will also remove all the files that git is watching from the working directory and pull the files that are stored in the last commit of the branch that we are switching to.

This command also can create branches if we provide it with the *-b* tag. The difference between this case and *git branch* one is that if we use *git checkout* we will automaticaly be switched to the new branch.

#### Reverting changes of a file
We can use _git checkout_ to revert a file to a state that is was in at some point, we just need to know commit's SHA. We can use it like this: _git checkout 123123 hello.txt_.

### $ git rebase
This command allows you to squash several commits into one. You just need to select the commit you want to squash to and follow the instructions.

#### -i
This flag allows interactive work with rebase command.


## Merging

### $ git merge

We use this command to merge branches.

Merging is a process of adding changes from one branch to another. There are two types of merging: a regular **merge** and a **fast-forward merge**. Regual merge happens when you merge a branch that has some unique commits to another branch that has another unique commits. When you merge a branch **A**, that has some commits that are ahead of **B** branch, but also has all commits from **B** branch, to a **B** branch, this is called **fast-forward merge**.

Also, making a merge create a new commit on the current branch.

Verbally, when we say "I want to merge master branch in footer branch", this means that *footer* branch has some commits that *master* branch doesn't. And this merge means that *master* will have those changes.

### Merge conflicts

Merge conflicts usually occur, when the exact same lines in files ara changed in different branches. After that, when merging, Git doesn't know which changes to save. When these conflicts happen, Git will try and merge everything that is possible to merge, and then leave some marks to let programmer know what caused a conflict. We just need to find those conflicts (a simple search for "<<<" should be enough), edit them, than stage and commit it like a usual merge.

## Relative Commit References

Sometimes we might need to reference a commit without using it's SHA or other common sources. In these cases we can use special characters (those are called "Ancestry References").

- __^__ - indicates the parent commit
- __~N__ - indicates the __N__th parent commit

So the parent commit can be access in following ways:
- HEAD^
- HEAD~
- HEAD~1

The grandparent commit can be accessed by:
- HEAD^^
- HEAD~2

The great-grandparent commit:
- HEAD^^^
- HEAD~3

When a commit is created via branch merge, it has 2 parents. __^__ is used to indicate the first parent, while __^2__ indicates the second parent. The first parent is the branch you were on when you ran _git merge_ while the second parent is the branch that was mergen in.

## Remote work with Git
This section will provide information anything that is needed for a remote work with git repository.

### $ git clone
This command will create a local copy of the provided repository in the current folder. 

### $ git fetch
This command will download all current commits from the remote to your remote branch.

### $ git pull
This command will do the same as the _git fetch_ command but will also incorporate all changes to your local repository. You can provide the shortname for a particular remote repo and the branch you want to pull. Under the hood this command will _fetch_ commits to your __tracking branch__ and the merge you __local branch__ with __tracking branch__.

These is a scenario when you have unpushed commits on your branches, and there are unpulled commits on the remote repo. In this case, _git pull_ won't run. What can you do instead, you can _git fetch_ those changes from remote repo, look though them, and if everything's fine you can just merge you __local branch__(that is up to date with your local changes) to the __tracking branch__(that is up to date with changes in the remote repo) and then you can push the new commit to the remote.

### $ git push
This command works almost exactly like the _git pull_ one, but backwards. It will send your commits to your remote repository. And you can provide the shortname for remote repo as well as the name of the remote branch.

#### -f
Sometimes Git will reject your attempt to push commits. For example, after rebasing, if the result of the push would be a loss of some commits. You need to carefully read through all error messages and tips git gives you, and if necessary you can _push forcefully_ with `git push -f`.

### git cherry-pick
This command allows you to copy several commits below you current location.

### $ git remote
Allows you to work with remote repositories. Call without parameters will list all remotes for the current repository. 

#### -v
If you'll run `git remote -v`, the command will return full paths to the remote repositories.

#### add
With _git remote_ you can add new remote repository for your current one. You can run it like this: `git remote add remotename remoteurl`. You'll need to specifiy the _shortname_ for your remote so you can access it with ease. Followed by the remote repo url.

#### rename
This parameter allows us to rename remotes. Like this: `git remote rename old-name new-name`.

### $ git shortlog
This command will list all users that had created commits to the current repository and will show the names of their commits. It will sort them by name alphabetically.

#### -s
If you run the command with this flag it will only show the number of commits everyone's made.

#### -n
If you run the command with this flag it will sort the list by the number of commits everyone's made.

## Forking
When someone says `to fork` it means `to create an identical copy of a repo`. We use works if we want to work on someone else's remote repositories. Forks are not a part of Git but a part of remote repo hosting services like GitHub. If we want to make or propose changes on someone else's remote repository we can't just clone it, make changes, commit and push them. We are not the owner of that repo. The answer to this problem is _Forking_.

When we press _Fork_ button on GitHub's web interface, GitHub creates an identical copy of that repository on our account. Now, we are the owner of that copy and have all right's to change it how we want. We now can make some commits and create a __pull request__ to the original repo, so the owner of it can consider incorporating our changes to the original. 

### Pull request
Pull request is a request to merge some commits from another repo. It works like this:
1. You fork an existing repository which you want to change.
2. You clone it to your local machine.
3. You make some changes and commit them.
4. You push those changes to the fork you've created earlier.
5. Now you create a `Pull Request` via GitHub interface.
6. If the `Pull Request` is accepted, your commits are now appear in the original repository and GitHub creates a new commit to merge these new commits with existing branch.

### Upstream
When we create a fork of the repo to work on it, while we work there might be some commits in the original repository over time. So our fork and cloned lcoal repos become out of date. When we clone a repo, git creates a connection that is named _origin_. We can manually create a connection to the original repo. This connection typically is called __upstream__. We then can fetch commits from the original repo. We'll have a __tracking branch__ called __upstream/master__. We can now merge our local master to this fetched branch. Since we are making our changes on a separate branch, our local master will __fast-forward__ to the __upstream/master__.