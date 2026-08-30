## Sesión 20 — Python: análisis de logs con filtros y contadores
**Fecha:** 29/08/2026

---

## Objetivo

Leer un archivo de log real de Kali y contar líneas
que contienen palabras clave, como lo haría un analista SOC.

---

## Exportar logs del sistema a un archivo

```bash
journalctl -n 20 > mi_log.txt
```

Exporta las últimas 20 líneas del journal a un archivo de texto
para poder analizarlo con Python.

---

## Script de análisis de logs

```python
logs = open("mi_log.txt", "r")
i = 0

for linea in logs:
    if "cannot" in linea:
        i += 1

if i == 1:
    print("Se encontro " + str(i) + " error")
elif i == 0:
    print("No se encontraron errores")
elif i > 1:
    print("Se encontraron " + str(i) + " errores")

logs.close()
```

**Resultado:** Se encontraron 4 errores (líneas con "cannot")

---

## Errores encontrados en el log

Las 4 líneas con "cannot" correspondían a:

cannot aquire pidfile /home/kali/.vboxclient-seamless-tty7-control.pid
cannot aquire pidfile /home/kali/.vboxclient-hostversion-tty7-control.pid
cannot aquire pidfile /home/kali/.vboxclient-draganddrop-tty7-control.pid
cannot aquire pidfile /home/kali/.vboxclient-vmsvga-session-tty7-control.pid


---

## ¿Qué es un pidfile?

Archivo que guarda el PID (Process ID) de un proceso en ejecución.
Sirve para que el sistema sepa que ese proceso ya está corriendo
y no lo vuelva a iniciar dos veces.

Cuando VirtualBox no puede obtener su pidfile es porque ya existe
de una sesión anterior. Es un error menor, no crítico.

---

## Por qué usar tres condiciones en el print

Un solo print mostraría el número sin contexto.
Los tres casos cubren todas las situaciones posibles:
- `i == 0` → ningún error encontrado
- `i == 1` → exactamente un error (mensaje en singular)
- `i > 1` → más de un error (mensaje en plural)

Eso hace el output más legible y profesional.

---

## Errores comunes corregidos en esta sesión

- Usar nombre de variable diferente al definido: `archivo` vs `logs`
- Olvidar los dos puntos al cerrar un condicional: `if "x" in linea:`
- Concatenar integer sin convertir: necesita `str(i)`

---

## Conexión con ciberseguridad

Este script hace lo mismo que:
```bash
grep 'cannot' mi_log.txt | wc -l
```

Pero en Python podés extenderlo para:
- Guardar los resultados en otro archivo
- Filtrar por múltiples palabras clave
- Enviar alertas si el contador supera un umbral
- Integrarlo con otras herramientas
