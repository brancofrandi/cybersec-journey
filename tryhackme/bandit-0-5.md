## Bandit — Niveles 0 al 5
**Fecha:** [02/06/26]
**Plataforma:** OverTheWire — overthewire.org/wargames/bandit

---

### Comando de conexion SSH

ssh banditN@bandit.labs.overthewire.org -p 2220

| Parte | Significado |
|-------|-------------|
| ssh | Protocolo de conexion remota segura |
| banditN | Usuario del nivel actual |
| bandit.labs.overthewire.org | Direccion del servidor |
| -p 2220 | Puerto de conexion (defecto es 22) |

Para cambiar de nivel: exit para salir, reconectarse
con el usuario del nivel siguiente y la contrasena obtenida.

---

### Nivel 0

Conectarse al servidor por primera vez.
Leer el archivo readme en el directorio home.

Comandos: ls, cat readme

---

### Nivel 1 — archivo llamado -

Problema: cat - no funciona, Linux interpreta el guion
como leer desde el teclado, no como nombre de archivo.

Solucion:
cat ./-

./ indica ruta relativa, no flag ni comando.

Aprendizaje: sanitizacion de entrada. Archivos con nombres
especiales pueden explotar scripts mal escritos.

---

### Nivel 2 — archivo con espacios en el nombre

Problema: cat spaces in this filename falla porque Linux
interpreta cada palabra como un archivo separado.

Solucion:
cat "./spaces in this filename"

Las comillas encierran el nombre completo como una sola
cadena. El ./ indica que es una ruta relativa.

---

### Nivel 3 — archivo oculto en directorio

Problema: el archivo no aparece con ls normal porque
empieza con punto, convencion de archivos ocultos en Linux.

Solucion:
cd inhere
ls -la
cat "./... Hiding-From-You"

-a en ls muestra todos los archivos incluyendo ocultos.

Flags de ls aprendidos:
| Flag | Que hace |
|------|----------|
| -l | Formato largo con permisos, tamano y fecha |
| -a | Muestra archivos ocultos (empiezan con .) |
| -h | Tamano legible para humanos (KB, MB) |
| -t | Ordena por fecha de modificacion |
| -r | Invierte el orden |
| -R | Lista recursivamente subcarpetas |

---

### Nivel 4 — unico archivo legible entre varios

Problema: varios archivos en inhere, solo uno es texto
legible, el resto son datos binarios.

Primer intento: abrir uno por uno con cat (funciona
pero no es eficiente).

Solucion profesional:
file ./*

file muestra el tipo de cada archivo. El legible
para humanos aparece como ASCII text.

Aprendizaje: file es mas eficiente que abrir archivos
binarios con cat, que muestra caracteres ilegibles.

---

### Nivel 5 — archivo por tamano y propiedades

Problema: el archivo esta en alguna subcarpeta de inhere.
Propiedades: legible por humanos, no ejecutable, 1033 bytes.

Solucion:
find . -type f ! -executable -size 1033c -exec ls -lh {} +

Desglose:
| Parte | Significado |
|-------|-------------|
| . | Busca desde el directorio actual |
| -type f | Solo archivos, no carpetas |
| ! -executable | Archivos que NO son ejecutables |
| -size 1033c | Exactamente 1033 bytes (c = bytes) |
| -exec ls -lh {} + | Muestra resultado con detalle |

Tuve que buscar el comando en internet para este nivel.
El ! para invertir condiciones es nuevo.

---

### Lo que aprendi en estos niveles

- ./ indica ruta relativa, evita ambiguedad con flags
- Comillas encierran nombres con espacios como una sola cadena
- Archivos ocultos en Linux empiezan con punto
- file identifica el tipo de archivo sin abrirlo
- find combina multiples condiciones con flags
- ! invierte una condicion en find
