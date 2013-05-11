This page describes general install process of sup.

If your OS is not listed in [[Home#Platform-specific-instructions]],
this page might be helpful.

## C libraries

The following C libraries and their header files are required.
It's recommended to install them via your system's native package manager
like [rpm].

* ncursesw
* uuid
* zlib

Also, a C/C++ compiler like GCC or clang is needed.

## Ruby

Sup runs on [MRI Ruby](http://www.ruby-lang.org/).
Currently both 1.8.7 and 1.9.3 are supported but it's highly recommended to
use 1.9.3.

Also, ruby's header files are required since we need to build some
[extenions].

You can install Ruby via either your system's native package manager or
3rd-party tools like [rvm] or [rbenv].

## Rubygems

[Rubygems] is the package manager for Ruby. It is distributed with Ruby
since 1.9+.

## Rake

Required when installing ruby-xapian binding.
This is part of Ruby distribution since 1.9+.

Both rubygems and rake can probably be installed via your system's native
package manager if you're stuck with Ruby 1.8.

## Other dependencies in Ruby

Now you can just install sup via `gem i sup` and rubygems can install all
dependencies in Ruby automatically.

If you're running sup from source, see
[[Running from git#Install-dependencies-in-Ruby]] for how to
install dependencies via [bundler].

## Next step

See [[New User Guide]].

[rvm]: https://rvm.io/
[rbenv]: https://github.com/sstephenson/rbenv
[extenions]: http://www.ruby-doc.org/docs/ProgrammingRuby/html/ext_ruby.html
[Rubygems]: https://rubygems.org/
[bundler]: http://gembundler.com/
[rpm]: http://www.rpm.org/
