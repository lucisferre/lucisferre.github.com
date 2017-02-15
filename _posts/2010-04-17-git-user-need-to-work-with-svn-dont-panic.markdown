---
title: So you have to use Subversion? DONT PANIC
date: 2010-04-17 13:09:00 Z
categories:
- git
- svn
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '76'
---

![dontpanic_1][1]

Distributed source control (DSCM) using Git (or Hg) is truly a beautiful thing.  It's wonderful, it's revolutionary and it will _free your mind_.  But the reality is that subversion _is still out there_ and you are probably going to come into contact with it again at some point.  The important thing to remember is **DONT PANIC, git svn** is here for you.

For those of you who are reading this and saying, "I've worked with SVN for years, it's been good to me, what's the big deal with Git?" Learning to use Git is going to make your workflow more agile, make you less timid about changing code and exactly as promised it _will_ **free your mind.  **It may just save your code one day.

<!--more-->

**git svn** is a tool that almost every Git user has used at some point.  For most of us, one of the first things we did was run:
    
    git svn clone svn://the-source-of-my-pain/ /c/dev/freedom

Afterwards we never looked back.  Once you're weened off svn, you're done with that SVN thing, right?  Sure, occasionally you have to check-out some open source project from SVN (*cough* nHibernate *cough*) but a simple **git svn clone . -rHEAD** is enough, if you're not a committer.

Unfortunately, it may come to pass that you will actually have to do real work with SVN again, or perhaps you've just never been shown the way and still work with SVN.  That is where git svn can help you.  Recently, I once again have the pleasure of working with SVN.  No really, I actually mean_ _pleasure, because I've learned how to bolt my git workflow onto that SVN repository and it's great (well _almost_ great).  Sure, I still have to wait for Git to pull down those bulky SVN revisions but once I've got them I'm on my way and I'm working blazing fast.

## Installation and Setup

Ok so unfortunately I am going to have to break this into parts.  The first part will get just about everyone started.  If you have zero experience with Git then obviously the first thing to do is get it and install it, [MySysGit][3] is what you will use for Windows (also [GitExtensions][4] will give you some integration with VS2008).  Linux users simply have to pull it down with whatever package manager their distro uses.

Installation is pretty simple, but you may want to give some thought to how you want to handle LF/CRLFs.  If you are largely working in one platform with other devs doing the same then leave them unchanged.  If you are sharing code between platforms then the option to convert LFs to CRLFs when you checkout/checkin may be of use (you can set this option per repository so don't worry too much now, leaving them unchanged is usually a safe bet).

I also do some basic configuration to make it easier to work with Git.  Here is a copy of my [~/.gitconfig][5] file which contains some simple aliases for SVN like **spull **(fetch + rebase) and **spush**, which allow me to easily update my local repository and checkin a new commit to SVN.

## Cloning Your Repository

In order to start working with svn we need to **pull** down the source from the remote (git calls it pull to differentiate it from checkins which are done locally).  There are a couple of approaches we can take depending on your situation:

**A)** You have a clean and organized SVN repo with a **trunk, branches **and **tags** all in separate folders (for those of you who use SVN at work I know this situation is highly unlikely so bear with me).  Also you want to have full access to all of the SVN repository locally and don't mind that it may take a while to checkout the first time.
    
    git svn clone <remote address> -T <trunk dir> -b <branches dir> -t <tags dir> <dest dir>

This will take a while (depending on how much history your repo has) but it is a one time deal (if you don't want to wait use -**r HEAD** to only get the last revision).  This is actually cloning the entire svn repo locally, but don't worry it won't be as large as you think, Git's compression is far better than SVN.  This will ensure that you have easy access to checkout any branch or tag using git locally making it extremely easy to work with SVN.  After this is done, change  to the **<dest dir> **and use **git gc** to do a garbage collection.

**B)** You don't have a standard organized SVN repository and branches are everywhere.  So for now you just want to checkout one folder (i.e., branch) and/or you probably don't want to get all of the svn history right now either.
    
    git svn clone <remote address> -r HEAD

