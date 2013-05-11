If you want to run the bleeding edge version of sup or hack on sup,
this page is for you.

## Install git

Sup's source code is managed by [git]. You need to install it first.

## Get the source code

```sh
$ git clone git://github.com/sup-heliotrope/sup.git
$ cd sup # run later commands in this directory
```

## Install dependencies

See [[Installation: General Process]] or
[[Home#Platform-specific-instructions]] for how to install dependent
C libraries, C/C++ compiler and Ruby.

## Install dependencies in Ruby

You can use [bundler] to make this dead easy:

```sh
$ gem i bundler
$ bundle install
```

Note: if you're using [rvm], you probably want to create a dedicated
[gemset].

## Running sup

Finally:

```sh
$ ruby -Ilib bin/sup
```

If you wish to test the **development** branch (latest changes) you can do that by doing:

```sh
# After `bundle install`
$ git checkout -b develop origin/develop
$ ruby -Ilib bin/sup
```

[git]: http://git-scm.com/
[bundler]: http://gembundler.com/
[gemset]: https://rvm.io/gemsets/basics/
[rvm]: https://rvm.io/
