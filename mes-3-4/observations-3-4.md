## Dudas resueltas en sesion — puertos y red

---

**FTP vs SFTP**
FTP no cifra datos, es inseguro.
SFTP transfiere archivos cifrado sobre SSH.
SSH es para acceso a terminal, SFTP es para archivos.
Son protocolos distintos aunque SFTP use SSH por debajo.

**Puerto 80 vs 443**
80 = HTTP, sin cifrado.
443 = HTTPS, con cifrado SSL/TLS.
SSH es el puerto 22, no el 443.

**Puerto escuchando**
Un proceso esperando activamente conexiones entrantes.
No implica visibilidad desde afuera, un firewall puede
tenerlo filtrado mientras sigue escuchando internamente.

**127.0.0.1 vs 0.0.0.0**
127.0.0.1: solo acepta conexiones desde la misma maquina.
0.0.0.0: acepta desde cualquier interfaz de red.
Un servicio en 0.0.0.0 sin firewall es vector de ataque.

**netstat sin sudo**
Sin sudo la columna PID/Program name muestra guion.
Con sudo muestra numero de proceso y nombre del servicio.

**Mosquitto**
Broker MQTT, protocolo para IoT e industrial.
Estaba instalado del curso de Ingelearn, corriendo en
segundo plano sin saberlo. Puerto 1883.
Instalado como snap, nombre del servicio:
snap.mosquitto.mosquitto.service
Detenido y deshabilitado: no arranca mas automaticamente.
