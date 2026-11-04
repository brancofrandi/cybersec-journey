# Sesión 08 — Ventana ciega del feed: búsqueda en NVD, CVSS 4.0 y priorización
 
**Mes 7-8 (SIEM) · Semana 3 (Nessus Essentials) · Cierre de semana (30/10 al 03/11)**
 
## Objetivo
 
Cerrar la Semana 3: cubrir la ventana ciega del feed de Nessus Essentials buscando en NVD las vulnerabilidades publicadas después de la fecha del feed, leer el vector CVSS 4.0, redactar conclusiones sin afirmar de más y priorizar hallazgos con más de un criterio.
 
## 1. Cadena Nmap → NVD
 
Servicio de partida (escaneo de la sesión 07): **ProFTPD 1.3.1** en el puerto 2121 de Metasploitable2 (192.168.100.101).
 
1. Nmap entrega producto y versión.
2. En `nvd.nist.gov/vuln/search` (búsqueda avanzada): keyword = **solo el producto**, sin la versión. Una CVE puede decir "versiones anteriores a 1.3.9e" y no contener el texto "1.3.1".
3. Filtro **Published Date(s)** con un rango desde la fecha del feed hasta hoy. En este caso, del 20/09/2026 al 30/10/2026. *Last Modified* no sirve: trae CVE viejas con registros editados recientemente.
4. Se abre cada CVE y se verifica producto, versiones afectadas y si incluye la versión detectada.
**Resultado:** tres CVE de ProFTPD (2026-119146, 2026-119364 y 2026-119147), todas publicadas el 30/10, por lo que ninguna tiene plugin en el feed. La 1.3.1 está dentro del rango afectado en las tres.
 
**Notación de rangos:** `0 <= 1.3.9e` incluye la 1.3.9e; `0 < 1.3.9e` no la incluye.
 
## 2. Qué se puede afirmar con cero resultados
 
*Esa búsqueda, en esas condiciones, no encontró CVE que afecten a esa versión.* No equivale a "está libre", por tres razones: la palabra clave puede no coincidir con cómo la CVE nombra el producto, existen fallas todavía no públicas, y NVD puede ir atrasado en cargar una CVE o sus versiones.
 
## 3. Vector CVSS 4.0
 
| Campo | Qué responde |
|---|---|
| AV | Desde dónde se ataca (`N` = red) |
| AC | Qué tan difícil es el ataque en sí (`L` / `H`) |
| AT | Si el sistema tiene que estar en cierto estado o configuración (`N` / `P`) |
| PR | Qué cuenta necesita el atacante (`N` / `L`) |
| VC / VI / VA | Qué se pierde: confidencialidad, integridad, disponibilidad (`N` / `L` / `H`) |
 
AC y AT no son lo mismo: AC:H es un ataque complejo; AT:P exige condiciones previas en el entorno.
 
| CVE | PR | AC / AT | Impacto |
|---|---|---|---|
| 2026-119146 | Ninguna | H / P | Bajo en C, I y A |
| 2026-119364 | Usuario básico | H / P | Integridad total |
| 2026-119147 | Usuario básico | L / N | Integridad parcial |
 
**Conclusión redactada sin afirmar de más:** *la versión detectada (1.3.1) está en el rango afectado; puede llegar a ser explotable si se cumplen las condiciones del vector, que no verifiqué* (con las condiciones concretas de cada CVE).
 
EPSS y KEV no sirven para CVE publicadas el mismo día: no hay historial de explotación. El orden se apoya en el vector y se revisa en unos días.
 
## 4. Priorización con más de un criterio
 
Criterios que cambian entre hallazgos: posible compromiso previo (cambia el tipo de respuesta, pasa a incidente), explotación real (EPSS y KEV), qué necesita el atacante (cuenta, condiciones, ubicación en la red), qué se pierde y qué sistema es. Criterios que **no** ordenan: "es remoto" cuando todos lo son, "tiene CVE", "es fácil de arreglar" (eso habla del costo del arreglo, no de la gravedad).
 
**Ejercicio con 4 hallazgos inventados** (valores ficticios, de una pyme): orden final: base de datos de clientes con credenciales por defecto expuesta a Internet; falla conocida en KEV en un servidor de pruebas accesible desde Internet; falla de ejecución remota en un servidor de impresión interno; certificado TLS vencido en el sitio público.
 
## 5. Plugin, feed y Ghostcat (repaso)
 
- **Plugin:** prueba de una vulnerabilidad concreta, con ID (no siempre un CVE: el de Ghostcat cubre dos y el de VNC con contraseña trivial no tiene).
- **Feed:** conjunto de plugins que publica Tenable. Al actualizarlo aparecen plugins nuevos, pero el escaneo ya hecho no cambia (hay que volver a escanear) y el atraso de 30 días de Essentials sigue.
- **Ghostcat (8009/AJP):** sin condición, un atacante que llega al puerto puede leer archivos de la aplicación (configuración, credenciales). Con la condición de que la aplicación permita subir archivos, puede llegar a ejecutar código con los permisos del usuario de Tomcat.
- **Nmap vs Nessus:** Nmap dice qué hay; Nessus dice qué está mal. El banner de un servicio es lo que el servicio dice de sí mismo; Nessus lo comprueba con una prueba.
## 6. Errores propios y aprendizajes
 
- Afirmar de más: pasar de "está en el rango afectado" a "mi servidor está afectado". Se corrigió con la redacción condicional.
- Buscar la versión pegada en NVD y fijar mal el rango de fechas (usar el día de actualización del feed en vez de hoy, y el campo *Last Modified* en vez de *Published*).
- Confundir AC con AT y cruzar letras e IDs al transcribir (1.3.1 escrito como 3.1.3; 119364 como "368"). Releer antes de enviar.
- Usar criterios que no distinguen entre hallazgos. Se corrigió pidiendo siempre dos criterios que cambien de un hallazgo a otro.
## Pendientes
 
- Subir `sesion-07` y esta sesión a `mes-7-8/` (sin códigos de activación ni datos sensibles).
- Volver a probar la priorización con un caso nuevo en la Semana 4.
- Ver qué muestra Nessus tras vencer la licencia y cómo se renueva en Essentials (no verificado).
- Servicios vistos en Nmap que Nessus no nos hizo analizar (`vsftpd 2.3.4`, `UnrealIRCd`, `distccd`): material de la Semana 6.
- Semana 4: MITRE ATT&CK.
