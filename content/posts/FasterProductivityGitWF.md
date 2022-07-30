---
title: "Faster and Productive - Git Workflow"
date: 2022-07-26T21:58:04-04:00
draft: false
cover: 
   image: img/gitAliases.png
   alt: 'GitBash aliases'
   caption: 'GitBash aliases'
tags: ["git", "productivity"]
categories: ["tech"]
---

# Faster and Productive - Git Workflow
If you're used to using Linux bash commands and you're using Git Bash on Windows, it's likely that you'd wish to add aliases to make your work simpler.

I work on the same web project every day, like many other software engineers do. How my day starts, I have to pull any changes from the remote branch, open up my project directory in IntelliJ IDEA, compile the code, run my server, open Google Chrome, and then open an incognito window. Although carrying out this procedure manually doesn't take very long, each micro-task involves a change in context, which might distract you from the work you are about to complete. In this post, I'll show you how to automate your setup process in order to save time and improve the quality of your projects.

![Git Workflow](/img/gitWF.png "Git Workflow")

## GitBash Aliases
We'll need to create some aliases in order to automate our duties. Shortcuts like bash aliases allow you execute a series of operations with fewer keystrokes. They can have whatever name you like. Having stated that, you are free to use this information in as many projects and procedures as you choose!

We'll need to add the content to a file called .bashrc in order to build our aliases. `.bashrc` may be found in your home folder or user profile, and its location should be in the format `c:/Users/username>/.bashrc.`

So, launch any text editor, and then open the `.bashrc` file, it should look like this:

![Git Aliases](/img/gitAliasesBashrc.png "Git Aliases")

So having the `.bashrc` file as the screen above you should be able to execute the git aliases from a fresh git bash terminal.

![Git Aliases Usage](/img/gitAliasesBashrc.png "Git Aliases Usage")

You can find more alias usages following this link: https://gist.github.com/mwhite/6887990