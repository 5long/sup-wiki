# Development of Maildir Syncback

Usage instructions: [[Maildir-Syncback]]

## Syncing labels
Resources:
* notmuch syncback script: https://github.com/altercation/es-bin/blob/master/maildir-notmuch-sync
* mu: http://www.djcbsoftware.nl/code/mu/

## How they do it
mu stores keywords (aka labels) under a specific header in the email : `X-Keyword`. This allows seamless synchronisation with no additional tools or specific formats, at the cost of modifying user's emails with program-specific data.