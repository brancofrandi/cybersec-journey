# Sesión 06 - Bloque 4: Procesos y Servicios

Fecha: 03/05/2026 

## PARTE A: ps aux (104 procesos)
$ ps aux | wc -l # 104 procesos total
$ ps aux | awk '{print $1}' | sort | uniq -c | sort -nr
90 root
6 branco

text
**Columnas**: USER|PID|%CPU|%MEM|VSZ|RSS|TTY|STAT|START|TIME|COMMAND  
**TTY**: `?`=daemon | `pts/0`=gráfico | `tty1`=consola  
**Sort**: `ps aux --sort=-%mem | head -5`

## PARTE B: systemctl (213 servicios)
$ systemctl list-units --type=service --state=running | wc -l
213

Stop/Start cron
$ sudo systemctl stop cron
$ systemctl status cron
Active: inactive (dead) since Sun 2026-05-03 14:48:26 UTC
Main PID: 731 (code=killed, signal=TERM)

$ sudo systemctl start cron
$ systemctl status cron
Active: active (running) since Sun 2026-05-03 14:51:24 UTC
Main PID: 3097 (cron)

text
**stop**=mata ahora | **disable**=no boot | **cron**=scheduler tareas

## PARTE C: netstat/ss - Puertos abiertos
$ sudo netstat -tulpn | head -5
Proto Local Address State PID/Program
tcp 0.0.0.0:53 LISTEN 567/systemd-resolve
tcp 0.0.0.0:45537 LISTEN 797/containerd
udp 0.0.0.0:68 - systemd/networkd

text
**Puertos**: 53(DNS), 68(DHCP), 45537(containerd efímero)  
**0.0.0.0**=todas interfaces (normal en VM local)

**Flags `-tulpn`**:
- `t`=TCP | `u`=UDP | `l`=LISTEN | `p`=PID/programa | `n`=numérico

**ss vs netstat**: ss=moderno/rápido(kernel) | netstat=viejo(/proc/net)

## COMANDOS NUEVOS 
systemctl list-units --type=service --state=running | wc -l
sudo systemctl [stop|start|status|disable] SERVICIO
sudo netstat -tulpn | head -10
sudo ss -tulpn | head -10

text

**Criterios cumplidos**: [x] Identifico procesos/servicios con ps/systemctl/netstat
