# Intro to Git for Writers

These instructions are for writers who want to use Git to contribute to Rancher's blog.

**Prerequisites:**
To make changes to the `rancherlabs/website` GitHub repository, you have to be a member of the organization.

## Terminology

- **Repo:** Repository. You'll work with three repositories: the official Rancher blog repo at `github.com/rancherlabs/website`, your fork of that repo, which will be at `github.com/[your GitHub username]/website`, and your local repo on your computer. All three are versions of the same project.
- **Branch:** A series of commits in Git. For the purpose of these instructions, you'll work with the master branch and a feature branch. The feature branch will diverge from master, then merge back into master.
- **Fork:** A copy of a GitHub repository. When you go to `https://github.com/rancherlabs/website`, you'll see the **Fork** button in the upper right corner. When you click it, GitHub creates a copy of this project on your GitHub profile. After the "fork" takes place, the two copies can be updated independently of each other.
- **Origin:** Your copy of the Rancher blog repo, which is on your GitHub profile. If you make changes for someone else to review, you push to origin first, then make a pull request so that the changes on origin can be made to the official repo.
- **Clone:** Your local copy of a project on GitHub. Also a verb meaning to download a repo and initialize Git. When you clone a repo, Git assumes that the repo you cloned it from is the "origin" of the cloned project. That means that if you run `git push` in this project without specifying a branch or repo, Git assumes your changes should be made to origin. The `git clone` command should be used only once, when setting up your local copy.
- **Upstream:** Rancher's official blog repo. If you have merge access to make changes to this repo, you can push changes directly to upstream without pushing to origin first.
- **Remote:** This word describes repositories on GitHub that your local project is connected to. You'll work with two remote repos: upstream and origin. The names "upstream" and "origin" are configurable; you can call the Rancher blog repo "upstream" or "rancherRepo" or anything you want. Git does not know where to upload your changes if you have not configured any remote repos. To see what remote repos you have configured in your local project, run `git remote -v`. You can add and remove these remote repos.
- **Push:** To "push your changes" means to upload your changes to a GitHub repository. Whenever you run the `git push` command, you are updating a specific branch on a remote repository. If you don't specify which branch, Git assumes the changes will be made to the master branch. If you don't specify which repo, Git assumes the changes will be made to the origin repo.
- **Pull:** Pulling is used to incorporate changes from one branch into another. You can also use this command to incorporate changes from one branch on a remote repo into a branch on your local repo.
- **Commit:** A named version of a Git repo. If you look at the commits in a GitHub repository, the names of the commits should be clear enough that you can understand what kind of changes were made in each version. This is also a verb, as "commit your changes" means to save your changes as a named version.
- **Pull request:** A pull request allows a reviewer to see the exact differences between two Git branches before incorporating changes from one branch into another. One branch can be on the original repository, while the other branch can be on your fork. 

## One-time Setup

These steps should be done once, just to set up your workspace.

### 1. Install Git.

1. Install Git. Instructions are here: https://www.atlassian.com/git/tutorials/install-git
2. Open the terminal and verify that Git is installed:
  ```
  $ git version
  git version 2.15.0
  ```

### 2. Fork the Rancher blog repository.

1. Go to `https://github.com/rancherlabs/website`.
2. Click the **Fork** button in the upper right corner.

**Result:** You have copied the Rancher blog repo to your GitHub profile. It's available at `github.com/[your GitHub username]/website`.

### 3. Clone your fork of the Rancher blog repository.

1. Go to your fork at `github.com/[your GitHub username]/website`.
2. Click the **Clone or download** button on the right side.
3. Copy the link that appears.
4. Open the terminal in that new folder. To do this on a Mac, click the magnifying glass in the upper right corner of the screen, type **Terminal,** and press **Enter.** In the terminal, type:
  ```
  cd [path to the folder where you want the project to be]
  ```

  For example, if I wanted my local copy of the repo to be on my desktop, I would use:
  ```
  cd /Users/catherineluse/Desktop/
  ```

5. Clone your fork of the blog repo:
  ```
  git clone https://github.com/[your GitHub username]/website.git
  ```

6. This command has created a new folder named `website` (because the official blog repo is named `website`). It contains all the files in the repository. You can see the new files by running these  commands (these are not Git commands, but are built into the Mac):
  ```
  cd website
  ```
  This command lets you access the new folder, which is named `website`, from the terminal. Then run:
  ```
  ls
  ```
  You should see all the Rancher blog files listed in the terminal, confirming a successful download.
  
7. When you ran `git clone`, Git automatically initialized this directory as a Git repository. That means that you can now run Git commands in the terminal. Try getting the Git status:
  ```
  $ git status
  On branch master
  Your branch is up to date with 'origin/master'.
  nothing to commit, working tree clean
  ```
  This means that a) Git has been initialized in this folder, b) Git is set up so that it can connect to a remote repository named origin. By default, this "origin" is your fork on GitHub.com, because that's where you cloned it from. The origin repo is where Git assumes (by default) that you will want to upload your changes to.

