sup stores your email sources in a YAML file called `$HOME/.sup/sources.yaml`. There are two ways to define sources: editing that file directly, or using the `sup-add` script.

## Using the sup-add script
This is the safest way to add sources. Call `sup-add --help` to see the options.
```shell
$ sup-add --help
Adds a source to the Sup source list.

Usage:
  sup-add [options] <source uri>+

where <source uri>+ is one or more source URIs.

For mbox files on local disk, use the form:
    mbox:<path to mbox file>, or
    mbox://<path to mbox file>

For Maildir folders, use the form:
    maildir:<path to Maildir directory>; or
    maildir://<path to Maildir directory>

Options are:
            --archive, -a:   Automatically archive all new messages from these sources.
            --unusual, -u:   Do not automatically poll these sources for new messages.
         --labels, -l <s>:   A comma-separated set of labels to apply to all messages from this source
          --force-new, -f:   Create a new account for this source, even if one already exists.
  --force-account, -o <s>:   Reuse previously defined account user@hostname.
            --version, -v:   Print version and exit
               --help, -h:   Show this message
```
For example, to add a new maildir source:
```shell
$ sup-add maildir:/home/User/Mail/Inbox
Adding maildir:/home/User/Mail/Inbox...
[2013-04-20 21:32:53 +0200] Flushing Xapian updates to disk. This may take a while...
```
You can experiment with other options, e.g. to add labels automatically to mail from that source.

## Editing sources.yaml directly

Executing the command above will add this to your `sources.yaml`:
```yaml
--- 
- !masanjin.net,2006-10-01/Redwood/Maildir 
  uri: maildir:/home/User/Mail/Inbox
  usual: true
  archived: false
  id: 1
  labels: []
```

Using this information, you can add a second source manually, e.g. for the sup-talk mailing list. Entries will be archived and labelled with `sup` and `list` automatically:
```yaml
--- 
- !masanjin.net,2006-10-01/Redwood/Maildir 
  uri: maildir:/home/User/Mail/Inbox
  usual: true
  archived: false
  id: 1
  labels: []
- !masanjin.net,2006-10-01/Redwood/Maildir 
  uri: maildir:/home/User/Mail/sup-talk
  usual: true
  archived: true
  id: 2
  labels:
  - sup
  - list
```