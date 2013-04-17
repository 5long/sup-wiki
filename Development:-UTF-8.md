Before switch to Mail and deprecation of Iconv Sup is riddled with various encoding errors. The goal is to convert everything to UTF-8 and work with that. Possibly an option for encoding to a user specified encoding for outgoing mail. The deprecation of Iconv is an excellent opportunity to getting UTF-8 right.

A milestone (milestone 1) has been created to track the various issues related to bad UTF-8 handling.

## Default encoding of scripts
All ruby scripts should by default be UTF-8 (start script with, or second line):
```
# encoding: UTF-8
```

this is the default in Ruby 2.0.0.

## Encoding of files
All files: labels.txt, contacts.txt, etc should be written and loaded in UTF-8 (set external encoding to UTF-8).

## Resources
* http://ruby-doc.org/core-2.0/Encoding.html
* Equivalent of Iconv conversion: http://stackoverflow.com/questions/7870636/equivalent-of-iconv-convutf-8-ignore-in-ruby-1-9-x