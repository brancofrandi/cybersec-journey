# Observaciones de sesiones - Mes 1-2
---
## Sesion 1 - 19/03/2026

**Comandos que use:**
- ipconfig
- ipconfig /all
- ping google.com
- ping 8.8.8.8

**Lo que observe:**
- Mi red tiene 4 interfaces. Solo dos estaban activas.
- Una de las interfaces activas era de VirtualBox, no de Internet.
- El servidor DNS no es el mismo que el gateway en mi caso.
  Son servidores del proveedor asignados directamente.
- Para ver el DNS tuve que usar ipconfig /all, no aparece
  en ipconfig simple.
- No tenia ninguna IP 169.254.x.x, todo estaba bien.

**Algo que no sabia y aprendi hoy:**
- Que es el DNS y para que sirve en terminos practicos.
  En vez de recordar 104.26.10.229 escribimos tryhackme.com.
- Por qué mi IP empieza con 192.168 (rango privado reservado).
- La diferencia entre ping por nombre y ping por IP.
- Que la mascara 255.255.255.0 no permite 256 dispositivos
  sino 253, porque tres IPs estan reservadas.

**Pregunta que me queda:**
- Quién administra los rangos de IPs a nivel mundial
  y como se asignan en Argentina.
- Como funciona exactamente la mascara en binario
- Por que mi DNS no pasa por el router
---
## Sesion 2 - 20/03/2026
**Comandos que use:**
- ping google.com
- ping 8.8.8.8
- ping 192.168.x.1 (gateway)

**Lo que predije antes de ejecutar:**
- ping google.com: mostraria traslado de paquetes y verificaria
  conexion con google
- ping 8.8.8.8: lo mismo pero sin pasar por DNS, esperaba
  que fuera mas rapido
- ping gateway: no sabia bien para que servia, esperaba
  que tardara menos que los otros dos

**Lo que obtuve:**
- 4 paquetes enviados, 4 recibidos, sin perdidas en los tres casos
- Tiempos similares entre google.com y 8.8.8.8 (diferencia de 1-2ms)

**Lo que entendi solo:**
- La diferencia entre ping por nombre y ping por IP ya la tenia
  clara desde la sesion anterior, se confirmo
- TTL decrementa en 1 en cada router para evitar que los paquetes
  circulen infinitamente en bucles

**Lo que no sabia y tuve que investigar:**
- Por que difieren los ms entre 8.8.8.8 y google.com
  (tiene que ver con Anycast y que 8.8.8.8 conecta al nodo
  mas cercano fisicamente)
- Los valores TTL iniciales segun sistema operativo:
  Windows = 128, Linux/Mac = 64

**Lo que me queda pendiente para estudiar:**
- Como se diagnostica un problema de DNS paso a paso
- Que es exactamente el protocolo ICMP que usa ping
  
---
## Cierre de etapa — Mes 1-2
**Fecha:** [31/03-26]

---

### Lo que sabia al empezar

- Nociones basicas de redes del curso de Cisco
- VirtualBox instalado y Ubuntu configurado
- Algunos conceptos de ciberseguridad industrial sin profundidad

### Lo que se ahora

- Flujo DNS completo de memoria, paso a paso
- Comandos ipconfig, ping, tracert, nslookup, netstat, ss
- Diferencia entre puertos abiertos, cerrados y filtrados
- Puertos clave y que servicio corre en cada uno
- Modelo OSI completo con sus 7 capas
- Three-way handshake de TCP
- Encapsulacion y de-encapsulacion
- Como analizar mi propia red e interpretar cada resultado
- Como identificar servicios corriendo en mi sistema

### Lo mas importante de esta etapa

Pasar de ejecutar comandos a entender que significa
cada linea del resultado. La diferencia entre saber
que existe un comando y poder interpretar lo que devuelve.

Tambien descubrir que tenia Mosquitto corriendo en segundo
plano sin saberlo. Eso es exactamente lo que hace un
analista al revisar un sistema.

### Proximo foco

Mes 3-4: Linux como foco central.
OverTheWire Bandit cuando los comandos basicos esten solidos.
