# Conceptos clave para entrevistas — Ciberseguridad Junior

---

## CIA Triad

**Confidencialidad:** solo las personas autorizadas acceden a la información.
Ataque que la rompe: interceptación de comunicaciones, robo de credenciales.

**Integridad:** la información no es modificada sin autorización.
Ataque que la rompe: modificación de datos en tránsito, tampering.

**Disponibilidad:** los sistemas y datos están accesibles cuando se necesitan.
Ataque que la rompe: DoS, DDoS, ransomware.

---

## Amenaza, Vulnerabilidad y Riesgo

**Vulnerabilidad:** debilidad o falla en un sistema que puede ser explotada.
Ejemplo: un software desactualizado con un CVE conocido.

**Amenaza:** actor o evento que puede explotar una vulnerabilidad.
Puede ser intencional (atacante), no intencional (error humano) o natural (desastre).

**Riesgo:** probabilidad de que una amenaza explote una vulnerabilidad y cause daño.
Riesgo = Amenaza × Vulnerabilidad × Impacto

---

## DoS vs DDoS

**DoS (Denial of Service):** un solo dispositivo satura un servidor para dejarlo inaccesible.

**DDoS (Distributed Denial of Service):** múltiples dispositivos coordinados atacan simultáneamente.
Usa botnets: dispositivos de terceros comprometidos y controlados por el atacante.

La diferencia clave es el origen: uno vs muchos.

---

## Modelo OSI — 7 capas

| Capa | Nombre | Protocolo/Ejemplo |
|------|--------|-------------------|
| 7 | Application | HTTP, DNS, SSH, FTP |
| 6 | Presentation | Cifrado, compresión |
| 5 | Session | Gestión de sesiones |
| 4 | Transport | TCP, UDP, puertos |
| 3 | Network | IP, routers |
| 2 | Data Link | MAC, switches, frames |
| 1 | Physical | Cables, señales |

Mnemónico: **Please Do Not Throw Sausage Pizza Away**

El modelo OSI es teórico. La implementación real es TCP/IP (4 capas).

---

## TCP vs UDP

**TCP:** orientado a conexión. Garantiza entrega con three-way handshake.
- SYN → cliente quiere conectarse
- SYN-ACK → servidor confirma
- ACK → cliente confirma, conexión establecida

**UDP:** sin conexión. No garantiza entrega. Más rápido.
Uso: streaming, videollamadas, juegos online, DNS.

Ambos son protocolos de capa 4 (Transporte).

---

## Puertos conocidos

| Puerto | Servicio |
|--------|----------|
| 21 | FTP |
| 22 | SSH |
| 25 | SMTP (email) |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 1433 | SQL Server |
| 3389 | RDP (escritorio remoto Windows) |

---

## DNS

Domain Name System. Traduce nombres de dominio (google.com) a direcciones IP.
Sin DNS habría que memorizar las IPs de cada sitio.

Consulta DNS: tipo A para IPv4, tipo AAAA para IPv6.

---

## Firewall

Filtra el tráfico de red según reglas definidas: permite o bloquea conexiones
basándose en IP, puertos y protocolos.

Opera en capas 3 y 4 del modelo OSI.

**Arquitectura básica:**

Internet → Firewall → DMZ → Firewall → Red interna


**DMZ (Zona Desmilitarizada):** red intermedia donde van los servidores públicos
(web, email). Separa internet de la red interna.

---

## WAF vs Firewall

**Firewall tradicional:** filtra por IP, puerto y protocolo. Capas 3 y 4.

**WAF (Web Application Firewall):** filtra tráfico HTTP/HTTPS. Capa 7.
Protege contra SQL injection, XSS y otros ataques de aplicación web.

Son complementarios. El WAF va después del firewall, cerca del servidor web.

---

## Hashing vs Cifrado

**Hashing:** función unidireccional. Convierte datos en un hash de longitud fija.
No se puede revertir. Se usa para verificar integridad (contraseñas, archivos).
Ejemplos: MD5, SHA-256.

**Cifrado:** función bidireccional. Los datos se pueden descifrar con la clave correcta.
Se usa para proteger confidencialidad.
Ejemplos: AES, RSA.

---

## Cyber Kill Chain

Modelo que describe las fases de un ciberataque:

1. **Reconnaissance** → el atacante recopila información del objetivo
2. **Weaponization** → crea el arma (malware, exploit)
3. **Delivery** → entrega el arma (email, USB, web)
4. **Exploitation** → explota la vulnerabilidad
5. **Installation** → instala el malware en el sistema
6. **Command & Control (C2)** → establece comunicación con el atacante
7. **Actions on Objectives** → cumple el objetivo (robo, destrucción, cifrado)

---

## IP pública vs IP privada

**IP pública:** identificador único en internet. Asignada por el ISP.
Es la que ven los servidores externos cuando te conectás.

**IP privada:** identificador dentro de una red local. No es enrutable en internet.
Rangos privados: 192.168.x.x / 10.x.x.x / 172.16.x.x

El router hace NAT (Network Address Translation) para traducir entre las dos.

---

## Términos de equipo

**Red Team:** equipo ofensivo. Simula ataques para encontrar vulnerabilidades.

**Blue Team:** equipo defensivo. Detecta y responde a amenazas.

**Purple Team:** combinación de red y blue trabajando juntos.

**SOC (Security Operations Center):** centro de operaciones que monitorea
y responde a incidentes de seguridad 24/7.

---

## MFA (Multi-Factor Authentication)

Autenticación con más de un factor:
- Algo que sabés: contraseña
- Algo que tenés: token, celular
- Algo que sos: huella, cara

Reduce el riesgo de acceso no autorizado aunque la contraseña sea comprometida.
