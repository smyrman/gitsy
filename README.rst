==============
What is Gitsy?
==============

Do you ever operate on the same documents on multiple computers? Have you ever
wished that the document folder at work contained the same content as your
document folder at home? Then Gisty might be the tool for you!

*WARNING*: This script was created for my own private usage. It is only tested
on Linux, since that is what I use. If you want to use it on other platforms,
you are however welcome to try.

Typically I would recommend to use this tool for syncing your documents and
configuration files, but not for large binary files such as music and videos (or
anything of that kind). I would also NOT recommend that you use it to sync your
ENTIRE home directory. Please read on for more information.

The license for this software is GPL2.


===================
Install & configure
===================

I will operate with the terminology of client- and server computer here. The
client computer is where you typically work on your files, while the server is
where the git *bare repositories* will be stored; preferably a computer with a
domain name (i.e. example.com) or a static IP address. It can however also be a
mounted USB-pen or something like that!

1. First of all, Git must be installed on the computers that are going to be
   involved(server- and client computer). On your client computer, you also
   have to tell git who you are, and optionally enable colored output::

   $ git config --global user.name "your name"
   $ git config --global user.email youremail@example.com
   $ git config --global color.ui auto # optional!

2. Copy 'gitsy' to somewhere on your $PATH on your client computer (I
   personally prefer $HOME/bin/).

3. Copy 'gitsyrc' to "~/.gitsyrc".

4. Modify ~/.gitsyrc to your liking i.e. specify REMOTE_HOST, REMOTE_USER,
   REMOTE_DIR.

5. Make sure that the directory described by REMOTE_HOST, REMOTE_USER and
   REMOTE_DIR exist.

6. **Recomended**: Issue the following commands (replace REMOTE_USER and
   REMOTE_HOST)::

   $ ssh-keygen
   $ ssh-copy-id REMOTE_USER@REMOTE_HOST


=====
Usage
=====

All the Gitsy commands you need, are::

$ gitsy init
$ gitsy clone
$ gitsy sync

Most commands are explained in detail below.


gitsy init: Adding a directory to be synced by Gitsy
----------------------------------------------------

1. To setup a new directory to be synced by Gitsy, do the following on your
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

*IMPORTANT*: If you want to initialize and add a repository that is not located
in $HOME, you must do this manually::

    $ cd path/to/repo/projectx
    $ git init
    $ git remote add origin user@example.com:your_repos_dir/projectx
    $ ssh user@example.com git init --bare your_repos_dir/projectx
    $ cd
    $ edit .gitsyrc # add path/to/repo/projectx to REPOS (which is a bash array).


gitsy clone: Initially download a repository to a new machine
--------------------------------------------------------------

1. On a second machine (say for instance at the machine configured as the
   REMOTE_HOST on the first machine), setup Gitsy as described in the *Install
   & configure* section. Set up REMOTE_HOST="", REMOTE_DIR="$HOME/repos" (or
   whatever folder you used when you configured the 1. client).

2. Since 'project1' already exist in $HOME/repos, we will now issue::

    $ gitsy clone project1

This will clone project1 to $HOME, and add project1 to REPOS.


The rest of GiTsY's commands
----------------------------

The rest of GiTsY's commands, I will only explain briefly::

    gitsy status - show a summary of all changes done so far
    gitsy sync   - a shortcut for "gitsy pull && gitsy push"
    gitsy pull   - pull changes from REMOTE_HOST
    gitsy push   - push changes to REMOTE_HOST
    gitsy help   - print help text

See some quick tips and examples of usage:
http://wiki.github.com/smyrman/gitsy/


====================
Oh No! I Screwed up!
====================

GiTsY is just a simple layer on top of Git. If you do something wrong; like
configuring an invalid REMOTE_HOST in *./gitsyrc* before you called the *gitsy
init* command, or if you get merging issues; you will have to correct the error
in the projects' git repositories. Please see the Git documentation
(http://git-scm.com/doc) for more details.

Here are some useful commands that may help::

    $ edit path/to/your/project/.git/config
    $ git help
