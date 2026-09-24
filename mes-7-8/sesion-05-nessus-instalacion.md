Mes 7-8 (SIEM) — Sesión 05: Instalación de Nessus Essentials e incidente de disco

Fecha: 21/09/2026 Estado: Semana 3, día Lunes — objetivo no cerrado, continúa la próxima sesión antes de pasar a contenido de martes

Objetivo de la sesión

Arrancar Semana 3 (Nessus Essentials): registrar, instalar y activar Nessus Essentials en Kali, y lanzar el primer escaneo básico contra Metasploitable2. El escaneo en sí quedó pendiente — la sesión se fue en instalación, activación y un incidente real de infraestructura que hubo que resolver en el momento.

Concepto nuevo: Nessus vs Nmap

Nmap hace reconocimiento de superficie: qué puertos están abiertos y qué servicio corre en cada uno. Nessus va un paso más allá — toma esa información de servicios/versiones y la contrasta contra una base de plugins que conoce vulnerabilidades específicas (CVEs), devolviendo severidad y detalle por hallazgo.

Dentro de Nessus también hay una distinción a tener clara: un host discovery scan (lo que sugiere el asistente de bienvenida) solo detecta qué hosts están vivos en una red — no analiza vulnerabilidades. Un vulnerability scan real (plantilla "Basic Network Scan") sí lo hace. Si ya conocés el host objetivo, el discovery scan es innecesario.

Nessus Essentials vs Nessus Professional

Tenable ofrece varios productos bajo el mismo dominio. El formulario de registro correcto para uso personal/laboratorio es el de Nessus Essentials — no Nessus Professional, que es la versión paga pensada para uso comercial y aparece como una prueba gratuita por tiempo limitado en un formulario aparte. La forma de distinguirlos: el formulario correcto no menciona "Professional" en ningún lado y tiene la leyenda "Not intended for commercial use".

Dato actualizado sobre Nessus Essentials en 2026 (cambió respecto a versiones anteriores): licencia de 30 días renovable (no indefinida), límite de 5 IPs por escaneo (antes eran 16), estrictamente no comercial.

Instalación

El paquete se descarga directo desde la página de descargas de Tenable (sin necesidad de iniciar sesión — el login solo importa para el código de activación por mail, no para la descarga del instalador):

# Dentro de Kali, con salida a internet por NAT
sudo dpkg -i Nessus-10.12.4-ubuntu1604_amd64.deb

El build se llama "ubuntu1604" pero es el mismo paquete .deb válido para toda la familia Debian, Kali incluido.

sudo systemctl start nessusd.service
sudo systemctl enable nessusd.service

Activación: por navegador en https://localhost:8834 → "Continue" (no "Register Offline", porque Kali tiene salida a internet) → "Register for Nessus Essentials" (no "Professional" ni ninguna otra opción de la lista) → se carga el código de activación recibido por mail → se crea un usuario admin del panel → arranca la compilación del feed de plugins, que puede tardar bastante en la primera sincronización.

Incidente real: disco del host lleno durante la compilación de plugins

A mitad de la compilación de plugins, VirtualBox tiró un error: VERR_DISK_FULL sobre el medio ahci-0-0. El mensaje decía explícitamente que la operación se podía resumir una vez liberado espacio.

Diagnóstico: no era el disco virtual de Kali por dentro, era el disco físico del host (Windows) con 0 bytes libres sobre su capacidad total. La compilación de plugins de Nessus, sumada a los datos ya acumulados de varias semanas de laboratorio, agotó el espacio disponible.

Paso crítico — verificar antes de borrar: al revisar las carpetas más pesadas del disco apareció una carpeta con el mismo nombre que la VM de Kali en una ubicación distinta a la que la configuración general de la VM mostraba como carpeta de instantáneas. La hipótesis inicial fue que se trataba de una copia vieja y huérfana, segura de borrar. Antes de hacerlo, se verificó la pestaña Almacenamiento de la configuración de la VM (no la pestaña General, que solo muestra la carpeta de instantáneas) — y esa verificación reveló que esa carpeta "sospechosa" era en realidad la ubicación real del disco duro virtual activo de Kali, con todo el trabajo de las últimas semanas. Borrar esa carpeta hubiera significado perder la VM completa.

Conclusión operativa: cuando dos configuraciones distintas de una misma VM apuntan a rutas diferentes, hay que verificar cuál es la que realmente importa (el disco adjunto al controlador, no la carpeta de instantáneas) antes de tomar una acción destructiva — el mismo principio que aplica a cualquier operación irreversible sobre infraestructura real.

Resolución real: se liberaron ~7 GB borrando dos archivos .iso de instalación ya utilizados (uno de un sistema operativo sin relación con el laboratorio, y el instalador de la VM del agente Ubuntu, ya innecesario una vez que la VM está instalada). Eso fue suficiente para que la compilación de plugins continuara sin volver a trabarse. Sin pérdida de datos.

A mediano plazo: se evaluó la posibilidad de ampliar RAM del host — descartada, la memoria del equipo (ASUS Vivobook Go, Intel i3-N305) viene soldada a la placa (LPDDR5 onboard), confirmado contra la ficha técnica oficial, techo fijo de 8 GB. Queda pendiente conseguir un disco externo SSD para alojar las VMs y despresurizar el disco interno de forma definitiva.

Pendientes para la próxima sesión
Lanzar el primer escaneo básico ("Basic Network Scan") contra Metasploitable2 (192.168.100.101) — el objetivo técnico del día lunes no se llegó a cumplir
Recién después de eso arranca el contenido de martes (tipos de escaneo, política propia, análisis de resultados)
