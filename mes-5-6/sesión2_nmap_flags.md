## Sesión 02 — Flags principales de Nmap
**Fecha:** 19/07/2026

---

## Flags trabajados hoy

| Flag | Qué hace | Cuándo usarlo |
|------|----------|---------------|
| -p | Especifica puertos a escanear | Cuando sabés qué puerto buscar |
| -sV | Detecta versión del servicio | Reconocimiento discreto |
| -sC | Ejecuta scripts por defecto | Combinado con -sV para más detalle |
| -A | Escaneo agresivo completo | Cuando necesitás máximo detalle |

---

## Diferencia entre -sV y -A

**-sV devuelve:**
- Puerto y estado
- Nombre del servicio
- Versión del servicio
- CPE básico

**-A devuelve todo lo anterior más:**
- Tipo de dispositivo
- Sistema operativo y versión
- Distancia de red (hops)
- Scripts adicionales
- Traceroute

---

## Cuándo usar cada flag

- **-sV** → reconocimiento discreto, no querés levantar sospechas
- **-A** → necesitás máximo detalle, no importa ser detectado
- **-p** → cuando sabés qué puerto buscar o querés acotar el escaneo

En un pentest real: empezás discreto con -sV y usás -A
cuando ya tenés autorización y necesitás profundidad.

---

## Ejercicios ejecutados

```bash
nmap -p 22 127.0.0.1          # escaneo de un puerto específico
nmap -sV -p 22 127.0.0.1      # versión del servicio en puerto 22
nmap -A -p 22 127.0.0.1       # escaneo agresivo en puerto 22
nmap -A 127.0.0.1              # escaneo agresivo completo
nmap -p 1-100 127.0.0.1       # rango de puertos
```

---

## Resultado de nmap -A 127.0.0.1 interpretado

- **Host is up (0.00028s latency):** servidor activo, latencia insignificante
- **999 closed tcp ports:** puertos accesibles sin servicios activos
- **22/tcp open ssh OpenSSH 10.2p1:** único servicio activo, versión identificada
- **OS details: Linux 5.0 - 6.2:** sistema operativo detectado
- **Network Distance: 0 hops:** objetivo en la misma máquina, sin saltos de red

---

## Network Distance

Indica cuántos saltos de red hay entre el escáner y el objetivo.
- 0 hops → misma máquina
- 1 hop → mismo segmento de red
- Más hops → más routers intermedios

---

## Observaciones

- -sC solo ejecuta sripts cuando tiene información del servicio.
  Combinado con -sV es más efectivo.
- OSScan may be unreliable: aparece cuando Nmap no tiene
  suficientes puertos para detectar el SO con precisión.
- SSH apagado y deshabilitado al cerrar la sesión.
