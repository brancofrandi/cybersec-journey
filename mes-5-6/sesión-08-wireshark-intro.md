## Sesión 08 — Introducción a Wireshark
**Fecha:** 28/07/2026

---

## ¿Qué es Wireshark?

Herramienta de captura y análisis de tráfico de red en tiempo real.
Intercepta los paquetes que viajan por una interfaz de red
y permite ver exactamente qué datos se transmiten.

**Diferencia con Nmap:**
- Nmap → dice qué puertos están abiertos
- Wireshark → muestra el contenido real del tráfico

---

## Interfaz de Wireshark

**Columnas principales:**
- No. → número de paquete
- Time → tiempo desde el inicio de la captura
- Source → IP origen
- Destination → IP destino
- Protocol → protocolo usado
- Length → tamaño del paquete
- Info → resumen del paquete

**Panel inferior izquierdo:**
Muestra las capas del paquete seleccionado (modelo OSI en acción).

---

## Protocolos capturados hoy

**ICMP (ping)**
Protocolo de control de mensajes de internet.
Usado por ping para verificar conectividad y medir latencia.
No transfiere datos, solo mensajes de control.
Aparece en pares: Echo Request y Echo Reply.

**DNS**
Consulta de resolución de nombres.
Antes de conectarse a example.com, la VM preguntó
qué IP corresponde a ese dominio.

**TCP**
Protocolo de transporte usado por HTTP.
Establece la conexión con three-way handshake.

**HTTP**
Tráfico web sin cifrado.
Todo el contenido es visible en texto plano.

---

GET / HTTP/1.1

Host: example.com

User-Agent: curl/8.18.0

Accept: /

**Qué significa cada línea:**
- `GET / HTTP/1.1` → método HTTP, ruta solicitada y versión del protocolo
- `Host: example.com` → dominio al que se conecta
- `User-Agent: curl/8.18.0` → programa que hizo la solicitud

**Relevancia en seguridad:**
HTTP viaja en texto plano. Un atacante con acceso a la red
puede ver qué sitios visitás, qué datos enviás y con qué programa.
HTTPS cifra todo este contenido, haciendo ilegible la captura.

---

## Filtros en Wireshark

Permiten mostrar solo los paquetes del protocolo que necesitás.

## Análisis de un paquete HTTP

Al expandir la capa HTTP se ve:

http → solo paquetes HTTP

dns → solo consultas DNS

icmp → solo paquetes ping

tcp → solo tráfico TCP

Se escriben en la barra superior y se aplican con Enter.

---

## Capas del modelo OSI en un paquete real

Al hacer click en un paquete HTTP se ven todas las capas:
- Frame → información del paquete físico
- Ethernet II → capa de enlace, MAC addresses
- Internet Protocol → capa de red, IPs
- Transmission Control Protocol → capa de transporte
- Hypertext Transfer Protocol → capa de aplicación

---

## Comandos usados para generar tráfico

```bash
ping google.com          # genera tráfico ICMP
curl http://example.com  # genera tráfico HTTP y DNS
```
