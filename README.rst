==============
What is GiTsY?
==============

Do you ever operate on the same documents on multiple machines? Have you ever
wished that the document folder at work contained the same content as your
document folder at home? Then GiTsY might just be the tool for you!

*WARNING*: This script was created for my own private usage, but it might be of
use to others as well (for example you). It is only tested on Linux, since that
is what I use.

Typically I would recommend to use this tool for syncing your documents and
configuration files, but not for large binary files such as music and videos (or
anything of that kind). I would also NOT recommend that you use it to sync your
ENTIRE home directory. Please read on for more information.

The license for this software is GPL2.

===================
Install & configure
===================

1. First of all, git must be installed and configured, i.e., after installing
   git, issue::

   $ git config --global user.name "your name"
   $ git config --global user.email youremail@example.com
   $ git config --global color.ui auto # optional!

2. Copy 'gitsy' to somewhere on your $PATH (I personally prefer $HOME/bin/).

3. copy 'gitsyrc' to "$HOME/.gitsyrc".

4. modify .gitsyrc to your liking i.e. specify (REMOTE_HOST, REMOTE_USER,
   REMOTE_DIR)

5. make sure that the directory described by REMOTE_HOST, REMOTE_USER and
   REMOTE_DIR exist.


=====
Usage
=====

Given that you have used ssh-keygen and ssh-copy-id to allow no-password login
to the remote host (Not required, but recommended as it will save you from
typing your password a million times), You are ready to go!

All the GiTsY commands you need, are::

$ gitsy init
$ gitsy clone
$ gitsy sync

Moste commands are explained in detail below.

gitsy init: Adding a directory to be synced by GiTsY
----------------------------------------------------

1. To setup a new directory to be synced by GiTsY, do the following on your
   local machine (given that the folder $HOME/project1 exist, and include some
   content other then empty folders)::

    $ gitsy init project1

   This command will set up a git repository at "$HOME/project1", and a 'bare'
   repository at "$REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR/project1.git". It will
   also use 'sed' to add project1 to the REPOS variable in "$HOME/.gitsyrc".

2. Next thing is to sync the repository to the remote host::

    $ gitsy push
    OR
    $ gitsy sync

*IMPORTANT*: If you wan't to initialize and add a repository for sync, not
located in $HOME, you must do this manually::

    $ cd path/to/repo/projectx
    $ git init
    $ git remote add origin user@example.com:your_repos_dir/projectx
    $ ssh user@example.com git init --bare your_repos_dir/projectx
    $ cd
    $ edit .gitsyrc # add path/to/repo/projectx to REPOS (which is a bash array).

gitsy clone: Initially download a repository to a new machine
--------------------------------------------------------------

1. On a second machine (say for instance at the machine configured as the
   REMOTE_HOST on the first machine), setup GiTsY as described in Install & configure.
   Set up REMOTE_HOST="", REMOTE_DIR="$HOME/repos" (or whatever folder you use instead
   of repos).

2. Since 'project1' already exist in $HOME/repos, we will now issue::

    $ gitsy clone project1

This will clone project1 to $HOME, and add project1 to REPOS.



The rest of GiTsY's commands
----------------------------

The rest of GiTsY's command's I will onliy explain briefly::

    gitsy status - show a summary of all changes done so far
    gitsy sync   - a shortcut for "gitsy pull && gitsy push"
    gitsy pull   - pull changes from REMOTE_HOST
    gitsy push   - push changes to REMOTE_HOST
    gitsy help   - print help text

Some quick usage tips
http://wiki.github.com/smyrman/gitsy/

===============================
Oh No! I (or GiTsY) Screwed up!
===============================

GiTsY is just a simple layer on top of git. If you do something wrong; like
configuring an invalid REMOTE_HOST in *./gitsyrc* before you called the *gitsy
init* command, you will have to correct the error in the projects' git
repositories::

    $ edit path/to/your/project/.git/config
    $ edit $HOME/.gitconfig
    $ git help

