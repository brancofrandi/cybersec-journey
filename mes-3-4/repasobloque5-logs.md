## Sesion — Repaso Bloque 5: Logs
**Fecha:** [31/05/26]

---

### Preguntas de arranque resueltas

1. /var/log/auth.log registra inicios de sesion,
   autorizaciones y comandos con permisos privilegiados.

2. tail -20 muestra las ultimas 20 lineas.
   tail -f muestra las lineas en tiempo real,
   se actualizan mientras el archivo crece.

3. 'Failed' en auth.log indica inicios de sesion fallidos,
   posible intento de penetracion por fuerza bruta.

---

### Linea de sudo en auth.log

2026-05-25 branco sudo: branco : TTY=tty1 ;
PWD=/home/branco ; USER=root ; COMMAND=/usr/bin/grep

| Campo | Significado |
|-------|-------------|
| Fecha y hora | Cuando ocurrio exactamente |
| branco sudo | Usuario que uso sudo |
| TTY=tty1 | Terminal desde la que ejecuto |
| PWD=/home/branco | Carpeta donde estaba parado |
| USER=root | Como que usuario ejecuto |
| COMMAND= | Comando exacto ejecutado |

---

### Otros registros en auth.log

CRON: tareas automaticas del sistema ejecutadas
como root en intervalos regulares. No las ejecuta
el usuario, las ejecuta el sistema solo.

Failed: servicios que no pudieron iniciarse.
fwupd y UPower fallaron. No son criticos.

pam_lastlog.so: modulo de autenticacion que no
pudo cargarse. Es advertencia, no acceso no autorizado.

---

### Por que auth.log es critico en ciberseguridad

Permite reconstruir exactamente que hizo cada usuario,
cuando, desde donde y con que permisos.
Si hubo acceso no autorizado, la evidencia esta ahi.

Comandos utiles sobre logs:
- tail -20 /var/log/auth.log
- tail -f /var/log/auth.log
- grep 'Failed' /var/log/auth.log
- grep 'sudo' /var/log/auth.log | tail -5
- grep -i 'error' /var/log/syslog | wc -l