8. To see what your origin repository is, run:
  ```
  $ git remote -v
  origin	https://github.com/catherineluse/website.git
  origin	https://github.com/catherineluse/website.git
  ```

  This means that if I run `git push`, without specifying a repo or branch, Git will try to make my local changes to this "origin" repo. Note: The origin repository is configurable. It's possible to change the name "origin," or the repository that it points to. You can add and remove remote repositories with no limits.

### 4. Set up the upstream repository.

1. From your local repo, run:
  ```
  git remote add upstream https://github.com/rancherlabs/website.git
  ```

2. This should have configured the official Rancher blog repo as a remote repo named "upstream." That means that you now have the option to run Git commands to make changes directly to the official blog repo, in addition to your origin repo. To confirm that your remote repositories are configured properly, run:
  ```
  $ git remote -v
  origin	https://github.com/catherineluse/website.git (fetch)
  origin	https://github.com/catherineluse/website.git(push)
  upstream	https://github.com/rancherlabs/website.git (fetch)
  upstream	https://github.com/rancherlabs/website.git (push
  ```

**Result:** You now have two remote repositories: upstream and origin. That means that hypothetically, if you made commits and then ran `git push upstream master`, Git would know you want to make changes to the master branch of the official Rancher blog. Likewise, if you made commits and ran `git push origin master`, Git will know you want to update the master branch on your fork. Note: Git doesn't allow you to push if you don't have any commits yet.

This concludes the one-time setup of your workspace with Git.

## How to Make Changes that Someone Will Review

This section describes how to make a pull request so that your changes can be reviewed before they are incorporated into the official Rancher blog repo.

### 1. Confirm that your terminal is in the right folder.

Run this command:
```
pwd
```

If the path to your local repo is displayed, great. If not, run:
```
cd [path to your project]
```

### 2. Update your local copy of master.
Check out the master branch:
```
git checkout master
```

Then make sure your local master branch is synchronized with the upstream repo's master branch. This is important because you're going to eventually incorporate your changes into this branch, and it may have been updated by someone else, so ideally you should work from the most recent version.
```
git pull -r upstream master
```

### 3. Create a feature branch based on master.
To keep your set of changes isolated from unrelated changes, it's generally a good practice to work on a feature branch most of the time.

Assuming you currently have the master branch checked out, run:
```
git checkout -b [feature branch name]
```

This creates a feature branch that starts out identical to master. You will then make changes to this feature branch.

### 4. Make a commit on your feature branch.

1. Now you can add, remove, or change files in your local copy of the Rancher blog. Blog posts go in this folder: `[your-project-folder]/website/content/blog/[current year]`. Images go in this folder: `[your-project-folder]/website/static/img/blog/[current year]`. Author bios go under `website/data/authors`. A script has been provided to help you more quickly create a new file for the blog post in the proper folder. To use this script, run:
  ```
  ./scripts/new-post My Super Awesome Blog Post Title
  ```

2. From the terminal in your local project directory, run:
  ```
  git add .
  ```

  This tells Git that all the changed files in your local repo should be included in your next commit.

3. Then create a commit by running:
  ```
  git commit -m "Added a blog post"
  ```

  The part in quotation marks is the commit message. It needs to be a descriptive, yet concise summary of your change. Commit messages allow people to read the revision history of a GitHub repository.

### 5. If possible, build the site locally to view your changes in a web browser.

Seeing the website with your changes in it can help you make sure that your changes are what you intend.

[Detail to be added later. You should be able to run `./scripts/dev` and view the changes in Chrome or Firefox at `localhost:9001`, but when I tried this, I got errors.]

### 6. Push the commit to your origin repo.

Run this command:
```
git push --set-upstream origin [feature branch name]
```
**Result:** Your fork should have been updated.

Optionally, to verify that it was updated:

1. Go to your fork at `https://github.com/[your GitHub username]/website`.
2. Click **Branch: master** and click your feature branch name. Then, if you go to a file that you worked on, you should see that your changes have been incorporated.

### 7. Create a pull request.

1. Go to `https://github.com/rancherlabs/website`.
2. Click **New pull request.**
3. Click the **Compare across forks** link.
4. Set the **base repository** to `rancher/docs`, and the **base** to **master.** This means you intend to make changes to the master branch. Then set the **head repository** to `[your GitHub username]/website`, and set **compare** to your feature branch.
5. You will see your changes with red showing deletions and green showing additions. Review these changes to make sure it's what you intended. If you see a bunch of changes that you didn't make, go back and make sure that a) you're making changes to the right branch and b) you created your feature branch based on master.
6. If everything looks good, click **Create pull request.**

**Result:** The repository maintainers can review and give feedback on your changes. If they leave comments on your pull request, you can make additional commits and push them to your origin repo. Then those changes will be also be reflected in the pull request.

## How to push changes directly to the upstream repository, with no one reviewing the changes

This workflow is easier because there's no pull request, and you're making changes directly to Rancher's repo instead of making changes to your fork.

**Prerequisites:** 
Must have merge access to Rancher's git repo.

1. Go through steps 1-5 above to create a feature branch and make a commit with your changes.

2. Run:
  ```
  git push upstream master
  ```
  Warning: This updates the master directly with no one reviewing it. To update the official blog repo without affecting the live website, you could push to a different branch than master.

**Result:** Your changes should appear in the live website.