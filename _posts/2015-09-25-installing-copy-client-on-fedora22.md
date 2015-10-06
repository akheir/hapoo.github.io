---
layout: post
title: Installing Copy client on Fedora 22
---

It's been more than three years that Google released the *Google Drive* but the client for Linux isn't ready yet. There is even a [Petition][1] on [change.org][2] signed by more than 28,000 people asking Google to *Create a Native Linux Google Drive Application*.

The issue was important to me because our university used to pay for Google Apps service and each of us students had 25GB of storage which could be very useful to save coursework material and keep them in sync with several devices or share research documents between our group.

Now the university in a drastic move switched to Microsoft's *Office 365*, the amount of storage in new service is the same but I don't believe Microsoft would ever provide official Linux support for its *OneDrive*.

So I started my search to find an alternative solution for my storage problem. [Dropbox][3] seams to be a very good option. It has graphical Linux client with background sync for many years. It also has official client for Windows, Mac, Android and iOS, but the problem is it only offers 2GB of storage with its free account.

The best alternative option would be [Copy][4]. Actually I believe the Copy is a better option for following reasons:

+	Copy offers 15GB of storage with its free account.
+	It has graphical client for Linux as well as Windows and Mac.
+	The Linux client has also a command line version, which I find it very interesting and it is extremely useful for remote management.

I will discuss the command line features in a later post, right now I want to show you how to install the client and how to enable the agent.

First download the latest version of client and expand it wherever you usually install your third party application. I personally prefer the */opt/* . Note that you must have root privilege to be able to create files in */opt/*

{% highlight bash %}
# cd /opt
# tar xzvf Copy-3.2.01.0481.tgz
{% endhighlight %}

You're done, this version of Copy client will extract in folder named *copy*. Now you need to run the client and login for the first time. Assuming your are running on *x86_64* architecture, run the following command with your user (running with root will configure the client for root user)

{% highlight bash %}
$ /copy/x86_64/CopyAgent
{% endhighlight %}

A window will pop up and ask for your Copy credential, which you should already have. Then the client will create a subdirectory named *Copy* in your home directory and start to sync all your files. The client configuration will be stored in *.copy* subdirectory in your home.

Now you have a running application, and you don't need to run the application any more, next time you login to your account the Agent would startup and keep all files would be in sync.

[1]: https://www.change.org/p/google-create-a-native-linux-google-drive-application
[2]: https://www.change.org/
[3]: https://www.dropbox.com/
[4]: https://www.copy.com/
