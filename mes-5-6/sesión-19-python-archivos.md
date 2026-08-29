## Sesión 19 — Python: leer archivos y filtrar contenido
**Fecha:** 28/08/2026

---

## Leer un archivo en Python

```python
archivo = open("resultados.txt", "r")

for linea in archivo:
    print(linea.strip())

archivo.close()
```

- `open("archivo.txt", "r")` → abre el archivo en modo lectura
- `"r"` → read, solo lectura
- `for linea in archivo` → recorre cada línea del archivo
- `.strip()` → elimina saltos de línea al inicio y final de cada línea
- `archivo.close()` → cierra el archivo y libera recursos

---

## Crear archivos desde la terminal

```bash
echo "192.168.0.1 - puerto 22 abierto" > resultados.txt
echo "192.168.0.2 - puerto 80 abierto" >> resultados.txt
echo "192.168.0.3 - puerto 443 abierto" >> resultados.txt
```

- `>` → crea el archivo y escribe (sobreescribe si ya existe)
- `>>` → agrega al final sin sobreescribir

---

## Filtrar líneas con un condicional

```python
archivo = open("resultados.txt", "r")

for linea in archivo:
    if "22" in linea:
        print(linea.strip())

archivo.close()
```

`if "22" in linea` → solo imprime las líneas que contienen "22".

---

## Contar líneas que cumplen una condición

```python
archivo = open("resultados.txt", "r")
i = 0

for linea in archivo:
    if "22" in linea:
        i += 1

print("Se encontraron " + str(i) + " lineas con puerto 22")

archivo.close()
```

- `i = 0` → variable contador inicializada en cero
- `i += 1` → suma 1 cada vez que se cumple la condición
- `str(i)` → convierte el número a string para concatenar

---

## Conexión con ciberseguridad

Leer archivos en Python permite automatizar el análisis de logs.
En vez de usar `grep 'Failed' /var/log/auth.log | wc -l` en la terminal,
podés escribir un script Python que lea el log, filtre por
la palabra "Failed" y cuente cuántas veces aparece.

---

## Operadores de asignación

| Operador | Significado |
|----------|-------------|
| `i = 0` | asigna el valor 0 a i |
| `i += 1` | suma 1 a i (equivale a i = i + 1) |
| `i -= 1` | resta 1 a i |
