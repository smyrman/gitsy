==============
What is Gitsy?
==============

Gitsy is a file synchronization tool based on Git. Basically it allows you
to synchronize multiple git repositories simultaneously by the use of push,
pull and automated commits.

The tool can act as a replacement for tools like Dropbox, but it requires you to
have your own server, and the synchronizations must in principal be run
manually from the command line. It is possible to set up a cron job to
run 'gitsy sync' regularly, but this would make it harder for you to discover
merge conflicts. A better approach would be to create GUI frontend to Gitsy
that creates a message for merge conflicts and updates (please feel free to
contact me if you are interested in developing such a thing).

I would not recommend you to use gitsy to sync large binary files such as
photos, music or videos.

The license for this software is GPL2 (the same license as Git).


===================
Install & configure
===================

This guide assumes basic knowledge of Git and Unix-like systems.

 1. Copy the gitsy bash script to somewhere on your PATH.
 2. Find a place to set up your bare repositories.
 3. Configure Git and clone all repositories to be synced by gitsy to somewhere on your computer.
 4. Create the file ~/.gitsyrc, and make it look something like this::

    # file ~/.gitsyrc
    # All paths in REPOS should be given relative to your home folder.
    REPOS = ('doc' '.config' 'some/deep/path')

A nice tip would be to create .gitignore files inside your repositories to
avoid synchronizing editor backup files (.*~), hidden folders (.*), etc.

=====
Usage
=====

The most commonly used command will be:

$ gitsy sync   - Synchronize your repositories (auto commit, pull, push)
$ gitsy status - Show the Git status for all repositories

To see what other commands gitsy offers, issue 'gitsy help'.

