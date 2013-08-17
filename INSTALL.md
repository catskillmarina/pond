Building
========

These are very simple build instructions that will get Pond running from source. There are no binaries because that would suggest that it's reasonable for non-experts to be playing with Pond and that's not currently the case.

You should read the commands before running them because, if nothing else, it'll put your GOPATH in ~/gopkg, which may not be where you want.

Ubuntu 13.04
============

Ubuntu only has Go 1.0, but that's sufficient (although future commits may accidently break compatibility because I develop with 1.1).

sudo apt-get install golang git libgtk-3-dev libgtkspell-3-dev libtspi-dev trousers tor mercurial
cd
mkdir gopkg
export GOPATH=$HOME/gopkg
go get github.com/agl/pond/client
go install github.com/agl/pond/client
$GOPATH/bin/client

Fedora 19
=========

Fedora's golang package appears to be completely broken, so this installs Go from source.

sudo yum install gtk3-devel gtkspell3-devel gcc trousers-devel git mercurial tor
sudo systemctl start tor
cd
hg clone https://code.google.com/p/go
cd go/src
./all.bash
cd
export PATH=$PATH:$HOME/go/bin
mkdir gopkg
export GOPATH=$HOME/gopkg
go get -tags fedora github.com/agl/pond/client
go install github.com/agl/pond/client


Debian 7.1 (wheezy)
===================
sudo apt-get install libgtk-3-dev libpango1.0-dev libgtkspell0 libgtkspell-dev golang git libtspi-dev trousers tor mercurial libgtkspell-3-dev
cd
mkdir gopkg
export GOPATH=$HOME/gopkg
go get -v github.com/agl/pond/client
$GOPATH/bin/client


Arch
====

I don't have tested instructions for Arch, but one thing that will go wrong is 
that Arch's Trousers build enables a GTK-2 based UI. Pond uses GTK-3 and one 
cannot link GTK 2 and 3 into the same binary. You'll need the trousers package 
from AUR and you'll need to edit the PKGBUILD so that the configure command has --with-gui=openssl, not --with-gui=gtk. Then makepkg and pacman -U as normal. 
In order to actually use the TPM, you'll need to systemctl start tcsd.

OS X
====

It's possible to get Pond building on OS X after spending lots of time with 
homebrew. I'm afraid that I didn't keep a log of all the commands but, if you 
get gtk+3 and gtkspell3 built, then it should work. See the Fedora section for 
instructions on fetching and building Go itself.


(Unless you brew edit gtk+3 and add --enable-quartz-backend to the configure 
arguments, you'll need an X server running in order to launch Pond.)

However, unless you are already familiar with GTK development on OS X, I'd 
suggest using a Linux machine at this point.

WARNING: there are no TPM chips in Macs and, since they generally use SSDs, 
which are log structured internally, messages cannot be safely erased. There 
is a firmware NVRAM on Macs that could be used for erasure storage, but I 
haven't written support for that yet.
