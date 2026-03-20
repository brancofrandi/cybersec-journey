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
**Qué hace:** 
Prueba conectividad y mide tiempo de respuesta.
Mide el tiempo de traslado de paquetes entre dos
dispositivos y si la conexion es real o hay algun problema.

**Diferencia clave:**
- ping 8.8.8.8    -> prueba conectividad IP pura, va directo sin DNS
- ping google.com -> prueba IP + DNS, primero resuelve el nombre
  
**Lo que observé:**
- 8.8.8.8 es el servidor DNS publico de Google.
  1.1.1.1 es el de Cloudflare.

- ping google.com usa DNS para resolver el nombre a una IP
  antes de enviar los paquetes.
  ping 8.8.8.8 va directo, sin pasar por DNS.

- Si ping 8.8.8.8 funciona pero ping google.com falla,
  el problema es el DNS, no Internet.
  
---
## tracer
**Que hace:** Muestra el camino salto a salto hasta el destino.
**Lo que observe:** (escribi aca)
---
## nslookup
**Que hace:** Consulta DNS para resolver un dominio a IP.
**Lo que observe:** (escribi aca)
