# RVM
[rvm](https://rvm.io/) can be used to easily set up different ruby environments for testing and running sup and gemsets:

# Installation and usage
Use installation script by [wayneeseguin](https://github.com/wayneeseguin/rvm/blob/master/binscripts/rvm-installer):
```
bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
rvm reload
rvm install 1.9.3
rvm 1.9.3
rvm gemset create sup-develop
rvm gemset use sup-develop
rvm 1.9.3 do gem intall ncursesw
rvm 1.9.3 do gem intall trollop
rvm 1.9.3 do gem intall gettext
rvm 1.9.3 do gem intall rmail
rvm 1.9.3 do gem intall lockfile
rvm 1.9.3 do gem intall mime-types
rvm 1.9.3 do gem intall xapian-ruby
git clone git://github.com/sup-heliotrope/sup.git
cd sup
ruby -Ilib bin/sup
```

or just do ```rvm 1.9.3 gem install pkg/sup-999.gem``` to get dependencies automatically.

add ~/.rvm/bin to your PATH for easy access.

Source: mailing list post by Michael Stapelberg, http://rubyforge.org/pipermail/sup-devel/2011-November/001256.html