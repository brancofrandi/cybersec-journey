## Unidad 1 — Introduccion a redes
**Fecha:** [29/03/2026]
---

### Conceptos de comunicacion

**Protocolo**
Conjunto de reglas para que dos dispositivos se comuniquen.
Dos dispositivos directamente conectados deben usar
el mismo protocolo.

**Tipos de senales**
- Analógica: valores continuos, infinitos valores posibles
- Digital: valores discretos, binario (0 y 1)

Las computadoras usan internamente codificacion binaria.
Las senales analógicas se digitalizan para transmitirlas.

**Comunicación serie vs paralelo**
- Serie: bits van uno detras del otro (un cable TX, uno RX)
- Paralelo: varios bits al mismo tiempo en cables separados

---

### Medios de transmision guiados

| Cable | Característica principal |
|-------|-------------------------|
| UTP   | Par trenzado sin blindaje, hasta 100 metros |
| STP   | Par trenzado con blindaje por par |
| FTP   | Apantallamiento global, lugares con interferencia |
| Coaxial | Nucleo de cobre, usado en TV y redes antiguas |
| Fibra optica | Luz, larga distancia, alta velocidad, estandar actual |

---

### Medios de transmision no guiados

WiFi (2.4 y 5.8 GHz), telefonía móvil (3G/4G/5G),
Bluetooth (corto alcance), radioenlaces, satelital.

---

### Tipos de red por cobertura

| Tipo | Alcance | Caracteristica |
|------|---------|----------------|
| LAN  | Edificio | Alta velocidad, bajo costo |
| MAN  | Ciudad   | LANs vinculadas por fibra |
| WAN  | Paises   | Multiples proveedores |

---

### Flujo de datos — modelos

**Cliente-Servidor**
Roles diferenciados: el cliente solicita, el servidor responde.
Flujo jerarquico y controlado.
Ejemplo: navegador pidiendo una pagina web.

**Punto a punto (P2P)**
Todos los dispositivos tienen el mismo rol.
Flujo distribuido y dinamico.
Ejemplo: BitTorrent.

| Caracteristica | Cliente-Servidor | P2P |
|----------------|-----------------|-----|
| Roles          | Diferenciados   | Iguales |
| Flujo          | Centralizado    | Distribuido |
| Escalabilidad  | Limitada        | Alta |

---

### Tipos de flujo segun direccion

- Simplex: una sola direccion
- Half-duplex: ambas direcciones, no simultaneo
- Full-duplex: ambas direcciones al mismo tiempo

---

### Lo que ya conocia antes de la clase
- Protocolos TCP y UDP
- Diferencia bit vs byte
- Redes LAN y concepto de gateway

### Lo que fue nuevo
- Tipos de cables y sus caracteristicas tecnicas
- Senales analogicas vs digitales en detalle
- Modelos cliente-servidor y P2P formalmente
- Tipos de flujo simplex, half-duplex, full-duplex
- Medios de transmision no guiados en detalle
