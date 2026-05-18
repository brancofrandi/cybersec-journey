## Sesión 08 — Práctica libre: cierre Bloques 2, 3 y 4
**Fecha:** [17/05/26]

---

### Resultado general

Bloques 2, 3 y 4 practicados sin tutorial ni notas.
Todo completado en orden. Dos conceptos requieren
refuerzo en el bloque de fricción del sábado.

---

### Parte A — Búsqueda: find y grep

**Comandos ejecutados:**

find /etc -name '.conf' | wc -l

find /var/log -name '.log'

find / -name 'shadow' 2>/dev/null

grep 'branco' /etc/passwd

grep -i 'error' /var/log/syslog | wc -l

grep '/bin/bash|nologin' /etc/passwd | wc -l

**Conceptos confirmados:**
- 2>/dev/null: envía los errores al dispositivo nulo,
  limpia la salida de find
- grep -i: busca sin distinguir mayúsculas ni minúsculas
- '*.conf': cualquier archivo que termine en .conf
- 'passwd': busca un archivo con ese nombre exacto

---

### Parte B — Permisos: chmod

**Comandos ejecutados y resultados:**

| Comando | Resultado |
|---------|-----------|
| chmod 644 test1.txt | -rw-r--r-- |
| chmod 755 test2.txt | -rwxr-xr-x |
| chmod u-x test2.txt | -rw-r-xr-x |
| chmod o+w test1.txt | -rw-r--rw- |

**Preguntas de cierre:**
- /etc/shadow es más restrictivo porque contiene
  las contraseñas cifradas de los usuarios
- chmod o+w es peligroso porque permite a cualquiera
  escribir en el archivo sin restricción
- chmod 750: usuario rwx, grupo r-x, otros sin permisos

---

### Parte C — Procesos y servicios

**Comandos ejecutados:**

ps aux | wc -l

ps aux --sort=-%mem | head -5

ps aux --sort=-%cpu | head -5

ps aux | grep 'docker'

systemctl list-units --type=service --state=running | wc -l

systemctl status cron

sudo netstat -tulpn

**Preguntas de cierre:**

- Columnas más útiles en seguridad: USER, COMMAND,
  STAT, PID (necesité Google para esta)
- STAT Ssl: S=sleeping, s=líder de sesión, l=multihilo
  (tuve que repasar)
- Proceso desconocido: primero identificar qué función
  cumple y de dónde viene, nunca detener sin saber eso

---

### Pendiente para bloque de fricción del sábado

- TTY: ? — significado y cuándo aparece (tuve que verlo)
- STAT: Ssl — los tres caracteres y qué significan (tuve que verlo)
- Columnas de ps aux más útiles en seguridad (usé Google)

cat /etc/passwd | grep '/bin/bash'
