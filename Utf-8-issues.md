# UTF-8 Issues

## OfflineIMAP and folders with incorrect character encoding

For IMAP folders to show up with correct characters they can be translated to UTF-8. IMAP folders are using an special version of UTF-7. The blog post here describes how to do that translation using pythonfiles and nametrans (remember to do the reverse nametrans when and if you are syncing back): http://piao-tech.blogspot.no/2010/03/get-offlineimap-working-with-non-ascii.html#resources

Essentially you create a translation module for IMAP-UTF-7 to UTF-8 and uses nametrans on the remote source to translate to UTF-8 (from IMAP-UTF-7) and nametrans on the local source to translate from UTF-8 to IMAP-UTF-7.