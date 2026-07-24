## Sesión 06 — Scripts NSE de Nmap
**Fecha:** 24/07/2026

---

## ¿Qué es NSE?

Nmap Scripting Engine. Motor de scripts integrado en Nmap.
Permite ejecutar scripts sobre los servicios encontrados
para obtener información extra o detectar vulnerabilidades.

`-sC` ejecuta los scripts por defecto.
`--script nombre` ejecuta un script específico.

---

## Dónde están los scripts

```bash
ls /usr/share/nmap/scripts/
ls /usr/share/nmap/scripts/ | grep http
ls /usr/share/nmap/scripts/ | grep ssh
```

Kali tiene cientos de scripts preinstalados organizados
por protocolo y función.

---

## Sintaxis para ejecutar scripts

```bash
nmap --script [nombre-del-script] [IP] -p [puerto]
```

Ejemplos:
```bash
nmap --script http-headers 127.0.0.1 -p 80
nmap --script ssh-auth-methods 127.0.0.1 -p 22
nmap --script http-sql-injection 127.0.0.1 -p 80
nmap --script http-php-version 127.0.0.1 -p 80
```

---

## Scripts practicados hoy

### http-headers
Muestra los headers HTTP que devuelve el servidor.

| ssh-auth-methods:
| Supported authentication methods:
| publickey
|_ password

**Por qué importa en seguridad:**
Si el servidor acepta `password`, es vulnerable a ataques
de fuerza bruta. Un atacante puede probar miles de
contraseñas automáticamente.

**Recomendación en producción:**
Deshabilitar autenticación por contraseña y usar
solo `publickey`. Sin el archivo de clave privada
el ataque de fuerza bruta no es posible.

### http-sql-injection
Busca vulnerabilidades de inyección SQL en formularios web.
No devuelve resultados en páginas estáticas sin base de datos.

### http-php-version
Detecta la versión de PHP en el servidor.
No devuelve resultados si PHP no está instalado.

---

## Por qué un script puede no devolver resultados

Tres razones posibles:
1. El servicio no tiene la vulnerabilidad o configuración buscada
2. El script requiere una aplicación específica que no está
3. El script tiene un bug en esa versión de Nmap

Un resultado vacío no es un error, es información:
el servicio no tiene lo que el script busca.

---

## Sobre los headers HTTP en producción

**Problema:** Apache muestra su versión en el header Server.
Un atacante obtiene la versión sin necesidad de escanear,
solo haciendo una petición HTTP normal.

**Solución:** configurar Apache para ocultar o falsificar
el header Server. Se llama security through obscurity.
No es protección completa pero reduce la superficie visible.

---

## Comandos del día

```bash
nmap -sC 127.0.0.1                              # scripts por defecto
nmap -sC -sV 127.0.0.1                          # scripts + versiones
nmap --script http-headers 127.0.0.1 -p 80      # headers HTTP
nmap --script ssh-auth-methods 127.0.0.1 -p 22  # métodos auth SSH
ls /usr/share/nmap/scripts/ | grep http         # buscar scripts HTTP
```
