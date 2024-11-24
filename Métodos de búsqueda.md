Buscar un archivo por su nombre:

```bash
find / -type f -name user.txt 2> /dev/null
```

Buscar archivos con permisos SUID (para escalar privilegios):

```bash
find / -type f -user root -perm -u=s 2> /dev/null
```

<b>2> /dev/null </b>Se utiliza para que solo liste los resultados positivos y omite aquellas rutas a las que no tenemos permisos para acceder.

