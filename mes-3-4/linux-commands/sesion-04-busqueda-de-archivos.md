## Sesion 04 - Búsqueda de archivos
**Fecha:** [14/03/26]

---

### Comandos aprendidos

| Comando | Qué hace |
|---------|----------|
|  find   |  busca archivos y muestra su ruta |
|  grep   | busca caracteres o palabrasas específicas en archivos |

---
# find — búsqueda en el sistema

### Sintaxis:

find [ruta] [expresión]

### Ejemplos:

find / -name "passwords.txt"

find /etc -name "*.conf"

find /home -name "*.sh"

find / -type d -name "secret"

find / -type f -size +10M

### Flags importantes:

| Flag | Uso |
|------|-----|
|-name | nombre |
|-type f|	archivos|
|-type d|	directorios|
|-size|	tamaño|
|-mtime|	fecha|
|-user|	propietario|
|-perm|	permisos |

---

# grep — búsqueda de contenido

## Sintaxis:

grep [opciones] "patrón" archivo

## Ejemplos:

- grep "root" /etc/passwd
- grep -i "error" /var/log/syslog
- grep -r "root" /etc/
- grep -n "error" sistema.log

---

# Parte A — find

- find /etc -name "passwd" (Busca archivos llamados exactamente passwd) 

- find /var/log -name "*.log" (Busca archivos .log)

- Los .log registran eventos del sistema (errores, accesos, servicios).

- find /etc -name "*.conf" | wc -l (Cuenta archivos .conf)

- find / -name "hostname" 2>/dev/null 

|detalle| qué hace|
|-------|---------|
| 2 | stderr|
|> | redirección|
|/dev/null | descarta datos|

Resultado: oculta errores de permisos y muestra solo resultados válidos.

---

# Parte B — grep

grep "root" /etc/passwd (Muestra líneas que contienen root)

grep -i "error" /var/log/syslog (Busca errores sin distinguir mayúsculas/minúsculas)

grep -E "bin/false|nologin" /etc/passwd (Usuarios sin login)

grep -E "bin/false|nologin" /etc/passwd | wc -l (Cantidad de usuarios sin login)

---

# Parte C — Combinaciones

## Pipe | (Redirige stdout → stdin)

find /etc -name "*.conf" | wc -l (Cantidad de archivos .conf)

grep "branco" /etc/passwd | wc -l (Cantidad de coincidencias)

cat /etc/passwd | grep "bin/bash" | wc -l (Usuarios con shell válida)

find /var/log -name "*.log" | wc -l (Cantidad de logs)

---

### Conceptos clave

find devuelve rutas

grep devuelve líneas

"*" = wildcard (cualquier cadena)

2>/dev/null elimina ruido de errores

---

### Observaciones

### El contexto importa:

- grep "root" /etc/passwd → usuario
- grep "root" /var/log/auth.log → intentos de login

---

### Nota sobre sudo

sudo eleva privilegios
No siempre cambia resultados, solo permite acceder a más rutas

---

### Ejemplo:

Sin sudo: errores de permisos
Con sudo: acceso completo
Si no hay coincidencias nuevas → el resultado no cambia

---

### Conclusión
find = ubicación
grep = contenido
combinados = herramienta real de análisis
