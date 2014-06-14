---
author: Chris Nicola
date: '2010-07-21 19:47:00'
layout: post
slug: simple-tools-make-integration-testing-easy
status: publish
title: Simple tools make integration testing easy
comments: true
wordpress_id: '82'
categories:
- .NET
- agile
- csharp
- TDD
---

I've been working on a small library that requires using an FTP client to upload and access files stored on a remote FTP.  Now I want to do TDD, but the .NET FTP libraries (FtpWebRequest/FtpWebResponse) are absolutely untestable.  They have no interfaces, you can't mock them at all (though I've heard TypeMock may be capable of doing this) to TDD they are an immovable object.  I should point out my team has also committed to maintaining over 80% code coverage on things like libraries, APIs and services and for this library the FTP related code is currently about 50% of the code.

Even if I wanted to simply ignore this from coverage metrics, that would still leave a lot of code that will need to be written and likely maintained without _any_ tests.

<!--more-->

Now, you could, if you wanted to, simply wrap the .NET FTP classes and delegate all their members (or just the ones you needed) but to successfully TDD you will need to wrap and delegate three different classes (including the WebException class) and perform a little C# wizardry, not an entirely tall task (particularly if you have ReSharper) but an unattractive one at best.

After traversing down the wrapper route a little way I switched gears and figured the simplest solution would be, in fact, to put all the FTP responsibility behind a single interface and implementation class and then just write very simple "_almost unit"_ integration tests against the implementation.  The integration tests could then be run quickly against a local FTP daemon even on the build server.  However I still didn't feel like setting up an FTP server under IIS on the build server (or on my local machine).  I wanted something quick, simple and small enough that I could include it in the project and check it in.

Whenever I work with nHibernate, I often write [simple DB tests][1] against a SQLite in memory database, so I figured there might be something similar I use for an FTP testing (at this point I feel it necessary to point of that if this was Linux this would have been a piece of cake).  After about 15 minutes on Google I found [FTPDMIN][2] a 65kb executable, no-frills FTP daemon.  Just for a few laughs here is what my integration tests look like:
    
```csharp
[TestFixture]
public class FtpServiceIntegrationTests
{
  private Process _ftpd;
  private FtpService _ftpService;
  private string _filename;
  private string _server;
  private readonly byte[] _data = new byte[10];

  [TestFixtureSetUp]
  public void OneTimeSetup()
  {
    new Random().NextBytes(_data);
    var pi = new ProcessStartInfo("ftpdmin.exe", "./")
    {
      CreateNoWindow = true,
      UseShellExecute = false
    };
    _ftpd = Process.Start(pi);
    _ftpService = new FtpService(new NetworkCredential("me@localhost.com", ""));
    var address = Dns.GetHostAddresses(Dns.GetHostName())[0];
    _server = "ftp://" + address;
    _filename = "Test.txt";
  }

  [Test]
  public void CanUploadData()
  {
    _ftpService.Upload(new MemoryStream(_data), _server + "/" + _filename);
    Assert.That(_ftpService.FileExists(_server + "/" + _filename));
  }

  [Test]
  public void CanListFiles()
  {
    _ftpService.Upload(new MemoryStream(_data), _server + "/" + _filename);
    var files = _ftpService.ListFiles(_server);
    Assert.That(files.Any(f => f.ToLower() == _filename.ToLower()));
  }

  [Test]
  public void CanCreateFolders()
  {
    var randnum = new Random().Next(100);
    _ftpService.CreateFolder(_server + "/" + "Folder" + randnum);
    Assert.That(_ftpService.FolderExists(_server + "/" + "Folder" + randnum));
  }

  [Test]
  public void CanGetFileData()
  {
    _ftpService.Upload(new MemoryStream(_data), _server + "/" + _filename);
    var returnedData = new byte[_data.Length];
    _ftpService.Get(_server + "/" + _filename).Read(returnedData, 0, _data.Length);
    Assert.AreEqual(_data, returnedData);
  }

  [Test]
  public void CheckingForANonExistantFolderReturnsFalse()
  {
    Assert.IsFalse(_ftpService.FolderExists(_server + "/Folder/Test"));
  }

  [Test]
  public void CheckingForANonExistantFileReturnsFalse()
  {
    Assert.IsFalse(_ftpService.FileExists(_server + "/" + "File" + new Random().Next(101, 200)));
  }

  [TestFixtureTearDown]
  public void OneTimeTearDown()
  {
    _ftpd.Kill();
    _ftpd.Dispose();
  }
} 
```

   [1]: http://ayende.com/blog/3983/nhibernate-unit-testing
   [2]: http://www.sentex.net/~mwandel/ftpdmin/

