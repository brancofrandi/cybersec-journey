## Sesión 13 — Wireshark: detección de anomalías
**Fecha:** 05/08/2026

---

## Objetivo

Analizar tráfico real capturado con Firefox e identificar
posibles anomalías usando filtros avanzados de Wireshark.

---

## Herramienta: Protocol Hierarchy

**Statistics → Protocol Hierarchy**

Muestra todos los protocolos capturados organizados por capas
con porcentaje de paquetes y bytes de cada uno.
Permite tener una visión general sin scrollear la captura.

**Resultado de la sesión:**
- Total: 26219 paquetes
- QUIC IETF: 78.3% de paquetes, 83.4% de bytes
- TLS: 6.9% de paquetes, 12.7% de bytes
- HTTP: 8 paquetes (0.0%)
- DNS: 208 paquetes

---

## QUIC IETF

Protocolo de transporte moderno desarrollado por Google.
Base de HTTP/3. Reemplaza TCP para conexiones web modernas.

**Características:**
- Corre sobre UDP en vez de TCP
- Establece conexiones más rápido
- Cifrado integrado
- Usado por Chrome, Firefox y servicios de Google y Cloudflare

**Implicancia en seguridad:**
QUIC está cifrado y es casi imposible de analizar en Wireshark.
En 2026 la mayor parte del tráfico web es QUIC o TLS.
Wireshark muestra que hay tráfico pero no qué dice.

---

## Filtros para detectar anomalías

dns.flags.rcode != 0

Muestra consultas DNS que fallaron. Resultado: 0 errores.

tcp.flags.reset == 1

Muestra conexiones rechazadas abruptamente (RST).
Resultado: 90 paquetes RST, todos en puerto 443.

tcp.port != 80 && tcp.port != 443

Muestra tráfico TCP en puertos inusuales.
Resultado: ningún puerto inusual.

---

## Análisis de RST

90 paquetes RST encontrados, todos en puerto 443.

**RST en puerto 443:** normal. Los servidores cierran
conexiones abruptamente cuando el cliente no las necesita
o hay timeouts. Es tráfico esperado en navegación moderna.

**RST sería sospechoso si aparece en:**
- Puerto 4444, 1337 o cualquier puerto no reconocido
- Grandes cantidades hacia una misma IP desconocida
- Combinado con otros indicadores anómalos

---

## Conclusión del análisis

No se encontraron anomalías en la captura.

- Sin errores DNS
- RST solo en puerto 443, tráfico normal
- Sin puertos inusuales
- Tráfico esperado para navegación web con Firefox

**Una captura sin anomalías también es un resultado válido.**
Un analista que documenta la ausencia de anomalías con
evidencia es tan valioso como uno que encuentra algo.

---

## Señales de anomalía a buscar

- Predominancia de HTTP sobre HTTPS
- Conexiones a dominios desconocidos en segundo plano
- Ausencia de OCSP en tráfico HTTPS
- RST en puertos inusuales
- Grandes volúmenes de tráfico hacia una sola IP externa
