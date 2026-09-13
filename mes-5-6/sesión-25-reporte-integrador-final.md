Fecha: 13/09/2026
Escaneo completo y reporte

ESCANEO DE PUERTOS:

Se procede a escanear el equipo con la herramienta nmap, apuntado a la IP local 127.0.0.1 utilizando el filtro -A. Siendo este el filtro el más potente pero más detectable a la vez, se elije porque no es necesario no levantar sospecha de escaneo.

Se detectan dos puertos abiertos los cuales presentan versiones de servicios desactualizadas, por ende vulnerables a un ataque:

Puerto 22 SSH: OpenSSH 10.2p1
CVE-2026-60002 
CVSS: 9.4 CRITICAL
Esta vulnerabilidad afecta a versiones anteriores a 10.4, el equipo escaneado tiene 10.2p1. Afecta como cliente, se produce un ataque de corrupción de memoria.

Puerto 80 HTTP: Apache httpd 2.4.66
CVE-2026-24072
CVSS: 8.8 HIGH
Esta vulnerabilidad permite a los autores locales de archivos .htaccess leer archivos con los privilegios del usuario httpd. Requiere privilegios bajos, no acceso anónimo. 

Tanto para SSH y HTTP se requiere actualización de servicios de manera urgente.

ANÁLISIS DE TRÁFICO:

Se procede a capturar el tráfico generado con el escaneo de puertos:

Se detecta un volumen de captura de 2464 paquetes. Se pueden observar los siguientes protocolos: Lc-mc, IPv4, UDP, TCP, SSH, HTTP, L-btd, HTML, ICMP

Se aplica un filtro DNS, para ver los dominios consultados, en este caso no se registran ya que nos comunicamos directamente con la IP, tampoco errores del mismo.

Se filtra el tráfico HTTP, 58 paquetes.
Se encuentran 1011 paquetes con conexiones rechazadas. 
Se encuentran 2145 paquetes con conexiones a puertos diferentes del 80 y el 443.

**Sin DNS,** directo a la IP. **HTTP,** Nmap hizo requests para detectar el servicio, normal **Muchos RST,** normal, son los puertos cerrados respondiendo **Muchos puertos distintos a 80 y 443:** normal, Nmap escaneó del 1 al 1024.
