# Sesión 09 — Bloque 5: Análisis de logs
**Fecha:** [25/04/26]
 
---
 
## ¿Qué son los logs?
 
Registros cronológicos de eventos del sistema y aplicaciones.
Son la primera fuente de información ante un incidente de seguridad.
 
---
 
## Archivos de log más importantes
 
| Archivo | Qué registra | Por qué importa en seguridad |
|---------|-------------|------------------------------|
| /var/log/syslog | Actividad general del sistema | Eventos de servicios, kernel, red |
| /var/log/auth.log | Autenticación y accesos | Intentos de login, sudo, SSH |
| /var/log/kern.log | Mensajes del kernel | Errores de hardware |
| /var/log/boot.log | Arranque del sistema | Diagnóstico de inicio |
 
**Regla:** ante una intrusión sospechada, el primero en revisar es `/var/log/auth.log`.
 
---
 
## Comandos para leer logs
 
| Comando | Qué hace |
|---------|----------|
| `ls /var/log` | Lista todos los archivos de log |
| `cat /var/log/syslog \| wc -l` | Cuenta cuántas líneas tiene el log |
| `tail -20 /var/log/syslog` | Muestra las últimas 20 líneas |
| `tail -f /var/log/syslog` | Muestra el log en tiempo real (Ctrl+C para salir) |
 
**tail -f** es la herramienta de monitoreo en vivo. Se usa cuando sospechás que algo está pasando ahora mismo.
 
---
 
## Filtrar logs con grep
 
```bash
grep 'error' /var/log/syslog | wc -l
grep -i 'error' /var/log/syslog | wc -l
grep 'branco' /var/log/auth.log
grep 'sudo' /var/log/auth.log
grep 'Failed' /var/log/auth.log
grep 'Failed' /var/log/auth.log | wc -l
```
 
**Diferencia entre grep y grep -i:**

grep distingue mayúsculas. 

grep -i no distingue.

'error' no encuentra 'ERROR'. 

'-i error' encuentra ambos.
 
**Qué buscar en auth.log:**
 
- `branco` → eventos de mi usuario (logins, sudo)
- `sudo` → comandos ejecutados con privilegios de root
- `Failed` → intentos de login fallidos, posibles ataques de fuerza bruta
- `session opened` → sesiones iniciadas con éxito
---
 
## Combinaciones
 
```bash
grep 'branco' /var/log/auth.log | tail -10
cat /var/log/auth.log | grep 'session opened' | wc -l
tail -50 /var/log/syslog | grep -i 'error'
ls /var/log/*.log | wc -l
```
 
**Lógica del pipe en logs:**
- Primero filtrás con grep, después acotás con tail
- O primero acotás con tail, después filtrás con grep
- El orden depende de lo que necesitás ver
---
 
## Resultados en mi sistema
 
- syslog tiene 3043 líneas
- 9 archivos .log en /var/log
- grep 'Failed' en auth.log: sin intentos fallidos registrados
---
 
## Relevancia en ciberseguridad
 
Los logs son evidencia. En un análisis forense o en un pentest,
leer logs correctamente permite reconstruir qué pasó,
cuándo pasó y desde dónde.
 
auth.log específicamente revela:
- Intentos de fuerza bruta (muchos 'Failed' seguidos)
- Escalada de privilegios (uso de sudo inusual)
- Accesos en horarios sospechosos
---
 
## Pendiente para bloque de fricción
 
- Entender las líneas individuales que aparecen en syslog
- Practicar lectura de auth.log identificando patrones
