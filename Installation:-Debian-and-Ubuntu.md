This page describes how to install sup 0.13.x on recent versions of Ubuntu
and Debian.

You might want to prepend commands with sudo where root access is needed.

* Install system packages

```bash
apt-get install build-essential libncursesw5-dev libncurses5-dev uuid-dev zlib1g-dev
```

* Install Ruby, Rubygems, and Rake

  - Install system Ruby: run `apt-get install ruby1.9.1-full`.

    Note: It is highly recommended to use Ruby 1.9.x instead of 1.8.x.
    But currently running sup on 1.8.7 is supported as well.
    In this case you'll need to install Ruby by running 

    ```bash
    # On Ubuntu:
    apt-get install ruby1.8-full rubygems rake
    # On Debian 6.0:
    apt-get install ruby-full rubygems rake
    ```

    And replace `1.9.1` in following instructions with `1.8`.

  - Install Ruby via [rvm] or [rbenv]. Note that sup 0.13.x doesn't run on Ruby 2.0.

* `rake1.8` issue

If you're using system Ruby of 1.8,
You have to do this before going for the next step:

```bash
ln -s /usr/bin/rake /usr/local/bin/rake1.8
```

* Install sup via rubygems

```bash
gem install sup # sudo is not needed if not using system ruby
# On Debian 6.0 with ruby1.9.1-full
gem1.9.1 install sup
```

If all goes well, you should be able to run `sup-config` to get started.

* Rubygems `PATH` issue on old systems

On Debian 6.0 and lower (and other old Ubuntu systems),
You'll have to manually add `/var/lib/gems/1.9.1/bin` to `PATH`
environment variable. Or you'll get an `command not found` error when
running `sup`.

## Next step

See [[New User Guide]].

## On even older versions of Debian / Ubuntu

[[See this page|Debian-Ubuntu-Hints]] if you're on a really old system
and previous instructions still doesn't work.

[rvm]: https://rvm.io/
[rbenv]: https://github.com/sstephenson/rbenv
