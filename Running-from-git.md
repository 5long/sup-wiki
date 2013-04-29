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