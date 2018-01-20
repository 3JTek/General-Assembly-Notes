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

Let's look at that again, but with actual Git commands now:

1. Create a new file:
  ```
  touch index.html
  ```
  The file is **untracked**
1. Add the file to the staging area:
  ```
  git add index.html
  ```
  The file is **staged**
1. Make a commit:
  ```
  git commit -m "Adding index.html"
  ```
  The file is now **unmodified** and a record of it at this moment has been recorded.
  The `-m` option stands for **message**. All commits must have an accompanying message.
1. Add the following code to the file:
  ```
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
  The file is now **modified**. To take a snapshot of this update we need to repeat steps 2 and 3.
