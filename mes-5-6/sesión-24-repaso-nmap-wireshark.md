## Sesión 24 — Repaso integrador: Nmap + Wireshark
**Fecha:** 08/09/2026

---

## Objetivo

Repaso general del Mes 5-6 combinando Nmap y Wireshark
sobre la propia VM como práctica de cierre.

---

## Escaneo con Nmap

```bash
systemctl start apache2 ssh
nmap -p 1-1024 -sV 127.0.0.1
```

**Resultado:**
- Puerto 22: SSH — OpenSSH 10.2p1
- Puerto 80: HTTP — Apache httpd 2.4.66
- 1022 puertos cerrados con RST

---

## Análisis con Wireshark — 7 pasos

**Paso 1 — Volumen y protocolos:**
2140 paquetes capturados.
Protocolos: IPv4, TCP, SSH, HTTP.

**Paso 2 — DNS:**
Sin tráfico DNS. El escaneo fue directo a IP, sin resolución de nombres.

**Paso 3 — Errores DNS:**
Sin errores DNS.

**Paso 4 — HTTP:**
16 paquetes HTTP. Nmap usó requests GET para detectar el servicio.
El flag -sV funciona enviando requests HTTP reales al servidor para identificar la versión.

**Paso 5 — Conexiones rechazadas:**
1024 paquetes RST. Corresponden a los 1022 puertos cerrados más
los intercambios de apertura y cierre en los puertos abiertos.

**Paso 6 — Puertos inusuales:**
Filtro: tcp.port != 80 && tcp.port != 443 && tcp.flags.reset != 1
El puerto 22 apareció con intercambio real, confirmando que está abierto.
Sin anomalías detectadas.

**Paso 7 — Conclusión:**
No se detectaron anomalías en la captura analizada.
Los servicios activos corresponden a los esperados: SSH en puerto 22 y HTTP en puerto 80.

---

## Filtros usados

http → tráfico HTTP
dns → consultas DNS
dns.flags.rcode != 0 → errores DNS
tcp.flags.reset == 1 → conexiones rechazadas
tcp.port != 80 && tcp.port != 443 → puertos inusuales
tcp.port != 80 && tcp.port != 443 && tcp.flags.reset != 1 → intercambio real sin RST

---

## Lección clave

Sin tráfico DNS en un escaneo a localhost porque Nmap va directo a la IP.
Los 1022 puertos cerrados generan RST, que es el comportamiento esperado.
Para identificar puertos con intercambio real excluir los RST con tcp.flags.reset != 1.
