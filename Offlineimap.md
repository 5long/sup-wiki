See an example here: [[Gmail-Offline-Imap-Smtp]]

## Folder translation
Warning: These settings might eat your mail.

To be able to correctly map folders both remotely and locally (in the case you are syncing back maildir to IMAP) the inverse folder translation has to match exactly and uniquely your local translation. By using a following setup it is possible to copy the dictionary between the two repositories:
```
[Repository RemoteGmail]
type = Gmail
nametrans: lambda s: {  '[Gmail]/Starred' : 'starred',
                        '[Gmail]/Trash'   : 'trash',
                        '[Gmail]/Spam'    : 'spam',
                        '[Gmail]/Sent Mail' : 'sent',
                        '[Gmail]/Important' : 'important',
                        '[Gmail]/Drafts'    : 'drafts',
                        '[Gmail]/All Mail'  : 'archive',
                        'INBOX'             : 'inbox',
                      }.get (s, s).decode ('imap4-utf-7').encode ('utf8')


[Repository LocalGmail]
type = Maildir
nametrans: lambda s: dict((value,key) for key,value in
                      {  '[Gmail]/Starred' : 'starred',
                         '[Gmail]/Trash'   : 'trash',
                         '[Gmail]/Spam'    : 'spam',
                         '[Gmail]/Sent Mail' : 'sent',
                         '[Gmail]/Important' : 'important',
                         '[Gmail]/Drafts'    : 'drafts',
                         '[Gmail]/All Mail'  : 'archive',
                         'INBOX'             : 'inbox',
                      }.iteritems()).get (s, s).decode ('utf8').encode ('imap4-utf-7')

```

Note that for some users, the gmail folders are like `[Google Mail]/All Mail` rather than `[Gmail]/All Mail`

This example uses the 'utf7.py' script described in the section below. It might be possible to extract the folder translation so that is not necessary to write it two times.

### UTF-8 issues
IMAP has its own kind of encoding (IMAP-4-UTF-7), using a pythonfile it is possible to translate this into regular UTF-8. This has to be done both ways (Local and Remote) so that the folders map correctly. This gist holds an implementation of this conversion: https://gist.github.com/gauteh/5402888

After downloading it can be loaded with a:
```
[general]
pythonfile = path/to/utf7.py
```

Some more information on UTF-8: [[UTF-8 issues]]