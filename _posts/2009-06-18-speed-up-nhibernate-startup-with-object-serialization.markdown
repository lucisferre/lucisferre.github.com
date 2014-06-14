---
author: Chris Nicola
date: '2009-06-18 15:48:00'
layout: post
slug: speed-up-nhibernate-startup-with-object-serialization
status: publish
title: Speed up nHibernate startup with object serialization
comments: true
wordpress_id: '18'
categories:
- .NET
- nHibernate
---

**Edit:** [Tuna][1] pointed out to me that [his solution][2] is not, in fact, Castle specific and it clearly isn't I really don't know what I was smoking back then.  Also I noticed that somewhere in my code I am getting the assInfo from ass.Location so my code is also not age appropriate.  So for the sake of the children, definitely feel free to [check out Tuna's post][2] on nhForge.

This is gonna be a good one.  I spent some time yesterday trying to speed up my applications start time.  I knew from reading a few other blogs that nHibernate is often the culprit due to the time it takes to process XML configuration files and then create the SessionFactory.  However, a discussion in nhusers mentioned using serialization of the Configuration object after it has loaded the XML files to speed this up in subsequent startups.

<!--more-->

One of the reasons nhibernate can take a while to start up is that it is validating each XML configuration file individually.  If you are like me (an anyone else who follows best practices) you have seperated you hbm.xml files into one-per-entity.  Unfortunately that likely means that the more entities you have the longer nhibernate will take to create the Configuration object.  This is not a good thing.  You could try just having one hbm, but that is also not a good thing.  Persisting your Configuration after the first time you create it seems to be the best solution available.

It would be nice if persisting the Configuration using serialization was part of nHibernate, but as of yet I don't believe it is so you have to "roll your own" which isn't all that hard.  You can read a bit more on it [here][2], however that is specific to the Castle project **(note: I am an idiot and Tuna's post is not Castle specific at all...)** and not much use to people who are not using Castle.  Still it didn't seem that hard, create the configuration, save it to a file using BinaryFormatter and the load it the next time the program starts.  It took me a little bit to figure it out, but the results were dramatic.  On my development machine a 4-5s start time became <2s.  For one of my users a 20second start time became 5s.

I use a Singleton object to setup the Configuration, SessionFactory and provide Session objects.  It has a Property for Configuration:

```csharp
public Configuration Configuration
{
    get
    {
        if (_configuration == null)
        {
            //Check for previously serialized configuration

            if (File.Exists(ConfigFile) && IsConfigurationFileValid())
            {
                _configuration = LoadConfigurationFromFile();
                if (_configuration != null)
                {
                    _sessionFactory = _configuration.BuildSessionFactory();
                    return _configuration;
                }
            }
            _configuration = new Configuration()
                .SetProperty(Environment.Dialect, typeof (MsSql2005Dialect).AssemblyQualifiedName)
                .SetProperty(Environment.ConnectionDriver, typeof (SqlClientDriver).AssemblyQualifiedName)
                .SetProperty(Environment.ConnectionString,
                             "Server=10.10.0.185;initial catalog=Accounting_Test;Integrated Security=SSPI")
                .SetProperty(Environment.ProxyFactoryFactoryClass,
                             typeof (ProxyFactoryFactory).AssemblyQualifiedName)
                .AddAssembly(Assembly.Load(AccountingManager.Data));
#if DEBUG
            _configuration.SetProperty(Environment.ShowSql, "true");
#endif
            SaveConfigurationToFile();
            _sessionFactory = _configuration.BuildSessionFactory();
        }
        return _configuration;
    }
}
```

A few things to notice.  First, I only persist the configuration in the Release version of my software.  I see no reason to worry about startup time if I am debugging.  I have set the filename as a constant but you can set the filename however you want.  Now I also have logic to tell the program when it needs to create the configuration from scratch and when it doesn't in IsConfigurationFileValid():

```csharp
private Boolean IsConfigurationFileValid()
{
    var ass = Assembly.GetCallingAssembly();
    if (ass.Location == null)
        return false;
    var configInfo = new FileInfo(ConfigFile);
    var assInfo = new FileInfo(ass.Location);
    if (configInfo.LastWriteTime < assInfo.LastWriteTime)
        return false;
    return true;
}
```

All this method really does is return false if the calling assembly (which contains my hbms) is newer than the saved configuration file. If a more recently compiled version of the assembly is present I assume the configuration is out of date and tell the program to create a new configuration and overwrite the old one. So every time a publish a new version the first start up will be slower but subsequent startups will use the saved configuration.

```csharp
private static void SaveConfigurationToFile()
{
#if DEBUG
    return;
#endif
    var file = File.Open(ConfigFile, FileMode.Create);
    var bf = new BinaryFormatter();
    bf.Serialize(file, _configuration);
    file.Close();
}
```

```csharp
private static Configuration LoadConfigurationFromFile()
{
#if DEBUG
    return null;
#endif
    try
    {
        var file = File.Open(ConfigFile, FileMode.Open);
        var bf = new BinaryFormatter();
        var config = bf.Deserialize(file) as Configuration;
        file.Close();
        return config;
    }
    catch (Exception)
    { 
        return   
    }
}
```

Again we see that I do not use this code if we are debugging.  I always want to create a new configuration for that.

All in all a very simple, though perhaps not elegant, solution that you can use if you are not using Castle.  If you are using Castle then I suggest using the methods it provides for this as mentioned [here][2].

   [1]: http://www.tunatoksoz.com/
   [2]: http://nhforge.org/blogs/nhibernate/archive/2009/03/13/an-improvement-on-sessionfactory-initialization.aspx

