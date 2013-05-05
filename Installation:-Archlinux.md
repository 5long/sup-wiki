This page describes how to install sup 0.13.x on an up-to-date Archlinux
system.

* Install System packages

Install the [base-devel] group by running:

```bash
pacman -S base-devel
```

* Install Ruby, Rubygems and Rake

Unfortunately sup doesn't run on Ruby 2.0
which is in Archlinux's official repository.
You'll have to install a lower version Ruby via [rvm] or [rbenv].

For now it is recommended to use Ruby 1.9.3 to run sup.
But development to make sup run on Ruby 2.0 is [[on the way|Development#Sup-0.14]].

* Install sup via rubygems

This should be easy:

```
gem i sup
```

[base-devel]: https://www.archlinux.org/groups/x86_64/base-devel/
[rvm]: https://rvm.io/
[rbenv]: http://github.com/sstephenson/rbenv
