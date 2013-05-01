This page describes how to install sup 0.13.x on recent versions of Ubuntu.

The following steps are tested on Ubuntu 13.04 and should probably work on 12.04 / 12.10, too.

* Install system packages

```bash
sudo apt-get install build-essential libncursesw5-dev libncurses5-dev uuid-dev zlib1g-dev
```

* Install Ruby and Rubygem

  - Use system Ruby: run `sudo apt-get install ruby1.9.1-full`.
    It is highly recommended to use Ruby 1.9.x instead of 1.8.x.
    But currently running sup on 1.8.7 is supported as well.
    In this case you'll need to install Ruby by executing `sudo apt-get install ruby1.8-full`

    Or:

  - Use [rvm] or [rbenv]. Note that sup 0.13.x doesn't run on Ruby 2.0.

* Install sup via rubygem

```bash
sudo gem install sup # sudo is not needed if not using system ruby
```

If all goes well, you should be able to run `sup-config` to get started.

[rvm]: http://rvm.io/
[rbenv]: http://github.com/sstephenson/rbenv