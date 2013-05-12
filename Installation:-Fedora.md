This page describes how to install sup 0.13.x on Fedora 18.
It might work on other versions of Fedora, too.

You might want to prepend commands with sudo where root access is needed.

## Install system packages

```bash
yum install gcc gcc-c++ ncurses-devel libuuid-devel libuuid zlib-devel
```

## Install Ruby, Rubygems, and Rake

  - Install system Ruby: run `yum install ruby-devel rubygem-rake`

Or:

  - Install Ruby via [rvm] or [rbenv]. Note that sup 0.13.x doesn't run on
    Ruby 2.0 yet. 1.9.3 is recommended.

## Install sup via rubygems

```bash
gem install sup
```

## Next step

See [[New User Guide]].

[rvm]: https://rvm.io/
[rbenv]: https://github.com/sstephenson/rbenv
