# How-To: Installing MysticBBS on Debian-testing
(Debian Bullseye at time of writing)

Also published locally as "mysticbbs_install_debian_bullseye.txt" 

### v20190901 Copyright 2019 Bradley D. Thornton - http://NorthTech.US

#### Licensed under the CC BY-SA 3.0 license here:
https://creativecommons.org/licenses/by-sa/3.0/  

+---------------------------------------+
| Mystic BBS install/config/maintenance |
+---------------------------------------+

# mystic stuffs

The following cookbook style tutorial will take you through all of the
Steps required to install your Mystic Internet Server (Mystic BBS).

All of the steps, in the order they occur here, have been tested and
verified to work on a fresh Debian GNU/Linux install as of the date of
this version of the document - you can perform the following on bare
metal or a virtual machine in the cloud (VPS).

The particular versions this has been tested with are:

Debian 10 (Buster)

Debian Testing (Bullseye)

Let's get started, shall we?

------------------------------------------------------------------------

# firewall!!!

# Before anything we need a firewall, so let's install a utility to
# manage our firewall and open up the ports for ssh, telnet, and mail

apt-get install -y ufw
ufw allow 22
ufw allow 23
ufw allow 25
ufw status enable
systemctl start ufw

* bullet one
* bullet two

------------------------------------------------------------------------
# Installing Mystic BBS

These instructions presume starting in /root as root

Instructions to be completed by the mystic user will start with a "$" 

I choose to install mystic in /home/mystic and have it run as that user.

Since the install script (which you run as root) can't install mystic
into a pre-existing dir, this is one way to get around that:

adduser mystic # go ahead and add the user with a shell or noshell.

cp -av /home/mystic /home/mysticb # or mv, preserves the user's dotfiles

# if you used cp -a, then remove the directory now:

rm -frv /home/mystic # install script will recreate this dir

mkdir -pv stufff/mystic # We're going to put the package there

cd stuff/mysticbbs

wget http://www.mysticbbs.com/downloads/mys112a43_l64.rar # prolly

apt-cache install -y unrar # we need this to explode the archive

unrar e mys112a43_l64.rar # a/o 20190831, this is the latest tarball

./install # choose /home/mystic/ and down arrow - autofils other dirs

chown -Rv mystic:mystic /home/mystic

cp -av /home/mysticb/* /home/mystic/ # just moves back the dotfiles

rm -frv /home/mysticb

cd /home/mystic

./mystic -l # create new user (you). Then run mystic -cfg after exiting

./mystic -cfg # and configure your BBS (is beyond the scope of this doc) 


------------------------------------------------------------------------

# Instructions at the end of the install script - keep this info ;)

<snip>
Switch to the Mystic directory (/home/mystic/) and then:

Please read the Wiki at wiki.mysticbbs.com for installation
instructions and notes on using Mystic under Linux

Type "./mystic -l" to login (do this first to make your SysOp account)
Type "./mystic -cfg" to run the configuration utility
</snip>

# don't do any of this yet - instead, complete the steps below and then
# read the tl;dr at the very end so you know exactly what you need to
# do once you start your Mystic Internet Server (aka, your shiney BBS!)

# hint: There's some kewl videos by Paul Hayton to walk you through
# all of those steps!


------------------------------------------------------------------------

System Directory /home/mystic/
              Data Directory /home/mystic/data/
              Text Directory /home/mystic/text/
              Menu Directory /home/mystic/menus/
          Msg Base Directory /home/mystic/msgs/
         Semaphore Directory /home/mystic/semaphore/
            Script Directory /home/mystic/scripts/
            Attach Directory /home/mystic/attach/
              Logs Directory /home/mystic/logs/

------------------------------------------------------------------------

# supporting ssh:

# make sure we have gcc installed by entering, "which gcc". If not, then

apt-get -y install build-essential manpages-dev

# Now we need to get Cryptlib, then compile and install the shared .so

mkdir -pv /root/stuff/cryptlib; cd root/stuff/cryptlib

wget https://cryptlib-release.s3-ap-southeast-1.amazonaws.com/cryptlib345.zip

unzip -a cryptlib345.zip

make shared

# now let's move the shared Cryptlib library to the proper place
# and rename it so Mystic can find it.

cp -v libcl.so.3.4.5 /usr/lib/x86_64-linux-gnu/libcl.so 

# More info on the how and why can be found in the mystic wiki here:
# http://wiki.mysticbbs.com/doku.php?id=cryptlib

------------------------------------------------------------------------

# Spellcheck

# Let's start by installing the depdendancy, the libhunspell package:
apt-get -y install libhunspell-dev

$ mkdir -pv /home/mystic/stuff/spellcheck 
$ cd /home/mystic/data/stuff/spellcheck

$ wget http://www.mysticbbs.com/downloads/mystic_spellcheck_v2.zip

$ unzip mystic_spellcheck_v2.zip

$ cp -v dictionary.aff /home/mystic/data/
$ cp -v dictionary.dic /home/mystic/data/
$ cp -v wordlist.txt /home/mystic/data/

# You can customize your dictionary by adding commonly used words not
# already in the dictionary one word per line, in wordlist.txt




------------------------------------------------------------------------

# set up custom country ban capabilities

$ mkdir -pv /home/mystic/stuff/ip2location
$ cd /home/mystic/stuff

$ wget http://download.ip2location.com/lite/IP2LOCATION-LITE-DB1.IPV6.BIN.ZIP

$ unzip IP2LOCATION-LITE-DB1.IPV6.BIN.ZIP

$ cp -v IP2LOCATION-LITE-DB1.IPV6.BIN /home/mystic/data/

------------------------------------------------------------------------



------------------------------------------------------------------------

# You are done!

# Ready to launch! But before you do, you need to watch a couple of
# videos by Paul Hayton, which covers in detail the installation we've
# just completed (but he demonstrates on a Pi)

# the tl;dr 

+---------------------------------+
| Install (Pi) I, II, First Steps | 
+---------------------------------+

https://www.youtube.com/watch?v=kBlH2hwxuzQ

https://www.youtube.com/watch?v=rOgt4gnUR9k

https://www.youtube.com/watch?v=uzWf6ezc5lU

------------------------------------------------------------------------



------------------------------------------------------------------------



------------------------------------------------------------------------



------------------------------------------------------------------------



