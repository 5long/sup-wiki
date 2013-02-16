You can use the `before-add-message.rb` hook to add labels to messages as they
arrive.

    before-add-message
    ------------------
    File: ~/.sup/hooks/before-add-message.rb
    Executes immediately before a message is added to the index.
    Variables:
      message: the new message


## Add label based on sender

    if message.from.email.start_with? 'bill.gates@microsoft'
      message.add_label :microsoft
    end

## Add label based on recipient address

    if message.recipients.any? { |person| person.email =~ /admin-.*@example.org/i }
      message.add_label :admin
    end

## Add label and skip inbox based on recipient address

    if message.recipients.any? { |person| person.email =~ /admin-.*@example.org/i }
      message.remove_label :inbox
      message.add_label :admin
    end

## Add label for email list

    subj = message.subj.downcase
    if subj.start_with? '[sup-talk'
      message.add_label :sup
    end
