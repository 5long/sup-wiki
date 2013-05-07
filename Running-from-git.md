You can run the bleeding edge version of sup directly from a git clone:
```shell
$ git clone git://github.com/sup-heliotrope/sup.git
$ cd sup
$ ruby -I lib bin/sup
```

If you wish to test the development branch (latest changes) you can do that by doing:
```shell
$ git clone git://github.com/sup-heliotrope/sup.git
$ cd sup
$ git checkout -b develop origin/develop
$ ruby -I lib bin/sup
```

For more information on how to work with git see for instance: http://gitref.org/ or http://git-scm.com/book.

## Missing dependencies
Either install dependencies manually or install them using bundler from the sup.git directory:
```
bundle install
```

another option is to create the sup gem: ```rake gem```, which requires some dependencies and then install the gem afterwards: ```gem i pkg/sup-999.gem```.

## Different Ruby versions
Check out the wiki page on how to use [RVM](Using-rvm-for-different-ruby-environments)