## Sesión 16 — Python básico: variables, tipos y funciones
**Fecha:** 23/08/2026

---

## Por qué Python en ciberseguridad

- Automatizar tareas repetitivas
- Crear herramientas propias de escaneo y análisis
- Leer y procesar logs
- Interactuar con redes y puertos
- Complementar herramientas como Nmap y Wireshark

---

## Modo interactivo vs archivo .py

**Modo interactivo:**
```bash
python3
```
Ejecuta cada línea al instante. Útil para probar cosas rápido.
Salir: Ctrl+D o exit()

**Archivo .py:**
```bash
python3 scanner.py
```
Guarda el código permanentemente. Se ejecuta completo.
En ciberseguridad siempre se usan archivos porque los scripts
deben poder ejecutarse repetidamente.

---

## Tipos de datos

| Tipo | Nombre | Ejemplo |
|------|--------|---------|
| Texto | string | `ip = "192.168.0.1"` |
| Número entero | integer | `puerto = 80` |

**Diferencia clave:** los strings van entre comillas, los integers no.

---

## Variables

```python
ip = "192.168.0.1"
puerto = 80
```

Una variable es un dato con nombre que se puede reutilizar.

---

## print()

Imprime en pantalla cualquier cosa: texto, variables, resultados.

```python
print("Hola Franco")
print(ip)
print(puerto)
```

---

## Concatenación de strings

Para unir texto con texto se usa `+`.
Para unir texto con un número se necesita `str()` primero.

```python
mensaje = "Escaneando " + ip + " en puerto " + str(puerto)
print(mensaje)
```

`str()` convierte un número a texto para poder concatenarlo.

---

## Funciones

```python
def escanear(ip, puerto):
    print("Escaneando " + ip + " en puerto " + str(puerto))
```

- `def` define la función
- `escanear` es el nombre
- `ip` y `puerto` son los parámetros que recibe
- El cuerpo va indentado con 4 espacios

**Llamar una función:**
```python
escanear("192.168.0.1", 80)
escanear("192.168.0.1", 22)
escanear("10.0.0.1", 443)
```

---

## Script completo — scanner.py

```python
def escanear(ip, puerto):
    print("Escaneando " + ip + " en puerto " + str(puerto))

escanear("192.168.0.1", 80)
escanear("192.168.0.1", 22)
escanear("10.0.0.1", 443)
```

---

## Comandos de terminal usados

```bash
python3              # modo interactivo
python3 scanner.py   # ejecutar archivo
nano scanner.py      # editar archivo
Ctrl+O               # guardar en nano
Ctrl+X               # salir de nano
Ctrl+D               # salir del intérprete
```

