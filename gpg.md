sup allows sending signed/signed+encrypted messages. To tell sup what key to use add the following to your config.yaml:

    :accounts:
      :default:
        ...[your config details here]...
        :gpgkey: NUMERICKEYID

where NUMERICKEYID can be obtained from gpg --list-keys if you don't already know it, 12AB34CD in the following:

    pub   1024D/12AB34CD 2008-08-14 [expires: 2010-08-014]
    uid                  Your Name (Personal) <your.email@yourhost.com>

Further information regarding how sup handles gpg integration may be gleaned from `crypto.rb`

Note: As of March 9, 2010, sup seems to ignore the ":gpgkey:" setting and just use the "from" e-mail address to choose the gpg key to use. There doesn't seem to be any mention of ":gpgkey:" in sup's source. You can add an e-mail address to an existing key using adduid after gpg --edit-key as described in [gpg's manual].

Note2: As of sup 0.12 (released January 2011) the ":gpgkey:" option has been re-introduced.

After sup 0.12, sup has moved to using the gpgme gem instead of the gpg binary. To install the gem on ubuntu you should install the libgpgme11-dev package and then "gem install gpgme".

## Using pinentry-curses (GPG-agent without a new graphical window)

Starting from the [next branch of sup at 2009-10-11](http://gitorious.org/sup/mainline/commit/4812245a2f975370290fb4235b05e9eca793e318) you can use pinentry-curses without messing up sup’s curses interface.

If you never used GPG-agent, add the following to ~/.gnupg/gpg.conf:

    use-agent

Also add the following to ~/.gnupg/gpg-agent.conf:

    pinentry-program /usr/bin/pinentry-curses

Also make sure that you have the gnupg-agent and pinentry-curses packages installed. You need to reload your already running gpg-agent to actually use the new configuration:

    killall -SIGHUP gpg-agent

Alternatively, you can just re-login to X (the gpg-agent is usually started with your session).

## Relevant Hooks

Trusting all keys for sending

This changes after sup 0.12 (released January 2011. Up to, and including, sup 0.12, you should use the gpg-args hook. After sup 0.12 was released, code was merged to use the gpgme gem. From that point on you should use the gpg-options hook.

If you haven't verified many keys, it's useful to be able to trust all the keys in your keyring. Up to sup 0.12, this can be achieved by adding a hook in the file ~/.sup/hooks/gpg-args.rb

    gpg-args
    --------
    File: /home/user/.sup/hooks/gpg-args.rb
    Runs before gpg is executed, allowing you to modify the arguments (most
    likely you would want to add something to certain commands, like
    --trust-model always to signing/encrypting a message, but who knows).

    Variables:
    args: arguments for running GPG

    Return value: the arguments for running GPG

Example Hook:

    if args =~ /--encrypt/
      "--trust-model always #{args}"
    else
      args
    end

After sup 0.12, this can be achieved by adding a hook in the file ~/.sup/hooks/gpg-options.rb

    gpg-options
    --------
    File: /home/user/.sup/hooks/gpg-options.rb
    Runs before gpg is called, allowing you to modify the options (most
    likely you would want to add something to certain commands, like
    {:always_trust => true} to encrypting a message, but who knows).

    Variables:
    operation: what operation will be done ("sign", "encrypt", "decrypt" or "verify")
    options: a dictionary of values to be passed to GPGME

    Return value: a dictionary to be passed to GPGME

Example hook:

    if operation == "encrypt"
      options.merge!({:always_trust => true})
    end

    options

Note that the "!" at the end of "merge!" is important ...

## Always sign and/or encrypt to certain addresses

You can set this up using hooks if you put some stuff in ~/.sup/hooks/crypto-mode.rb :

    crypto-mode
    -----------
    File: /home/mish/.sup/hooks/crypto-mode.rb
    Modifies cryptography settings based on header and message content, before
    editing a new message. This can be used to set, for example, default cryptography
    settings.
    Variables:
        header: a hash of headers. See 'signature' hook for documentation.
        body: an array of lines of body text.
        crypto_selector: the UI element that controls the current cryptography setting.
    Return value:
         none

For example, to always sign messages, you could do

    crypto_selector.set_to :sign

You can also make it conditional on any part of the header. So you might do:

    crypto_selector.set_to :sign_and_encrypt if header["To"].to_s =~ /foo@bar.com/
    crypto_selector.set_to :encrypt if header["To"].to_s =~ /baz@bar.com/

In ruby versions later than 1.8 grep is no longer applicable to strings. Therefore this can be used:

    sign_addresses = `gpg --list-keys`.split("\n").grep(/<(.*)>/){$1}
    crypto_selector.set_to :sign_and_encrypt if not sign_addresses.select{|a| header["To"].include?(a) }.empty?

Note that header["To"] is an array when using reply-mode, but a string if you are composing a new message, so using to_s ensures you have a string before trying a regular expression match.

Below is a crypto-mode hook fragment so sup will default to Sign when it has selected a from address for the outgoing email for which there is a key which can sign emails.

Use case is an occasionally used address for outgoing mail which is not associated with your private key and which only posts to a mailing list which complains about signed email anyway.

    sign_addresses = `gpg -K`.grep(/<(.*)>/){$1}
    crypto_selector.set_to :sign if not sign_addresses.select{|a| header["From"].include?(a) }.empty?

## Sending encrypted mail to mailing lists

When encrypting mail to a mailing list, you may want to encrypt the message using the public keys of all list members. This is what the [gnupg --group](http://www.gnupg.org/documentation/manuals/gnupg/GPG-Key-related-Options.html#GPG-Key-related-Options) option does.

With sup, you can achieve the same using the ~/.sup/hooks/gpg-expand-keys.rb hook :

    gpg-expand-keys
    ---------------
    File: ~/.sup/hooks/gpg-expand-keys.rb
    Runs when the list of encryption recipients is created, allowing you to
    replace a recipient with one or more GPGME recipients. For example, you could
    replace the email address of a mailing list with the key IDs that belong to
    the recipients of that list. This is essentially what GPG groups do, which
    are not supported by GPGME.
    Variables:
        recipients: an array of recipients of the current email
    Return value:
        an array of recipients (email address or GPG key ID) to encrypt
        the email for

A simple hook that performs key expansion using existing gnupg groups might look like this:
```ruby
lookup_table = {}

`gpg --with-colons --list-config group 2> /dev/null`.each_line do |line|
  email_and_keys = line.split(':')[2..3]
  lookup_table[email_and_keys[0].gsub(/[<>]/, '')] = email_and_keys[1].strip.split(';')
end 

recipients.map { |r| lookup_table.has_key?(r) ? lookup_table[r] : r }.flatten
```