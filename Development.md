# Development

## Useful pages
* [[Running More Than One Sup At A Time]]
* [[Programmatically Accessing Sups Index]]
* [[Contributing]]
* [[Wishlist]]
* [[Running from git]]
* [[Heliotrope and Turnsole]]

## Roadmap

The roadmap to the next sup goes like this:

* Support Ruby 2.0.0 (milestone 2), 1.9 and 1.8
* Re-brand to the Sup developers and this infrastructure (issue #20)
* Migrate to Psych (issue #17)
  * Requires a configuration file migration
* Remove all deprecated and abandoned dependencies and components:
  * Iconv (issue #23)
  * Switch to Mail from RMail
  * Drop the bad ncursesw gem
* Implement IMAP syncback support
* Update to version 2 of the gpgme gem
* Get UTF-8 encoding right
* Get some response from the owners or get some control on the official infrastructure (we don't want to fork away, but rather continue the project) (comments?)
  * Mailing lists
  * Gem
  * Original web page?