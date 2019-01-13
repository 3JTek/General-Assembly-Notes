# Git & Github

## What is Git?

Git is version control software which lives on your computer. It comes pre-installed with macOS, but you can also install it with Homebrew.

Programmers use Git so that they can keep the history of all the changes to their code. This means that they can rollback changes (or switch to older versions) as far back in time as they started using Git on their project.

It works by allowing the developer to take snapshots (knows as _commits_) of the codebase. If the developer needs to rollback at any point, she can choose which commit to roll back to. **This is why it is always good to commit early, and commit often!**

Each project on Git is known as a _repository_ or repo for short.

## What is Github

- A hosting service for Git repositories.
- A web interface to explore Git repos.
- A networking tool for developers.
- A place to access public codebase.

Github uses Git on its servers to host and manage Git repositories, however you do not have to use Github if you are using Git on your laptop. Some alternatives include:

- [Bitbucket](bitbucket.io)
- [GitLab](https://gitlab.com/)
- [Beanstalk](http://beanstalkapp.com/)
- [SourceForge](https://sourceforge.net/)
- Any server located anyway (as long as it is correctly configured)

However Github is the most widely used, and is best to use for collaboration and creating publicly available software.

## Working with Git

Git can be a little confusing at first, so the best way to learn it is to get comfortable with the simplest commands first, then go from there.

In order to create a Git repository, simply navigate to a folder and type:

```sh
git init
```

You should see your terminal prompt change to something like this:

```
repo git:(master)
```

Git has created a `.git` folder inside the current directory which it uses to track file changes and so on.

**NEVER create a repo inside another repo.** This can cause problems later when committing code.

If you want to stop Git from tracking changes in to repository, simply remove the .git folder:

```sh
rm -rf .git
```

### File lifecycle

A file in a Git repo can be in one of four states:

- **Untracked**: the file exists, but Git has no snapshot of this file yet, basically it is not being _watched_ or tracked by Git.
- **Staged**: the file is in the _staging area_. A snapshot for this file has not be created yet, but when the next commit is made, this file will be included.
- **Modified**: there is a previous commit of this file, but since then the contents of the file has changed. No commit has been made containing those changes.
- **Unmodified**: there is a previous commit of this file, but the contents of the file has not changed.

![](https://cloud.githubusercontent.com/assets/40461/8226866/62730b4c-159a-11e5-89cd-20b72ed1de45.png)

A file can move between these states like so:

1. When a new file is created, it is **untracked**. Git knows it's there, but we haven't told it that we want it to track changes in that file yet.
1. We add the file to the staging area. It is now **staged**. We are telling Git that when we make our next commit, include that file.
1. We make a commit. A snapshot of the contents of the files in the staging area is taken and stored to memory. Those files are now **unmodified**. They haven't changed, since the last commit.
1. We add some code to the file. It is now **modified**. It's contents are different from the last commit. To take a snapshot of that file we would first stage it, then make a new commit.

### A practical example

Let's look at that again, but with actual Git commands now. We can use `git status` to see the status of our repo at any moment.

1. Create a new file:
  ```sh
  touch index.html
  git status
  ```
  ```
  On branch master

  Initial commit

  Untracked files:
    (use "git add <file>..." to include in what will be committed)

    index.html
  ```
  The file is **untracked**. Git has no previous record of it.

1. Add the file to the staging area:
  ```sh
  git add index.html
  git status
  ```
  ```
  On branch master

  Initial commit

  Changes to be committed:
    (use "git rm --cached <file>..." to unstage)

    new file:   index.html
  ```
  The file is **staged**. It is ready to be committed.
1. Make a commit:
  ```sh
  git commit -m "Adding index.html"
  ```
  ```sh
  [master (root-commit) 1197125] Adding index.html
   1 file changed, 0 insertions(+), 0 deletions(-)
   create mode 100644 index.html
  ```
  The file is now **unmodified** and a record of it at this moment has been recorded.
  The `-m` option stands for **message**. All commits must have an accompanying message.
1. Add the following code to the file:
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>GIT & GITHUB</title>
    </head>
    <body>

    </body>
  </html>
  ```
  ```sh
  git status
  ```
  ```
  On branch master
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   index.html

  no changes added to commit (use "git add" and/or "git commit -a")
  ```
  The file is now **modified**. To take a snapshot of this update we need to repeat steps 2 and 3.

## Rolling back

If you want rollback to a previous commit, you first need to find the commit you want to rollback to:

```
git log
```
```
commit a5cec00
Author: mickyginger <mickyginger@gmail.com>
Date:   Sat Jan 20 14:53:15 2018 +0000

    Updating index.html

commit 77914f2
Author: mickyginger <mickyginger@gmail.com>
Date:   Sat Jan 20 14:52:56 2018 +0000

    Adding index.html
```

The top commit (a5cec00) is the most recent commit. I want to roll back to the previous commit (77914f2). I can do this in two ways. I can do a `soft` reset (which will not lose any changes), or a `hard` reset (which is destructive)

#### Soft reset

```sh
git reset 77914f2 --soft
```
```sh
Unstaged changes after reset:
M  index.html

git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

My last commit has been deleted, but the code has not been reverted. The changes from the previous commit (a5cec00) are still there. index.html is back to the modified state.

#### Hard reset

```sh
git reset 77914f2 --hard
```
```sh
HEAD is now at 77914f2 Adding index
```

Not only has the last commit been deleted, but the code is also reverted back to the state it was at the moment the commit was made. **Beware** you have just lost all the work you did since the last commit!

## Connecting a repo to Github

Once we have a repo set up on our laptops, we can sync it to a repo on Github. Our version is called the _local_ version and the one on Github is the _remote_ version.

1. Navigate to github.com and log in.
1. Create a new repo by clicking the `+` icon in the top right and selecting `New repository`.
1. Name the repo and click `Create repository`.
1. On the next screen copy the link
![](https://user-images.githubusercontent.com/3531085/35184945-e53f526c-fdf3-11e7-8aed-db2bc46d92f4.png)
_It's best to use SSH rather than HTTPS_
1. In the terminal link the remote repo to the local one with the following command:
```sh
git remote add origin git@github.com:mickyginger/example.git
```
1. Now you can now `push` your code to github:
```sh
git push origin master
```
_`origin` is the name of the remote repo, `master` is the default branch. More on branches later._
1. On github, reload the page, you should see your code there.

Now if your laptop dies or you want to collaborate on a project you can do so.

### Cloning

If you want someone else to work on your code, or you want to copy down the code on to a different device, you can do so with `git clone`:

```sh
git clone git@github.com:mickyginger/example.git
```
A new folder will be created with all the code. It is automatically linked to Github already.

### Forking

You can make a copy of any public repo to your Github account by _forking_ it. Simply find a repo that you'd like to work on and click the `Fork` icon:

![](https://user-images.githubusercontent.com/3531085/35185011-005ee0d4-fdf5-11e7-8857-a982486ba4e6.png)

You will be prompted to select an account to fork to:

![](https://user-images.githubusercontent.com/3531085/35185030-63f2608a-fdf5-11e7-821f-6ca80f9d267f.png)

You can now clone this repo to your laptop and use it as if it is your own.

#### Making a pull request

If you make some changes to a fork, and would like to submit those changes to the owner of the original repo, you can do so by making a _pull request_ or _PR_ for short.

1. Navigate the the forked repo **on your Github account**.
1. Click on the `New pull request` button on the left.
1. On the next screen you will see any differences between your repo and the original repo.
1. Click on the `Create pull request` button.
  ![](https://user-images.githubusercontent.com/3531085/35185095-8a304450-fdf6-11e7-9c33-905a70806e4b.png)
1. Add a short description of the code you are submitting and click `Create pull request`.
  ![](https://user-images.githubusercontent.com/3531085/35185113-dce947f0-fdf6-11e7-93ac-c11113473c09.png)

You have just collaborated on an open source project! Sit back and wait for your code to become part of a piece of 3rd party software. (Or get rejected of course).

## Further reading

- [Git Tutorial](https://try.github.io/levels/1/challenges/1)
- [Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [Learn Git in 30 minutes](https://tutorialzine.com/2016/06/learn-git-in-30-minutes)
