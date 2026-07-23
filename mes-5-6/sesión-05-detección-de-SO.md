## Sesión 05 — Detección de SO y búsqueda de CVEs
**Fecha:** 23/07/2026

---

## Práctica de búsqueda de CVEs

### Flujo completo

1. Escanear con nmap -sV para obtener versiones
2. Ir a nvd.nist.gov
3. Buscar el nombre del servicio sin versión: `openssh`
4. Analizar los CVEs encontrados
5. Verificar si la versión detectada está afectada
6. Acción: actualizar si es vulnerable

### Ejemplo practicado

Servicio detectado: OpenSSH 10.2p1
CVE encontrado: CVE-2026-60002
CVSS: 9.4 CRITICAL
Descripción: uso de memoria liberada cuando el servidor
cambia su clave de host durante intercambio de claves.
Afecta: versiones anteriores a OpenSSH 10.4
¿Vulnerable?: Sí, 10.2 es anterior a 10.4
Acción correcta: actualizar a OpenSSH 10.4 o superior

### Regla de búsqueda

No pegar la línea completa de Nmap.
Tomar solo el nombre del servicio y buscar sin versión.
Filtrar los resultados por versión manualmente.

---

## Detección de sistema operativo con Nmap

### Flag -O

Detecta el sistema operativo del objetivo analizando
cómo responde a ciertos paquetes de red.

```bash
nmap -O 127.0.0.1
```
### Diferencia entre -O y -sV

| Flag | Detecta |
|------|---------|
| -O | Sistema operativo |
| -sV | Servicios y versiones |
| -A | Ambos + scripts + traceroute |

### Por qué Nmap da un rango y no la versión exacta

Nmap analiza la firma del tráfico de red para estimar el SO.
No puede leer el kernel directamente desde afuera.
Por eso devuelve un rango probable en vez de una versión exacta.

### Cómo obtener la versión exacta del kernel

Desde adentro del sistema:
```bash
uname -r
```
Resultado: 6.11.2+kali-amd64

Con la versión exacta podés buscar CVEs específicos del kernel.

---

## Escaneo más completo

```bash
nmap -A -p 1-65535 127.0.0.1
```

`-A` incluye: detección de SO (-O), versiones (-sV),
scripts (-sC) y traceroute.
`-p 1-65535` escanea todos los puertos posibles.

---

## Comandos del día

```bash
nmap -O 127.0.0.1              # detección de SO
nmap -O -sV 127.0.0.1          # SO + versiones de servicios
nmap -A -p 1-65535 127.0.0.1   # escaneo completo
uname -r                        # versión exacta del kernel
```
