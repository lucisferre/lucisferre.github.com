---
title: SVN, Trac and Windows, oh my!
date: 2009-05-08 22:55:20 Z
categories:
- svn
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '89'
---

All I have to say is ugh ![Annoyed][1]. While many of the Linux based open source projects are able to run under Windows, and some even have slick installers, there always seems to be some degree of frustration involved in getting them to work. I think it is some kind of punishment for using Windows to run these programs in the first place. 

Typically, I would rarely bother but in some cases Windows is a requirement so I settle in for a good session of heavy googling and try to figure it out. In this case what I wanted to do was get Trac setup with VisualSVN under windows so that it was integrated with VisualSVN and was able to use the VisualSVN configured NTLM authentication. Shouldn't be too big a deal right...? 

<!--more-->

First google search turned up [this][2]. Perfect, a piece of cake… for the most part. You still have to configure trac through the command prompt, but generally that should be considered par for the course with these systems. Only one problem, that link hasn't been updated in over a year and it doesn't work with the latest versions of VisualSVN and SVN. Well ok, I can probably figure this out, use the latest versions and get the latest version of Trac installed to boot, right? Well, yes and no, there are a million issues with this. 

Projects like this rely on a lot of bindings to get python to work with SVN, SVN to work with Trac, Apache to work with everything else, etc. Long story short after about a day of tooling around with it I found a solution so for those of you crazy enough to want to have VisualSVN 1.7.1 working with Trac _and _NTLM here is my solution: 

Firstly, I won't even explain how to install VisualSVN. It is dead simple and if you are having problems with that dare not to read on, this isn't for you. Once you have VisualSVN installed and set up with the NTLM authentication we can start with Trac.

> Note: Make sure to set up at least one VisualSVN repository so we can test out trac. Even better if you have an existing repository you can load into VisualSVN for the testing.

The trac install instructions are actually a good place to start. I recommend this [page][3] from the Trac Wiki. Follow the instructions making sure to choose a 2.5.x version of Python. Once you have it set up use one of your VisualSVN repositories to test it out as follows:

  1. Create a root folder for your Trac projects like C:\Trac 
  2. Initialize the trac environment. Go to the C:\Python25\Scripts folder (or wherever you installed it to) and do the following: `trac-admin C:\Trac\MyProjectName\ initenv`
  3. You will be asked a series of questions, enter your project name, select the default database, the svn repository option (also default) and the directory for your repository. If you chose the defauly when setting up VisualSVN that should be C:\VisualSVN\MyProjectName\ 
  4. Finally give yourself admin privledges: `trac-admin C:\Trac\MyProjectName permission add myusername`

Test it all out when you are done as the trac wiki suggests with the tracd server. Make sure it works. If it doesn't then you will have to figure out what is wrong because at this point you should be able to open up the trac page in a browser. Now we don't want to use the tracd server we want to use VisualSVN's apache server instead so shut down tracd and we will set up VisualSVN's apache settings to work with our trac setup. First we need to install mod_python which you can get [here][4]. Get the one for apache 2.2 and python 2.5. Before you install it, go to C:\Program Files\VisualSVN Server\ and create a folder called "modules" (if you installed elsewhere you know what to do ![Winking smile][5]). Now install mod_python and when it asks where apache is point it to your VisualSVN Server folder. It will put the mod_python.so file in there. Next we use the instructions from the VisualSVN trac page:

> Add following line at the top of file C:\Program Files\VisualSVN Server\httpd-wrapper.bat: `set PYTHONHOME=C:\Python25` I'm not sure this is absolutely necessary but it is just a modification of the original instructions to fit the default python location (obviously if you have it installed somewhere else use that path).

Also we need to add the following to the /conf/httpd-custom.conf file:
    
    LoadModule python_module "modules/mod_python.so" 
    LoadModule authz_user_module bin/mod_authz_user.so
    
    <location /trac> 
      SetHandler mod_python 
      PythonInterpreter main_interpreter 
      PythonHandler trac.web.modpython_frontend 
      PythonOption TracEnvParentDir C:\Trac 
      PythonOption TracUriRoot /trac 
      AuthName "Trac" 
      AuthType Basic 
      AuthBasicProvider visualsvn 
      Require valid-user
    </location>
    

If you have used different folder location for your trac folders then adjust as needed. Now if you try to restart Visual SVN and go to the /trac/Myproject url on your VisualSVN http address you will get a SVN error from trac. This is caused by an incompatibility between DLL versions in your version of the python-subvsion binding and the DLL in the apache version from VisualSVN (pretty stupid, I know). This is fixed by copying the dll here: 
    
    C:\Python25\Lib\site-packages\libsvn\libapr-1.dll

Into here: 
    
    C:\Program Files\VisualSVN Server\bin

How did I find this? After hours of googling a million potential solutions for this problem I came across this [post][6]. The first two items were not relevant to my problem, but the third one was right on the money. Anyways, if you have followed these instructions so far you should be able to go to your VisualSVN URL and add /trac/MyProjectName and browse your SVN repository through Trac. VisualSVN is promising an integrated plugin for trac (or trac like functionality) "soon". Which could easily be never. Until then this is one way to get it working "soon-er".

> Pro tip: Unless you absolutely need/want NTLM authentication and trac integrated with the VisualSVN apache server I suggest either using Linux for this or install trac separately using a trac stack (http://bitnami.org/stack/trac). It will save a lot of headaches.

> Update 24/07/2009: Ivan just posted below to let me know that they have updated the Visual SVN plugin for 2.0. Go check it out and save yourself all this hassle ;-).

   [1]: /images/wlEmoticon-annoyed.png
   [2]: http://www.visualsvn.com/server/trac/
   [3]: http://trac.edgewall.org/wiki/TracOnWindows
   [4]: http://apache.sunsite.ualberta.ca/httpd/modpython/win/3.3.1/
   [5]: /images/wlEmoticon-winkingsmile.png
   [6]: http://www.mail-archive.com/trac-users@googlegroups.com/msg06937.html

