---
title: Version Control with Git, GitHub, and RStudio
author: Justin Millar
date: '2019-11-10'
slug: version-control-git
categories: []
tags:
  - git
  - github
  - rstudio
  - reproducibility
type: ''
subtitle: 'Science is collaborative!'
image: 'https://cdn2.iconfinder.com/data/icons/social-aquiocons/512/Aquicon-Github.png'
---

## Some Background

Science is collaborative. Even a "solo" projects should be reproducible, meaning we should be thinking about how other researchers can benefit from our work. And we will always have at least one collaborator - **our future selves**. Each of us has at some point struggled to pick up a project after weeks or months (or even years) of inactivity.

![](http://www.phdcomics.com/comics/archive/phd101212s.gif)

Science is collaborative, and collaboration is hard. Often we aren't trained to collaborate effectively and may be unaware of tools and practices to improve our collaborations. Most of us have had similar experiences to the comic above when writing a manuscript with coauthors, however the stakes are even higher when collaborating on source code, such as R scripts. It is easy to introduce bugs which can break your code, which is why software programmers often use tools like Git and why these tools have more recently been adopted by academic researchers and data scientists. 

### What is Git and why should I bother?

Git is an open source program used for [version control]( https://en.wikipedia.org/wiki/Version_control), originally used to help developers collaborate on large software projects. You can also think of Git (or git with a lowercase "g", these are the same) as Microsoft Word's Track Changes feature ["on steriods"]( https://happygitwithr.com/big-picture.html ). 

Git is a cross-platform (meaning is works on Windows, Mac, and Linux) program typically operated from the command line, and provides many, many tools for collaboration on software (which can be extended to other types of files).  Once you install Git, you can go to a folder (also called a repository) on your computer and initialize Git, which can then track how all of the files in that folder (and any subfolder) change over time. 

Fair warning, learning Git is difficult and frustrating. Adding Git into your daily workflow will seem strange at first and will probably cause you some headaches. Thankfully you will typically only need a handful of the Git commands to do most of your versioning!

### The standard Git workflow

![](https://i.stack.imgur.com/UvZ0M.png)

The diagram above illustrates how Git operates on your local computer. Once we initialize Git within a folder it will track our progression as we make changes and commit them. Some of these terms may sound strange but will become familiar over time. This section introduces the Git tools for using this workflow, but note that we'll be using RStudio to do all of this. 

Focus on the **Local** part of the flowchart. As you work on your code, when you reach a save point (maybe you just added a new feature or fixed a bug) you will add your progress to the "staging area" using `git add`. Once we've added these changes to the staging area and are satisfied with them, we can commit the changes using `git commit`. Typically we will include a message with each commit that states what these changes have done. 


### What is GitHub, and is it different from Git?

GitHub is a hosting service, which provides you with place on the internet for your Git projects.  You can think of it [like Dropbox or Google Drive (but much better)]( https://happygitwithr.com/big-picture.html ). There are many benefits for using hosting platforms like GitHub, but you *are not required to use GitHub (or any other hosting platform) while using Git*. It is perfectly fine to only use Git locally, though I'd argue you'd losing out on many of the benefits. 

Also, you should be aware that there are other Git-based hosting services, such as [Bitbucket](https://bitbucket.org/) and [GitLab](https://about.gitlab.com/). We choose GitHub because it is the biggest and most well-known, but you can use any other platform.

Hosting platforms like GitHub provide a "remote" repository for our Git-based projects. Using the flowchart from the previous section, you can see that we can push the commits from our local repository to the remote repository using `git push`. This will update the remote repo with our new progress. 

We can then pull in the remote repository to a local repo using `git pull`. This can be a really nice workflow if you often work on multiple computers (much more robust than emailing yourself!). Sometimes there may be differences between your local and remote repo, which is were tools list `git fetch`, `git diff`, and `git merge` can be helpful.

There were a lot of weird terms in these sections, but don't worry! We will use RStudio to do most of this for us. 

## Getting Started with Git, GitHub, and RStudio

### Installing Git

#### [Windows]( https://gitforwindows.org/ )

Download from the link ( https://gitforwindows.org/) above. This installation will also include a Bash shell, and place Git in an easy place for RStudio to use. When asked about your PATH environment, select "Git from the command line and also from 3rd party software". 

#### Mac

More than likely you'll already have Git, but it is probably with checking and potentially update. Using the Terminal, run the following lines:

```
git --version
git config
```

If prompted, click on "Install". Alternatively you can also install Git for Mac from http://git-scm.com/downloads, or you can use Homebrew (if already installed):

```
brew install git
```

#### Linux

Again, you probably already have Git installed, but if not you can use your distro's package manager. For instance, if you use Ubuntu you can install Git using:

```
sudo apt-get install git
```

### Setting up your global options for Git

For simplicity's sake, it is a good idea to set a few "global options" in your Git settings so that you don't have to re-enter things over and over again. From the Git Bash shell (Windows) or Terminal (Mac), you can set your name and email (which should be the email used to  set up got GitHub account):

```
git config --global user.name 'First Last'
git config --global user.email 'me@example.com'
git config --global --list
```

Alternatively, we can do this in R using the [`usethis` package](https://usethis.r-lib.org/):

```R
## install if needed (do this exactly once):
## install.packages("usethis")

library(usethis)
use_git_config(user.name = "Jane Doe", user.email = "jane@example.org")
```

### Creating a GitHub account

Create an account for free at [https://github.com](https://github.com/). Accounts are free, so it is totally fine to make a temporary account that you use to test and learn. However, one of the great things about GitHub is creating a portfolio for your work that you can share with collaborators (or even future employers). [Here are some tips/ideas to think about went setting up your "official" GitHub account]( https://happygitwithr.com/github-acct.html ).

#### Creating a [new repository](https://github.com/new)

Starting a new project on GitHub, called a "repository" or "repo", can be done by clicking the green "New" button on your Repositories panel (on the left), or clicking the **+** icon on the upper right. 

By default, new repos are **public**, which means that anyone with the URL can see the contents of your repository. They can also download, or "clone", your repo. You can also choose to make your repo **private**, which means only invited collaborates can see and download your repo. Private repos on GitHub typically are not free, however you can apply for academic accounts which will provide you unlimited free private repos.  

Some people get caught up on the public versus private repos thing, don't let this stop you from using Git. One could argue that there are merits to doing science in the open, but even if your concerned about scooping you should still use Git. Typically academics won't have any problems getting free private repos from GitHub. Other platforms, such as GitLab, have unlimited free private repos. Please don't let this be the thing that stops you from using Git. 

### Starting a new project with Git

There are a few different ways to start up a new project using R and Git. Below is just one option that I've found to be an easier place to start.

#### Make a new repo on GitHub

I've found that it is typically easier to start from GitHub when you start learning Git. On [https://github.com](https://github.com/) create a new repository. Give the repo a name (this will be part of the repo's URL), and (if desired) a description. Select either public or private, then select *"Initialize this repository with a README"*. Click "Create repository".

You will then be taken to your new GitHub repository, which will have a single file called README.md. This is a [Markdown]( https://en.wikipedia.org/wiki/Markdown ) file where you can place information about your repo later. On the right you should see a green button that says "Clone or download". Click this button and copy the link under "Clone with HTTPS".

#### Create new Project in RStudio

Open RStudio. If you haven't already, now is a good time to set your Git global options with the `usethis` package (see code above). 

Under the File tab (upper left) select "New Project...". Using an [R Project]( https://support.rstudio.com/hc/en-us/articles/200526207-Using-Projects ) is a natural way to use R and Git, since both Projects and Git will contain all of your work in a single folder. On the next prompt select "Version Control", then choose "Git". Then paste in the address that you copied from GitHub in the "Repository URL" box. You can then change the name of the folder than will be created on you computer (I wouldn't recommend doing this), and select when on your computer the folder will be created. 

This has just created a local copy of your GitHub repo on your computer (try finding the folder just to convince yourself!). 

#### Making your first commit

You'll notice in the Files pane (probably the lower right) that there are three files it your project: The README.md file, something called `.gitignore`, and your R Project file ending in `.proj`. The README.md came from your remote repo on GitHub, but the other two were newly created by R. 

Let's create a new R script as well, under File > New File > New R Script. You can write some R code in this script (for example, `head(mtcars)`). Then save your script, giving it a name (for instance, `example.R`). You should see this new file appear in the Files pane. 

This is a good place to make a commit. In your Environment panel (probably on your upper right), you should see a tab called "Git". In this tab, you'll see the three new files we've created. Click on the button that says "Commit". This will local a new window that shows us the changes we've made since the last commit (which in this case is making these three new files). 

To add the new files to the staging area, click each of the checkboxes under "Staged", you see see a green **A** appear signaling that these files have been added to the staging area. Basically RStudio is doing `git add` for us.

Now we need to commit the changes in the staging area to Git. First, write a commit message in the box on the upper right. For example, something like "created new R project and started writing example.R". Then click the "Commit" button. Congrats, you've made your first Git commit! 

#### Pushing commit to GitHub

Your progress on your local directory has been committed to Git, however if you go to your repo on GitHub you will not see your new files. That is because we haven't pushed our commit to the remote repo yet.

In fact, if you look back in RStudio under the Git tab it probably will say "Your branch is ahead of `origin/master` by 1 commit". This is Git-speak for saying that we have push our latest update to our remote repository on GitHub. As you might have guessed, we can do this by clicking the green "Push" arrow.

A dialog box will popup stating the the new commit has been pushed to the remote repo. Now go back on refresh your page on GitHub, and your files from commit will appear!

#### Pulling in changes from GitHub

Finally, let's try pulling in updates from our remote repository into our local repo. First let's make a change on the remote repository. We can do this straight from GitHub. Click on `example.R` (or whatever you named your R script). On the right-hand side click on the pencil icon which says "Edit this file". Then make some change (for example, change the script to `head(mtcars, n = 10)`).

Once you've made your change, go down to the Commit changes panel at the bottom of the page. Provide a commit message in the first box (for example, "Minor edit for testing"). If you go back to the main page, you should see that this file now has a new commit message.

Now go back to RStudio. To pull in the new commit from your remote repo click on the blue "Pull" arrow on the Git panel. A Git pull prompt should pop up, which contains some information on the changes to your local directory. Close the prompt and open your R script, and you should see the changes you made directly to the remote repo!

## Other Helpful Resources

### Using the .gitignore file

You don't have to tell Git to track all of the files in your repo. Git will be limited in tracking changes to complex file types (for instance, an Excel document instead of a CSV). You still can use Git on these files, but it won't be able to give you very informative outputs. Also large files can be an issue, as GitHub as limits on how large individuals files can be. We also might not want to include private or sensitive data. 

Adding files to you .gitignore files is a convenient way to tell Git to not track certain files. You can open this file in RStudio, and you'll notice that there are already some files listed (R Projects do this automatically.). You can add single files to you .gitignore, but I find it easier to have entire sub-folders where I place the files I don't won't to track. For example, I can make a subfolder called "large-files" (you can do this in the Files panel in RStudio), then add `large-files/` to my .gitignore. 

When I push my commits to GitHub it will ignore anything in the .gitignore file, however these files will still be in my local repo. This is a great way to exclude certain files from GitHub while still leveraging the power of Git for our source code files. 

### Git clients

While some people are comfortable using the command line for all of their Git work, other's will prefer to use Git clients, which provide GUIs for working with Git. You can find a rundown of what a Git client is and some recommended free Git clients at  https://happygitwithr.com/git-client.html.  

### General Git links

* [Git website](https://git-scm.com/book/en/v1/Getting-Started) where you can download Git also contains helpful guides.

* [Git - the simple git](https://rogerdudler.github.io/git-guide/) provides a great "getting started" workflow.

* [A visual Git reference](http://marklodato.github.io/visual-git-guide/index-en.html) has nice visual guides for many Git functions. 

* [GitHub's Git cheatsheet](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/) is a great reference. 

### R-centered links

* [Happy Git and GitHub for the useR](https://happygitwithr.com/index.html) from Jenny Bryan and Jim Hester is a great place to continue from here, with a focus on R, RStudio, and GitHub. 

* [Excuse me, do you have a momment to talk about version control](https://peerj.com/preprints/3159/) by Jenny Bryan is a great preprint on the utility of Git for reproducible research. 

* [Using version control with Git and SVN](https://support.rstudio.com/hc/en-us/articles/200532077-Version-Control-with-Git-and-SVN) by Nathan Stephens is a nice resource, specifically for integrating Git and RStudio. 

* [Git and GitHub](http://r-pkgs.had.co.nz/git.html#git-pull) chapter from Hadley Wickham's *R packages* book is another great resource that covers more indepth topics (including pull requests). 