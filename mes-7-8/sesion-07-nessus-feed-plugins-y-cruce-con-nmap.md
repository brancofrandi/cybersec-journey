# Sesión 07 — Nessus: feed, plugins, escaneo sin autenticar y cruce con Nmap
 
**Mes 7-8 (SIEM) · Semana 3 (Nessus Essentials) · Jueves y Viernes**
 
## Objetivo
 
Cerrar la Semana 3: entender qué puede y qué no puede afirmar un escaneo de Nessus (feed de plugins, escaneo sin credenciales), mover el laboratorio a un disco externo y cruzar los hallazgos de Nessus con un escaneo de Nmap sobre Metasploitable2 para armar un mini-reporte priorizado.
 
## 1. Infraestructura: laboratorio en un SSD externo
 
El disco C: del host (117 GB) se llenó dos veces durante la compilación de plugins y pausó la VM de Kali. Solución: SSD externo de 512 GB en NTFS, con las tres VMs movidas desde VirtualBox.
 
- Comprobaciones al recibirlo: capacidad real (476 GB visibles para 512 GB nominales por la diferencia entre GB y GiB), formato (venía en exFAT, se pasó a NTFS por el journaling) y uso sostenido.
- **Aprendizaje:** el "Mover" del menú *Máquina* solo movió la configuración de la VM (1,36 MB); el disco virtual se quedó en C:. El disco se mueve desde *Archivo → Herramientas → Media → Mover*, que además actualiza las rutas registradas.
- Resultado: C: pasó de 2,9 GB a unos 57 GB libres. El disco externo tiene que estar conectado antes de abrir VirtualBox, y todavía no hay copia de seguridad de las VMs.
- Razonamiento de peor caso: el `.vdi` de Kali ocupa 46,45 GB y su máximo es 80,09 GB, o sea que solo puede crecer unos 34 GB, muy por debajo del espacio libre del SSD.
## 2. Feed de plugins y búsqueda de plugins
 
- **Plugin:** prueba pequeña que verifica una vulnerabilidad concreta, con su ID. **Feed:** el conjunto de plugins que Tenable publica.
- Nessus Essentials recibe los plugins con **30 días de retraso**. Tras actualizar, *Plugins Last Updated* pasó de 11/sep a **20/09**, que coincide con 20/10 menos 30 días.
- Una lista de eventos (*Settings → About → Events*) mostró la descarga de plugins, componentes y licencia con éxito y sin errores. El "N/A" que apareció en *Last Updated* era solo de pantalla.
- Los plugins se ven desde *Scans → escaneo → Configure → Plugins*, agrupados por familias. Confirmamos que existen el 51988 (Bind Shell Backdoor Detection) y el 134862 (Ghostcat).
- **Dos escalas de gravedad:** el escaneo clasifica con CVSS v3.0 (*Severity Base* en *Scan Details*), por eso Ghostcat figura como Critical (9,8). La ficha del plugin muestra *Risk Factor: High*, calculado con CVSS v2 (7,5).
- Datos de priorización de Ghostcat: EPSS 0,9927 (probabilidad de explotación muy alta) y presencia en el catálogo KEV de CISA (explotada en ataques reales).
- Limitación: Nessus Essentials no permite exportar reportes. La licencia mostraba vencimiento el 22/10/2026.
## 3. Qué puede afirmar un reporte limpio
 
Un reporte sin un hallazgo no distingue entre "no es vulnerable" y "no existe un plugin para eso". Dos razones por las que no alcanza para decir "seguro":
 
1. El feed va 30 días atrasado, así que los CVE recientes no tienen plugin (**ventana ciega**).
2. El escaneo fue **sin autenticar** (*Auth: Fail*): Nessus se comportó como un desconocido en la red y no pudo revisar lo que solo se ve desde adentro (paquetes y versiones instaladas, parches faltantes, permisos, configuración interna). Los 67 hallazgos son un mínimo, no el total.
Para cubrir la ventana ciega: Nmap entrega servicio y versión; con eso se busca en NVD por producto y versión, filtrando por **fecha de publicación** desde la fecha del feed. (Ejercicio pendiente de hacer.)
 
