the sendmail line in config.yaml should be one of these:

    ":sendmail: /usr/local/bin/putmail.py -t" (direct sending)
    ":sendmail: /usr/local/bin/putmail\_enqueue.py -t" (queue for later)
