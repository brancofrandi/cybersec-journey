## Sesión 01 — Introducción a Nmap
**Fecha:** 13/07/2026

---

## ¿Qué es Nmap?

Herramienta de escaneo y mapeo de redes. Se usa para
identificar puertos abiertos, servicios y versiones
en un sistema. Usarla en redes ajenas sin autorización
es ilegal. En redes propias y laboratorios es legítima.

**Diferencia con netstat:**
- netstat → muestra puertos desde adentro del sistema
- Nmap → escanea desde afuera, como lo haría un atacante

---

## Estados de un puerto en Nmap

| Estado | Qué significa |
|--------|--------------|
| open | Hay un servicio escuchando y acepta conexiones |
| closed | Accesible pero sin servicio escuchando. Responde activamente |
| filtered | Firewall bloqueando. No hay respuesta. Nmap no puede determinar el estado |

---

## Comandos ejecutados hoy

```bash
nmap 192.x.x.x        # escaneo básico del router
nmap 127.x.x.x          # escaneo desde adentro de la VM
nmap 10.x.x.x          # escaneo desde afuera de la VM
nmap -sV 127.x.x.x      # detección de versiones de servicios
ip addr                 # ver interfaces de red y sus IPs
```

**Diferencia entre 127.x.x.x y 10.x.x.x:**
- 127.x.x.x: loopback, escaneo interno, muestra closed
- 10.x.x.x: IP de la VM en VirtualBox, escaneo externo,
  muestra filtered por el NAT de VirtualBox

---

## Flag -sV

Detecta qué servicio y qué versión corre en cada puerto abierto.

Ejemplo de resultado:
```bash
22/tcp open ssh OpenSSH 10.2p1 Debian 5 (protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

**CPE (Common Platform Enumeration):**
Identificador estándar de software y sistemas operativos.
Se usa para buscar vulnerabilidades en bases de datos como:
- nvd.nist.gov
- cvedetails.com
- cve.mitre.org

Si sabés la versión exacta de un servicio podés buscar
CVEs específicos para esa versión.

---

## Observaciones del día

- El router (192.168.0.x) tiene todos los puertos filtrados.
  El firewall del router funciona correctamente.
- La VM sin servicios activos no tiene puertos abiertos.
- Al levantar SSH aparece el puerto 22 en el escaneo interno
  pero no en el externo porque VirtualBox lo filtra.
- Un espacio entre - y sV rompe el comando: usar -sV sin espacio.

---

## Pendiente para mañana

Flags principales: -sC, -A y -p
