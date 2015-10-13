---
layout: post
title: Fixing WPA-Enterprise (eduroam) problem in fc22
---

Recently I had problem to connect to eduroam on fedora 22. For those who are not familiar with eduroam, it is world-wide roaming wireless connection setup, which is used by many universities in the US and around the world, eduroam enables the user to connect the network and obtain Internet access when visiting another participating university providing only their home university credentials. (see more [here][1])

Given the extend of use of eduroam and robustness of Linux community I expected the problem will resolve with the next update or at least someone in somewhere on the web will post a solution, but there is no news yet. 

While I had this problem on Linux, I had no problem for connecting to eduroam using my Android phone or Windows laptop. (By the way I think Android 6.0 has same or similar problem, but I'm not sure yet, I'll back to on this later)

Using `journalctl` command the problem looks like this:
{% highlight bash%}
# journalctl -f

NetworkManager: (wlp3s0): supplicant interface state: associating -> associated
kernel: wlp3s0: Limiting TX power to 14 dBm as advertised by ---
NetworkManager: (wlp3s0): supplicant interface state: associated -> 4-way handshake
NetworkManager: (wlp3s0): Activation: (wifi) association took too long
NetworkManager: (wlp3s0): device state change: config -> need-auth
NetworkManager: (wlp3s0): Activation: (wifi) asking new secrets
kernel: wlp3s0: deauthenticating from --- by local choice (Reason: DEAUTH_LEAVING)
{% endhighlight %}

`wpa_supplicant` is the open source responsible for negotiation whit RADIUS authentication server as well as many other responsibilities and it's used in Linux, BDS, Mac OS X and Windows. (see more [here][2])

The problem occurs after certain update on `wpa_supplicant`, the original version of application shipped by fc22 worked fine. You can find the version by:

{% highlight bash%}
# wpa_supplicant -v
wpa_supplicant v2.4
Copyright (c) 2003-2015, Jouni Malinen <j@w1.fi> and contributors
{% endhighlight %}

If you look at your `yum` or in this case `dnf` history, you will find that this upgrade actually happened:

{% highlight bash%}
# dnf history info wpa_supplicant
    Upgraded   wpa_supplicant-1:2.3-3.fc22.x86_64                (unknown)
    Upgrade                   1:2.4-4.fc22.x86_64                @updates
{% endhighlight %}

Now you can downgrade the `wpa_supplicant` by:
{% highlight bash%}
# dnf downgrade wpa_supplicant

# wpa_supplicant -v
wpa_supplicant v2.3
Copyright (c) 2003-2014, Jouni Malinen <j@w1.fi> and contributors
{% endhighlight %}

Now everything should work fine until the next time update your machine, to prevent `dnf` to upgrade `wpa_supplicant` you can run the following command each time you want to update the machine:
{% highlight bash%}
# dnf update --exclude=wpa_supplicant
{% endhighlight %}

Or you can make this exclusion permanent by changing `fedora-updates.repo`
{% highlight bash%}
# vim /etc/yum.repos.d/fedora-updates.repo

[updates]
metalink=https://mirrors.fedoraproject.org/metalink?repo=updates
enabled=1
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False
exclude=wpa_supplicant
{% endhighlight %}


[1]: https://www.eduroam.us/
[2]: http://w1.fi/wpa_supplicant/