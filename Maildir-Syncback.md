## Introduction

Sup currently does not synchronize index state back into your maildir. So, read/unread status or favorites are not reflected in your maildir and not picked up by other email clients.

An [experimental branch](https://github.com/sup-heliotrope/sup/tree/maildir-sync) attempts to fix this (thanks to Damien Leone and [@ezyang](https://github.com/ezyang) for their previous work in this direction!). You can follow these instructions to experiment with this functionality.

## Backup your data!

Just in case :-) You should backup your mail directories, as well as your `$HOME/.sup` directory. Alternatively, you can use a separate `.sup` directory, see [[Running More Than One Sup At A Time]].

## Check out the branch
```shell
$ git clone -b maildir-sync git://github.com/sup-heliotrope/sup.git sup-maildir-sync
$ cd sup-maildir-sync
```

## Update your configuration

Syncback is disabled by default. To enable it, add this line to your `$SUP_BASE/config.yaml`:
```yaml
:sync_back_to_maildir: true
```
As soon as you add this setting, sup will warn you when you start it:
```shell
WARNING
-------

It appears that the "sync_back_to_maildir" option has been changed
from false to true since the last execution of sup.

It is *strongly* recommended that you run "sup-sync-back-maildir"
before continuing, otherwise you might lose informations in your
Xapian index.

This script should be executed each time the "sync_back_to_maildir" is
changed from false to true.

Please run "sup-sync-back-maildir -h" to see why it is useful.

Are you really sure you want to continue? (y/N)
```
You can ignore this warning at your peril, but sup will show this warning every time.

## Initial Syncback

As sup helpfully mentions, you must run a script to synchronize the state of your existing sup index back into your maildir sources. You can read the included documentation first:
```shell
$ bin/sup-sync-back-maildir --help
Export Xapian entries to Maildir sources on disk.

This script parses the Xapian entries for a given Maildir source and
renames e-mail files on disk according to the labels stored in the
index. It will hence export all the changes you made in Sup to your
Maildirs so it can be propagated to your IMAP server with offlineimap
for instance.

It also merges some Maildir flags that were not supported by Sup such
as R (replied) and P (passed, forwarded), for instance suppose you
have an e-mail file like this: foo_bar:2,FRS (flags are favorite,
replied, seen) and its Xapian entry has labels 'starred', the merge
operation will add the 'replied' label to the Xapian entry.

If you choose not to merge you will lose information because in the
previous example the file will be renamed to foo_bar:2,FS.

Running this script is *strongly* recommended when setting the
"sync_back_to_maildir" option from false to true.

Usage:
  sup-sync-back-maildir [options] <source>*

where <source>* is source URIs. If no source is given, the default
behavior is to sync back all Maildir sources.

Options include:
    --no-confirm, -n:   Don't ask for confirmation before synchronizing
      --no-merge, -m:   Don't merge new supported Maildir flags (R and P)
  --list-sources, -l:   List your Maildir sources and exit
       --version, -v:   Print version and exit
          --help, -h:   Show this message
```
Pick your options and go for it! After the import, sup won't bug you anymore.

**Congratulations!** Sup should now update your maildir correctly. If you encounter any problems, please [create an issue](https://github.com/sup-heliotrope/sup/issues) or send an email to sup-devel@rubyforge.org.