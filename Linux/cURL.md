
para poder ver la cabecera de la petición
```bash
curl -i http://<SERVER_IP>:<PORT>/
```

para poder ingresar credenciales en la petición:
```bash
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

```bash
curl http://admin:admin@<SERVER_IP>:<PORT>/
```

para obtener mas detalles de la conexión:
```bash
curl -v http://admin:admin@<SERVER_IP>:<PORT>/
```

para pasar parámetros en la cabecera:
```bash
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

pasar una cookie de sesión en la petición (-b):
```bash
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```

Con este comando especificamos una cadena de agente de usuario y -L es utilizado para seguir redirecciones (en formularios de login se suele utilizar ya que nos redirecciona a otro sitio).
```bash
curl -A "User-Agent-String" -L {IP}
```

<b>API CRUD CURL</b>

| Operation | HTTP Method |
| --------- | ----------- |
| Create    | POST        |
| Read      | GET         |
| Update    | PUT         |
| Delete    | DELETE      |

**READ**

Leer la informacion que se encuentra en esa seccion de la API
```bash
curl <SERVER_IP>:<PORT>/api.php/city/london
```

Aplicamos pipe jq para que el formato de salida sea JSON y -s para omitir cualquier output innecesario.
```bash
curl -s <SERVER_IP>:<PORT>/api.php/city/london | jq
```


**CREATE**

Se puede ingresar nueva informacion por medio de POST (-d se usa para enviar datos en una solicitud ya sea API o formulario)
```bash
curl -X POST <SERVER_IP>:<PORT>/api.php/city/ -d {"city_name":"nueva", "country_name":"nuevo"} -H 'Content-Type: application/json' 
```

**UPDATE**

Utilizamos PUT si vamos a cambiar toda la informacion de la entrada y PATCH si solo vamos a hacer una modificacion parcial. Si la entrada a modificar no existe, esta se creara
```bash
curl -X PUT <SERVER_IP>:<PORT>/api.php/city/london -d {"city_name":"new_london", "country_name":"new_country"} -H 'Content-Type: application/json' 
```

**DELETE**

Se utiliza para eliminar una entrada
```bash
curl -X DELETE <SERVER_IP>:<PORT>/api.php/city/london
```

