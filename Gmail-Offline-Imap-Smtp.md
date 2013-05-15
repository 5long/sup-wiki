Getting the sup mail client to work with GMail/Google Apps (IMAP+SMTP)
with local mirroring.

## Setup overview:

We'll be using Offlineimap - a tool to mirror IMAP mail in many
formats - to store our email in the Maildir format. We'll then set
up msmtp - a package to send email through SMTP - to use GMail's
system to send messages.

This tutorial is tested on Archlinux as of 2013-05-13. It should work
suitably well on most other distributions.

## Install offlineimap & msmtp

Sup doesn't transfer mail in or out of your machine. You'll need to
install these packages:

```sh
$ pacman -S offlineimap msmtp sqlite
```

> Note: If you're not familiar with `pacman` command, look [Pacman Rosetta]
  for the equivalent on your system. But the package names may vary.


## Configuring offlineImap

Create a `~/.offlineimaprc` and configure it based on the following
annotated minimal config:

```ini
[general]
accounts = personal
ui = ttyui

[Account personal]
localrepository = personal-local
remoterepository = personal-remote
status_backend = sqlite

[Repository personal-local]
type = Maildir
localfolders = ~/mail/personal
# Spaces in pathname are bad. Lets use `archive` which is a simple word
# Besides, we only need `All Mail` folder.
# Sup would manage mails on its own.
nametrans = lambda folder: {'archive': '[Gmail]/All Mail',
                            }.get(folder, folder)

[Repository personal-remote]
# IMAP with hardcoded GMail config
type = Gmail
# The path of ca-certfile might be different on your system.
sslcacertfile = /etc/ssl/certs/ca-certificates.crt
# Remember that GMail requires full mail address as username
remoteuser = user@domain.com 
remotepass = password
nametrans = lambda folder: {'[Gmail]/All Mail': 'archive',
                            }.get(folder, folder)
folderfilter = lambda folder: folder == '[Gmail]/All Mail'
# Or, if you have a lot of mail and don't want to wait for a long time before
# using sup, you can archive all your old mails on Gmail and only sync the
# inbox with the following line replacing the previous `folderfilter` line:
# folderfilter = lambda folder: folder == 'INBOX'
```

After replacing mail account and password with your own, try running:

```sh
$ offlineimap
```

Oh, if you haven't installed sup yet, you may do it now by following
[[Home#Installation]] while leaving offlineimap working hard to download
all your mails.

Now that you're back, let's continue:

## Configuring msmtp

Create a `~/.msmtprc`, it should look something like this (again, fill in
your own email account and password):

```
defaults
auth on
tls on
# Same as sslcacertfile in ~/.offlineimaprc
tls_trust_file /etc/ssl/certs/ca-certificates.crt

account personal
host smtp.gmail.com
user user@domain.com
# The value of `from` is only used when you're not using sup.
# But it is necessary if you're testing things out.
from user@domain.com 
password password
port 587

account default : personal
```

> Note: If you're feeling uncomfortable storing cleartext password on disk,
  don't worry for now. We'll come back to handle this later.

Besides, you need to run `chmod 600 ~/.msmtprc` to make the file
only readable to your user.

Now you can test if msmtp works correctly by sending yourself a mail:

```sh
$ echo Wheee! | msmtp user@domail.com
```

## Add maildir source to sup

Now you can go read the [[New User Guide]] to get a sense of how sup looks
like. Keep reading until you hit running `sup-config`, which is a wizard
to get you started.

In the process of `sup-config`, when asked to add a source,
choose "maildir" and enter `~/mail/personal/archive`. Or
`~/mail/personal/INBOX` if you choosed so.

After you're done with `sup-config`, you can start `sup` to see all your
mails!

## Receiving mails

Create a file at `~/.sup/hooks/before-poll.rb` with the following Ruby
code:

```ruby
say "Running offlineimap..."
system "offlineimap", "-o", "-u", "quiet"
```

Now sup will launch offlineimap everytime you poll for email.
Including:

* When sup starts
* When sup polls by the `poll_interval` config value
* When you asks sup to poll for mails instantly

## Configuring Sup to use msmtp

In `~/.sup/config.yaml`, for each account, search for `sendmail` and modify
the lines you've found to make them like this:

```
:sendmail: msmtp -t --read-envelop-from
```

So finally your account config looks like:

```
:accounts:
  :default:
    :signature: /home/user/.signature
    :email: username@domain.com
    :name: Your Name
    :sendmail: msmtp -t --read-envelop-from
```

Now sup should be able to send mails using msmtp. Press `c` in sup to
compose a mail to yourself to test it out.

## Further reading

By now you've successfully configured most of offlineimap, msmtp and sup.
And here're some more to read to make your mailing experience even better:

* Read the full [[New User Guide]]
* [[Securely store password]]
* [Run offlineimap outside of sup][external offlineimap]
* [[More topics on advanced sup usage|Home#Advanced-Usage]]

## Switching cold turkey

Unfortunately, any changes you make inside sup, like reading,
archiving, deleting, etc, will not be reflected in GMail. In other
words - the syncing is only one way. While there is some work in
progress to support two-way syncing, it still currently means that
the best way to use sup is to leave GMail's web interface from
regular use.

[MTA]: http://en.wikipedia.org/wiki/Message_transfer_agent
[external offlineimap]: https://wiki.archlinux.org/index.php/Offlineimap#Running_offlineimap_in_the_background
[Pacman Rosetta]: https://wiki.archlinux.org/index.php/Pacman_Rosetta
