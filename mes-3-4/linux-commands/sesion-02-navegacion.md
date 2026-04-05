## Sesion 02 - Navegacion del sistema de archivos
**Fecha:** [05/04/2026]

---

### Estructura de carpetas en la raiz de Linux

| Carpeta | Para que sirve |
|---------|----------------|
| /       | Raiz del sistema, jerarquia mas alta |
| /etc    | Archivos de configuracion del sistema y aplicaciones |
| /home   | Directorios personales de los usuarios |
| /root   | Carpeta home del usuario root |
| /bin    | Comandos esenciales del sistema (ls, cat, grep) |
| /var    | Datos variables. Logs en /var/log |
| /tmp    | Archivos temporales |
| /usr    | Utilidades y aplicaciones instaladas |
| /proc   | Procesos en tiempo real, no es directorio real |

### Caracteres especiales en navegacion

- .  representa el directorio actual
- .. representa el directorio padre, un nivel arriba
- cd .. sube un nivel desde donde estas parado

### Ruta absoluta vs relativa

- Absoluta: empieza desde /, siempre apunta al mismo lugar
- Relativa: depende de donde estas parado al ejecutar el comando

---

### ls -la — interpretacion de columnas

-rw-rw-r-- 1 branco branco 4 abr 3 19:06 welcome

| Campo | Significado |
|-------|-------------|
| - | Tipo: - archivo, d directorio, l enlace |
| rw-rw-r-- | Permisos: propietario, grupo, otros |
| 1 | Cantidad de enlaces |
| branco branco | Propietario y grupo |
| 4 | Tamanio en bytes |
| abr 3 19:06 | Fecha de ultima modificacion |
| welcome | Nombre del archivo |

### Permisos

- r (read)    → puede leer
- w (write)   → puede escribir
- x (execute) → puede ejecutar
- - (dash)    → ese permiso no esta otorgado

Tres grupos de permisos: propietario | grupo | otros

---

### Logs del sistema

**Ubicacion:** /var/log

| Archivo | Contenido |
|---------|-----------|
| syslog | Registro general del sistema |
| auth.log | Intentos de login, sudo, accesos SSH |
| kern.log | Eventos del kernel |
| dpkg.log | Paquetes instalados o desinstalados |

**auth.log es el primero que se revisa ante una intrusion.**
Registra quien ejecuto que comando, cuando, desde donde
y con que permisos.

Ejemplo de linea relevante:

branco : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/sbin/shutdown now

Muestra: usuario, terminal, carpeta, usuario destino y comando exacto.

---

### Comandos practicados

| Comando | Que hace |
|---------|----------|
| pwd | Muestra la ruta actual |
| ls | Lista archivos del directorio actual |
| ls -la | Lista con detalle y archivos ocultos |
| cd /ruta | Entra a una carpeta por ruta absoluta |
| cd .. | Sube un nivel |
| cat archivo | Muestra el contenido completo |
| head -5 archivo | Muestra las primeras 5 lineas |
| tail -5 archivo | Muestra las ultimas 5 lineas |
| grep "texto" archivo | Busca lineas que contienen texto |
| wc -l archivo | Cuenta las lineas del archivo |

### Ejercicios resueltos en sesion

- wc -l /etc/passwd → 34 usuarios en el sistema
- grep "/bin/bash" /etc/passwd → 2 usuarios: root y branco
- grep "/bin/false\|nologin" /etc/passwd | wc -l → 31 sin login
- grep "root" /etc/passwd → 1 linea, usuario root
- tail -5 /etc/passwd → ultimos 5: landscape, fwupd, usbmux, branco, dnsmasq
- head -5 /etc/passwd → primeros 5: root, daemon, bin, sys, sync

---

### root — superusuario

Root tiene acceso total al sistema sin restricciones.
sudo permite ejecutar comandos como root temporalmente.
Escalar privilegios hasta root es el objetivo de un atacante
una vez que accede a un sistema.

---

### Pendiente para proxima sesion

Bloque 2: permisos con chmod.
