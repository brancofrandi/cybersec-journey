## netstat y ss en Ubuntu
**Fecha:** [31/03/26]

---

### Que hacen

Muestran los puertos abiertos y servicios escuchando
desde adentro del sistema.
Diferencia con Nmap: Nmap escanea desde afuera,
netstat y ss muestran el estado interno del sistema.

### Flags -tulpn

| Flag | Significado |
|------|-------------|
| -t   | TCP |
| -u   | UDP |
| -l   | Listening, solo puertos escuchando |
| -p   | Program, que programa usa cada puerto |
| -n   | Numeric, muestra IPs y puertos como numeros |

Sin sudo la columna PID/Program name muestra guion.
Con sudo muestra numero de proceso y nombre del servicio.

### Diferencia entre direcciones

- 127.0.0.1: solo acepta conexiones desde la misma maquina
- 0.0.0.0: acepta conexiones desde cualquier interfaz

Un servicio en 0.0.0.0 sin firewall es vector de ataque.

### Lo que encontre en mi sistema

| Puerto | Servicio | Escucha en |
|--------|----------|------------|
| 1883 | Mosquitto (MQTT) | 127.0.0.1 |
| 40585 | containerd | 127.0.0.1 |
| 53 | systemd-resolved (DNS) | 127.0.0.1 |

Todos en 127.0.0.1, ninguno expuesto a la red externa.

### Mosquitto

Broker MQTT, protocolo de mensajeria para IoT e industrial.
Estaba instalado del curso de Ingelear, corriendo en segundo
plano sin saberlo. Puerto estandar: 1883.

Instalado como snap. Para gestionar servicios snap:
sudo systemctl stop snap.mosquitto.mosquitto.service
sudo systemctl disable snap.mosquitto.mosquitto.service

Lo detuve y deshabilite para reducir superficie de ataque.
Lo reinstalo cuando retome seguridad industrial.

### Lo que aprendi sobre servicios desconocidos

Antes de apagar un servicio que no reconoces:
1. Identificar que es
2. Evaluar si es necesario
3. Recien entonces detenerlo

En un sistema ajeno apagar sin entender puede romper
algo critico.
