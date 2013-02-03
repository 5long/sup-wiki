How to get sup to collect mail from remote servers.
**\~/.sup/hooks/before-poll.rb**

      File: ~/.sup/hooks/before-poll.rb
      Executes immediately before a poll for new messages commences.
      No variables.

Run fetchmail before polling for new messages:

    if (@last_fetchmail_time || Time.at(0)) < Time.now - 60
      say "Running fetchmail..."
      system "fetchmail > /dev/null 2>&1"
      say "Done running fetchmail."
    end
    @last_fetchmail_time = Time.now

A similar example for offlineimap:

    def offlineimap(*folders)
      cmd = "offlineimap -q -u Noninteractive.Basic"
      cmd << " -f #{folders * ','}" unless folders.compact.empty?
      `#{cmd} 2>&1`
    end
    
    def folder_names(sources)
      sources.map { |s| s.uri.split('/').last }
    end
    
    def inbox_sources(sources = Index.usual_sources)
      sources.find_all { |s| !s.archived? }.sort_by {|s| s.id }
    end
    
    if (@last_fetch || Time.at(0)) < Time.now - 120
      say "Running offlineimap..."
      # only check non-auto-archived sources on the first run
      log offlineimap(@last_fetch ? nil : folder_names(inbox_sources))
      say "Finished offlineimap run."
    end
    @last_fetch = Time.now

