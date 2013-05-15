This pages describes how to store password in a secure fashion and
retrieve password from offlineimap and msmtp

## Introduce password manager

Both offlineimap and msmtp can read password from config file or
`~/.netrc`. But that doesn't sounds very secure. Luckily we have [password
managers] tackling this problem. The following instructions will guide you
configuring offlineimap and msmtp to integrate with your system's password
manager(referred to as "keyring").

First you need a keyring installed and running. For OS X, Gnome and KDE
users, you should already have the default keyring ready to serve. For
users of other desktop environment, see
<https://wiki.archlinux.org/index.php/Gnome-keyring> for how
to get gnome-keyring running.

## Access keyring from Python

Install Python and the
[python-keyring](https://pypi.python.org/pypi/keyring) module for your system:

* Archlinux: [python2-keyring on AUR](https://aur.archlinux.org/packages/python2-keyring)
* Debian / Ubuntu: [python-keyring](http://packages.debian.org/wheezy/python-keyring)
* Fedora: [python-keyring](https://apps.fedoraproject.org/packages/python-keyring)
* General method (should work on Mac OSX as well): `pip install keyring`

Optionally, if you want to use gnome-keyring / kwallet as the backend for
python-keyring, you'll need to install corresponding adapter module like
python-gnomekeyring.

Now you can store your password securely via Python:

```sh
$ python -c "import keyring; keyring.set_password('gmail', 'personal', 'PASSWORD')"
# Test that the password is successfully stored:
$ python -c "import keyring; print keyring.get_password('gmail', 'personal')"
PASSWORD
```

## Retrieve password from offlineimap

Offlineimap can run Python code to retrieve password.

Open your `~/.offlineimaprc` with your editor. Find the remote repository
and edit like this:

```ini
[general]
pythonfile = ~/.offlineimap.py

[Repository personal-remote]
remoteuser = user@domain.com
# Comment out or remove the `remotepass` line
# remotepass = password 
# Use remotepasseval instead:
remotepasseval = keyring.get_password('gmail', 'personal')
```

And create `~/.offlineimap.py` by running:

```sh
$ echo import keyring >> ~/.offlineimap.py
```

By now offlineimap should be able to read password from keyring.

## Retrieve password from msmtp

Msmtp can read password from any process's stdout. Open your `~/.msmtprc`
and edit it like this:

```
# Find the account section
account personal
# Again, comment out or remove the `password` line
# password PASSWORD
# Use passwordeval instead:
passwordeval python -c "import keyring; print keyring.get_password('gmail', 'personal')"
```

Easy, isn't it?

> Note: msmtp supports reading from gnome-keyring natively. But we are not
introducing how to do that here. It's not as portable as python-keyring.
And it'd be a little more tedious to retrieve password from offlineimap.

## See also

* Archlinux Wiki: [Offlineimap#python-keyring](https://wiki.archlinux.org/index.php/Offlineimap#python-keyring)


[password managers]: http://en.wikipedia.org/wiki/Password_manager
