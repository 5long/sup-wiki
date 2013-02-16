Here is an example of how to read HTML only emails, using the
`mime-decode.rb` hook:

    unless sibling_types.member? "text/plain"
      case content_type
      when "text/html"
        `/usr/bin/w3m -dump -T #{content_type} '#{filename}'`
      end
    end

Here is the documentation for the hook:

    mime-decode
    -----------
    File: ~/.sup/hooks/mime-decode.rb
    Decodes a MIME attachment into text form. The text will be displayed
    directly in Sup. For attachments that you wish to use a separate program
    to view (e.g. images), you should use the mime-view hook instead.

    Variables:
       content_type: the content-type of the attachment
            charset: the charset of the attachment, if applicable
           filename: the filename of the attachment as saved to disk
      sibling_types: if this attachment is part of a multipart MIME attachment,
                     an array of content-types for all attachments. Otherwise,
                     the empty array.
    Return value:
      The decoded text of the attachment, or nil if not decoded.
