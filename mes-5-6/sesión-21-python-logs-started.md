## Sesión 21 — Python: análisis de logs con búsqueda de eventos
**Fecha:** 30/08/2026

---

## Script: contar y mostrar líneas con "Started"

```python
logs = open("mi_log.txt", "r")
i = 0

for linea in logs:
    if "Started" in linea:
        print(linea.strip())
        i += 1

if i == 1:
    print("Se encontro " + str(i) + " Started")
elif i == 0:
    print("No se encontraron Started")
elif i > 1:
    print("Se encontraron " + str(i) + " Started")

logs.close()
```

**Resultado:** Se encontraron 5 Started

**Verificación con Linux:**
```bash
grep "Started" mi_log.txt | wc -l
```

El resultado coincide. Python hace lo mismo que grep + wc -l.

---

## Lección clave

Un script Python puede reemplazar comandos de Linux combinados.
La ventaja es que podés extenderlo: guardar resultados,
enviar alertas, combinar con otras herramientas.
