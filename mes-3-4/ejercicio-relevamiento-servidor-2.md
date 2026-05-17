## Ejercicio — Relevamiento de servidor Linux
**Fecha:** [17/05/26]

---

### Consigna

Servidor Linux con comportamiento lento. Identificar
que esta consumiendo recursos y reportar si hay algo
que no deberia estar corriendo.

---

### Proceso realizado

**Memoria:**

ps aux --sort=-%mem | head -5

Proceso que mas memoria consume: PID 738, docker, 4.3%

**CPU:**

ps aux --sort=-%cpu | head -5

Proceso que mas CPU consume: PID 133 (comando ejecutado),
comportamiento normal y esperado.

**Servicios activos:**

systemctl list-units --type=service --state=running | wc -l

29 servicios en ejecución. Se revisaron y todos eran
esperables: docker, containerd, systemd-resolved.
No se identificó ningun servicio anomalo.

**Puertos:**

sudo netstat -tulpn

sudo ss -tulpn

Puertos activos identificados:

- 127.0.x.x:x  systemd-resolved

- 127.0.x.x:x  containerd

- 10.0.x.x:x  systemd-network (IP de VirtualBox)

Todos en loopback o interfaz interna.

Ningun puerto expuesto en 0.0.0.0.

---

### Reporte final

Realizado el relevamiento se recomienda mantener control
diario y analisis de comportamiento del sistema por eventual
alentamiento de funcionamiento, ya que:

- Los puertos identificados estan en loopback, solo aceptan
  conexiones desde la misma maquina.
- Los servicios identificados eran esperables, no se registro
  la ejecucion de ningun servicio extrano.
- Con las herramientas disponibles no se identifico la causa
  del lentamiento. Se recomienda monitoreo continuo y revision
  de logs del sistema.

---

### Lo que aprendí en este ejercicio

- ps aux da una instantanea puntual, no historial.
  No permite determinar que paso en el pasado.
- Un reporte profesional nombra lo que encontro,
  no solo lo que no encontro.
- La honestidad sobre el límite de las herramientas
  es mas profesional que inventar una causa.
- loopback (127.0.0.1): solo acepta conexiones desde
  la misma maquina, no es accesible desde el exterior.

---

### Errores cometidos en el ejercicio

- Tipografico: ps aus en lugar de ps aux
- Tipografico: ] head en lugar de | head
- Conclusion inicial generica sin responder
  la pregunta original del jefe
- Reporte de puertos incompleto: decia que no habia
  nada expuesto sin nombrar lo que estaba corriendo

### Dudas pendientes
-
