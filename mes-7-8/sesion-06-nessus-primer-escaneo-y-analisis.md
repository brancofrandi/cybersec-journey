# Mes 7-8 (SIEM) — Sesión 06: Primer escaneo con Nessus y análisis de resultados
 
**Fecha:** 24/09/2026 (cierre del lunes) a 11/10/2026 (análisis de resultados)
**Estado:** Semana 3, Lunes **cerrado**; Martes (análisis de resultados) **cerrado** con cuatro hallazgos trabajados de punta a punta. Continúa Miércoles (repaso liviano, sin abrir Kali).
 
Continúa la [sesión 05](sesion-05-nessus-instalacion.md) (instalación y activación de Nessus). Esta sesión combina el cierre del lunes (primer escaneo real) con el contenido del martes, por decisión de agrupar ambos en un solo registro.
 
## Objetivo
 
Lanzar el primer escaneo de vulnerabilidades contra Metasploitable2 y aprender a **analizar los resultados**: clasificar cada hallazgo, interpretar su gravedad (CVSS), entender su impacto y redactar una línea de reporte accionable.
 
## Parte 1 — Cierre del lunes: el primer escaneo real
 
**Intento 1 (falso positivo de "Completed").** El escaneo terminó en pocos segundos sin resultados. La causa: Metasploitable2 estaba apagada y el escaneo se lanzó sin verificar el objetivo. Aprendizaje: un tiempo de ejecución sospechosamente corto es una señal de alerta, no un éxito; y **antes de escanear se verifica que el objetivo esté encendido y responda** (`ping -c 4 192.168.100.101`).
 
**Intento 2 (objetivo sin conectividad).** Con la VM encendida, el ping seguía fallando (*Destination Host Unreachable*). La IP de Metasploitable2 se había asignado solo de forma **efímera** (`ip addr add`, se pierde al reiniciar), a diferencia de Kali y de la VM del agente Wazuh, que tienen IP persistente. Se reasignó:
 
```
sudo ip addr add 192.168.100.101/24 dev eth0
ip addr          # verificar
```
 
Detalles útiles del camino: en Kali la persistencia se resuelve con `nmcli`, pero Metasploitable2 es un Ubuntu 8.04 sin NetworkManager, así que ahí `nmcli` no aplica. Un error de tipeo en un octeto de la IP se deshace con `ip addr del ... dev eth0`.
 
**Intento 3 (escaneo real).** Con la IP corregida, el escaneo tardó más de una hora (esperable por la cantidad de servicios vulnerables y el hardware ajustado) y completó con resultados reales:
 
| Dato | Valor |
|---|---|
| Objetivo | Metasploitable2, `192.168.100.101` |
| Plantilla | Basic Network Scan, sin autenticar (Auth: Fail, esperado) |
| Resultado | **67 vulnerabilidades: 9 críticas, 6 altas, 23 medias** |
 
Nessus mostró además un aviso de "IP tracking irregularities" sin impacto funcional confirmado; queda a vigilar si reaparece.
 
## Parte 2 — Conceptos del martes
 
- **Host discovery vs vulnerability scan:** el primero solo detecta hosts vivos (como `nmap -sn`); el segundo (Basic Network Scan) analiza servicios contra la base de plugins de CVEs.
- **Política (Policy):** configuración de escaneo guardada y reutilizable. Vale la pena cuando se repetirá el mismo escaneo sobre los mismos objetivos; para un laboratorio puntual no es necesaria, pero sirve de práctica.
- **CVSS v2:** vector `AV:N/AC:L/Au:N/C:C/I:C/A:C` (vector de ataque, complejidad, autenticación, impacto en confidencialidad/integridad/disponibilidad).
- **CVSS v3:** agrega **PR** (privilegios requeridos), **UI** (interacción del usuario) y **S** (alcance):
| Sigla | Qué mide |
|---|---|
| AV | Vector de ataque (N = por red) |
| AC | Complejidad del ataque |
| PR | Privilegios requeridos antes de explotar |
| UI | Si hace falta que un usuario haga algo |
| S | Si el impacto sale del componente vulnerable (U = no se extiende) |
| C / I / A | Confidencialidad, integridad, disponibilidad (H = alto) |
 
Los tres impactos conectan directo con la tríada CIA vista en la preparación de entrevistas.
 
## Parte 3 — Análisis de cuatro hallazgos
 
Para cada uno: leer el plugin, interpretar el CVSS, ubicar la categoría de la guía de vulnerabilidades, razonar la remediación y redactar una línea de reporte con tres partes (**qué y dónde / acción / estado deseado**).
 
