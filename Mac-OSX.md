# Ruby Gems

Ensure Ruby and [[RubyGems]] are
[upgraded](http://www.google.com/search?q=mac+os+x+upgrade+ruby+gems).
Then install the sup gem:

     sudo gem install sup

# Sending Mail

[MacPorts](http://www.macports.org/) is an excellent software
distribution system similar to Debian's APT. You can use
[[MacPorts]] to install one of the null mailers (ssmtp, msmtp) discussed in this
wiki.

Or [setup your postfix similar to this](http://linsec.ca/Using_mutt_on_OS_X#mailcap)



# command completion from OS X Address Book

run sup -l and make sure your version of sup supports the
extra-contact-addresses hook. This is only tested against 10.6
(Snow Lepoard) copy and paste this into your
`~/.sup/hooks/extra-contact-addresses.rb` file:

      contacts=[]
     `sqlite3 -separator ':' ~/Library/Application\\ Support/AddressBook/AddressBook-v22.abcddb "select e.ZADDRESSNORMALIZED,p.ZFIRSTNAME,p.ZLASTNAME,p.ZORGANIZATION from ZABCDRECORD as p,ZABCDEMAILADDRESS as e WHERE e.ZOWNER = p.Z_PK;" | awk -F":" '{print $2" "$3" "$4 , "<"$1">"}'`.each { |c|
        contacts.push(c)
     }
     contacts

That is a CRAZY long line, and in case things go badly, I'll
explain what it does.. Basically you are querying the sqlite
database that Address Book.app uses, and having it return a their
Organization First and Last name, and of course their email
address. This possibly returns multiple rows for each record out of
the address book (1 per email address). This seems to work ok for
this purpose.

After querying, we are using AWK to put it into something
resembling the "Name <email\>" format that sup wants it in. I just
append the organization name to the Name part.

Alternative that only uses ruby. Requires sqlite3 gem being
installed.

    contacts=[]
    addressbook='~/Library/Application Support/AddressBook/AddressBook-v22.abcddb'
    if File.exists?(File.expand_path(addressbook))
      require 'sqlite3'
      db = SQLite3::Database.new(File.expand_path(addressbook))
      sql="select e.ZADDRESSNORMALIZED,p.ZFIRSTNAME,p.ZLASTNAME,p.ZORGANIZATION " +
             "from ZABCDRECORD as p,ZABCDEMAILADDRESS as e WHERE e.ZOWNER = p.Z_PK;"
      contacts = db.execute(sql).map {|c| "#{c[1]} #{c[2]} #{c[3]} <#{c[0]}>" }
    end
    contacts



# Opening attachments using mime-view hook

run sup -l and make sure your version of sup supports the mime-view
hook.. if so:
set your \~/.sup/hooks/mime-view.rb file to:

     `open '#{filename}'`

