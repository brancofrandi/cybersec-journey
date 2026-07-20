## Sesión 11 — Bandit niveles 10 al 14
**Fecha:** [fecha de hoy]

---

## Nivel 10 → 11: Base64

La contraseña estaba codificada en base64.

**¿Qué es base64?**
Sistema de codificación que convierte datos binarios en texto
usando 64 caracteres posibles (A-Z, a-z, 0-9, +, /).
No es cifrado, no protege la información. Solo cambia el formato.
Se reconoce porque suele terminar con = o ==.

**Comando:**
base64 -d data.txt
- -d significa decode, decodificar.

---

## Nivel 11 → 12: ROT13

Todas las letras estaban rotadas 13 posiciones.

**¿Qué es ROT13?**
Cifrado por rotación simple. No es cifrado real, es una
codificación básica. Como el alfabeto tiene 26 letras,
rotar 13 dos veces te devuelve al inicio. Es simétrico.

**Comando:**
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

**tr (translate):** reemplaza caracteres. Le das dos grupos
del mismo tamaño y cambia cada carácter del primero por
el correspondiente del segundo.

---

## Nivel 12 → 13: Hexdump y descompresión en capas

El archivo era un hexdump de un archivo comprimido
múltiples veces con diferentes herramientas.

**Proceso:**
1. Crear directorio temporal: mktemp -d
2. Copiar archivo: cp ~/data.txt /tmp/tmp.XXXXX
3. Convertir hexdump a binario: xxd -r data.txt > data.bin
4. Repetir hasta llegar a texto plano:
   - file data.bin → identificar tipo
   - renombrar con mv según extensión
   - descomprimir con la herramienta correcta

**Herramientas de compresión:**

| Herramienta | Extensión | Comando |
|-------------|-----------|---------|
| gzip | .gz | gzip -d archivo.gz |
| bzip2 | .bz2 | bzip2 -d archivo.bz2 |
| tar | .tar | tar xf archivo.tar |

**Capas que tenía el archivo:**
gzip → bzip2 → gzip → tar → tar → bzip2 → tar → gzip → texto

**Comandos nuevos:**

| Comando | Para qué sirve |
|---------|----------------|
| xxd -r archivo | Convierte hexdump a binario |
| mktemp -d | Crea directorio temporal con nombre aleatorio |
| gzip -d archivo.gz | Descomprime gzip |
| bzip2 -d archivo.bz2 | Descomprime bzip2 |
| tar xf archivo.tar | Extrae tar |

---

## Nivel 13 → 14: Clave SSH privada

No había contraseña, había una clave privada SSH para
conectarse al siguiente nivel.

**¿Qué es un par de claves SSH?**
- Clave privada: la tenés vos, nunca se comparte
- Clave pública: la tiene el servidor
El servidor verifica que ambas coinciden. Si sí, te deja
entrar sin contraseña.

**Proceso:**
1. Descargar la clave desde el servidor a Kali con scp:
   scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private ./sshkey.private

2. Darle permisos correctos (SSH rechaza claves muy abiertas):
   chmod 600 sshkey.private

3. Conectarse usando la clave:
   ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

**scp (Secure Copy):**
Copia archivos entre computadoras usando SSH.
Sintaxis: scp -P puerto usuario@servidor:origen destino_local

---

## Nivel 14 → 15: Enviar datos a un puerto con netcat

La contraseña del nivel actual había que enviarla al
puerto 30000 del mismo servidor para obtener la siguiente.

**nc (netcat):**
Herramienta para conectarse a un puerto y enviar o recibir
datos en texto plano. Se la llama la navaja suiza de redes.

Sintaxis: nc host puerto

**Diferencia con SSH:**
- nc: texto plano, sin cifrado
- ssh: cifrado, conexión segura

**Proceso:**
nc localhost 30000
[escribir contraseña y Enter]
Respuesta: contraseña del siguiente nivel

---

## Comandos nuevos aprendidos en esta sesión

| Comando | Para qué sirve |
|---------|----------------|
| base64 -d archivo | Decodifica base64 |
| tr 'grupo1' 'grupo2' | Reemplaza caracteres |
| xxd -r archivo | Convierte hexdump a binario |
| mktemp -d | Crea directorio temporal |
| gzip -d archivo.gz | Descomprime gzip |
| bzip2 -d archivo.bz2 | Descomprime bzip2 |
| tar xf archivo.tar | Extrae tar |
| scp -P puerto user@host:origen destino | Copia archivos entre servidores |
| nc host puerto | Conectarse a un puerto |
| mkdir nombre | Crea un directorio |

---

## Lo que tuve que investigar

- Todos los comandos de compresión (gzip, bzip2, tar)
- Sintaxis de scp
- Uso de nc para enviar datos
- Autenticación SSH con clave privada