## 4. Cruce Nmap + Nessus sobre Metasploitable2 (192.168.100.101)
 
Comando: `nmap -sV -p- -oN metas_nmap.txt 192.168.100.101`. Resultado: **30 puertos abiertos**. Nmap dice qué hay; Nessus dice qué está mal.
 
| Puerto | Nmap (servicio y versión) | Nessus | ¿Nmap alcanza? |
|---|---|---|---|
| 5900 | vnc, protocolo 3.3 | VNC con contraseña trivial · Critical · 10,0 | No |
| 25 y 5432 | Postfix smtpd; PostgreSQL 8.3.0 – 8.3.7 | SSLv2/v3 · Critical · 9,8 | No |
| 8009 | ajp13, Apache Jserv (protocolo 1.3) | Ghostcat (CVE-2020-1938) · Critical · 9,8 | No (la versión de Tomcat sale del 8180, con otro método) |
| 1524 | bindshell, "Metasploitable root shell" | Bind Shell Backdoor · Critical · 9,8 | Solo da una pista por el banner |
 
En el 8180, `-sV` muestra "Coyote JSP engine 1.1", que es la versión del conector y no la de Tomcat. La versión se infiere con otro script.
 
## 5. Mini-reporte priorizado
 
Ordenar solo por CVSS no alcanza cuando todos son críticos. Criterios usados: gravedad, facilidad de explotación, condiciones que necesita el atacante, evidencia de explotación real y posibilidad de compromiso previo.
 
1. **Bind shell (1524/tcp):** puede significar que el equipo **ya está comprometido**; arreglar la puerta no sirve si alguien ya entró. Primero respuesta a incidentes.
2. **VNC (5900/tcp):** contraseña trivial, explotable por cualquiera sin esfuerzo, con control gráfico completo.
3. **Ghostcat (8009/tcp):** leer archivos internos no requiere ninguna condición salvo llegar al puerto, y tiene EPSS 0,99 y presencia en KEV. La ejecución de código sí depende de que la aplicación permita subir archivos.
4. **SSLv2/v3 (25 y 5432):** requiere que el atacante sea intermediario en la conexión, una condición más fuerte, y es un cambio de configuración servicio por servicio.
Nota: el puerto 8009 (AJP) es de uso **interno**; el problema es que está alcanzable desde la red, no que sea público por diseño.
 
## 6. Errores propios y aprendizajes
 
- Responder sobre lo que se asume y no sobre lo que se probó (por ejemplo, "salió limpio para todo lo documentado hasta 30 días antes"). Se corrigió con la fórmula: *esa prueba, en esas condiciones, no lo detectó*.
- Un dato de un solo dígito mal recordado (1555 en vez de 1524) y el parámetro de PostgreSQL `ssl_min_protocol_version`, que no existe en la versión 8.3: un dato específico sin verificar puede estar mal aunque esté bien escrito.
- Priorizar por un solo criterio (el número de gravedad) en vez de razonar con varios. Se corrigió aplicando el mismo criterio de "condición" a todos los hallazgos.
- Copiar explicaciones en vez de decirlas con palabras propias. Si no sale sin mirar, todavía no está entendido.
## Pendientes
 
- Hacer el ejercicio de buscar en NVD CVE publicados desde el 20/09 para los servicios detectados por Nmap.
- Ver qué muestra Nessus tras vencer la licencia y cómo se reactiva (*Settings → About → editar el código*); la documentación no dice cómo obtener un código nuevo en Essentials.
- Servicios vistos en Nmap que Nessus no nos hizo analizar (por ejemplo `vsftpd 2.3.4`, `UnrealIRCd`, `distccd`): material de la Semana 6.
- Sábado: bloque de fricción. Domingo: GitHub al día y criterios de avance.
 
