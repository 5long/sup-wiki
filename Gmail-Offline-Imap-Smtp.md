Getting the sup mail client to work with GMail(Imap+Smtp)/Google
Apps with local mirroring

# Setup overview:

We'll be using Offlineimap - a tool to mirror IMAP mail in many
formats - to store our email in the Maildir format. We'll then set
up msmtp - a package to send email through smtp - to use GMail's
system to send messages.

# Installation

This tutorial is tested on Ubuntu 9.10 (Karmic). It should work
suitably well on most other distributions. Sup installation is done
through Ruby Gems, as that is the recommended method. First, we
install the Gems package:

     # sudo apt-get install rubygems

Now install the "sup" gem:

     # sudo gem install sup

This should take a while but complete successfully.
We also need two other packages - offlineimap and msmtp, installed
by:

     # sudo apt-get install offlineimap msmtp

# Configuring OfflineImap

Create a `~/.offlineimaprc` and configure it based on my file.
Intially, you might want to have the full ui so that you can see
what is happening. If so, comment out the line
"ui=Noninteractive.Quiet"

     [general]
     metadata = ~/.offlineimap
     accounts = Personal, Account2
     # ui = Noninteractive.Quiet # Enable this for running offlineimap in the background / as a cron job
     #
     [Account Personal]
     autorefresh = 5
     localrepository = MboxPersonal
     remoterepository = ImapPersonal
     #
     [Account Account2]
     autorefresh = 10
     localrepository = MboxAccount2
     remoterepository = ImapAccount2
     #
     [Repository MboxPersonal]
     type = Maildir
     localfolders = ~/personal/Mail?/Personal?
     #
     [Repository ImapPersonal]
     type = Gmail
     remoteuser = user@domain.com
     remotepass = password
     #
     [Repository MboxAccount2]
     type = Maildir
     localfolders = ~/personal/Mail?/Account2?
     #
     [Repository ImapAccount2]
     type = Gmail
     remoteuser = username@domain.com
     remotepass = password

Run offlineimap to download all of your messages. It should take a
few minutes to complete.

Now navigate to the path where your mail has been downloaded and
create a symlink to the folder: "[Gmail].All Mail" as "target" in
the same path. The rationale for doing this is because sup has some
issues with the maildir path when there are spaces, and other
special characters in there. You might consider using INBOX or any
of the maildirs for your seperate GMail labels.

# Configuring Msmtp

Sup can use msmtp to send messages, instead of the built-in imap
functionality. Create a `~/.msmtprc` and specify names for the
accounts. Your configuration file should look something like this:

     account personalgmail
     host smtp.gmail.com
     from user@domain.com
     auth on
     tls on
     tls_certcheck off
     user user@domain.com
     password password
     port 587
     account account2
     host smtp.gmail.com
     #
     from andy@ninjagod.com
     auth on
     tls on
     tls_certcheck off
     user username@domain.com
     password password
     port 587
     #
     account default : personalgmail

If you don't specify your password in the text file, you will have
to enter it every time you're sending an email.



# Configuring Sup

Run sup-config and follow the "wizard". When asked to add a source,
choose "maildir" and enter the complete path to the "target"
folder. Try to avoid spaces and special characters in the path, as
the paths will be converted into URI format.

Now run sup-sync, which will sync all your emails and index them.
Run sup from the console which should show you the default UI with
all your emails.



# Configuring Sup to use msmtp

In `~/.sup/config.yaml`, to each account, add the line

     :sendmail: msmtp --account=personalgmail -t

So finally your account config looks like:

     :accounts: 
       :default: 
         :signature: /home/user/.signature
         :email: username@domain.com
         :name: Your Name
         :sendmail: msmtp --account=personalgmail -t 

# Switching cold turkey

Unfortunately, any changes you make inside sup, like reading,
archiving, deleting, etc, will not be reflected in GMail. In other
words - the syncing is only one way. While there is some work in
progress to support two-way syncing, it still currently means that
the best way to use sup is to leave GMail's web interface from
regular use.