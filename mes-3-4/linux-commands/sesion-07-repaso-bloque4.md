## Sesión 07 — Repaso Bloque 4: Procesos y servicios
**Fecha:** 10/05/2026

---

### Conceptos consolidados

**Proceso vs servicio**

- Proceso: cualquier programa en ejecución, puede ser
  interactivo o en segundo plano.
- Servicio (daemon): proceso diseñado para correr en segundo
  plano de forma continua, gestionado por el sistema.
  Inicia con el SO y finaliza cuando se apaga.

---

### ps aux — columnas principales

| Columna | Qué muestra |
|---------|-------------|
| USER | Usuario que ejecuta el proceso |
| PID | ID único del proceso |
| %CPU | Porcentaje de CPU usado |
| %MEM | Porcentaje de memoria usada |
| TTY | Terminal asociada (? = sin terminal) |
| STAT | Estado del proceso |
| TIME | Tiempo de CPU acumulado |
| COMMAND | Comando que lo ejecuta |

**STAT: Ssl**
- S → sleeping, esperando un evento
- s → líder de sesión
- l → multihilo

**TTY: ?**
No tiene ninguna terminal asociada.
Es un daemon que corre independiente de cualquier terminal.

**Comandos practicados:**

ps aux | wc -l

ps aux | grep 'root' | wc -l

ps aux --sort=-%mem | head -5

ps aux --sort=-%cpu | head -5

**Resultados del sistema:**
- 109 procesos corriendo
- 90 pertenecen a root
- Procesos que más memoria usan: containerd y dockerd
- Docker: plataforma de contenedores, más liviana que una VM
  porque comparte el kernel del SO

---

### systemctl — gestión de servicios

| Comando | Qué hace |
|---------|----------|
| systemctl list-units --type=service --state=running | Lista servicios activos |
| systemctl status servicio | Ver estado de un servicio |
| sudo systemctl stop servicio | Detiene el servicio ahora |
| sudo systemctl start servicio | Inicia el servicio ahora |
| sudo systemctl disable servicio | No arranca al iniciar el SO |
| sudo systemctl enable servicio | Vuelve a habilitarlo al inicio |

**Diferencia clave:**
- stop → detiene en el momento, vuelve a arrancar al reiniciar
- disable → no arranca automáticamente al iniciar el sistema

**Nota:** SSH no está instalado por defecto en Ubuntu Desktop,
solo en Ubuntu Server.

---

### netstat y ss

sudo netstat -tulpn

sudo ss -tulpn

Ningún puerto en 0.0.0.0 → sistema no expuesto innecesariamente.

**Por qué 0.0.0.0 es peligroso:**
Acepta conexiones desde cualquier interfaz de red.
El servicio queda expuesto a cualquier red a la que
esté conectado el equipo.

---

### Relevancia en ciberseguridad

Al entrar a un sistema, ps aux y netstat permiten ver
qué está corriendo y qué está expuesto.
Identificás procesos que no deberían estar ahí,
servicios mal configurados y puertos innecesariamente abiertos.
Es uno de los primeros pasos en un pentest o análisis forense.

---

### Dudas pendientes

- Memorizar columnas de ps aux y valores de STAT
