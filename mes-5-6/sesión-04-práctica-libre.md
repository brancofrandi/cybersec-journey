## Sesión 04 — Práctica libre Nmap + búsqueda de CVEs
**Fecha:** 20/07/2026

---

## Ejercicio integrador — Semana 1

Relevamiento completo de un sistema con dos servicios activos.

---

## Proceso ejecutado

**1. Levantar servicios:**
```bash
sudo systemctl start ssh apache2
```

**2. Identificar IP:**
```bash
ip addr
```
IP interna: 127.0.0.x

**3. Relevamiento completo:**
```bash
nmap -p 1-65535 -A 127.0.0.x
```


**4. Búsqueda de CVEs:**
Se buscó en cvedetails.com con los servicios identificados.
No se encontraron CVEs públicos para estas versiones
en la fecha del relevamiento.

---

## Análisis de líneas nuevas

**`|_http-server-header: Apache/2.4.66 (Debian)`**
Header que el servidor devuelve al conectarse.
Expone la versión del software públicamente.
En producción debería ocultarse o falsificarse.

**`|_http-title: Apache2 Debian Default Page: It works`**
Título de la página que está sirviendo Apache.
Indica que el servidor no fue configurado todavía.
Un atacante interpreta esto como servidor nuevo o sin uso.

---

## Cómo buscar CVEs correctamente

**En cvedetails.com:**
1. Vulnerable Software → Products
2. Buscar nombre del producto: Apache HTTP Server
3. Seleccionar producto → Vulnerabilities
4. Filtrar por versión

**En nvd.nist.gov:**
Buscar: `apache http server 2.4.66`

**Regla:** nunca pegar la línea completa de Nmap.
Tomar solo nombre del producto y versión.
Ejemplo: de `Apache httpd 2.4.66 ((Debian))` → buscar `Apache HTTP Server 2.4.66`

---

## Conclusión del relevamiento

Sistema con dos servicios activos: SSH en puerto 22
y Apache en puerto 80. No se encontraron CVEs públicos
para estas versiones en la fecha del análisis.

Se recomienda:
- Ocultar el header de versión en Apache
- Reemplazar la página por defecto
- Monitoreo periódico de nuevos CVEs

---

## Cierre de Semana 1 — Estado

Semana 1 completa. Comandos dominados:
- nmap IP (escaneo básico)
- nmap -sV (detección de versiones)
- nmap -sC (scripts por defecto)
- nmap -A (escaneo agresivo)
- nmap -p (puertos específicos o rangos)
- Flujo completo: escaneo → versión → CVE → acción
