## Sesion 01 - Linux: sistema de archivos y usuarios
**Fecha:** [03/04/2026]

---

### Estructura de carpetas en la raiz de Linux

| Carpeta | Para que sirve |
|---------|----------------|
| /etc    | Archivos de configuracion del sistema y aplicaciones |
| /home   | Directorios personales de los usuarios |
| /bin    | Comandos esenciales del sistema (ls, cat, grep) |
| /var    | Datos variables, logs en /var/log |
| /tmp    | Archivos temporales |
| /root   | Carpeta home del usuario root |
| /usr    | Programas y utilidades instaladas |
| /proc   | Informacion de procesos en tiempo real |

---

### /etc/passwd — archivo de usuarios

Contiene la lista de todos los usuarios del sistema.
No contiene contrasenas, solo informacion de cada usuario.

**Los 7 campos de cada linea:**
| Campo | Significado |
|-------|-------------|
| 1 | Nombre de usuario |
| 2 | x — puntero, contrasena esta en /etc/shadow |
| 3 | UID — numero unico que identifica al usuario |
| 4 | GID — numero del grupo principal |
| 5 | Descripcion o nombre completo |
| 6 | Carpeta home del usuario |
| 7 | Shell que usa al iniciar sesion |

**Campo 7 — relevancia en seguridad:**

- /bin/bash → puede iniciar sesion interactiva
- /bin/false o /usr/sbin/nologin → no puede iniciar sesion

---

### /etc/passwd vs /etc/shadow

- /etc/passwd: lista de usuarios con sus datos. Legible por todos.
- /etc/shadow: contrasenas cifradas. Solo root puede leerlo.
- La x en el campo 2 de passwd es un puntero que le dice
  al sistema que busque la contrasena en shadow.

---

### Comandos practicados

cat /etc/passwd | head -5
grep "/bin/bash" /etc/passwd
grep "/bin/false|nologin" /etc/passwd | wc -l
grep "localhost" /etc/hosts
wc -l /etc/hosts

**Lo que encontre en mi sistema:**
- 2 usuarios con /bin/bash: root y branco
- 31 usuarios que no pueden iniciar sesion (servicios del sistema)
- /etc/hosts tiene 9 lineas
- localhost aparece en IPv4 (127.0.0.1) y IPv6 (::1)

---

### grep — repaso

| Comando | Que hace |
|---------|----------|
| grep "texto" archivo | Busca lineas que contienen texto |
| grep "a\|b" archivo | Busca lineas que contienen a o b |
| grep -i "texto" archivo | Busca sin distinguir mayusculas |
| grep "texto" archivo \| wc -l | Cuenta cuantas lineas contienen texto |
| wc -l archivo | Cuenta las lineas del archivo |

---

### Pendiente para repasar proxima sesion

- Carpetas principales de la raiz de Linux de memoria
- Los 7 campos de /etc/passwd de memoria
- ::1 es el loopback de IPv6, equivalente a 127.0.0.1 en IPv4
