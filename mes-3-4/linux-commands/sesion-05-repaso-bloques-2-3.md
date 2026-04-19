## Sesión 05 — Repaso Bloques 2 y 3
**Fecha:** 17/04/2025

---

### Bloque 2 — Búsqueda: conceptos consolidados

**find vs grep**

- find: busca archivos e imprime su ruta. Busca el contenedor.
- grep: busca texto dentro de archivos. Busca el contenido.

Ejemplo:
- find /var/log -name '*.conf' → busca archivos .conf en /var/log
- grep 'error' /var/log/syslog → busca la palabra error dentro de syslog

---

**2>/dev/null**

Redirige los errores (stderr) al dispositivo nulo del sistema.
Se usa cuando queremos limpiar la salida y que no aparezcan
los mensajes de "Permiso denegado" mezclados con los resultados.

Ejemplo: find / -name 'hostname' 2>/dev/null

---

**El asterisco * en búsquedas**

Representa cualquier cadena de caracteres.
'*.conf' significa cualquier archivo que termine en .conf.

---

**El operador \\| en grep**

Funciona como un "o". Busca una palabra o la otra.
Ejemplo: grep '/bin/false\\|nologin' /etc/passwd

---

**Diferencia de contexto en grep**

Buscar 'root' en /etc/passwd → muestra que el usuario existe.
Buscar 'root' en /var/log/auth.log → muestra las acciones que
realizó root, quién intentó entrar como administrador y cuándo.

El mismo patrón cobra significado diferente según dónde se busca.

---

### Bloque 3 — Permisos: conceptos consolidados

**Comandos practicados**

| Comando | Qué hace |
|---------|----------|
| touch ruta/archivo | Crea un archivo en la ruta especificada |
| ls -la | Muestra permisos, links, dueño, grupo, tamaño, fecha y nombre |
| chmod número ruta/archivo | Cambia permisos con notación numérica |
| chmod simbolo ruta/archivo | Cambia permisos con notación simbólica |
| cmd1 && cmd2 | Ejecuta cmd2 solo si cmd1 se ejecutó correctamente |

---

**Notación numérica**

| Número | Permisos | Ejemplo |
|--------|----------|---------|
| 7 | rwx | usuario con todo |
| 6 | rw- | leer y escribir |
| 5 | r-x | leer y ejecutar |
| 4 | r-- | solo leer |
| 0 | --- | sin permisos |

750 → usuario rwx, grupo r-x, otros sin permisos.
664 → usuario rw-, grupo rw-, otros r--.

---

**Notación simbólica**

- u-x → quita permiso de ejecución al usuario
- o+w → agrega permiso de escritura a otros

**Por qué o+w es peligroso:**
Permite que cualquier usuario o proceso del sistema modifique,
borre o reemplace el contenido del archivo sin restricción.
Facilita inyección de código malicioso y escalada de privilegios.

---

**Archivos críticos del sistema**

| Archivo | Por qué es crítico |
|---------|--------------------|
| /etc/passwd | Controla identidad de usuarios del sistema |
| /etc/shadow | Almacena contraseñas cifradas |
| /etc/hosts | Controla resolución de nombres de red |

Su compromiso puede resultar en robo de contraseñas,
acceso no autorizado como root o desvío de tráfico a sitios maliciosos.

---

### Dudas pendientes

-
