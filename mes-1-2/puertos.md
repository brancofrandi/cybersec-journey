
## Puertos de red
**Fecha:** [31/03/26]

---

### Qué es un puerto

Identificador lógico (del 0 al 65535) que permite al SO
dirigir el tráfico hacia una aplicacián o servicio específico.
La IP identifica el dispositivo, el puerto identifica el servicio.

### Rangos

- 0-1023: bien conocidos, servicios estandar
- 1024-49151: registrados, aplicaciones específicas
- 49152-65535: dinámicos, asignados temporalmente

### Puertos clave

| Puerto | Protocolo | Para que sirve |
|--------|-----------|----------------|
| 21 | FTP | Transferencia de archivos sin cifrado. Inseguro, reemplazado por SFTP |
| 22 | SSH | Acceso remoto seguro a terminal. Crítico si esta mal configurado |
| 80 | HTTP | Tráfico web sin cifrado |
| 443 | HTTPS | Tráfico web cifrado con SSL/TLS |
| 3389 | RDP | Escritorio remoto Windows. Alto riesgo si esta expuesto a internet |

### Estados de un puerto

- Abierto: hay una aplicación escuchando, acepta conexiones
- Cerrado: accesible pero sin servicio escuchando
- Filtrado: firewall bloqueando, no hay respuesta. Nmap no puede
  determinar si esta abierto o cerrado

### Conceptos relacionados

**Puerto escuchando:** hay un proceso esperando activamente
conexiones entrantes en ese puerto. No implica que sea visible
desde afuera, un firewall puede tenerlo filtrado internamente.

**FTP vs SFTP:** FTP no cifra los datos, inseguro.
SFTP es FTP sobre SSH, transfiere archivos cifrados.
Son distintos: SSH es para acceso a terminal, SFTP para archivos.

**Telnet:** protocolo antiguo de acceso remoto, opera en puerto 23.
Transmite en texto plano, sin cifrado. Reemplazado por SSH.

### Relevancia en ciberseguridad

Identificar puertos abiertos en un sistema es uno de los
primeros pasos en un pentest. Cada puerto abierto es un
posible vector de entrada si el servicio esta mal configurado
o tiene vulnerabilidades conocidas.

### Dudas pendientes
- Profundizar SSL/STL