### 3.1 VNC con contraseña trivial — credenciales débiles (Critical, CVSS 10.0)
 
Nessus se autenticó en el servicio VNC (5900/tcp por convención) con una credencial trivial. CVSS 10.0 porque VNC no es solo una línea de comandos: da **control gráfico completo** del escritorio con todos los permisos de la sesión, con impacto total sobre la tríada CIA.
 
Remediación razonada: contraseña fuerte y única; restringir por firewall quién puede conectarse **por IP, subred y puerto** (el firewall actúa sobre la red, no sobre usuarios o grupos del sistema operativo); deshabilitar el servicio si no se usa; MFA donde sea posible.
 
> **Línea de reporte:** El servicio VNC del host 192.168.100.101 acepta autenticación con una contraseña trivial, lo que permite a un atacante sin credenciales previas obtener control gráfico completo (CVSS 10.0). Cambiar la contraseña por una fuerte y única, restringir por firewall el acceso a la IP o subred de administración y deshabilitar el servicio si no es necesario. Estado deseado: acceso solo desde equipos autorizados con credencial robusta, o servicio desactivado.
 
### 3.2 SSL versiones 2 y 3 — configuración sin hardening (Critical, CVSS v3 9.8)
 
Detectado en dos servicios independientes: **25/tcp (Postfix/SMTP)** y **5432/tcp (PostgreSQL)**. SSLv2/v3 no es "texto plano": es cifrado, pero **roto** (padding inseguro, renegociación insegura, vulnerable a downgrade tipo POODLE), lo que permite a un atacante en posición de intermediario degradar la conexión y descifrar el tráfico.
 
La pista para categorizarlo está en el campo *Solution*: pide **deshabilitar** protocolos en el mismo software (cambio de configuración), no actualizar a otra versión. Un servidor completamente actualizado puede tener este mismo hallazgo. Como cada servicio tiene su propia configuración TLS, el arreglo es **servicio por servicio**, no hay un interruptor central.
 
> **Línea de reporte (Postfix, 25/tcp):** El servicio SMTP acepta conexiones con SSLv2/v3. Se recomienda ajustar su configuración TLS, según la documentación oficial de Postfix, para deshabilitar esos protocolos y exigir TLS 1.2 o superior.
>
> **Línea de reporte (PostgreSQL, 5432/tcp):** El servicio PostgreSQL acepta conexiones con SSLv2/v3. Se recomienda ajustar su configuración TLS, según la documentación oficial de la versión instalada, para deshabilitar esos protocolos y exigir TLS 1.2 o superior.
 
Lección: en un primer intento se nombró un parámetro de PostgreSQL sin verificarlo. Ese parámetro (`ssl_min_protocol_version`) existe recién desde PostgreSQL 12, y Metasploitable2 trae una versión mucho más antigua. Un dato específico sin verificar puede estar mal aunque esté bien escrito: el "cómo" va en términos generales hasta comprobarlo.
 
### 3.3 Ghostcat — Apache Tomcat AJP, CVE-2020-1938 (puerto 8009/tcp)
 
AJP es un protocolo binario interno de Tomcat. Expuesto sin control, permite **leer archivos de la aplicación web sin autenticación** (Nessus leyó `WEB-INF/web.xml`, carpeta que debería ser privada) y, si hay carga de archivos, llegar a ejecución remota de código.
 
Proceso de investigación (CVSS no registrado en esta sesión):
 
1. **Componente y versión:** `nmap -sV 192.168.100.101 -p 8009` devuelve `ajp13 / Apache Jserv (Protocol v1.3)`, que no revela la versión. En el 8180 (HTTP), `nmap -sV` devuelve "Coyote JSP engine 1.1", que es la versión del conector, no la de Tomcat. La versión salió de `nmap --script http-title 192.168.100.101 -p 8180` → **Apache Tomcat/5.5**. Es una **inferencia**: se asume que ambos puertos pertenecen a la misma instancia.
2. **Búsqueda:** componente + versión + nombre del problema + palabra de fuente (`apache tomcat 5.5 ghostcat advisory`, `CVE-2020-1938 tomcat`).
3. **Fuente primaria:** `nvd.nist.gov`, donde el CVE figura publicado por el propio fabricante (CNA: Apache Software Foundation). Blogs y sitios de terceros funcionan como pista, no como fuente.
4. **Aviso oficial:** afectadas 9.0.0.M1–9.0.30, 8.5.0–8.5.50 y 7.0.0–7.0.99; corregido en 9.0.31, 8.5.51 y 7.0.100. El conector AJP venía habilitado por defecto; la mitigación es necesaria si el puerto es alcanzable por usuarios no confiables.
5. **Documentación vs sistema real:** la 5.5 no figura por ser una versión sin soporte, pero Nessus logró leer el archivo en esta máquina. La prueba real pesa más que la lista.
> **Línea de reporte:** El conector AJP de Apache Tomcat (8009/tcp) permite leer archivos internos de la aplicación web sin autenticación (CVE-2020-1938, "Ghostcat"); Nessus leyó `WEB-INF/web.xml`. La versión (Tomcat 5.5) se infiere del servicio HTTP del puerto 8180/tcp, asumiendo la misma instancia; no figura en el aviso de Apache por estar sin soporte, pero la explotación fue comprobada. Deshabilitar el conector AJP si no se usa, o restringir el puerto 8009 a equipos de confianza mediante firewall, y migrar a una versión con soporte que incluya la corrección. Estado deseado: puerto 8009 inaccesible desde redes no confiables y Tomcat en una versión corregida (9.0.31, 8.5.51 o 7.0.100, o superior).
 
