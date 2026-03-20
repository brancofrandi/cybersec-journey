# Comandos de red - Mes 1-2
Explicaciones con mis propias palabras.
---
## ipconfig

**Qué hace:** Muestra la configuracion de red de mi PC.
Para ver mas detalle, incluyendo el DNS, usar ipconfig /all.

**Cómo usarlo:**
```
ipconfig
```
**Lo que aprendí:**
- IPv4: tu dirección dentro de la red local
- Gateway: la IP de tu router, primer salto a Internet
- DNS: traduce nombres (google.com) a IPs numericas
  
**Lo que entendí con mis palabras:**

- Mi IP empieza con 192.168 porque los primeros dos octetos
  identifican que es una IP privada. Este rango esta reservado
  para redes hogarenas.

- El gateway es el dispositivo que conecta la red local con otra
  red. En mi casa es el router. Aparece en CMD como "Puerta de
  enlace predeterminada".

- Para ver mi servidor DNS tuve que ejecutar ipconfig /all.
  No aparece en ipconfig simple.

- No tengo ninguna interfaz con IP 169.254.x.x, lo cual es normal.
  Esa IP aparece cuando la PC no puede obtener una del router.

- Mi mascara de subred es 255.255.255.0. Los primeros tres octetos
  identifican la red y el ultimo identifica el dispositivo.
  (Nota: no son 256 dispositivos disponibles sino 253, porque
  el .0, el .1 del gateway y el .255 de broadcast no se pueden usar)
  
**Lo que observé en mi red:** 

  Tengo 4 interfaces de red. La que uso para Internet es el WiFi.
  La interfaz con IP 192.168.56.x no es de Internet, es de
  VirtualBox, una red virtual sin gateway.
  
---
## ping

**Que hace:** Envia paquetes a una direccion y mide si llegan
y en cuanto tiempo. Verifica conectividad y estabilidad
de la conexion.

**Como usarlo:**
- ping google.com      → prueba conexion + DNS
- ping 8.8.8.8         → prueba conexion IP directa, sin DNS
- ping 192.168.x.1     → prueba conexion con el gateway

**Lo que entendi:**

- Mide el tiempo de traslado de paquetes entre dos dispositivos.
  No son archivos, son paquetes.

- La diferencia entre los dos usos:
  ping por nombre (google.com) primero resuelve el DNS,
  ping por IP (8.8.8.8) va directo sin ese paso.

- Si los 4 paquetes enviados vuelven sin perdida,
  la conexion esta estable.

- TTL (Time To Live): contador que decrementa en 1 en cada
  router que atraviesa el paquete. Cuando llega a 0 el paquete
  se descarta. Evita que los paquetes circulen en bucles
  infinitamente.

**Tabla de diagnostico:**

| Situacion           | ping 8.8.8.8 | ping google.com | Problema    |
|---------------------|--------------|-----------------|-------------|
| Todo bien           | OK           | OK              | Ninguno     |
| DNS caido           | OK           | Falla           | DNS         |
| Sin internet        | Falla        | Falla           | Red/router  |

**Comandos relacionados si hay problema de DNS:**
- ipconfig /flushdns   → limpia la cache DNS
- ipconfig /release    → libera IP
- ipconfig /renew      → pide IP nueva al router
  
---
## tracer
**Que hace:** Muestra el camino salto a salto hasta el destino.
**Lo que observe:** (escribi aca)
---
## nslookup
**Que hace:** Consulta DNS para resolver un dominio a IP.
**Lo que observe:** (escribi aca)