You can substitute any SVN revision number after -r if you want to checkout another revision.  This is good if you only need to work in one branch.  You will unfortunately have to checkout other SVN branches separately or add then as svn-remotes manually which is explained [here][6].

You will also need to setup a .gitignore file for ignoring artifact paths and user settings files.  Here is a copy of my global [~/.gitignore][7].

## Git Workflow

This is my current Git<->SVN workflow, but remember Git is all about flexibility, you can define your own workflow once you are comfortable with the tools.  I am going to use the full command names for the common git command below, but my .gitconfig contains shortened aliases like **co** instead of **checkout**.

  1. Create a new branch, typically named after the feature or bug I am working on like "FeatureA"
    `git checkout -b FeatureA`
  2. Make changes to my source code to complete the feature.  At any point I can view changes using `git status` and `git diff`.  If you have it installed `gitk` will bring up a GUI as well. 
  3. 'Stage' my changes using the `add `command.  The command below will stage all my changes including adding and removing files.  However you can specify a specific file or folder in your project to add changes from.  You can also use the -`p` option to add changes from a file line-by-line.  Git allows you to stage changes and commit them with any level of granularity you wish. 
    `git add -A .`
  4. Now I can commit my staged changes.  Note that un-staged changes will not be part of the commit.  That way I can separate my changes by selectively using `add` into multiple small or _atomic_ commits where the commit message clearly states what changed and is closely matched by the _diff_.  This makes code changes easy for other team members to understand.  If you understand what "clean code" means then think of this approach as "clean source control". 
    `git commit -m 'commit message (leave it blank to use an editor (usually vim)'`
  5. Finally, once my Feature is complete I want to merge my branch back to the master branch before pushing it up to the SVN repo.  There are a number of way to do this.  I can `rebase` or I can do a `merge.`  To closely match the current behavior of my colleagues using SVN where every commit to trunk is a complete feature or bug fix I merge and "squash" my commits into one single commit like this: 

```
    git co master   
    git spull #my fetch + rebase alias   
    git merge FeatureA --no-commit #merge branch but don't commit  
    git ci -m #I use vim to create a SVN-like commit message  
    git spull #pull again just to be sure we are on the latest SVN commit  
    git spush #my git svn dcommit alias
```

The reason I create a single large commit is because where I work they have a workflow where a single commit is used with a message that summarizes all of the details of the bug-fix or feature being committed.

## Other Resources

That is all for the basics.  This intro should get any SVN user away from using mouse-driven tools like SVN and into a more agile git based workflow locally.  It will make you a less timid developer because you will commit more often and always know you can go back a step or two, or even "cherry-pick" changes.

I could go on and on about the features available in git on my blog, but there are so many great resources already and this is only meant as a "getting started" guide for SVN users.  I've decided not to make this into a multi-part post, instead here is a lot of some excellent resources that I have found invaluable:

  * `git help <any command>` - brings up the man pages, loads them in a web browser on windows 
  * [Pro-Git][8]- completely free only book, but buy a print edition for your workplace) 
    * [Chapter on git svn][9]
  * [Working with SVN branches][6]- a blog post on svn branches.  Very useful if you SVN repo doesn't conform to the standard tags/branches layout 
  * [GitHub][10]- a great place to setup free (public) repositories and learn to work with git

   [1]: /images/dontpanic_1_thumb.png (dontpanic_1)
   [2]: /images/dontpanic_1.png
   [3]: http://code.google.com/p/msysgit/
   [4]: http://code.google.com/p/gitextensions/
   [5]: https://gist.github.com/836388
   [6]: http://www.dmo.ca/blog/20070608113513/
   [7]: https://gist.github.com/836388#file_.gitignore
   [8]: http://progit.org/book/
   [9]: http://progit.org/book/ch8-1.html
   [10]: http://github.org

