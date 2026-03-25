## Introductory Networking
**Fecha:** [24/03/2025]
**Plataforma:** TryHackMe

---

### Temas vistos

**Modelo OSI - 7 capas**
Modelo estandarizado para explicar teoria de redes.
En la practica se usa TCP/IP, pero OSI es mas util para aprender.

| Capa | Nombre | Funcion |
|------|--------|---------|
| 7 | Application | Interfaz entre programas y la red |
| 6 | Presentation | Traduce, cifra y comprime datos |
| 5 | Session | Establece y mantiene la conexion |
| 4 | Transport | Elige TCP o UDP, divide los datos |
| 3 | Network | Direccionamiento logico, IP, ruteo |
| 2 | Data Link | Direccionamiento fisico, MAC |
| 1 | Physical | Pulsos electricos, bits |

**Encapsulacion**
Cada capa agrega un encabezado al dato al enviarlo.
Cada capa elimina ese encabezado al recibirlo (de-encapsulacion).
Los datos cambian de nombre segun la capa:
- Capas 7-5: data
- Capa 4: segment (TCP) o datagram (UDP)
- Capa 3: packet
- Capa 2: frame
- Capa 1: bits

**Modelo TCP/IP**
Cuatro capas: Application, Transport, Internet, Network Interface.
Es el modelo real usado en redes modernas.
OSI se usa para aprender, TCP/IP para implementar.

**Three-way handshake**
Proceso para establecer una conexion TCP antes de enviar datos:
1. SYN: la computadora envia solicitud de conexion
2. SYN-ACK: el servidor responde confirmando
3. ACK: la computadora confirma, conexion establecida
Sin este proceso no hay transmision TCP.

**ping**
Usa protocolo ICMP, opera en capa de red (OSI capa 3).
Sirve para verificar conectividad y obtener la IP de un dominio.

**traceroute**
Mapea la ruta salto a salto hasta el destino.
Linux usa UDP por defecto, Windows (tracert) usa ICMP.

**whois**
Consulta informacion sobre el registro de un dominio:
propietario, registrar, fechas de vencimiento, nameservers.
En Europa los datos personales estan ocultos por privacidad.

**dig**
Consulta servidores DNS recursivos para obtener info de dominios.
Muestra el TTL del registro DNS, que indica cuanto tiempo
la computadora guarda ese resultado en cache antes de
volver a consultarlo. TTL en DNS se mide en segundos.

**Flujo DNS completo cuando escribis un dominio:**
1. Revisa el archivo Hosts local
2. Revisa cache DNS local
3. Consulta servidor DNS recursivo (del ISP o publico)
4. El recursivo consulta un Root Name Server
5. Root redirige a servidor TLD (.com, .org, etc)
6. TLD redirige a Authoritative Name Server del dominio
7. Authoritative devuelve la IP
8. La computadora se conecta a esa IP

---

### Lo que fue nuevo
- Modelo OSI completo y sus 7 capas
- Concepto de encapsulacion y de-encapsulacion
- Three-way handshake de TCP
- Herramientas whois y dig
- Flujo completo de resolucion DNS

### Lo que ya conocia
- ping y traceroute, los practique en sesiones anteriores
- DNS como concepto general
- Diferencia TCP vs UDP

### Dudas
Repasar las 7 capas de OSI
Repasar  three-way handshake y para qué sirve
### Dudas pendientes
- Memorizar las 7 capas del modelo OSI con sus funciones
- Entender mejor el three-way handshake en detalle
