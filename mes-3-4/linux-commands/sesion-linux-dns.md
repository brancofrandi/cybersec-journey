## Sesion Linux — /etc/hosts y dig
**Fecha:** 29/03/2026
**Sistema:** Ubuntu en VirtualBox

---

### /etc/hosts

Ejecuté: cat /etc/hosts

**Lo que contiene:**
Mapeos manuales de nombre a IP. El sistema los usa
antes de consultar cualquier servidor DNS.

Entradas que ví en mi sistema:
- 127.x.x.x  localhost
- 127.x.x.x  branco (nombre de mi maquina)
- Entradas IPv6 estandar del sistema

**Dónde entra en el flujo DNS:**
Es el PRIMERO que revisa el sistema, no el segundo.
El orden completo es:
1. /etc/hosts (mapeos locales manuales)
2. Cache DNS local
3. Servidor DNS recursivo del ISP o publico
4. Root Name Server
5. Servidor TLD
6. Authoritative Name Server

Si /etc/hosts tiene una entrada para un dominio,
el sistema la usa directamente sin consultar DNS.
Esto se puede usar en ataques para redirigir trafico
(modificar hosts para que google.com apunte a otra IP).

---

### dig vs nslookup

Ejecuté: dig google.com

**Diferencia concreta:**

| Información          | nslookup | dig |
|----------------------|----------|-----|
| TTL exacto           | No       | Si  |
| Puerto usado (#53)   | No       | Si  |
| Tiempo de consulta   | No       | Si  |
| Flags de respuesta   | No       | Si  |
| Tipo de registro     | Parcial  | Si  |

**Por qué dig se prefiere en ciberseguridad:**
Muestra informacion crítica que nslookup oculta.
El TTL indica cuanto tiempo esta cacheada la respuesta.
El puerto 53 es el puerto estandar de DNS.
Los flags indican si la consulta fue exitosa y como.

**Lo que encontré en el resultado de dig:**
- ANSWER SECTION contiene la IP del dominio
- TTL: 300 segundos (5 minutos de cache)
- SERVER: 8.8.8.8 (mi DNS actual, no el de Fibertel)
- status: NOERROR (consulta exitosa)

**Comandos de dig útiles:**
- dig google.com        → registro A (IPv4)
- dig google.com AAAA   → registro IPv6
- dig google.com MX     → servidores de correo
- dig google.com NS     → servidores DNS autoritativos

---
