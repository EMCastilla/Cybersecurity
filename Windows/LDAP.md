AD sopsorta Lightweight Directory Access Protocol (LDAP) para consultas de directorio. LDAP es cross platform usado para utenticar contra varios servicios de directorio (como AD)

LDAP utiliza el <span style="color:yellow">puerto 636</span>

AD almacena informacion de cuenta de usuarios e informacion de seguridad como passwords y facilita el intercambio de esta informacion con otros dispositivos en la red. LDAP es el lenguaje que las aplicaciones utilizan para comunicarse con otros servidores que proveen servicios de directorio.
Simplificado LDAP es como los sistemas en la red pueden "hablar" con el AD.

![[Pasted image 20241115002447.png]]

La relacion entre AD y LDAP se puede comprar con la de Apache y HTTP.

AD LDAP Authentication
Hay dos tipos de autenticaciones:

1. Autenticacion simple (Simple Authentication): esta incluye autenticaciones anonimas, autenticacion no autenticada y autenticacion usuario/password.

2. Autenticacion SASL (SASL Authentication): el framework The Simple Authentication and Security Layer (SASL) utiliza otros serivcios de autenticacion como Kerberos.
