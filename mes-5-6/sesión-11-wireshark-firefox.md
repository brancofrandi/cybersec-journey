## Sesión 11 — Wireshark: análisis de tráfico real con Firefox
**Fecha:** 31/07/2026

---

## Ejercicio practicado

Captura de tráfico real generado por Firefox navegando
a httpbin.org usando la interfaz **any** en Wireshark.
Total de paquetes capturados: 6442

---

## Protocolos identificados

| Protocolo | Qué hace en este contexto |
|-----------|--------------------------|
| DNS | Resolución de nombres de dominio |
| TCP | Transporte de datos |
| HTTP | Tráfico web sin cifrado |
| OCSP | Verificación de certificados SSL |
| TLSv1.2 | Tráfico HTTPS cifrado |

---

## DNS — 60 consultas registradas

Firefox genera decenas de consultas DNS en segundo plano
sin intervención del usuario: telemetría, actualizaciones,
verificación de certificados, CDNs, íconos de sitios.

**Dominios identificados:**

- `httpbin.org` → dominio objetivo de la prueba
- `o.pki.goog` → servidor OCSP de Google
- `httpbin.org.org` → consulta errónea automática del sistema,
  devolvió `No such name`

**Tipos de consulta DNS:**
- `A` → consulta por dirección IPv4
- `AAAA` → consulta por dirección IPv6
  (cuatro A porque IPv6 tiene cuatro veces más bits que IPv4)

**CNAME:**
Alias DNS que apunta a otro nombre.
`o.pki.goog` es un CNAME que apunta a `pki-goog.l.google.com`.

---

## OCSP — Online Certificate Status Protocol

Protocolo que usa Firefox para verificar si un certificado
SSL sigue siendo válido antes de establecer una conexión HTTPS.

**Flujo:**
1. Firefox quiere conectarse a un sitio HTTPS
2. El sitio presenta su certificado SSL
3. Firefox consulta a la CA: "¿este certificado sigue vigente?"
4. La CA responde: válido o revocado
5. Firefox completa o rechaza la conexión

**Servidor consultado:** `o.pki.goog` (Google como CA)
**Método:** POST (envío de datos al servidor OCSP)

---

## ¿Qué es un certificado SSL?

Archivo digital que identifica y autentica un sitio web.

**Tres funciones:**
- Prueba que el sitio es quien dice ser
- Establece la conexión cifrada HTTPS
- Emitido por una autoridad de certificación (CA) de confianza

**Revocación:** si una clave privada es comprometida,
la CA revoca el certificado. Los navegadores dejan de
aceptarlo y muestran advertencia de seguridad.

---

## HTTP analizado

Paquetes HTTP identificados: 12

Eventos relevantes:
- `GET /get HTTP/1.1` → solicitud a httpbin.org
- `503 Service Temporarily Unavailable` → servicio no disponible
- `404 GET /favicon.ico` → ícono del sitio no encontrado
- `POST /we2` a `o.pki.goog` → verificación OCSP

**Campo `origin` en respuesta de httpbin.org:**
Muestra la IP pública del cliente que hizo la solicitud.
En este caso: `201.179.96.17`
El servidor destino siempre ve tu IP real en HTTP plano.

---

## Tráfico en segundo plano de Firefox

Firefox genera conexiones automáticas sin intervención:
- Verificación de certificados OCSP
- Consultas DNS de dominios relacionados
- Telemetría y actualizaciones
- Íconos de sitios web (favicon)

Eso explica los 6442 paquetes capturados en una sesión corta.

---

## HTTP vs HTTPS en Wireshark

**HTTP:** contenido visible en texto plano.
IP de origen, headers y datos legibles por cualquiera
en la misma red con Wireshark.

**HTTPS (TLSv1.2):** contenido cifrado.
No se puede leer sin la clave privada del servidor.
Los dominios consultados siguen siendo visibles en DNS.

---

## Comandos y filtros usados

```bash
# Generar tráfico HTTP
curl http://httpbin.org/get

# Filtros en Wireshark
http      → paquetes HTTP
dns       → consultas DNS
tcp       → tráfico TCP
ocsp      → verificaciones de certificados
```
