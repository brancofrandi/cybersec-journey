## Sesión 22 — Python: detector de puertos anómalos en captura de red
**Fecha:** 30/08/2026

---

## Objetivo

Leer una captura exportada desde Wireshark en CSV y detectar
conexiones RST en puertos que no sean 80 ni 443.
Un RST en puerto inusual puede indicar una anomalía de seguridad.

---

## Exportar captura desde Wireshark

File → Export Packet Dissections → As CSV

---

## Script: detector de RST en puertos inusuales

```python
captura = open("captura.csv", "r")
i = 0

for linea in captura:
    if "RST" in linea and "80" not in linea and "443" not in linea:
        print(linea.strip())
        i += 1

print("Los puertos a tener en cuenta por anomalias son: " + str(i))

captura.close()
```

---

## Conceptos nuevos

**`not in`:** condicional que verifica si algo NO está en una cadena.
```python
if "443" not in linea:   # verdadero si 443 no aparece en la línea
```

**`grep -v`:** equivalente en Linux. Muestra líneas que NO contienen el texto.
```bash
grep "RST" captura.csv | grep -v "443" | grep -v "80" | wc -l
```

**`i =+ 1` vs `i += 1`:**
- `i += 1` → suma 1 a i (correcto)
- `i =+ 1` → asigna el valor +1 a i, no acumula (error común)

---

## Prueba de funcionamiento

Se agregó manualmente una línea con RST en puerto 4444:
```bash
sudo chmod 666 captura.csv
echo '"999","1.0","10.0.2.15","192.168.1.1","TCP","60","4444 → 1234 [RST]"' >> captura.csv
```

El script detectó la anomalía y mostró la línea.
Resultado: 1 puerto a tener en cuenta.

---

## Conexión con ciberseguridad

Este script automatiza parte del análisis que hacés manualmente
en Wireshark con el filtro `tcp.flags.reset == 1 && tcp.port != 443`.
La ventaja de Python es que podés procesarlo sin abrir Wireshark,
integrarlo en un pipeline y escalar a miles de archivos.

---

## Permisos en Linux aplicados

El archivo CSV fue creado por root con Wireshark.
Para poder escribir en él:
```bash
sudo chmod 666 captura.csv
```

`chmod 666` da lectura y escritura a propietario, grupo y otros.
Equivalente simbólico: `chmod a+rw captura.csv`
