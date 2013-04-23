# Development

* [[Development:-Administration-and-Team]]

* [[Development:-Maildir-SyncBack]]
* [[Development:-UTF-8]]

## Useful pages
* [[Running More Than One Sup At A Time]]
* [[Using RVM for different ruby environments]]
* [[Programmatically Accessing Sups Index]]
* [[Contributing]]
* [[Wishlist]]
* [[Running from git]]
* [[Heliotrope and Turnsole]]

## Roadmap

## Sup-0.13
* Release sup-0.13 very soon: will _not_ include maildir support or work on Ruby 2.

## Sup-0.14
* Support Ruby 2.0.0 (milestone 2), 1.9 and 1.8
* Re-brand to the Sup developers and this infrastructure (issue #20)
* Migrate to Psych (issue #17)
  * Requires a configuration file migration
* Remove all deprecated and abandoned dependencies and components:
  * Iconv (issue #23) ([[Development:-UTF-8]])
  * Switch to Mail from RMail
  * Drop the bad ncursesw gem
* Implement IMAP syncback support ([[Development:-Maildir-SyncBack]])
* Update to version 2 of the gpgme gem
* Get UTF-8 encoding right ([[Development:-UTF-8]])

## Roles and tasks
This list shows who is the main responsible for each task, the responsibility may be shared and several will be contributing:
- Getting a release of sup-0.13 (for ruby 1.8 and ruby 1.9): 
- Managing the 'maildir-sync' branch:
- Home page:
- Setting up a consistent infrastructure:
- Managing the 'sup-two' branch: