AD requiere los siguientes protocolos:
- Lightweight Directory Access Protocol (LDAP)
- Kerberos
- DNS
- MSRPC (implementacion de Microsoft de Remote Procedure Call (RPC))

Kerberos ah sido el protocolo de autenticacion por defecto para cuentas de dominio desde Windows 2000.
Utiliza un sitema de tickes en lugar de transmitir passwords por la red. Los DC tienen un centro de distribucion de claves Kerberos (KDC) que emite tickets.

El protolo Kerberos utiliza el <span style="color:yellow">puerto 88</span> (tanto TCP como UDP) 

Kerberos Authentication Process
1. El usuario se loguea, su contraseña es convertida a un hash NTLM que es usado para encriptar el TGT ticket. Esto desacopla las credenciales de usuario de las solicitudes a los recursos.

2. El servicio KDC en el DC chequea el pedido de servicio de autenticacion (AS-REQ), verifica la informacion del usuario y crea un Ticket Granting Ticket (TGT) que es entregado al usuario.

3. El usuario presenta el TGT al DC y pide un Ticket Granting Service (TGS) ticket para un servicio especifico. Esto es TGS-REQ. Si el TGT es validado exitosamente su data es copiada para crear un TGS ticket.

4. El TGS es encriptado con el hash password NTLM de la cuenta de servicio o de equipo en cuyo contexto se ejecuta la instancia del servicio y se entrega al usuario en el TGS_REP.

5. El usuario presenta el TGS al servicio y si es valido se le permite al usuario conectarse al recurso (AP-REQ).

![[Pasted image 20241115000041.png]]