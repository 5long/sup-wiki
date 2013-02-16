# Searching your mail

**TODO: Starting with Sup 0.10, Sup uses Xapian as indexing backend. The following documentation on searching with Xapian was copied from the [NewUserGuide](http://sup.rubyforge.org/wiki/wiki.pl?NewUserGuide). It should be enhanced and completed by somebody who knows.**

Press 'L' to do a quick label search. You'll be prompted for a
label; hit enter to bring up scrollable list of all the labels
you've ever used, along with some special labels (Draft, Starred,
Sent, Spam, etc.). Highlight a label and press enter to view all
the messages with that label.

What you just did was actually a specific search. For a general
search, press '\\' (backslash---forward slash is used for in-buffer
search, following console conventions). Now type in your query
(again, Ctrl-G to cancel at any point.) You can just type in
arbitrary text, which will be matched on a per-word basis against
the bodies of all email in the index, or you can make use of the
full Xapian query syntax
([http://xapian.org/docs/queryparser.html](http://xapian.org/docs/queryparser.html)):



Phrasal queries using double-quotes, e.g.:
**"three contiguous words"**


Queries against a particular field using <field name\>:<query\>,
e.g.: **label:ruby-talk**, or **from:matz@ruby-lang.org**. (Fields
include: **body**, **from**, **to**, and **subject**.)


Search for all emails with attachments using **has:attachment**
(mails with attachments automatically get labelled with the label
"attachment"). Specific filetypes can be search for using
**has:attachment filetype:pdf**. You can also specify filenames
using **filename:somefile.txt**. Spaces in filenames require
parentheses around the filename
**filename:(some file with spaces.txt)**.


Wildcards are allowed in most searches: **subject:\*viagra\***,
**from:william\***


Force non-occurrence by **-**, e.g.: **-body:"hot soup"**.


If you have the chronic gem installed, date queries like
**before:today**, **on:today**, **after:yesterday**,
**after:(2 days ago)**, **during:february** (parentheses required
for multi-word descriptions).


You can combine those all together. For example:
**label:ruby-talk subject:\\[ANN\\] -rails on:today**


By default, query terms are combined with AND, i.e. all conditions
must be true. The example above is equivalent to:
**label:ruby-talk AND subject:\\[ANN\\] AND -rails AND on:today**


Exception: Query terms within the same field type are combined as
OR. **subject:apples subject:oranges** will finds apples as well as
oranges, it is equivalent to **subject:apples OR subject:oranges**


You can make this explicit by using conjunctions like "AND", "OR",
"NOT". Have a look at the
[[Xapian QueryParser documentation]](HTTP://xapian.org/docs/) for
more information.


**is:spam** is translated into **label:Spam**, likewise for some
other shortcut queries ("in", "has"). Note that it will be OR'd
with other **label:** queries!


**msgid:123456789@example.com** is useful if you have a unique
identifier for an email.




Play around with the search, and see the
[[Xapian documentation]](HTTP://xapian.org/docs/) for details on
more sophisticated queries (date ranges, "within n words", etc.).

For a complete description of all search keywords, check
`lib/sup/index.rb`, in particular, search for `PREFIX`.





# Searching with Ferret (for Sup 0.9.1 and older)

Note that searches return entire threads, each containing at least
one message within the search bounds.



## From/to/subject searches



from:william@domain.com
to:will@domain.com
subject:\*wildcards\*
from:will\*
from:\*@domain.com


## Body searches

body:foobar


## Date searches

You need the chronic gem installed to use this functionality. See
more examples
[[here]](HTTP://chronic.rubyforge.org/files/README.html).



on:(16th feb)
before:yesterday
after:(last friday)
during:(last week)


## Attachments



has:attachment (attachment is a label)
filetype:xls
filename:thisisafilename.txt
filename:(a filename with spaces)


## Labels



is:starred
is:read
is:spam
is:deleted
has:somelabel
label:someotherlabel


## More search tips

You can search for mail sent to one of your own e-mail addresses as
defined in \~/.sup/config.yaml using "me":

from:me
If you search for more than one search term, they are combined by
"AND":

is:unread label:somelabel
returns unread mail with label "somelabel" To combine them using
"OR", use "||":
from:me || to:me
returns mail from and to you.
Excluding a search term:

! label:nothislabel
One search term but not another:
during:(last month) ! label:notthislabel
