---
author: Chris Nicola
date: '2009-08-15 22:43:00'
layout: post
slug: get-visualsvn-to-play-nice-with-websvn-and-more
status: publish
title: Get VisualSVN to play nice with WebSVN and more
comments: true
wordpress_id: '35'
categories:
- svn
---

VisualSVN is a easy and free way to have a Subversion repository running under Windows in no time flat.  That said it still leaves a lot to be desired.  It has no built in bug/feature tracking, as I [posted ][1]about before even getting Trac setup with it can be a pain (though if you read the comments you will see that they have remedied that with an update).  Here I am going to talk about adding WebSVN and even svn:// protocol support to your VisualSVN with relative ease.

I've recently been working with [Adam Dymitruk][2] who has been brought into the team I am working with as our Agile coach.  This has involved getting a few other things up and running along side source code versioning such as continuous integration with TeamCity and feature/bug tracking with FogBugz.  Getting TeamCity up and running with VisualSVN is definitely easy enough, however FogBugz - SVN integration is somewhat dependent on having a working WebSVN installation.

<!--more-->

VisualSVN comes with a decent, but somewhat limited web interface (in fact it is the only interface it comes with as it doesn't even support svn:// protocol out of the box).  In order to provide a bit more functionality and get it working with Fogbugz I had to get WebSVN installed.  This is actually fairly simple.

### Part 1: Setting up WebSVN

  * First you need to install PHP5 on the server and tell it to install the Apache mod in the `C:\Program Files\VisualSVN Server `folder (or wherever you installed VisualSVN to).  You may need to tell it where the \bin\ folder is I can't remember. 
  * Next download an up-to-date version of WebSVN from http://websvn.tigris.org/ and extract the files to `C:\Program Files\VisualSVN Server\htdocs\websvn `
  * Edit the `httpd-custom.conf` file and add the following to it 
    
```
<Location /websvn/> 
  Options FollowSymLinks SVNListParentPath on SVNParentPath "C:/Repositories/" 
  AuthName "Subversion Repositories" 
  AuthType Basic 
  AuthBasicProvider visualsvn 
  AuthzVisualSVNAccessFile "C:/Repositories/authz-windows" 
  AuthnVisualSVNUPN Off Require valid-user
</Location>
```
    
     

  * This assumes your repo's are in `C:/Repositories/` change this as needed.  Note that the `AuthzVisualSVNAccessFile `like is setup for the Windows authentication option in VisualSVN.  If you are using normal subversion authentication it should probably just be `C:/Repositories/authz`
  * You can also edit httpd.conf and move the section that starts `#BEGINPHP INTSALLER EDITS` to httpd-custom.conf (this way VisualSVN will not overwrite the PHP settings when you upgrade. 
  * Now you need to install [Cygwin ][3]so you have all the utilities WebSVN needs to function property.  Be sure to select the following packages when you install 
    * Diff, Sed, Enscript (for syntax coloring), Tar, Gzip and Zip 
    * Not all of those are required but you will lose some functionality if you don't have them.  At the very least you should have Diff, Tar and Gzip. 
  * Go to `VisualSVN Server\htdocs\websvn\include` and edit the` distconfig.php`.  You can pretty much follow along and set all the options as instructed at a minimum you should do the following: 
    * Point it to your cygwin tools and SVN binaries: 

```
  $config->setSVNCommandPath('C:/Program Files/VisualSVN Server/bin');
  $config->setDiffPath('C:/cygwin/bin');
  $config->setEnscriptPath('C:/cygwin/bin');
  $config->setSedPath('C:/cygwin/bin');
  $config->setTarPath('C:/cygwin/bin');
  $config->setGZipPath('C:/cygwin/bin');
  $config->setZipPath('C:/cygwin/bin');
```

    * Setup your repository config: `$config->parentPath('C:\\Repositories');`
    * Setup your authentication: `$config->useAuthenticationFile('C:/Repositories/authz');`
    * Go through the configuration and set any other options you would like to use. 
  * Finally save `distconfig.php` as `config.php`

That should be it. Hopefully I have not forgotten anything. Restart the VisualSVN service so that it will restart apache. Now instead of going to http://localhost/svn go to http://localhost/websvn (of course, use https:// if that is how you have it configured).

### Part 2: Setting up svn://

I really don't know why but for some reason VisualSVN does not come with svn protocol enabled by default.  It seems pretty silly since it is easy to do and it is a bit faster than access through http:// or https://.  Nonetheless, it does come with a full SVN install and therefore it has svnserve, so just open a command prompt and we will create a service neat and quick:

```
sc create SVNService binpath= "c:\Program Files\VisualSVN Server\bin\svnserve.exe --service --root C:\Repositories\" displayname= "Subversion Service" depend= Tcpip
```

You can, of course remove the service with

```
sv delete SVNService
```

I hope this has been helpful, to be honest while I was glad I was able to get all this done, Adam shortly thereafter decided to show me Git and, well, I'm a Git convert now I guess. Let me leave you with a warning though, webgit is _not nearly as easy to set up under Windows _(I'm not sure you even can) as WebSVN is, so it looks like it will be time to switch to a Linux box (thank God...).  Honestly, I am still a firm believer that if you want to use either SVN or Git you should setup a Linux box to host your repositories and to provide Apache and any web interfaces that depend on Apache. Windows was never meant to run this stuff (and screw NT authentication!)

   [1]: http://lucisferre.net/2009/05/08/svn-trac-and-windows-oh-my/
   [2]: http://adventuresinagile.blogspot.com/
   [3]: http://www.cygwin.com/

