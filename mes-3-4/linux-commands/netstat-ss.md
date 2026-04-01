## netstat y ss en Ubuntu
**Fecha:** [31/03/26]

---

### Qué hacen

Muestran las conexiones de red activas y los puertos que
estan escuchando en el sistema en ese momento.
Son comandos para ver el estado de red desde adentro.
Distintos a Nmap, que escanea desde afuera.

### Flags -tulpn

| Flag | Significado |
|------|-------------|
| -t   | TCP |
| -u   | UDP |
| -l   | Listening (solo puertos escuchando) |
| -p   | Program (que programa usa cada puerto) |
| -n   | Numeric (muestra IPs y puertos como numeros) |

Requiere sudo para ver el nombre del programa.
Sin sudo la columna PID/Program name muestra guión.

### Lo que encontre en mi sistema

| Puerto | Protocolo | Servicio | Escucha en |
|--------|-----------|----------|------------|
| 1883   | TCP/UDP   | Mosquitto (broker MQTT) | 127.0.0.1 |
| 40585  | TCP       | containerd | 127.0.0.1 |
| 53     | TCP/UDP   | systemd-resolved (DNS) | 127.0.0.1 |
| 1883   | TCP       | Mosquitto | IPv6 local |

### Mosquitto

Broker MQTT, protocolo de mensajeria para dispositivos IoT
e industriales. Estaba instalado del curso de ciberseguridad
industrial (honeypot). Quedó corriendo como servicio en
segundo plano sin que lo supiera.

Puerto estandar MQTT: 1883

### Diferencia 127.0.0.1 vs 0.0.0.0

- 127.0.0.1: solo acepta conexiones desde la misma maquina
- 0.0.0.0: acepta conexiones desde cualquier interfaz,
  incluyendo red local e internet

Un servicio en 0.0.0.0 sin firewall que lo filtre
es un vector de ataque potencial.

### Relevancia en ciberseguridad

Al entrar a un sistema, listar puertos activos con netstat
o ss es uno de los primeros pasos. Permite identificar
que servicios estan corriendo, cuales estan expuestos
y cuales son vectores potenciales de ataque.

### Dudas pendientes
-
