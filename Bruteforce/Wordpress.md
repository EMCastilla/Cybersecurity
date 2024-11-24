
Previamente añadir la IP a /etc/hosts

Enumeración de usuarios:
```bash
wpscan --url {direccion web} --enumerate u
```

Una vez obtenido usuarios podemos hacer bruteforce para el password:
```bash
wpscan --url {direccion web} --passwords /usr/share/wordlists/rockyou.txt --usernames {usuario obtenido}
```

