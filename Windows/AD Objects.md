
![[Pasted image 20241114000623.png]]

Usuarios:
Son considerados objetos hoja (leaf object) ya que no pueden contener otro objeto dentro de ellos. Un objeto usuario tiene SID y GUID.
Es considerado un security principal.

Contactos:
Se utiliza para representar un usario externo y contiene todos los atributos de informacion. No es un security principal y es un objeto hoja (leaf object). Cuentan solo con GUID.

Impresoras:
Este objeto apunta a una impresora accesible desde la red AD. No security principal y es un objeto hoja (leaf object). Solo cuenta con GUID

Computadora:
Se refiere a cualquier computadora que se haya unido a la red del AD (workstation o server). Son objetos hoja (leaf object). Son consideradas security principal por lo cual cuenta con SID y GUID.

Carpetas compartidas:
Apunta a las carpetas compartidas de una computadora en especifico donde residen. Pueden ser configuradas para acceder por fuera del AD, por medio de autenticacion o accedida solo por usuarios/grupos con privilegio.

Grupos:
Un grupo es considerado un objeto contenedor (container object) ya que puede contener otros objetos. Es un security principal por lo cual tiene SID y GUID. Los grupos son la manera de administrar permisos de acceso y otros objetos seguros.
Se suelen encontrar "nested groups" que serian subgrupos dentro de grupos (esto es una mala practica ya que puede conducir a usuarios a tener permisos que no deberian).

Unidades organizacionales (OUs):
Es un contenedor que pueden utilizar los administradores de sistemas para almacenar objetos similares para facilitar la administracion. Se utilizan tambien para delegaciones administrativa de tareas.
OUs son muy utiles para administrar politicas de grupo (group policy).

Dominio:
Un dominio es la estructura de red de un AD. Los dominios contienen objetos como usuarios y computadoras, que estan organizados en objetos contenedores: grupos y OUs.

Domain Controller (DC):
Es la parte esencial de una red AD. Maneja pedidos de autenticacion, verifica usuarios en la red y controla quien puede acceder a los varios recursos del dominio.

Sitios:
Un sitio en AD e sun set de computadoras en una o mas subnets conectadas usando links de alta velocidad.

Built-in:
En AD, buil-in es un contenedor que tiene los grupos por defecto en un dominio AD. Son predefinidos cuando un dominio AD es creado.

Foreign security principals:
Un FSP es un objeto creado en el AD para representar un security principal que pertenece a un bosque (forest) de ocnfianza.