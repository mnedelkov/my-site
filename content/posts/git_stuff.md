---
title: "git_stuff"
date: 2023-01-29T09:16:44-07:00
draft: false
toc: false
images:
tags:
  - git
  - web_dev
  - programming
---

Finally got a Github repository set up for this project. It took me a little bit to figure out how to use the site.

Making a commit and pulling it into the main branch from the terminal took a bit of research to figure out. Turns out, Github depreciated the username and password feature for utilizing the command-line to create push and pull requests. 

The new system requires a user to create a unique token that essentially functions as a password for authorizing actions.

I have everything in place now though. I made some changes, created a commit with a new branch, pushed that branch to the repository, and then pulled it into the main branch. As I am not a programmer by trade, I'm not sure how useful this knowledge will be but it was fun to figure out!

Below I am including a couple of the useful commands I have been using:

---

**Tracks Changes Made**

`git status`

**Add All Changed Files to the Staging Environment**

`git add --all`

**Commit All Changed Files and Add a Message**

`git commit -am "Message goes here"`

**Create a New Branch**

`git checkout -b <new_branch_name>`

**Switch the Current Branch**

`git checkout <branch_name>`

**View Available and Current Branches**

`git branches`

**Push a Branch to the Repository**

`git push -u origin <branch_name>`
*as a note, "origin" is simply a shorthand for the repository's URL

**Bring Local Main Branch Up To Date**

`git pull origin <primary_branch_name>`