## Sesión 10 — Wireshark: análisis de HTTP en profundidad
**Fecha:** 30/07/2026

---

## Ejercicio practicado

Captura de tráfico HTTP con `curl http://httpbin.org/get`
usando la interfaz **any** en Wireshark.

Total de paquetes capturados: 18
Paquetes HTTP visibles con filtro: 2 (request y response)

---

## Análisis del paquete HTTP Request

GET /get HTTP/1.1

Host: httpbin.org

User-Agent: curl/8.18.0

Accept: /

**Desglose:**

**`GET /get HTTP/1.1`** tiene tres partes:
- `GET` → método HTTP. Pide un recurso sin modificar nada.
- `/get` → ruta del recurso dentro del servidor.
- `HTTP/1.1` → versión del protocolo.

**Métodos HTTP principales:**
- GET → pedir un recurso
- POST → enviar datos al servidor
- PUT → actualizar un recurso
- DELETE → borrar un recurso

**`Host: httpbin.org`** → servidor al que se conecta.
**`User-Agent: curl/8.18.0`** → programa que hizo la solicitud.

---

## Análisis del paquete HTTP Response

HTTP/1.1 200 OK

Content-Type: application/json

**Códigos de respuesta HTTP:**
- 200 → éxito
- 404 → no encontrado
- 403 → prohibido
- 500 → error del servidor

**`application/json`** → el servidor respondió con datos JSON,
no HTML. httpbin.org es una herramienta de prueba que devuelve
información sobre la solicitud recibida.

---

## Datos revelados por httpbin.org

La sección **origin** del JSON mostró:

String value: x.x.x.x

Esa es la IP pública desde donde se hizo la solicitud.
El servidor sabe exactamente desde dónde te conectás.
En HTTP plano esa información viaja sin cifrar.

---

## Panel hexadecimal de Wireshark

Muestra el contenido del paquete en dos formatos:
- **Izquierda:** datos en hexadecimal (base 16)
- **Derecha:** mismos datos en ASCII (texto legible)

Al seleccionar un campo en el panel de capas, se resalta
la parte correspondiente en el hexadecimal.
Útil para analizar paquetes malformados o tráfico anómalo.

---

## HTTP vs HTTPS en seguridad

**HTTP plano:**
- Contenido visible en texto plano
- IP de origen visible en el payload
- Cualquier persona en la misma red puede leerlo con Wireshark

**HTTPS:**
- Contenido cifrado con TLS
- No se puede leer sin la clave privada
- Aparece como TLSv1.2 en Wireshark

**En redes WiFi públicas:**
Un atacante con Wireshark puede capturar todo el tráfico HTTP
de los dispositivos conectados: IPs, solicitudes y credenciales.
Usar solo sitios con HTTPS (candado en el navegador).

---

## Cómo proteger la IP de origen

- **HTTPS:** cifra el contenido pero la IP sigue visible en la capa de red
- **VPN:** el servidor destino ve la IP de la VPN, no la real
- **Para uso cotidiano:** verificar siempre el candado HTTPS
  en sitios donde ingresás datos

---

## Comandos usados

```bash
curl http://httpbin.org/get    # genera tráfico HTTP con respuesta JSON
```

**Filtros de Wireshark usados:**

http → solo paquetes HTTP

dns → solo consultas DNS

tcp → solo tráfico TCP
