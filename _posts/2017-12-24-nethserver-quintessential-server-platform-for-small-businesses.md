---
layout: post
title: 'NETHSERVER: Quintessential Server Platform for Small Businesses'
date: '2017-12-24 03:28:36'
desc: 'NETHSERVER: Quintessential Server Platform for Small Businesses'
keywords: 'Shabbir,Hasan,Shabbir-Hasan,Jekyll,gh-pages,website,blog'
categories:
  - Linux
tags:
  - Nethserver
  - Linux
  - Webserver
  - Server
icon: icon-centos
---
If you run a small business, you might need an in-house operating system to serve as a veritable multi-tool. Many businesses opt for Microsoft Windows Small Business Server. However, if you’re looking to cut costs and work with open source software, you have plenty of choices, each of which can perfectly function to meet your small business needs.

<p align="center"><img src="{{ site.img_path }}/linux/AI-Artificial-Intelligence.jpg" alt="Thanks to Stefan Stefancik for making this photo available freely on @unsplash" height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Nethserver: Follow these steps to get started with Nethserver and see if it doesn’t make for an outstanding solution for your small business. Originally published at* [Linux.com](https://goo.gl/TrzkJU)

One such option is the CentOS 7 based [Nethserver](http://www.nethserver.org/). It’s an outstanding small business platform that’s flexible enough to be just what you need and nothing more. Once installed, you can add the software necessary make business happen. Nethserver is quick to install, easy to set up, and simple to manage.

----------

# The Versions

When you visit the official site of Nethserver, you will notice there are different versions of the platform. Specifically, a Community and an Enterprise edition. I’m going to be discussing the Community options as it offers plenty of features, is community supported, and free.

Did I say “plenty of features”? I did. The feature list for the Community edition of Nethserver includes:

- Easy to use web-based interface
- List item
- Software Center, where you can add only the packages you need
- Full data backup
- Manual upgrades
- CentOS 7 foundation for solid security and reliability
- Built-in Samba Active Directory Controller
- Nextcloud integration
- Certificate management
- Transparent HTTPS proxy
- Greatly improved firewall
- Built-in email server

The Software Center especially should appeal to many administrators and business owners. Why? Because Nethserver allows you to install only what you need to make your workflow manageable and easy.

----------

# Let’s Install Nethserver

Installing Nethserver is as easy as installing [CentOS 7](https://www.centos.org/download/). In fact, it’s exactly like the installation of everyone’s favorite open source server platform. If you’ve installed CentOS 7, you won’t have any trouble installing Nethserver. And the installation of the basic platform can be completed in about five to ten minutes.

Once you’ve downloaded the Nethserver ISO, burn it to a disk or USB drive, or place it in a directory your virtual machine platform can access. I’ll be installing Nethserver via VirtualBox, so there is at least one small variation to the installation. Said variation is making sure to set the Networking option (in [VirtualBox](https://www.virtualbox.org/)) to Bridged mode (otherwise, the machines on your network will not be able to reach your Nethserver instance). Other than that, boot the Nethserver ISO and begin the installation.

As you can see (Figure 1), the Nethserver installation doesn’t change anything from CentOS 7.

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_1.jpg" alt="Figure 1: A familiar face for the installation should make Nethserver a cinch to get up and running" height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 1: A familiar face for the installation should make Nethserver a cinch to get up and running.*

After completing the basic installation, Nethserver will reboot. Upon rebooting, you will need to log in with the credentials you created during the installation. Once authenticated, you will be dropped into a bash prompt. Chances are, you may have not configured networking to use a static address. If that’s the case, issue the command ifconfig from the prompt to find your Nethserver IP address (we’ll change it to static in a bit).

With that IP address in hand, point a browser (on the same network) to https://SERVER_IP (Where SERVER_IP is the actual address of your Nethserver machine). In the next few screens you will need to answer some fairly simple questions. The first of these screens is just to welcome you to the setup wizard. Click NEXT. In the resulting window, you are asked if you want to skip the manual configuration and restore a backup file (Figure 2).

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_3.jpg" alt="Figure 2: You can restore Nethserver from a backup file" height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 2: You can restore Nethserver from a backup file.*

If this is a new installation, keep the box unchecked and click NEXT.

It’s time to set a fully qualified domain name. This is especially important for two reasons:

- If you need to access this server from outside your LAN.
- If you will need to send email from Nethserver to the outside world.

Chances are, you’re going to need that FDQN here (Figure 3). If you don’t have one, you can always use something like nethserver.localhost.localdomain and use the box for test purposes. However, once you need to start using Nethserver as a real business-class solution, you’ll need that FDQN.

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_4.jpg" alt="Figure 3: Setting your FDQN." height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 3: Setting your FDQN.*

The next few screens require you to do the following:

Set your timezone.

Set the SSH port (the default is 22, Nethserver recommends using port 2222).

Agree (or disagree) to send usage statistics.

Once you’ve completed the above screens of the wizard, you will land on the main Nethserver page, where you will be prompted to change the server from a DHCP to Static IP address (Figure 4).

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_5.jpg" alt="Figure 4: Switching the network interface from DHCP to Static." height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 4: Switching the network interface from DHCP to Static.*

Click the Edit button, select static (when prompted), and fill out the details for the static address (Figure 5).

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_9.jpg" alt="Figure 5: Setting the details for a static address." height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 5: Setting the details for a static address.*

Once you’ve done that, you will see a DNS tab, where you can set the necessary DNS servers. Chances are, Nethserver picked up the DNS servers from your network’s router. If you find Nethserver cannot reach the outside world, make sure to visit the DNS option and make that change.

----------

# Adding Software

At this point, you need to install software. To do this, click on the Software Center entry under Administration. The first time you click the Software Center entry, it will take some time for it to populate the titles, before it becomes available. Give it time and the Software Center will finally appear, ready for you to install everything you need (Figure 6).

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_10.jpg" alt="Figure 6: The Nethserver Software Center." height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 6: The Nethserver Software Center.*

As this is a new installation, you will probably be informed of available updates. Before you install any software, click on the Updates tab and then click DOWNLOAD AND INSTALL. I will warn you that, because this is a new install, the upgrade process can take some time. Step away from the keyboard and undertake some other task. When you come back, you will probably see yet another DOWNLOAD AND INSTALL button. I recommend rebooting before you attempt to download and install the next upgrade. Keep repeating that until there are no more updates to download and install. Once there are no more updates, comb through the listing of software and install everything you need to make Nethserver your perfect small business server.

----------

# Adding Users

Adding users for Nethserver isn’t quite the same as it might be on other Linux servers. You must first decide the method to be used to serve as the user directory. When you go to Management > Users and groups, you will be prompted to select between LDAP and Active Directory (Figure 7).

<p align="center"><img src="{{ site.img_path }}/linux/nethserver_11.jpg" alt="Figure 7: Selecting your directory service for users and groups." height="auto" width="75%" max-height="100%" max-width="100%"><!-- .element height=auto width=auto max-height=100% max-width=100% --></p>
*Figure 7: Selecting your directory service for users and groups.*

The route you choose will depend upon your needs. If you select LDAP, you will then have to set up a local LDAP server or bind a remote LDAP server. If you go the Active Directory route, you will have to either join a domain or create a new domain. Once you’ve either created a new local LDAP server or created a new Domain, you can then begin the process of adding users and groups.

----------

# Make it yours

That’s the gist of getting Nethserver up and running. Beyond that point, you will have to install and configure the server to make it perfectly fit your small business needs. Nethserver is very powerful and could easily take the place of the more costly Microsoft Small Business server. Give Nethserver a go and see if it doesn’t make for an outstanding solution for your business.
