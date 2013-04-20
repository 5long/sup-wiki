# RVM
[rvm](https://rvm.io/) can be used to easily set up different ruby environments for testing and running sup and gemsets:

# Installation and usage
Use installation script by [wayneeseguin](https://github.com/wayneeseguin/rvm/blob/master/binscripts/rvm-installer):
```
bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
rvm reload
rvm install 1.9.3
rvm 1.9.3
rvm gemset create sup2011-11-18
rvm 1.9.3 at sup2011-11-18
rvm 1.9.3 at sup2011-11-18 do gem install ncursesw
rvm 1.9.3 at sup2011-11-18 do gem install trollop
rvm 1.9.3 at sup2011-11-18 do gem install gettext
rvm 1.9.3 at sup2011-11-18 do gem install rmail
rvm 1.9.3 at sup2011-11-18 do gem install lockfile
rvm 1.9.3 at sup2011-11-18 do gem install mime-types
rvm 1.9.3 at sup2011-11-18 do gem install xapian-ruby
git clone git://github.com/sup-heliotrope/sup.git
cd sup
ruby -Ilib bin/sup
```

add ~/.rvm/bin to your PATH for easy access.

Source: mailing list post by Michael Stapelberg, http://rubyforge.org/pipermail/sup-devel/2011-November/001256.html