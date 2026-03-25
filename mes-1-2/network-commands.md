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
## tracert

**Que hace:** Muestra el camino salto a salto hasta el destino.

## Analisis de tracert

**Fecha:** [24/03/2026]

---

### Que es tracert

Comando para diagnosticar redes mostrando la ruta salto a salto
y la latencia hacia un destino. Permite identificar donde se
pierde la conexion o donde hay cuellos de botella.

Sintaxis: `tracert <destino>`
En Linux el equivalente es: `traceroute <destino>`

**Campos del resultado:**
hop | RTT1 | RTT2 | RTT3 | Nombre del host

---

### Analisis de mis resultados

**tracert google.com — 10 saltos**
- Salto 1: gateway, red local (192.168.x.1)
- Saltos 2-3: asteriscos, routers del proveedor con firewall
- Salto 4: primera IP visible del proveedor (telecom.net.ar)
- Saltos 5: asterisco
- Saltos 6-10: ruta hasta google.com

**tracert tryhackme.com — 8 saltos**
- Salto 1: gateway, red local
- Saltos 2-3: asteriscos
- Salto 4: primera IP visible del proveedor
- Saltos 5-7: ruta intermedia
- Salto 8: IP de destino (tryhackme.com)

**Conclusion:** tryhackme.com llego en menos saltos que google.com.
Diferencia de 2 saltos, probablemente por infraestructura
de red distinta entre ambos destinos.

---

### Lo que aprendi

**Sobre los asteriscos:**
No siempre indican problema. Pueden ser:
- Firewall bloqueando el comando
- Router configurado para no responder ICMP
- Perdida real de paquetes (si aparecen al final)

**Sobre los tiempos:**
- Latencia progresiva alta: posible problema a partir de ese salto
- Latencia alta en el medio: puede ser normal, baja prioridad
- Latencia alta al comienzo: problema en red local o ISP

**Donde sale el trafico de mi red local:**
- Salto 1: gateway, todavia en mi red
- Salto 2: ya salio de mi red, aunque no responda

**Relevancia en ciberseguridad:**
- Muestra IPs de routers intermedios, posibles vectores de ataque
- Permite identificar puntos de congestion
- Util para mapear infraestructura de red de un objetivo

**Comando para guardar resultado en archivo:**
```
tracert <destino> > <ruta>\<nombre>.txt
```
**Flag util:**
```
tracert -d <destino
```
El -d acelera el comando sin convertir IPs en nombres de host

---
## nslookup
**Que hace:** Consulta DNS para resolver un dominio a IP.
**Lo que observe:** (escribi aca)
