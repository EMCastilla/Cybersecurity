Ademas de Kerberos y LDAP, AD utiliaza otros metodos de autenticacion. Esto incluye LM, NTLM, NTLMv1 y NTLMv2. LM y NTLM son los hash names y NTLMv1 y NTLMv2 son protocolos de autenticacion que utilizan hash LM o NT.


| Hash/Protocol | Cryptograph technique                                  | Mutual Authenticacion | Message Type                    | Trusted Third Party                |
| ------------- | ------------------------------------------------------ | --------------------- | ------------------------------- | ---------------------------------- |
| NTLM          | Criptografia llave simetrica                           | No                    | Random number                   | DC                                 |
| NTLMv1        | Criptografia llave simetrica                           | No                    | MD4 hash, random number         | DC                                 |
| NTLMv2        | Criptografia llave simetrica                           | No                    | MD4 hash, random number         | DC                                 |
| Kerberos      | Criptografia llave simetrica y criptografia asimetrica | Yes                   | Encrypted ticket using DES, MD5 | DC / Key Distribution Center (KDC) |

<h2>LM</h2>
LAN Manager (LM o LANMAN) hashes son el mecanismo mas antiguo utilziado por Windows para almacenar passwords. Estan limitados a un maximo de <span style="color:lightgreen">14</span> caracteres. Los passwords no son case sensitive y son convertidos a mayuscula antes de generar el valor del hash.
Un hash LM toma la forma de <span style="color:lightgreen">299bd128c1101fd6</span>.

<h2>NTHash (NTLM)</h2>
NT LAN Manager (NTLM) hashes son utiliados por sistemas modernos de Windows. Es un protocolo de autenticacion challenge-response.
El algoritmo puede ser visualizado como <span style="color:lightgreen">MD4(UTF-16-LE(password))</span>.
Un hash NT toma la forma de <span style="color:lightgreen">b4b9b02e6f09a9bd760f388b67351e2b</span>, que es la segunda mitad de un hash NTLM completo. Un hash NTLM se ve de la siguiente manera:

```bash
Rachel:500:aad3c435b514a4eeaad3b935b51304fe:e46b9e548fa0d122de7f59fb6d48eaa2:::
```
* <span style="color:lightgreen">Rachel</span> - username
* <span style="color:lightgreen">500</span> - RID
* <span style="color:lightgreen">aad3c435b514a4eeaad3b935b51304fe</span> - LM hash
* <span style="color:lightgreen">e46b9e548fa0d122de7f59fb6d48eaa2</span> - NT hash

![[Pasted image 20241123195217.png]]

<h2>NTLMv1</h2>
