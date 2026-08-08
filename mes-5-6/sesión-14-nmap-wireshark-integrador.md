## Sesión 14 — Práctica integradora: Nmap + Wireshark
**Fecha:** 08/08/2026

---

## Objetivo

Usar Nmap y Wireshark de forma combinada sobre el mismo objetivo.
Nmap para identificar servicios, Wireshark para capturar el tráfico del escaneo.

---

## Escaneo con Nmap

**Herramienta:** Nmap 7.98
**Objetivo:** 127.0.0.1
**Comando:** nmap -p 1-65535 -sV 127.0.0.1

**Puertos abiertos encontrados:**

- `22/tcp open ssh OpenSSH 10.2p1 Debian 5 (protocol 2.0)`
- `80/tcp open http Apache httpd 2.4.66 ((Debian))`

---

## Búsqueda de CVEs

### OpenSSH 10.2p1
**Fuente:** nvd.nist.gov — búsqueda: `openssh`

**CVE-2026-60002 — CRITICAL 9.4**
SSH en OpenSSH en versiones anteriores a 10.4 puede tener
un uso después de la liberación cuando un servidor cambia
su clave de host durante un cambio de clave.
Solo ocurre en el lado del cliente.

- Privilegios requeridos: NONE
- Interacción del usuario: NONE
- No requiere autenticación previa
- **Solución:** actualizar a OpenSSH 10.4p1

---

### Apache httpd 2.4.66
**Fuente:** nvd.nist.gov — búsqueda: `Apache HTTP Server 2.4.66`

**CVE-2026-29169 — más crítico**
Desreferenciación de puntero nulo en mod_dav_lock.
Un atacante puede bloquear el servidor con una solicitud maliciosa.
Ataque de tipo Denial of Service (DoS).
No requiere autenticación previa.
**Solución:** actualizar a Apache 2.4.67.

**CVE-2026-33007**
Desreferenciación de puntero nulo en mod_authn_socache.
Bloquea un proceso hijo del servidor en configuración de proxy con caché.
**Solución:** actualizar a Apache 2.4.67.

**CVE-2026-33006**
Ataque de temporización en mod_auth_digest.
Permite eludir autenticación Digest.
Requiere que el servidor use ese sistema de autenticación.
**Solución:** actualizar a Apache 2.4.67.

---

## Captura con Wireshark

**Herramienta:** Wireshark 4.6.4
**Interfaz:** any
**Paquetes capturados:** 108427

Se observa gran volumen de paquetes TCP ya que Nmap escanea
cada puerto mediante este protocolo.
Los puertos distintos al 22 y 80 respondieron con RST,
indicando que no tienen servicios activos.

---

## Conclusión

Nmap detectó dos puertos abiertos escaneando el servicio.
Wireshark confirmó que los puertos 22 y 80 tuvieron más
intercambio de datos que el resto, mientras los demás
respondieron con RST.

Ambas herramientas se complementan: Nmap identifica
qué está abierto, Wireshark muestra cómo se ve ese
tráfico desde la red.

---

## Conceptos aprendidos

**Puerto escaneado vs puerto abierto:**
Nmap escanea miles de puertos pero solo reporta como abiertos
los que reciben respuesta de un servicio activo.
Los cerrados responden con RST. Los filtrados no responden.

**RST en Wireshark:**
Conexión rechazada abruptamente. Normal en puertos sin servicio.
Sospechoso si aparece en puertos inusuales como 4444 o 1337.

**Flujo de análisis de CVEs:**
1. Buscar en nvd.nist.gov por nombre del servicio
2. Verificar puntaje CVSS
3. Verificar si requiere autenticación previa
4. Verificar qué versiones están afectadas
5. Aplicar solución: generalmente actualizar versión
