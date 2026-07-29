## Sesión 09 — Wireshark: filtros y análisis de protocolos
**Fecha:** 29/07/2026

---

## Flujo completo de una solicitud HTTP

Cuando ejecutás `curl http://neverssl.com` en la red
se generan estos protocolos en orden:

TU VM SERVIDOR

| |

|--- DNS: ¿IP de neverssl.com? ->|

|<-- DNS: 5.22.145.16 ----------|

| |

|--- TCP SYN ------------------->| three-way handshake

|<-- TCP SYN-ACK ---------------|

|--- TCP ACK ------------------->|

| |

|--- HTTP GET / ---------------->| solicitud

|<-- HTTP 200 OK + HTML --------| respuesta

| |

|--- TCP FIN ------------------->| cierre

|<-- TCP FIN-ACK ---------------|

Cada fila es un paquete visible en Wireshark.

---

## Filtros en Wireshark

Se escriben en la barra superior y se aplican con Enter.
Muestran solo los paquetes del protocolo o condición indicada.

http → solo paquetes HTTP

dns → solo consultas DNS

icmp → solo paquetes ping

tcp → solo tráfico TCP

---

## Análisis de un paquete DNS

Al expandir la capa DNS se ve:

Domain Name System (response)

Transaction ID: 0xd4d5

Flags: Standard query response, No error

Questions: 1

Answer RRs: 2

Answers:

5.22.145.16

5.22.145.121

**Answer RRs:** cantidad de registros en la respuesta.
**Answers:** las IPs que corresponden al dominio consultado.
Dos IPs significa redundancia o distribución de tráfico.

---

## Análisis de un paquete HTTP

Al expandir la capa HTTP se ve:

GET /1.1 HTTP/1.1

Request Method: GET

Request URI: /1.1

Host: www.HTTP.com

User-Agent: lwp-perl/6.81

Connection: close

**Request Method:** GET pide un recurso al servidor.
**Host:** dominio al que se conecta.
**User-Agent:** programa que hizo la solicitud.

---

## User-Agent en ciberseguridad

Identifica el cliente que hizo la solicitud HTTP.

Ejemplos:
- curl/8.18.0 → solicitud con curl
- Mozilla/5.0 Chrome/125 → solicitud desde Chrome
- python-requests/2.28 → script Python

**Por qué importa:**
- Un atacante puede falsificarlo para parecer un navegador normal
- Un analista puede detectar herramientas de ataque por su User-Agent
- Permite identificar bots y scripts automatizados

---

## HTTP vs HTTPS en Wireshark

- **HTTP:** todo el contenido visible en texto plano.
  Se pueden leer headers, datos y credenciales.
- **HTTPS (TLS):** contenido cifrado, aparece como TLSv1.2.
  No se puede leer el contenido sin la clave privada.

---

## Comandos usados para generar tráfico

```bash
ping google.com &        # tráfico ICMP en segundo plano
curl http://neverssl.com # tráfico HTTP + DNS + TCP
pkill ping               # detener el ping
```

---

## Observación importante

Para capturar HTTP plano usar la interfaz **any** en Wireshark,
no solo eth0. Algunos protocolos pasan por otras interfaces.
neverssl.com es útil para pruebas porque no redirige a HTTPS.
