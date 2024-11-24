![[Pasted image 20241109201623.png]]

GUID (Global Unique Identifier) - Valor asignado cada vez que se crea un usuario o grupo o de dominio -> `ObjectGUID`

Security principals - Todo aquello que el SO puede autenticar. Administrado por SAM (Security Accounts Manager).

SID (Security Identifier) - identificador unico para un grupo de seguridad. Cada cuenta, grupo o proceso tiene su unico SID.

Distinguished Name (DN) - describe el path completo a un objeto en el AD (ejemplos `cn=bjones, ou=IT, ou=Employees, dc=inlanefreight, dc=local`)

Relative Distinguished Name (RDN) - identidica el objeto como unico de otros objetos del mismo nivel jerarquico,

![[Pasted image 20241109233122.png]]

sAMAccountName - es el nombre de login del usuario

userPrincipalName - este atributo es otra manera de identificar usuarios en AD.

FSMO Roles - Microsof separo la variedad de responsabilidades que el DC puede tener en Flexible Single Master Operation (FSMO) roles. Esto permite al DC seguir autenticando usuarios y brindando permisos sin interrupcion. Cuenta con 5 roles:

| Roles                    | Description                                                                                                                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Schema Master            | Este rol gestiona la copia de lectura/escritura del esquema de AD, que define todos los atributos que pueden aplicarse a un objeto en AD.                                                                                                |
| Domain Naming Master     | Administra los nombres de dominio y asegura que se creen dos dominios con el mismo nombre en el mismo bosque.                                                                                                                            |
| Relative ID (RID) Master | El RID Master asigna bloques de RIDs a otros DC dentro del domino, que se pueden usar para nuevos objetos. El RID Master ayuda a asegurar que no se asignen SIDs duplicados a multiples objetos.                                         |
| PDC Emulator             | El host con este rol es el DC autoritativo en el dominio y responde a solicitudes de autenticacion, cambios de contraseña y gestiona los GPOs.                                                                                           |
| Infrastructure Master    | Este rol traduce GUIDs, SIDs, and DNs entre dominion. Se utiliza en organizaciones con multiples dominios en un solo bosque. Si este rol no funciona correctamente, las ACLs mostraran SIDs en lugar de nombres completamente resueltos. |


Global Catalog (GC) - es un DC que alamcena copia de todos los objetos en un AD forest.

Read-Only domain Controller (RODC) - tiene una base de datos AD de solo lectura. Tambien posee un servidor DNS de solo lectura.

Service Principal Name (SPN) - unicamente identifica la instancia de un servicio. Son utilizados por la autenticacion de Kerberos para asocias la instancia de un servicio con la cuenta logueada.

Group Policy Object (GPO) - coleccion virtual de configuraciones de politicas. Cada GPO tiene una unica GUID.

Access Control List (ACL) - coleccion de Access Control Entries (ACEs) que se aplican a un objeto.

Access Contro Entities (ACEs) - en un ACL identifica a un destinatario de permisos (cuenta de usuari, cuenta de grupo, o sesion logueada) y lista los accesos permitidos, denegados o auditados.

Discretionary Access Control (DACL) - define que principios de seguridad tiene garantizados o denegados para acceder un objeto. Cuando un proceso trata de acceder a un objeto seguro , el sistema chequea los ACEs en el DACL para determinar si brinda o no acceso.

System Access Control Lists (SACL) - permite a los administradores hacer un log de los intentos de acceso a objetos seguros.

Fully Qualified Domain Name (FQDN) - es el nombre especifico para una computadora o host. Es usado para especificar la ubicacion de un objeto dentro de un arbol jerarquico de DNS. Puede ser usado para localizar hosts en un AD sin necesidad de saber la direccion IP.
El formato es: `[hostname].[domain name].[tld]`

Tombstone - es un objeto contenedor en el AD que tien los objetos eliminados del AD.

AD Recycle Bin - cuando esta habilitado permite preservar objetos eliminados del AD por un periodo de tiempo, facilitando su restauracion.

SYSVOL - la carpeta SYSVOL almacena copias de archivos publicosen el dominio como pueden ser configuraciones de politicas de grupos, login/logoff scripts, etc.

AdminSDHolder - actua como un contenedor almacena los descriptores de seguridad aplicados a miembros de grupos protegidos.

dsHeuristics - este atributo de AD es utilizado para habilitar o deshabilitar configuraciones avanzadas y ajustes de comptibilidad.

adminCount - este atributo determina si el proceso SDProp protege o no a un usuario.

Active Directory Users and Computers (ADUC) - es una consola GUI utilizada mada administrar usuarios, grupos, computadoras y contactos en el AD. Las modificaciones al ADUC pueden realizarse a traves de Powershell tambien.

ADSI Edit - herramienta GUI para administar objetos en el AD.

sIDHistory - es un atributo en AD que almacena los SIDs anteriores de un objeto de usuario, grupo o computadora cuando se migra de un dominio a otro.

NTDS.DIT - este archivo puede ser considerado el corazon del AD. Se almacena en el DC en `C:\Windows\NTDS\` y es una base de datos que almacena los datos del AD como informacion acerca de objetos de usuarios y grupo, y lo mas importante para los atacantes, los hashes de los passwords para todos los usuarios del dominio.

MSBROWSE - es un protolo de red de Microsoft que era usado para proveer servicios de navegacion. A dia de hoy este protolo es obsoleto ya que se utiliza el protocolo SMB para compartir archivos e impresoras, y el protocolo CIFS para navegar por los servicios.

Organizational Units (OUs) - es un contenedor que los administradores de sistemas pueden utilizar para almacenar objetos similares para facilitar la administracion.

