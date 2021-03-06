Sup will generate a complete configuration file
(\~/.sup/config.yaml) with default settings for all the options the
very first time you run it. If you've been using Sup for a while,
you may be missing some options that have been added since your
config.yaml file was generated.

Some config options that warrant mention. A complete list can be
found in
[[sup.rb, circa line 265]](http://gitorious.org/sup/mainline/blobs/master/lib/sup.rb#line265).(Look
for "def load\_config filename")

     :confirm_no_attachments

If true, and you use words like "attach", "attachment", or
"attached" in your email and don't have any attachments, Sup will
prompt you before sending.

     :confirm_top_posting

If true, and you top-post, Sup will tell you that you are a bad
person and will prompt you to confirm that before posting.

Both of these will be true when a default config.yaml is generated,
but are considered false if they're not explicitly specified. So if
you already have a config.yaml and want these behaviors you'll have
to add them.

      :ask_for_cc
      :ask_for_bcc
      :ask_for_subject

These determine which fields you're asked for when composing and
(except for subject) forwarding messages.

Configuring multiple accounts to work intelligently with the Reply
function has its own section: [[MultipleAccountsAndReply]]
