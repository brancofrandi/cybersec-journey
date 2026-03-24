## Linux Fundamentals Part 1 - Tasks 1 al 5

**Fecha:** [22/03/2026]
**Plataforma:** TryHackMe

---

### Lo mas relevante para ciberseguridad

En ciberseguridad se usa CLI en lugar de GUI porque es mas
directo, rapido y efectivo. La mayoria de herramientas corren
desde terminal, por eso Linux es el sistema dominante en el area.

---

### Comandos aprendidos

| Comando | Qué hace |
|---------|----------|
| echo    | Devuelve el texto que escribiste |
| whoami  | Muestra el nombre del usuario activo |
| ls      | Muestra los archivos de la carpeta actual |
| cd      | Entra a la carpeta que le indicás |
| cat     | Abre y muestra el contenido de un archivo |
| pwd     | Muestra la ruta completa de donde estás |

---

### Lo que fue nuevo

- CLI vs GUI como concepto formal
- Comandos basicos de navegacion en Linux

### Dudas pendientes

- Diferencia entre proceso y servicio (razonado solo,
  necesito asentarlo mejor)
- Seguramente hay mas dudas que no identifiqué todavia,
  las voy a descubrir en la proxima sesion cuando
  practique los comandos en Ubuntu real

---

## Sesion 3 - Practica Linux en Ubuntu real
**Fecha:** [23/03/2026]

---

### Comandos practicados

| Comando | Qué hace |
|---------|----------|
| `ls` | Muestra los archivos y carpetas del directorio actual |
| `cd` | Entra a la carpeta indicada |
| `pwd` | Muestra la ruta completa donde estas parado |
| `cat` | Muestra el contenido de un archivo |
| `find` | Busca archivos por nombre |
| `grep` | Busca texto dentro de archivos |
| `wc` | Cuenta lineas, palabras y caracteres |
| `\|` | Pipe: conecta la salida de un comando con la entrada del siguiente |

---

### Lo que practiqué

- Navegue el sistema de archivos desde la raiz /
- Explore carpetas del sistema en /etc
- Use find para buscar archivos por nombre
- Use grep para buscar contenido dentro de archivos
- Use wc para contar lineas de una salida
- Combine grep con wc usando pipe

---

### Ejercicios que resolví solo

`cat /etc/os-release`
`find / -name hostname`
`grep "ubuntu" /etc/os-release`
`wc /etc/os-release`
`grep "ubuntu" /etc/os-release | wc -l`
`grep "/bin/bash" /etc/passwd`
`cat /etc/passwd | wc -l`

---

### Lo que aprendí sobre /etc/passwd

- Lista todos los usuarios del sistema
- Tiene 7 campos separados por dos puntos:
  usuario:contraseña:UID:GID:descripcion:home:shell
- La x en contrasena significa que esta cifrada en /etc/shadow
- UID 0 = root, siempre es el superusuario
- /bin/bash = puede iniciar sesion interactiva
- /bin/false o /usr/sbin/nologin = no puede iniciar sesion
- Mi sistema tiene 34 usuarios pero solo 2 reales:
  root y mi usuario personal
- El resto son servicios del sistema corriendo en segundo plano

---

### Conceptos nuevos

**Ruta absoluta vs ruta relativa**
- Ruta absoluta: empieza desde la raiz, ejemplo /etc/os-release
- Ruta relativa: desde donde estas parado, ejemplo cat os-release
- Los comandos dependen de donde estas parado

**Pipe ( | )**
- Conecta la salida de un comando con la entrada del siguiente
- Ejemplo: grep "ubuntu" os-release | wc -l
- Es uno de los conceptos mas importantes de Linux

**find con y sin ruta**
- find / -name hostname busca desde la raiz
- find -name hostname busca solo desde donde estas parado

---

### Relevancia en ciberseguridad

Leer /etc/passwd es uno de los primeros pasos en un pentest
para identificar que usuarios reales existen en el sistema
y cuales pueden iniciar sesion interactiva. Con dos comandos
se puede mapear todos los usuarios reales de un sistema.

---

### Dudas pendientes

- Profundizar grep con mas opciones y flags
- Entender mejor wc y sus variantes (-l, -w, -c)


