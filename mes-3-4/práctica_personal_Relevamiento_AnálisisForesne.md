## Ejercicio — Situación real: Relevamiento y Forense
**Fecha:** [12/07/2026]
**Contexto:** Cierre Mes 3-4 — Ejercicio integrador

---

## Escenario 1 — Relevamiento

Llegué a un sistema Linux desconocido. Se documentan
los pasos y hallazgos del relevamiento completo.

### Comandos utilizados y resultados

**Usuarios del sistema:**
cat /etc/passwd | wc -l
Resultado: 57 usuarios en el sistema

grep "/bin/bash" /etc/passwd | wc -l
Resultado: 1 usuario puede iniciar sesión (kali)

**Procesos que más memoria consumen:**
ps aux --sort=-%mem | head -10

Proceso que más consume: xfwm4 (gestor de ventanas Kali)
Usuario con más procesos: kali (entorno gráfico)

**Puertos escuchando:**
sudo netstat -tulpn
Resultado: ningún puerto escuchando en este momento

**Servicios corriendo:**
systemctl list-units --type=service --state=running
Resultado: 19 servicios activos (todos esperables para Kali)
Servicios identificados: cron, NetworkManager, lightdm,
systemd-journald, virtualbox-guest-utils, entre otros

**Archivos de log:**
find /var/log -type f 2>/dev/null | wc -l
Resultado: 40 archivos de log en /var/log

### Conclusión del relevamiento

El equipo no presenta anomalías. Ningún puerto expuesto,
servicios esperables para el sistema operativo,
un único usuario con acceso interactivo.

---

## Escenario 2 — Análisis forense

Se reportó un acceso al servidor el viernes 10 de julio
fuera del horario habitual. Se investigaron los logs
del período indicado.

### Comandos utilizados

**Comandos con privilegios de root el viernes:**
journalctl --since "2026-07-10" --until "2026-07-11" --grep="sudo"
Resultado: No entries — ningún comando con sudo ese día

**Intentos de login fallidos el viernes:**
journalctl --since "2026-07-10" --until "2026-07-11" --grep="Failed"
Resultado: solo errores normales de VirtualBox y servicios
del sistema. Ningún "Failed password" ni
"authentication failure".

**Arranques del sistema ese día:**
Se registraron 3 boots el viernes 10. El sistema fue
iniciado y apagado varias veces pero sin actividad
de usuario privilegiado.

### Conclusión del análisis forense

No se encontraron evidencias de acceso no autorizado
en los logs disponibles. Los registros de "Failed"
corresponden a errores normales de VirtualBox.

**Limitaciones del análisis:**
- ps aux no permite ver procesos del pasado
- Sin auditd configurado no hay registro histórico
  de procesos ni comandos
- No hay logs de conexiones de red entrantes

Se recomienda configurar auditd para registro continuo
y verificar si el servidor tiene SSH habilitado.

---

## Comandos nuevos aprendidos en este ejercicio

| Comando | Qué hace |
|---------|----------|
| journalctl --since "fecha" | Filtra logs desde una fecha |
| journalctl --until "fecha" | Filtra logs hasta una fecha |
| journalctl --grep="texto" | Busca texto en todos los logs |
| journalctl _COMM=sudo | Muestra comandos ejecutados con sudo |

---

## Conceptos clave

**journalctl vs grep:**
journalctl --grep busca en todos los logs del sistema.
No es exclusivo de logins fallidos, busca cualquier
línea que contenga el texto indicado.
Hay que leer el contexto de cada resultado.

**-- Boot -- en los logs:**
Indica que el sistema fue reiniciado en ese punto.
Permite reconstruir la línea de tiempo del sistema.

**ps aux y el pasado:**
ps aux muestra solo el momento actual.
Sin auditd configurado previamente, los procesos
del pasado no se pueden recuperar.

**Failed en los logs:**
No todo "Failed" es una intrusión.
Hay que leer qué servicio falló y por qué.
En seguridad buscás específicamente:
- Failed password
- authentication failure
- Invalid user
