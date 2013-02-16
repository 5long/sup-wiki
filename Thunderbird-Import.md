This Thunderbird was on Windows. The Windows path to the data files
was:
     '.../Documents and Settings/mywindowslogin/Application Data/Thunderbird/Profiles/clds8flz.default/Mail'

clds8flz is probably some random thing generated by Thunderbird in
lieu of a named profile.

Each directory under Mail is a main folder in Thunderbird (like an
email account, Local Folders, or News). In each of those
directories, each file without an extension is a mail collection
(like Inbox, Drafts, Sent, etc.). The mail collections are some
form of mbox, but people have had various problems importing them
into Sup.

This solution involves converting the mbox files into maildir
collections using the mb2md utility, which in this case was
obtained through apt-get on Ubuntu. Sup was able to import the
maildir collections problem free.

Create two directories, mboxes and mdirs. Under mboxes, create a
directory for each Thunderbird folder and copy the mail collections
into each folder, excluding collections you don't want (like
Trash).

In the mdirs directory, also create a directory for each
Thunderbird folder you wish to import. Within each of those
directories create an empty directory for each mail collection
(e.g. Inbox). Here's some ruby used in irb to accomplish this.
Perhaps this can serve as the beginning of a script to automate the
conversion.

     `ls mboxes`.split("\n").each {|dir| `ls mboxes/#{dir}`.split("\n").each {|mbox| `mkdir mdirs/#{dir}/#{mbox}`}}

Now use mb2md to convert each mbox file into a maildir.

     `ls mboxes`.split("\n").each {|dir| `ls mboxes/#{dir}`.split("\n").each 
       {|mbox| `mb2md -s /home/dave/Mailstore/mboxes/#{dir}/#{mbox} -d /home/dave/Mailstore/mdirs/#{dir}/#{mbox}`}}

Add the maildirs to sup, adding two tags, one for the Thunderbird
folder and one for the name of the mail collection.

     `ls mdirs`.split("\n").each {|dir| `ls mdirs/#{dir}`.split("\n").each 
       {|mdir| `sup-add -ul #{dir.downcase},#{mdir.downcase} maildir:/home/dave/Mailstore/mdirs/#{dir}/#{mdir}`}}