### 3.4 Bind Shell Backdoor Detection — backdoor conocido (Critical, CVSS v3 9.8, plugin 51988)
 
Servicio `wild_shell` en **1524/tcp**: una *bind shell* abre un puerto y entrega a quien se conecte una consola de comandos del sistema, **sin autenticación**. Vector: `AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`.
 
El *Solution* de Nessus ("verificar si el host fue comprometido y reinstalar si es necesario") es un tercer tipo de solución, distinto de "actualizar" o "cambiar una configuración": **respuesta a incidentes**. Un backdoor no se asegura, se reemplaza. Para un analista de SOC, un backdoor abierto es un **posible incidente**, no un ticket más: se eleva, se contiene (aislar el equipo) y se preserva evidencia antes de reinstalar, porque reinstalar borra las pruebas. (En el laboratorio, Metasploitable2 viene "comprometida de fábrica" a propósito.)
 
> **Línea de reporte:** El servicio wild_shell del host 192.168.100.101 en el puerto 1524/tcp está abierto y escuchando; permite que un atacante se conecte y ejecute comandos directamente sin autenticación (CVSS 9.8). Verificar si el host está comprometido, contener y preservar evidencia, elevar el caso y reemplazar el sistema en vez de parchearlo. Estado deseado: puerto 1524/tcp cerrado e inaccesible desde la red, y sistema verificado como no comprometido o reinstalado desde una fuente limpia.
 
## Parte 4 — Método de análisis
 
**Versión corta (la que cubre la mayoría de los casos):** (1) ¿qué es y dónde está? servicio y puerto; (2) ¿qué tan grave es? CVSS y evidencia del Output; (3) ¿cómo se arregla? el campo *Solution* indica si hay que actualizar, cambiar una configuración o responder a un incidente.
 
**Versión larga (cuando el Solution no alcanza):** identificar el componente real → armar una búsqueda precisa → priorizar fuentes oficiales → navegar la documentación con `Ctrl+F` → confirmar contra el sistema real antes de escribir.
 
| Hallazgo | Categoría | Tipo de solución | Investigación |
|---|---|---|---|
| VNC con contraseña trivial | Credenciales débiles | Procedimental | No |
| SSLv2/v3 (25 y 5432) | Configuración sin hardening | Cambiar un ajuste, servicio por servicio | Sí |
| Ghostcat (8009) | Software desactualizado con CVE | Restringir AJP y actualizar | Sí |
| Bind shell (1524) | Backdoor conocido | Respuesta a incidentes y reemplazo | No |
 
## Fricciones y aprendizajes
 
- **Verificar antes de interpretar:** una IP escrita mal (`127.x`, rango *loopback*) hizo escanear el propio Kali. Un resultado negativo contra el objetivo equivocado no prueba nada. Hábitos: releer la IP dígito a dígito y recuperar comandos anteriores con la flecha ↑.
- **Estado deseado vs acción:** tendencia a repetir la acción en la tercera parte de la línea de reporte; el estado debe describir cómo queda el sistema y poder comprobarse.
- **Versión que no es versión:** "Coyote 1.1" es el conector HTTP de Tomcat, no la versión del servidor.
- **No inventar datos específicos** (archivos, parámetros) que no se verificaron.
- **Repasos con palabras propias**, no copiados de los apuntes.
## Pendientes
 
- Miércoles: repaso liviano de memoria, sin abrir herramientas.
- Jueves: escaneo más profundo / con credenciales si aplica, exportar reporte.
- Viernes: integradora, correlacionar con Nmap/Wireshark y armar un mini-reporte priorizado por CVSS.
- Evaluar disco externo SSD para alojar las VMs (el disco del host sigue ajustado).
