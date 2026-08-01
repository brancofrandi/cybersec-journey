# Sesión 10 — Bandit niveles 0 al 9
**Fecha:** [04/06/26]
 
---
 
## Qué es Bandit
 
Plataforma OverTheWire. Juego de terminales donde cada nivel
es un desafío real de Linux. Se accede por SSH a un servidor
remoto y hay que encontrar una contraseña para pasar al siguiente.
 
Conexión: ssh banditN@bandit.labs.overthewire.org -p 2220
 
---
 
## Conceptos aprendidos por nivel
 
### Nivel 0 → 1
Leer un archivo con cat y ls básico.
Comandos: ls, cat readme
 
### Nivel 1 → 2
Archivo llamado con guión (-). Linux lo interpreta como flag.
Solución: usar ruta relativa ./
Comando: cat ./-
 
**Por qué ./ antes de un nombre:**
Indica a Linux que es una ruta relativa, no un flag ni comando.
Sin ./ el sistema interpreta - como una opción del comando.
 
### Nivel 2 → 3
Archivo con espacios en el nombre.
Solución: encerrar entre comillas y usar ruta relativa.
Comando: cat "./spaces in this filename"
 
### Nivel 3 → 4
Archivo oculto dentro de un directorio.
Solución: ls -la muestra archivos ocultos (los que empiezan con punto).
Comando: ls -la inhere/ → cat "./inhere/...Hiding-From-You"
 
**Cómo identificar archivos ocultos en Linux:**
Empiezan con punto (.). Solo visibles con ls -a o ls -la.
 
### Nivel 4 → 5
Encontrar el único archivo legible entre varios binarios.
Solución: file ./* muestra el tipo de cada archivo.
El legible para humanos es ASCII text.
Comando: file ./*
 
**Cuándo usar file:**
Cuando necesitás saber el tipo de un archivo sin abrirlo.
Especialmente útil con archivos sin extensión o binarios.
 
### Nivel 5 → 6
Buscar archivo por propiedades: no ejecutable, tamaño 1033 bytes.
Comando: find . -type f ! -executable -size 1033c
 
**El operador ! en find:**
Invierte la condición que le sigue.
! -executable = busca archivos que NO sean ejecutables.
 
### Nivel 6 → 7
Buscar archivo por usuario, grupo y tamaño desde la raíz.
Comando: find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
 
Flags de find usados:
- -user: filtra por propietario del archivo
- -group: filtra por grupo del archivo
- -size: filtra por tamaño (c = bytes)
### Nivel 7 → 8
Buscar una palabra específica dentro de un archivo de texto.
Comando: grep 'millionth' data.txt
 
### Nivel 8 → 9
Encontrar la línea que aparece una sola vez en un archivo.
Comandos: sort data.txt | uniq -u
 
**sort:** ordena líneas alfabéticamente (necesario para que uniq funcione).
**uniq -u:** muestra solo las líneas que aparecen exactamente una vez.
uniq solo detecta duplicados si están juntos, por eso sort va primero.
 
### Nivel 9 → 10
Extraer texto legible de un archivo binario y filtrar por patrón.
Comando: strings data.txt | grep "=."
 
**strings:** extrae cadenas de texto ASCII de archivos binarios.
Se usa cuando cat muestra caracteres ilegibles.
 
**grep "=.":**
= busca el signo igual literal.
. en expresiones regulares significa cualquier carácter.
Encuentra líneas con = seguido de al menos un carácter.
 
---
 
## Comandos nuevos aprendidos en Bandit
 
| Comando | Para qué sirve |
|---------|----------------|
| ssh user@host -p puerto | Conectarse a servidor remoto |
| file ./* | Ver el tipo de cada archivo en el directorio |
| sort archivo | Ordenar líneas alfabéticamente |
| uniq -u | Mostrar solo líneas únicas (sin duplicados) |
| strings archivo | Extraer texto legible de archivos binarios |
| find -user/-group/-size | Buscar por propietario, grupo o tamaño |
| ! -executable | Buscar archivos NO ejecutables |
 
---
 
## Lo que tuve que buscar
 
- Flags -user, -group, -size en find (nivel 6)
- Comando strings (nivel 9)
- Combinación sort | uniq -u (nivel 8)
---
 
## Pendiente
 
- Continuar desde nivel 10 en la próxima sesión
