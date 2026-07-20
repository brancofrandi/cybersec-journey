## Sesión 03 — Nmap avanzado y búsqueda de CVEs
**Fecha:** 20/07/2026

---

## Escaneo de todos los puertos

Por defecto Nmap escanea los 1000 puertos más comunes.
Para escanear todos los puertos posibles:

```bash
nmap -p 1-65535 127.0.0.x
nmap -p 1-65535 -sV 127.0.0.x
nmap -p 1-65535 -A 127.0.0.x
```

El rango completo es del 1 al 65535.

---

## Combinación de flags

Los flags se pueden combinar en un solo comando:

```bash
nmap -sV -p 22,80 127.0.0.x       # puertos específicos con versión
nmap -A -p 1-65535 127.0.0.x      # escaneo completo en todos los puertos
nmap -sC -sV 127.0.0.x            # scripts + versiones
```

---

Cada servicio abierto es una superficie de ataque potencial.

---

## Flujo completo: Nmap → CVE

1. Escanear con Nmap para obtener versiones de servicios
2. Tomar el nombre y versión del servicio
3. Buscar en bases de datos de CVEs:
   - nvd.nist.gov
   - cvedetails.com
   - cve.mitre.org
4. Analizar los CVEs encontrados
5. Evaluar si aplican al sistema específico
6. Actuar: reforzar defensas o planificar ataque autorizado

---

## Puntajes de vulnerabilidades

**CVSS Base Score (0-10):**
Mide la severidad de la vulnerabilidad.
- 0-3.9: Bajo
- 4-6.9: Medio
- 7-8.9: Alto
- 9-10: Crítico

**EPSS Score:**
Mide la probabilidad de que la vulnerabilidad sea explotada
en los próximos 30 días. Se expresa en porcentaje.

---

## Una vulnerabilidad no aplica solo por la versión

Para que una vulnerabilidad aplique se necesita:
- La versión correcta del software
- El módulo o componente afectado instalado
- La configuración específica que la activa
- El sistema operativo compatible

Ejemplo: CVE-2010-1151 afecta a mod_auth_shadow de Apache.
Si ese módulo no está instalado, la vulnerabilidad no aplica
aunque la versión de Apache sea vulnerable.

---

## Puertos que hay que recordar

| Puerto | Servicio |
|--------|----------|
| 21 | FTP |
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |

---

## Comandos del día

```bash
nmap -p 1-65535 127.0.0.x          # todos los puertos
nmap -p 1-65535 -sV 127.0.0.x      # todos los puertos con versión
nmap -A 127.0.0.x                  # escaneo agresivo completo
sudo systemctl start apache2        # levantar Apache para práctica
sudo systemctl stop apache2 ssh     # apagar servicios al terminar
```









