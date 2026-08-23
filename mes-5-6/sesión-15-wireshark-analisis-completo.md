## Sesión 15 — Wireshark: análisis completo siguiendo los 7 pasos
**Fecha:** 23/08/2026

---

## Datos de la captura

**Herramienta:** Wireshark 4.6.4
**Interfaz:** any
**Sitios navegados:** google.com, youtube.com, reddit.com
**Total de paquetes:** 41801

**Protocolos identificados:**
IPv4, IPv6, TCP, UDP, QUIC, DNS, HTTP, TLS, OCSP, ICMP, ARP

---

## Análisis — 7 pasos

### Paso 1 — Volumen y protocolos
41801 paquetes capturados.
QUIC domina el tráfico moderno. HTTP plano es mínimo.
TLS confirma que la mayoría del tráfico está cifrado.

### Paso 2 — DNS
306 paquetes DNS. 153 son queries.

Dominios navegados directamente:
- google.com
- youtube.com
- reddit.com

Dominios en segundo plano identificados:
- w3-reporting.reddit.com → telemetría de Reddit
- static.doubleclick.net → publicidad de Google
- id.rlcdn.com → tracking de Ramp (publicidad)
- gstatic.com → recursos estáticos de Google

### Paso 3 — Errores DNS
Filtro: `dns.flags.rcode != 0`
Resultado: ningún error. Todas las consultas resueltas correctamente.

### Paso 4 — HTTP
Filtro: `http`
36 paquetes HTTP.

- OCSP requests y responses → verificación de certificados SSL
  con digicert.com y setigo.com
- GET /success.txt?ipv4 → Firefox verifica conectividad IPv4
- GET /success.txt?ipv6 → Firefox verifica conectividad IPv6

### Paso 5 — Conexiones rechazadas
Filtro: `tcp.flags.reset == 1`
79 paquetes RST, todos en puerto 443.
Normal: son timeouts o conexiones cerradas por el servidor.
No se encontraron RST en puertos inusuales.

### Paso 6 — Puertos inusuales
Filtro: `tcp.port != 80 && tcp.port != 443`
Resultado: ningún paquete. Sin puertos inusuales.

### Paso 7 — Anomalías identificadas

No se registraron anomalías de seguridad críticas.

**Observación sobre tracking:**
Se identificaron dominios de publicidad en segundo plano
(doubleclick.net, id.rlcdn.com) sin intervención del usuario.
En entornos corporativos este tráfico podría estar prohibido
y debería bloquearse a nivel de red o DNS.

---

## Conclusión

El análisis no registró anomalías severas.
El tráfico es el esperado para una sesión de navegación web moderna.

**Recomendación:**
Revisar la política de tracking en entornos corporativos.
Los dominios de publicidad registran el comportamiento de
navegación de los usuarios. En contextos donde la privacidad
es crítica, se recomienda bloquear esos dominios a nivel
de firewall o DNS.

---

## Conceptos aplicados en esta sesión

**Tracking:** dominios de terceros que aparecen en segundo
plano para registrar el comportamiento de navegación.
Legítimos pero problemáticos en entornos corporativos.

**OCSP:** verificación de certificados SSL antes de HTTPS.
Aparece como HTTP pero no es tráfico web normal.

**RST en 443:** normal. Timeouts o cierres de conexión
por parte del servidor. Sospechoso solo en puertos inusuales.

**Puerto origen vs puerto destino:**
Los números de puerto altos (59582, 54302) son puertos
de origen de la VM, no servicios remotos.
El puerto destino es el que identifica el servicio.
