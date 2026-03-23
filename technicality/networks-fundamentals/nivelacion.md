# Tecnicatura en Seguridad Informatica

## Clases de Nivelacion en Redes

---

## Clase 01

### Temas vistos

**IP publica vs IP privada**
- IP publica: identifica un dispositivo en Internet, asignada
  por el ISP, unica a nivel mundial.
- IP privada: se usa dentro de redes locales (LAN), puede
  repetirse en distintas redes, no accesible desde Internet.

**NAT (Network Address Translation)**
Traduce IPs privadas a una IP publica. Permite que muchos
dispositivos compartan una sola IP publica. Lo gestiona el router.

**DNS (Domain Name System)**
Traduce nombres de dominio a direcciones IP.
En vez de recordar 104.26.10.229 escribimos tryhackme.com.

**Mascara de subred**
Separa una IP en dos partes: red y host.
255.255.255.0 = /24 , primeros 24 bits son la red.
Ejemplo: 192.168.1 es la red, .10 es el dispositivo.

**TCP vs UDP**

| Caracteristica      | TCP              | UDP               |
|---------------------|------------------|-------------------|
| Conexion            | Orientado        | Sin conexion      |
| Entrega garantizada | Si               | No                |
| Orden               | Secuencial       | No garantizado    |
| Uso tipico          | Web, archivos    | Streaming, juegos |

**DHCP (Dynamic Host Configuration Protocol)**
Asigna IPs automaticamente a cada dispositivo que se conecta
a la red. Evita configurar IPs manualmente.

**Ruteo**
Proceso de determinar el camino que siguen los paquetes desde
el origen al destino. Lo realizan los routers usando tablas
de ruteo.

**Protocolos**
Conjunto de reglas que determinan como se comunican los
dispositivos. IP no garantiza entrega, TCP si.

---

## Clase 02

### Temas vistos

**Tipos de sistemas operativos**
- Proposito general: disenados para multiples usos.
  Ejemplos: Windows, Linux, MacOS.
- RTOS (Real Time Operating System): responde en tiempos
  exactos y garantizados. Se usa en equipos medicos,
  maquinaria industrial, controladores de vuelo.
  Relevante para ciberseguridad industrial (OT security).

**CLI vs GUI**
Linux usa CLI como lenguaje nativo. En ciberseguridad se
prefiere CLI porque es mas directo, rapido y efectivo.
La mayoria de herramientas corren desde terminal.

### Practica en Packet Tracer

Se configuro una red conectando dispositivos mediante
switching al router. Se asignaron IPs a cada dispositivo
y se verifico la comunicacion entre ellos.

**Comandos usados:**

| Comando | Que hace |
|---------|----------|
| `Router> enable` | Entra en modo privilegiado para configuraciones avanzadas |
| `Router# configure terminal` | Entra en modo de configuracion global |
| `Router(config)# interface gigabit0/0` | Accede a la interfaz para configurarla |
| `Router(config-ip)# ip address 192.168.x.x 255.255.255.0` | Asigna IP y mascara a la interfaz |
| `Router(config-ip)# no shutdown` | Activa la interfaz para que transmita trafico |
| `Router(config-ip)# exit` | Sale del modo de interfaz |
| `Router(config)# end` | Vuelve al modo privilegiado |
| `Router# copy running-config startup-config` | Guarda la config para que persista al reiniciar |

### Practica con Maquina Virtual

Se abrio Ubuntu Server en VirtualBox, se descargo la imagen
ISO y se ejecuto el sistema.

---

### Lo que ya conocia antes de las clases

- IP publica vs privada, DNS, mascara de subred, DHCP, gateway
- Diferencia entre ping por IP y ping por nombre
- VirtualBox y maquinas virtuales

### Lo que fue nuevo

- TCP vs UDP en detalle
- Packet Tracer como herramienta de simulacion de redes
- Comandos de configuracion de router en CLI
- Concepto de ruteo y tablas de ruteo
- RTOS y su relevancia en seguridad industrial

### Dudas pendientes

-
