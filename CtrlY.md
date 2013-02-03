**Ctrl-Y** in sup will scroll the screen up one line. But Ctrl-Y
may be gobbled up by your shell and interpreted as a "delayed
suspend" signal, returning control to the underlying shell.
If you have accidentally pressed Ctrl-Y and find yourself at a
shell prompt:

     fg

...and you will be returned to your sup session.

To prevent this behavior altogether so that you may use Ctrl-Y for
scrolling in sup, add the following line to your ***.bashrc***,
***.zsh***, .profile or equivalent shell startup script:

     stty dsusp undef

