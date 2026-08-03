## Sesión 12 — Wireshark: análisis de tráfico real y reporte profesional
**Fecha:** 04/08/2026

---

## Ejercicio practicado

Captura de tráfico real con Firefox navegando a httpbin.org.
Total de paquetes capturados: 1046
Interfaz: any

---

## Reporte profesional

**Análisis de tráfico — Sesión de navegación web**
**Herramienta:** Wireshark 4.6.4
**Interfaz:** any
**Tráfico generado:** Firefox navegando a httpbin.org

**Resumen:**
Se capturaron 1046 paquetes. Protocolos identificados:
DNS, HTTP, TLSv1.2, TLSv1.3, OCSP, TCP.

**DNS — 84 consultas registradas**

Dominios consultados:
- httpbin.org → dominio objetivo
- o.pki.goog → servidor OCSP de Google
- push.services.mozilla.com → servicio de Firefox
- cloudflare-dns.com → DNS alternativo
- safebrowsing.googleapis.com → verificación de seguridad de Google

Consultas tipo A (IPv4) y AAAA (IPv6).
IP recibida para httpbin.org: 100.61.253.169

**HTTP — 14 paquetes**

Eventos identificados:
- GET /get HTTP/1.1 → solicitud a httpbin.org
- Respuesta 200 OK con JSON
- GET /success.txt?ipv6 → verificación de conectividad IPv6 de Firefox
- GET /success.txt?ipv4 → verificación de conectividad IPv4 de Firefox
- Respuestas 200 OK en ambas verificaciones

**OCSP**
Firefox verificó certificados SSL con o.pki.goog antes
de establecer conexiones HTTPS.

**Observaciones:**
- Tráfico HTTP plano mínimo. La mayoría es HTTPS cifrado.
- Firefox genera decenas de conexiones en segundo plano.
- User-Agent: Mozilla/5.0 identifica el navegador Firefox.

**Recomendaciones:**
- Evitar sitios HTTP en redes públicas.
- HTTPS protege el contenido pero no oculta los dominios
  consultados, visibles en el DNS.

---

## Verificación del reporte

El reporte fue verificado en Wireshark confirmando:
- IP de httpbin.org en DNS coincide con la del GET HTTP
- Campos HTTP correctos: Method, URI, Host, User-Agent
- User-Agent Mozilla/5.0 porque fue Firefox, no curl

---

## Criterio de análisis en Wireshark

Orden para analizar una captura profesionalmente:

1. **Volumen y protocolos** → cuántos paquetes y qué protocolos
2. **DNS** → qué dominios se consultaron
3. **TCP** → conexiones exitosas o rechazadas (RST, FIN)
4. **HTTP** → requests, métodos y códigos de respuesta
5. **Anomalías** → dominios desconocidos, puertos inusuales

---

## Diferencia entre curl y Firefox en Wireshark

| Herramienta | User-Agent | Tráfico generado |
|-------------|------------|-----------------|
| curl | curl/8.18.0 | Solo el request especificado |
| Firefox | Mozilla/5.0 | Decenas de conexiones en segundo plano |

---

## Campos de un paquete HTTP request

| Campo | Qué indica |
|-------|-----------|
| Method | Acción: GET, POST, PUT, DELETE |
| Host | Dominio solicitado |
| User-Agent | Programa que hizo la solicitud |
| Accept | Tipo de formato que puede recibir |

---

## GET /success.txt?ipv4 y ipv6

Firefox verifica automáticamente si tiene conectividad
a internet tanto por IPv4 como por IPv6.
Si el servidor responde 200 OK, confirma que la conexión funciona.
