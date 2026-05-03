# Sesión 06 — Procesos y Servicios
**Fecha:** 03/05/2026

---

### Bloque 4 — Procesos: conceptos consolidados

**ps aux — estructura de salida**

| Columna | Significado |
|---------|-------------|
| USER | Usuario que ejecuta el proceso |
| PID | Identificador único del proceso |
| %CPU | Porcentaje CPU consumida |
| %MEM | Porcentaje memoria consumida |
| VSZ | Memoria virtual (kB) |
| RSS | Memoria residente (kB) |
| TTY | Terminal asociada |
| STAT | Estado del proceso |
| START | Fecha/hora inicio |
| TIME | Tiempo CPU acumulado |
| COMMAND | Comando ejecutado |

**TTY descifrados:**
- `?` → daemon (sin terminal)
- `pts/0` → pseudo-terminal gráfica 
- `tty1` → consola virtual nativa

**Ordenamiento:**
- `--sort=-%mem` → mayor a menor memoria
- `--sort=-%cpu` → mayor a menor CPU

**Mi sistema:** 104 procesos total, 90 root, 6 branco

---

### Bloque 4 — Servicios: conceptos consolidados

**systemctl — comandos base**
list-units --type=service --state=running | wc -l → 213 servicios
status SERVICIO → estado actual
stop SERVICIO → detiene inmediatamente
start SERVICIO → inicia inmediatamente
disable SERVICIO → no arranca al boot

text

**Cron demo:**
Active: inactive (dead) → parado
Main PID: 731 (killed, signal=TERM) → proceso terminado
Active: active (running) → ejecutándose
Main PID: 3097 (cron) → proceso vivo

text

**Proceso vs Servicio:**
- Proceso → temporal, control manual (ps aux)
- Servicio → daemon persistente, systemd (systemctl)

---

### Bloque 4 — Puertos abiertos: netstat/ss

**Puertos detectados en mi sistema:**
netstat -tulpn | head -5
tcp 0.0.0.0:53 LISTEN 567/systemd-resolve (DNS)
tcp 0.0.0.0:45537 LISTEN 797/containerd
udp 0.0.0.0:68 systemd-networkd (DHCP)

text

**`-tulpn` descifrado:**
| Flag | Significado |
|------|-------------|
| t | TCP solamente |
| u | UDP solamente |
| l | LISTEN (escuchando) |
| p | PID/programa (requiere sudo) |
| n | Numérico (sin DNS lookup) |

**0.0.0.0** → todas las interfaces (normal en VM local)

**netstat vs ss:**
- netstat → obsoleto, lee /proc/net
- ss → moderno, acceso directo kernel (más rápido)

---

### Comandos nuevos dominados
ps aux --sort=-%mem | head -5
systemctl list-units --type=service --state=running | wc -l
sudo systemctl stop cron && systemctl status cron
sudo netstat -tulpn | head -10
sudo ss -tulpn | head -10

text

---

### Checklist Bloque 4 

- [x] ps aux → interpreto columnas y ordeno por memoria/CPU
- [x] systemctl → listo, controlo y verifico servicios
- [x] netstat/ss → identifico puertos abiertos y servicios expuestos

**Próximo:** Repaso Bloque 4 (Jueves Semana 2)
