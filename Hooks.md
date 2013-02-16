# Hooks

The full list of hooks (with links to relevant pages) is:

* after-poll - see [[Notification Of New Messages]] for an example.
* async-edit
* attribution
* before-add-message - see  [[Auto Add Labels To New Messages]]
* before-edit
* before-poll - see [[Triggering Mail Collection]] for an example.
* bounce-command
* crypto-mode - see [[GPG]] for more info
* custom-search
* detailed-headers
* extra-contact-addresses
* gpg-options - see [[GPG]] for more info
* index-mode-date-widget
* index-mode-size-widget
* keybindings
* label-list-filter
* label-list-format
* mark-as-spam
* mentions-attachments
* mime-decode
* mime-view - see [[Mac OSX]] for an example
* publish
* reply-from
* reply-to
* search-list-filter
* search-list-format
* sendmail
* shutdown
* sig-output
* signature
* startup
* status-bar-text
* terminal-title-text
* time-to-nice-string

## help output

Here is the full output of `sup --list-hooks`:

    Have 34 registered hooks:

    after-poll
    ----------
    File: ~/.sup/hooks/after-poll.rb
    Executes immediately after a poll for new messages completes.
    Variables:
                       num: the total number of new messages added in this poll
                 num_inbox: the number of new messages added in this poll which
                            appear in the inbox (i.e. were not auto-archived).
    num_inbox_total_unread: the total number of unread messages in the inbox
             from_and_subj: an array of (from email address, subject) pairs
       from_and_subj_inbox: an array of (from email address, subject) pairs for
                            only those messages appearing in the inbox

    async-edit
    ----------
    File: ~/.sup/hooks/async-edit.rb
    Runs when 'H' is pressed in async edit mode. You can run whatever code
    you want here - though the default case would be launching a text
    editor. Your hook is assumed to not block, so you should use exec() or
    fork() to launch the editor.

    Once the hook has returned then sup will be responsive as usual. You will
    still need to press 'E' to exit this buffer and send the message.

    Variables:
    file_path: The full path to the file containing the message to be edited.

    Return value: None

    attribution
    -----------
    File: ~/.sup/hooks/attribution.rb
    Generates an attribution ("Excerpts from Joe Bloggs's message of Fri Jan 11 09:54:32 -0500 2008:").
    Variables:
      message: a message object representing the message being replied to
        (useful values include message.from.name and message.date)
    Return value:
      A string containing the text of the quote line (can be multi-line)

    before-add-message
    ------------------
    File: ~/.sup/hooks/before-add-message.rb
    Executes immediately before a message is added to the index.
    Variables:
      message: the new message

    before-edit
    -----------
    File: ~/.sup/hooks/before-edit.rb
    Modifies message body and headers before editing a new message. Variables
    should be modified in place.
    Variables:
      header: a hash of headers. See 'signature' hook for documentation.
      body: an array of lines of body text.
    Return value:
      none

    before-poll
    -----------
    File: ~/.sup/hooks/before-poll.rb
    Executes immediately before a poll for new messages commences.
    No variables.

    bounce-command
    --------------
    File: ~/.sup/hooks/bounce-command.rb
    Determines the command used to bounce a message.
    Variables:
          from: The From header of the message being bounced
                (eg: likely _not_ your address).
            to: The addresses you asked the message to be bounced to as an array.
    Return value:
      A string representing the command to pipe the mail into.  This
      should include the entire command except for the destination addresses,
      which will be appended by sup.

    crypto-mode
    -----------
    File: ~/.sup/hooks/crypto-mode.rb
    Modifies cryptography settings based on header and message content, before
    editing a new message. This can be used to set, for example, default cryptography
    settings.
    Variables:
        header: a hash of headers. See 'signature' hook for documentation.
        body: an array of lines of body text.
        crypto_selector: the UI element that controls the current cryptography setting.
    Return value:
         none

    custom-search
    -------------
    File: ~/.sup/hooks/custom-search.rb
    Executes before a string search is applied to the index,
    returning a new search string.
    Variables:
      subs: The string being searched.

    detailed-headers
    ----------------
    File: ~/.sup/hooks/detailed-headers.rb
    Add or remove headers from the detailed header display of a message.
    Variables:
      message: The message whose headers are to be formatted.
      headers: A hash of header (name, value) pairs, initialized to the default
               headers.
    Return value:
      None. The variable 'headers' should be modified in place.

    extra-contact-addresses
    -----------------------
    File: ~/.sup/hooks/extra-contact-addresses.rb
    A list of extra addresses to propose for tab completion, etc. when the
    user is entering an email address. Can be plain email addresses or can
    be full "User Name <email@domain.tld>" entries.

    Variables: none
    Return value: an array of email address strings.

    gpg-options
    -----------
    File: ~/.sup/hooks/gpg-options.rb
    Runs before gpg is called, allowing you to modify the options (most
    likely you would want to add something to certain commands, like
    {:always_trust => true} to encrypting a message, but who knows).

    Variables:
    operation: what operation will be done ("sign", "encrypt", "decrypt" or "verify")
    options: a dictionary of values to be passed to GPGME

    Return value: a dictionary to be passed to GPGME

    index-mode-date-widget
    ----------------------
    File: ~/.sup/hooks/index-mode-date-widget.rb
    Generates the per-thread date widget for each thread.
    Variables:
      thread: The message thread to be formatted.

    index-mode-size-widget
    ----------------------
    File: ~/.sup/hooks/index-mode-size-widget.rb
    Generates the per-thread size widget for each thread.
    Variables:
      thread: The message thread to be formatted.

    keybindings
    -----------
    File: ~/.sup/hooks/keybindings.rb
    Add custom keybindings.
    Methods:
      modes: Hash from mode names to mode classes.
      global_keymap: The top-level keymap.

    label-list-filter
    -----------------
    File: ~/.sup/hooks/label-list-filter.rb
    Filter the label list, typically to sort.
    Variables:
      counted: an array of counted labels.
    Return value:
      An array of counted labels with sort_by output structure.

    label-list-format
    -----------------
    File: ~/.sup/hooks/label-list-format.rb
    Create the sprintf format string for label-list-mode.
    Variables:
      width: the maximum label width
      tmax: the maximum total message count
      umax: the maximum unread message count
    Return value:
      A format string for sprintf

    mark-as-spam
    ------------
    File: ~/.sup/hooks/mark-as-spam.rb
    This hook is run when a thread is marked as spam
    Variables:
      thread: The message thread being marked as spam.

    mentions-attachments
    --------------------
    File: ~/.sup/hooks/mentions-attachments.rb
    Detects if given message mentions attachments the way it is probable
    that there should be files attached to the message.
    Variables:
      header: a hash of headers. See 'signature' hook for documentation.
      body: an array of lines of body text.
    Return value:
      True if attachments are mentioned.

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

    mime-view
    ---------
    File: ~/.sup/hooks/mime-view.rb
    Views a non-text MIME attachment. This hook allows you to run
    third-party programs for attachments that require such a thing (e.g.
    images). To instead display a text version of the attachment directly in
    Sup, use the mime-decode hook instead.

    Note that by default (at least on systems that have a run-mailcap command),
    Sup uses the default mailcap handler for the attachment's MIME type. If
    you want a particular behavior to be global, you may wish to change your
    mailcap instead.

    Variables:
       content_type: the content-type of the attachment
           filename: the filename of the attachment as saved to disk
    Return value:
      True if the viewing was successful, false otherwise. If false, calling
      /usr/bin/run-mailcap will be tried.

    publish
    -------
    File: ~/.sup/hooks/publish.rb
    Executed when a message or a chunk is requested to be published.
    Variables:
         chunk: Redwood::Message or Redwood::Chunk::* to be published.
    Return value:
      None.

    reply-from
    ----------
    File: ~/.sup/hooks/reply-from.rb
    Selects a default address for the From: header of a new reply.
    Variables:
      message: a message object representing the message being replied to
        (useful values include message.recipient_email, message.to, and message.cc)
    Return value:
      A Person to be used as the default for the From: header, or nil to use the
      default behavior.

    reply-to
    --------
    File: ~/.sup/hooks/reply-to.rb
    Set the default reply-to mode.
    Variables:
      modes: array of valid modes to choose from, which will be a subset of
                 [:sender, :recipient, :list, :all, :user]
             The default behavior is equivalent to
                 ([:list, :sender, :recipent] & modes)[0]
    Return value:
      The reply mode you desire, or nil to use the default behavior.

    search-list-filter
    ------------------
    File: ~/.sup/hooks/search-list-filter.rb
    Filter the search list, typically to sort.
    Variables:
      counted: an array of counted searches.
    Return value:
      An array of counted searches with sort_by output structure.

    search-list-format
    ------------------
    File: ~/.sup/hooks/search-list-format.rb
    Create the sprintf format string for search-list-mode.
    Variables:
      n_width: the maximum search name width
      tmax: the maximum total message count
      umax: the maximum unread message count
      s_width: the maximum search string width
    Return value:
      A format string for sprintf

    sendmail
    --------
    File: ~/.sup/hooks/sendmail.rb
    Sends the given mail. If this hook doesn't exist, the sendmail command
    configured for the account is used.
    The message will be saved after this hook is run, so any modification to it
    will be recorded.
    Variables:
        message: RMail::Message instance of the mail to send
        account: Account instance matching the From address
    Return value:
         True if mail has been sent successfully, false otherwise.

    shutdown
    --------
    File: ~/.sup/hooks/shutdown.rb
    Executes when sup is shutting down. May be run when sup is crashing,
    so don't do anything too important. Run before the label, contacts,
    and people are saved.
    No variables.
    No return value.

    sig-output
    ----------
    File: ~/.sup/hooks/sig-output.rb
    Runs when the signature output is being generated, allowing you to
    add extra information to your signatures if you want.

    Variables:
    signature: the signature object (class is GPGME::Signature)
    from_key: the key that generated the signature (class is GPGME::Key)

    Return value: an array of lines of output

    signature
    ---------
    File: ~/.sup/hooks/signature.rb
    Generates a message signature.
    Variables:
          header: an object that supports string-to-string hashtable-style access
                  to the raw headers for the message. E.g., header["From"],
                  header["To"], etc.
      from_email: the email part of the From: line, or nil if empty
    Return value:
      A string (multi-line ok) containing the text of the signature, or nil to
      use the default signature, or :none for no signature.

    startup
    -------
    File: ~/.sup/hooks/startup.rb
    Executes at startup
    No variables.
    No return value.

    status-bar-text
    ---------------
    File: ~/.sup/hooks/status-bar-text.rb
    Sets the status bar. The default status bar contains the mode name, the buffer
    title, and the mode status. Note that this will be called at least once per
    keystroke, so excessive computation is discouraged.

    Variables:
             num_inbox: number of messages in inbox
      num_inbox_unread: total number of messages marked as unread
             num_total: total number of messages in the index
              num_spam: total number of messages marked as spam
                 title: title of the current buffer
                  mode: current mode name (string)
                status: current mode status (string)
    Return value: a string to be used as the status bar.

    terminal-title-text
    -------------------
    File: ~/.sup/hooks/terminal-title-text.rb
    Sets the title of the current terminal, if applicable. Note that this will be
    called at least once per keystroke, so excessive computation is discouraged.

    Variables: the same as status-bar-text hook.
    Return value: a string to be used as the terminal title.

    time-to-nice-string
    -------------------
    File: ~/.sup/hooks/time-to-nice-string.rb
    Formats time nicely as string.
    Variables:
      time: The Time instance to be formatted.
      from: The Time instance providing the reference point (considered "now").

