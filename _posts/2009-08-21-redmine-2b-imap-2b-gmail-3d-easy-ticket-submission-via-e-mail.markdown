---
author: Chris Nicola
date: '2009-08-21 02:15:00'
layout: post
slug: redmine-2b-imap-2b-gmail-3d-easy-ticket-submission-via-e-mail
status: publish
title: Redmine + IMAP + Gmail = Easy ticket submission via e-mail
comments: true
wordpress_id: '38'
categories:
- Software Development
- Redmine
---

![ticket][1] 

This is a short continuation of my [earlier post ][2]on getting Redmine setup under Windows. This post will be about getting the incoming e-mail functionality enabled so that users can send and even update tickets via e-mail. In-fact, this is quite a powerful feature, "**how powerful is it!?" **you ask... just a second I'll get to that in a moment. 

<!--more-->

First, if you are following along from before then I can assume you are using Redmine under Windows and possibly even have it installed using the Bitnami stack.  I can also assume you probably use Exchange for your e-mail system and more than likely it does not have a Ruby environment on it, nor does it have IMAP enabled.  Finally, I am going to assume that the IT guys would get agitated if you even glanced in the direction of the server room.  How am I doing so far?  Alright, no worries, you should know better than to be playing with the production servers anyways. We are going to get this incoming e-mail thing working for you in not time, just stay with me. 

For outgoing notifications Redmine uses SMTP, which should be no problem to get working in just about any environment.  If all else seems lost just set up a Gmail account an use that for SMTP forwarding.  I will assume there are no problems, just follow the instructions on the [Redmine Wiki ][3]for setting this up. Incomming e-mail can be a bit more tricky.  

There are [a few options][4] but most of them don't really work well in a Windows environment.  Forwarding e-mails from the mail server requires running a ruby script and is suited to Postfix, Sendmail environments (to be honest you may have your Exchange server behind one of those so you may be able to do this, but hey, I said leave the production systems alone dammit!). Reading e-mails for the standard input is also not the greatest option since it is a) slow and b) still not all that suitable for a Windows environment. Fortunately the IMAP option is perfect for us (and imho the best of any of them anyways) so we are going to go with that. Setting up Imap using a Gmail account is easy. I just created the account and enabled Imap in the settings. The harder part is configuring your Redmine server to check it's e-mails. The wiki says to create a pycron job to run the mail checking script: 

```
*/30 * * * * redmineuser rake -f /path/to/redmine/appdir/Rakefile redmine:email:receive_imap RAILS_ENV="production" host=imap.foo.bar username=redmine@somenet.foo password=xxx
```

Close... but I have to take the Windows environment into consideration.  For starters we need to setup the rails environment variables before we run that.  Here is what I did. 

  * First [download][5] and install the graphical version of pycron.  It will as to set up a service, say **yes**.
  * Next, I added a batch file called `imapjob.bat` that in the apps/redmine/scripts folder.  This is where Bitnami has all it's service scripts set up and it seemed like a good place for this.  The batch commands are as follows:

```
CALL C:\BITNAM~1\scripts\setenv.bat cd C:\BitNami Redmine Stack/apps/redmine CALL rake redmine:email:receive_imap RAILS_ENV="production" host=imap.gmail.com port=993 ssl=SSL username=myfakebugzemail@gmail.com password=I8URBugz project=unsorted tracker=bug allow_override=project,tracker,priority
```

  * Obviously you want to replace the relevant paths depending on your installation
  * You may also want to customize the parameters of the script.  The Wiki explains the options you have available.  My setup is like this (this is where you can see the power of this feature): 
    * project=unsorted - _Adds incomming e-mail tickets to the "unsorted" project by default_
    * tracker=bug _- Sets new e-mail tickts to "bug" by default_
    * allow_override=project,tracker,priority - _Allows the user to override (set) the project, tracker and priority values in the body of the e-mail_

  * Now you open up pycron and add a job to your crontab, I like it to run every minute so just set all Schedule options to '\*' and then set the command to: 
    
```
C:\BitNami Redmine Stack\apps\redmine\scripts\imapjob.bat
```

   * Start your pycron service and set it to Automatic if you want it to start automatically if the server is reset

That should do it.  Make sure you set up your "unsorted" project or whatever you decided to call it and send a test e-mail to that Gmail account.  Try out the overrides too, they are half the power of this feature.  You can e-mail tickets that go to a specific project and have specific options set.  You can also send a new e-mail to an existing ticket, just include the number in the subject, e.g., "Subject: I have fixed #13" if you wanted to update ticket 13. But wait, there's more!  It even takes attachments (apparently, I haven't had time to test all of the features as I am sitting here writing up this blog instead) so users can attach files, screen-shots, etc when they send an new ticket by e-mail.  Honestly, I can't think of anything here they didn't think of here. One more thing, if you want to use an internal e-mail account for this instead of the Gmail account, just have IT forward the e-mails to your Gmail account.  They should have no problems with that, as long as you keep your gaze averted from the server room at all times.

   [1]: /images/ticket.jpg (ticket)
   [2]: http://lucisferre.net/coding/2009/08/19/issue-feature-tracking-with-git-and-redmine-under-windows
   [3]: http://www.redmine.org/projects/redmine/wiki/EmailConfiguration
   [4]: http://www.redmine.org/projects/redmine/wiki/RedmineReceivingEmails
   [5]: http://www.kalab.com/freeware/pycron/pycron.htm

