---
title: Issue/feature tracking with Git and Redmine under Windows
date: 2009-08-20 02:50:00 Z
categories:
- agile
- git
- Redmine
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '37'
---

Over the last couple weeks, we've been trying to pick out a solid combination of tools for a Windows based dev environment.  The key components are: source control (SCM), continuous integration (CI) and issue/bug tracking.  _Our choices_: _Git_ for SCM, _TeamCity_ for CI and _Redmine_ for issue/feature tracking.

We originally tried out Fogbugz, but it's integration with SCM solutions, particularly Git is somewhat limited.  Even for SVN it relies on external web interfaces such as WebSVN.  Still, it does look like a great issue/bug tracker and I hope they can resolve their SCM integration issues soon.  Up next, Redmine.

<!--more-->

Redmine has excellent built in SCM integration for a wide range of systems including SVN, Git, Hg and more.  It has it's own repository browser built-in and thus is not dependant on any other components to make it work.  Even more, it comes with a host of community supported plugins.  Two that caught my eye were a charting plugin to provide burndown and other charts and a code review plugin.

Setting up Redmine is fairly easy, even for Windows.  You can [download ][1]a Redmine WAMP stack from Bitnami (or a LAMP stack for Linux), which will get you setup and running with Redmine, MySql, Ruby and Apache in minutes (it also includes a Subversion server which you can use if you really prefer SVN).  Unfortunately, while the 0.8.4 version included by Bitnami supports Git repositories it is missing support for Git branches.  That feature was added to the trunk after 0.8.4.

I decided I wanted to have that feature and I also wanted to have the ability to update my Redmine installation from the trunk whever I wanted.  I spent some time reading through their Wiki documentation on [upgrading][2] and it definitely seemed doable.  Sadly, though it was a breeze to get Bitnami's stack installed, it was not as straightforward to get it working with the trunk version of Redmine.  Below, I outline for posterity the steps I took which finally got it working for me.  I hope this will save others the pain and suffering (and a days worth of my time) that I spent fiddling around.

> Before starting I recommend reading the Wiki pages on [installing ][3]and [upgrading ][2]Redmine.  We will be performing several of the steps descibed there and the Wiki can be a good reference if you get stuck.

Updating Bitnami Redmine from 0.8.4 -> trunk

  * First install the Bitnami stack.  I will leave this part to you, it is pretty simple 
  * Stop all the Bitnami services except for MySQL, we will need this to migrate the database and test 
  * Start the Bitnami command environment (this sets up environment variables so you can use ruby from the command line) 
  * [Check-out the trunk ][4]of Redmine into a second folder in the bitnami apps folder, I called mine /redmine-trunk 
  * If you are upgrading a working version make sure to backup your database and files as outlined [here][2]
  * Copy the database.yml and email.yml file from /redmine/config to /redmine-trunk/config 
  * Run: 
    
```
rake db:migrate RAILS_ENV="production"
```

  * If you have plugins to update run: 
    
```
rake db:migrate:upgrade_plugin_migrations RAILS_ENV="production" rake db:migrate_plugins RAILS_ENV="production"
```

  * You must also run: 
    
```
rake config/initializers/session_store.rb
```

  * Bitnami comes with rail 2.3.2 but Redmine expects 2.2.2 (the included Redmine has 2.1.2 frozen to it, but the trunk needs 2.2.2 minimum).  For my purposes I just downgraded rails but I believe you can also set Set this: 
    
```
RAILS_GEM_VERSION = '2.3.2'
```
  
in environment.rb.  To downgrade you can run the following:   
    
```
gem install rails -v=2.2.2 gem uninstall rails -v=2.3.2
```

  * Finally you need install the mysql gem as the 
    
```
gem install mysql
```

you will get an error message about documentation, just ignore it.

  * Now here is where I got stuck for quite a while (though it wasn't the only sticking point).  It seems Ruby has a small issue with MySQL.  I found the solution [here][5].  Copy the dll they suggest in the Bitnami ruby/bin/ folder. 
  * Ok lets test it go to the /redmine-trunk folder and run 
    
```
ruby script/server -e production
```

you should be able to access http://localhost:3000/ and everything should be working.  Ctrl-C out of that we have some more work to do still.

  * Copy over the following folders from /redmine to /redmine-trunk 

    * `/conf` (has the apache conf file) 
    * `/scripts` (has the service scripts used by bitnami) 
    * `/lang` (probably not needed but lets get it anyways) 

  * Finally rename the folders.  I called `/redmine -> /redmine-old` and `/redmine-trunk -> /redmine`. 
  * Now start up the two mongrel services and then the apache service.  If everything went as planned you should be able to access Redmine at the port you originally setup during the Bitnami install. 

Wow, it seems a lot easier after all written out, but believe me this took all day to get working (and then some).   Please let me know if this doesn't work for you (because I am thinking I may have forgot something...).  While I prefer to set up software like this under Linux, we all know that isn't always an option.  Those of us chained to Windows environments shouldn't have to suffer alone.

Just from initial testing and playing around I can see that Redmine is going to be a great product.

   [1]: http://bitnami.org/stack/wampstack
   [2]: http://www.redmine.org/projects/redmine/wiki/RedmineUpgrade
   [3]: http://www.redmine.org/projects/redmine/wiki/RedmineInstall
   [4]: http://www.redmine.org/projects/redmine/wiki/CheckingoutRedmine
   [5]: https://wiki.appcelerator.org/display/tis/Home?f=20&t=7563

