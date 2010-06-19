==================
What is <-GiTsY->?
==================

Gitsy is a simple and stupid backup and synchronization tool on top of Git. It
allows you to push and pull multiple repositories between a local and a remote
host, or between two local hosts; it will automatically add and commit all
changes to all tracked repositories before each push.

*WARNING*: This script was created for my own private usage, but it might be of
use to others as well (for example you). It is released under the same license
as Git, which is GPL2.

Typically I would recommend to use this tool for syncing your documents and
configuration files, but not for large binary files such as music and videos (or
anything of that kind). I would also NOT recommend that you use it to sync your
entire home directory.

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

4. modify .gitsyrc to your liking.

5. make sure that the directory described by REMOTE_HOST, REMOTE_USER and
   REMOTE_DIR exist.


=====
Usage
=====

Given that you have used ssh-keygen and ssh-copy-id to allow no-password login
to the remote host (Not required, but recommended as it will save you from
typing your password a million times), You are ready to go!

Adding a directory to be synced by gitsy
----------------------------------------

1. To setup a new directory to be synced by gitsy, do the following on your
   local machine (given that the folder $HOME/project1 exist, and include some
   content other then empty folders)::

    $ gitsy init project1

   This command will set up a git repository at "$HOME/project1", and a 'bare'
   repository at "$REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR/project1.git". It will
   also use 'sed' to add project1 to the REPOS variable in "$HOME/.gitsyrc".

2. Next thing is to push the repository to the remote host::

    $ gitsy push

*IMPORTANT*: If you wan't to initialize and add a repository for sync, not
located in $HOME, you must do this manually::

    $ cd path/to/repo/projectx
    $ git init
    $ git remote add origin user@example.com:your_repos_dir/projectx
    $ ssh user@example.com git init --bare your_repos_dir/projectx
    $ cd
    $ edit .gitsyrc # add path/to/repo/projectx to REPOS (which is a bash array).


But that only gives me backup :*( I wanted sync!
------------------------------------------------

1. On a second machine (say for instance at the server configured to
   REMOTE_HOST in 5), setup gitsy as described in Install & configure.  Set up
   REMOTE_HOST="", REMOTE_DIR="$HOME/repos" (or whatever folder you use instead
   of repos).

2. Since 'project1' already exist in $HOME/repos, we will now issue::

    $ gitsy clone project1

This will clone project1 to $HOME, and add project1 to REPOS.



The rest of gitsy's commands
----------------------------

* gitsy status
* gitsy pull
* gitsy help

====================
OH NO, I SCREWED UP!
====================

Gitsy is simple and stupid, and if you do soemthing wrong, (like configuring an
invalid host name) you will have to fix it yourself. Some recomendations::

    $ edit $HOME/.gitconfig
    $ edit path/to/your/project/.git/config
    $ git help

Wiz1t wk t0 << some QiK tipz:
http://wiki.github.com/smyrman/gitsy/
