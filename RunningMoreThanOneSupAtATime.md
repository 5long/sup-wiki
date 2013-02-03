Ferret doesn't allow concurrent access to the same index. As of
0.2, Sup locks the index explicitly to prevent this. Thus, the only
way to run more than one Sup at a time is if they're pointed at two
different, independent indexes. This is probably not useful in
practice, but can be very useful for development purposes, as it
allows you to avoid running run untested code against your actual
mail index.
Sup checks the environment variable SUP\_BASE to determine the
location of the sup directory. If that's not specified, it uses
"\~/.sup/". So in bash you can do something like "export
SUP\_BASE=/tmp/sup-test; ruby -I lib bin/sup-add <some source\>" to
start up sup, create the new sup directory and all requisite files
automatically, and add the source. Then sup-sync and sup as
normal.

If you are developing on debian, be aware of bug
[http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=542491](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=542491)
which breaks your search path (remove the unshift line from
chronic.rb to fix that).
