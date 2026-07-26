## Sesión 07 — Práctica integradora Semana 2
**Fecha:** 25/07/2026

---

## Ejercicio integrador — Relevamiento completo

### Objetivo
Relevamiento de un servidor Linux con dos servicios activos.
Aplicación de todos los comandos vistos en la Semana 2.

---

## Proceso ejecutado

### 1. Identificar IP
```bash
ip addr
```
IP interna: 127.0.0.1 (loopback)

**Importante:** verificar la IP antes de ejecutar cualquier
escaneo. Un error tipográfico escanea un objetivo diferente.

### 2. Escaneo completo
```bash
nmap -p 1-65535 -sV -O 127.0.0.1
```

Servicios encontrados:

22/tcp open ssh OpenSSH 10.2p1 Debian 5 (protocol 2.0)

80/tcp open http Apache httpd 2.4.66 ((Debian))

SO: Linux 5.0 - 6.2

### 3. Headers HTTP
```bash
nmap -sV -sC 127.0.0.1
```
|_http-title: Apache2 Debian Default Page

|_http-server-header: Apache/2.4.66 (Debian)

**Problema:** el header Server expone la versión exacta
de Apache sin necesidad de escanear.
**Recomendación:** ocultar o falsificar el header Server.

### 4. Métodos de autenticación SSH
```bash
nmap --script ssh-auth-methods 127.0.0.1 -p 22
```

| ssh-auth-methods:

| Supported authentication methods:

| publickey

|_ password


**Problema:** autenticación por contraseña habilitada,
vulnerable a ataques de fuerza bruta.
**Recomendación:** deshabilitar autenticación por contraseña
y usar solo publickey.

### 5. Búsqueda de CVEs
Búsqueda en nvd.nist.gov: `Apache HTTP Server 2.4.66`

Se encontraron 3 vulnerabilidades:

**CVE-2026-29169 — más crítico**
Desreferenciación de puntero nulo en mod_dav_lock.
Un atacante puede bloquear el servidor con una solicitud
maliciosa. Ataque de tipo Denial of Service (DoS).
No requiere autenticación previa.
Solución: actualizar a Apache 2.4.67.

**CVE-2026-33007**
Desreferenciación de puntero nulo en mod_authn_socache.
Bloquea un proceso hijo del servidor en configuración
de proxy con caché. Más acotado que el anterior.
Solución: actualizar a Apache 2.4.67.

**CVE-2026-33006**
Ataque de temporización en mod_auth_digest.
Permite eludir autenticación Digest.
Requiere que el servidor use ese sistema de autenticación.
Solución: actualizar a Apache 2.4.67.

---

## Cómo leer un CVE

Tres preguntas clave al analizar cualquier CVE:
1. ¿Qué puede hacer el atacante? (bloquear, entrar, robar)
2. ¿Necesita autenticación previa o no?
3. ¿Cuál es la versión que soluciona el problema?

---

## Tipos de ataque vistos hoy

**DoS (Denial of Service):**
El atacante envía una solicitud maliciosa diseñada para
bloquear o hacer caer el servicio atacado.
El servidor deja de responder para todos los usuarios.

**Ataque de temporización:**
Explota diferencias en el tiempo de respuesta del servidor
para deducir información sin autenticarse.
Más sofisticado, requiere configuración específica.

**Fuerza bruta:**
Prueba miles de contraseñas automáticamente hasta encontrar
la correcta. Solo posible si SSH acepta autenticación
por contraseña.

---

## Recomendaciones del relevamiento

1. Actualizar Apache de 2.4.66 a 2.4.67
2. Ocultar el header Server en Apache
3. Deshabilitar autenticación por contraseña en SSH
4. Usar solo publickey en SSH
5. Monitoreo periódico de nuevos CVEs

---

## Lección aprendida

Un error tipográfico en la IP escanea un objetivo diferente.
Verificar siempre la IP antes de ejecutar cualquier comando.
En un entorno real ese error puede tener consecuencias legales.

---

## Comandos del ejercicio

```bash
ip addr
nmap -p 1-65535 -sV -O 127.0.0.1
nmap -sV -sC 127.0.0.1
nmap --script ssh-auth-methods 127.0.0.1 -p 22
nmap --script http-headers 127.0.0.1 -p 80
```
