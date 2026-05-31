## Bandit — Niveles 0 al 2
**Fecha:** [31/05/26]
**Plataforma:** OverTheWire — overthewire.org/wargames/bandit

---

### Comando SSH para conectarse

ssh bandit0@bandit.labs.overthewire.org -p 2220

| Parte | Significado |
|-------|-------------|
| ssh | Protocolo de conexion remota segura |
| bandit0 | Usuario en el servidor remoto |
| @ | Conector usuario/servidor |
| bandit.labs.overthewire.org | Direccion del servidor |
| -p 2220 | Puerto de conexion (defecto es 22) |

Por que el puerto 2220 y no el 22:
Muchos administradores cambian el puerto SSH del 22 a otro
numero para reducir ataques automatizados de fuerza bruta.
No es una medida fuerte pero reduce el ruido.

---

### Nivel 0

Conectarse al servidor por primera vez con SSH.
Leer el archivo readme en /home/bandit0.

Comandos usados:
- ls
- cat readme

---

### Nivel 1 — archivo llamado -

Problema: cat - no funciona porque Linux interpreta
el guion como "leer desde el teclado", no como archivo.

Solucion: indicar la ruta explicitamente.
cat ./-

El ./ le dice a Linux que el - es un archivo en el
directorio actual, no un flag del comando.

Comandos usados:
- find (para encontrar la ruta)
- cat ./-

Relevancia en ciberseguridad:
Sanitizacion de entrada. Programas mal escritos pueden
ser explotados con archivos de nombres especiales como
-, espacios o caracteres especiales.

---

### Lo que aprendi

- SSH permite acceso remoto seguro a otro sistema
- El puerto por defecto de SSH es el 22
- ./ antes de un nombre indica ruta relativa,
  no flag ni comando
- Las contrasenas de cada nivel se guardan localmente,
  no en GitHub
