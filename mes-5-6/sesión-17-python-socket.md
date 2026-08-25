## Sesión 17 — Python: librería socket y escáner de puertos
**Fecha:** 26/08/2026

---

## Librería socket

Permite a Python conectarse a puertos y resolver dominios.
Se importa con:

```python
import socket
```

---

## Resolver un dominio

```python
ip = socket.gethostbyname("google.com")
print(ip)
```

Hace lo mismo que una consulta DNS pero desde Python.

---

## Conectarse a un puerto

```python
s = socket.socket()
s.connect(("google.com", 80))
print("Conexión exitosa")
s.close()
```

- `socket.socket()` → crea el objeto de conexión (la línea telefónica vacía)
- `s.connect()` → establece la conexión (marcar el número)
- `s.close()` → cierra la conexión y libera recursos

---

## Manejo de errores con try/except

```python
try:
    s.connect((host, puerto))
    print(str(puerto) + " abierto")
except:
    pass
```

- `try` → intenta ejecutar el código
- `except` → si falla, toma otra acción sin romper el script
- `pass` → ignora el error y sigue

---

## Escáner de puertos completo

```python
import socket

def probar_puerto(host, puerto):
    s = socket.socket()
    s.settimeout(0.5)
    try:
        s.connect((host, puerto))
        print(str(puerto) + " abierto")
    except:
        pass
    s.close()

host = "google.com"
for puerto in range(1, 1024):
    probar_puerto(host, puerto)
```

**Resultado en google.com:**
- Puerto 80 abierto
- Puerto 443 abierto

---

## settimeout()

Define el tiempo máximo de espera por puerto.
Sin timeout el script tarda minutos. Con 0.5 segundos es razonable.

```python
s.settimeout(0.5)
```

---

## Por qué Nmap es más rápido

Este script escanea puertos uno por uno (secuencial).
Nmap escanea múltiples puertos en paralelo (concurrente).
Esa diferencia hace que Nmap sea órdenes de magnitud más rápido.
