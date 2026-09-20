Mes 7-8 (SIEM) — Sesión 04: Persistencia de red y triage de alertas MITRE ATT&CK

Fecha: 20/09/2026 Cierre: Semana 2 completa (objetivo técnico + bloque de fricción + investigación pendiente)

Objetivo de la sesión

Cerrar los pendientes que quedaron abiertos al final de la Semana 2: hacer persistente la IP estática de Kali, e investigar la alerta MITRE ATT&CK T1562 vista en el dashboard de Wazuh.

Comandos nuevos
Comando	Qué hace
nmcli connection show	Lista las conexiones que NetworkManager conoce, con su nombre, UUID, tipo y dispositivo asociado
sudo nmcli connection modify <nombre> ipv4.addresses <ip>/<mascara> ipv4.method manual	Modifica un perfil de conexión existente para que use una IP estática en vez de DHCP — el cambio queda escrito en disco (/etc/NetworkManager/system-connections/), persiste entre reinicios
sudo nmcli connection up <nombre>	Reactiva una conexión para aplicar los cambios sin esperar a un reinicio
sudo shutdown now	Apagado completo del sistema — usado como prueba real de persistencia (distinto de cerrar sesión o guardar el estado en VirtualBox)
Dudas resueltas

¿Por qué se perdió la IP de Kali al reiniciar, si en Ubuntu no pasó lo mismo?

No tiene que ver con cerrar sesión de usuario ni con cerrar una terminal — eso nunca afecta la configuración de red. ip addr add solo modifica el estado en memoria (RAM) del kernel; no escribe nada en disco. Un reinicio completo del sistema operativo reconstruye la red leyendo únicamente los archivos de configuración persistentes, y como esa IP nunca quedó guardada en ninguno, desaparece.

En Ubuntu no pasó porque ahí la IP se configuró desde el principio con netplan (/etc/netplan/00-installer-config.yaml), que sí escribe en disco. En Kali, que usa NetworkManager en vez de netplan, la forma de lograr lo mismo es con nmcli connection modify + ipv4.method manual, que guarda el perfil en /etc/NetworkManager/system-connections/.

Corrección de un error conceptual propio: cambiar la configuración de red de VirtualBox (modo NAT, Red interna, etc.) no tiene nada que ver con la persistencia de la IP dentro del sistema operativo invitado. VirtualBox controla a qué segmento de red está conectada la placa virtual — el "cable". La IP que tiene esa placa por dentro la decide el sistema operativo invitado con su propio mecanismo (netplan o NetworkManager, según el caso). Son dos capas distintas.

Verificación real: apagado completo de Kali (sudo shutdown now, no guardado de estado) y arranque de nuevo — la IP 192.168.100.10/24 en eth1 seguía ahí sin necesidad de reasignarla. Persistencia confirmada con la prueba que corresponde, no solo asumida.

MITRE ATT&CK — qué es y por qué apareció

MITRE ATT&CK es una base de conocimiento pública que cataloga tácticas y técnicas reales de atacantes, documentadas a partir de incidentes observados. Se organiza en tácticas (el objetivo del atacante en cada etapa — Acceso Inicial, Persistencia, Evasión de Defensas, etc., una versión más granular del Cyber Kill Chain) y técnicas (el método concreto, cada una con un ID como T1562). Los SIEM como Wazuh etiquetan sus reglas de detección con estas técnicas como metadato, dando un vocabulario común independiente del vendor.

Triage de la alerta T1562

La categoría que aparecía en el dashboard ("Disable or Modify Tools") es la clasificación MITRE de la regla, no el nombre del evento en sí — ese está en el campo rule.description de cada alerta individual, visible al bajar del gráfico agregado a la tabla de eventos.

Alerta encontrada: "Wazuh agent disconnected", nivel 3, agente wazuh-agent, tactic Defense Evasion (T1562). Wazuh clasifica esta regla bajo Evasión de Defensas porque, en teoría, un agente que se desconecta podría ser un atacante apagando la visibilidad del SIEM sobre esa máquina.

Conclusión del triage: falso positivo benigno. La desconexión la generó el propio apagado de la VM de Ubuntu durante las pruebas de persistencia de red de esta sesión — no una amenaza real. El criterio que resuelve el triage es correlacionar la alerta con el contexto (en este caso, un cambio conocido y esperado). Es el mismo ejercicio de decisión True Positive vs False Positive que hace un analista SOC a diario.

Cierre de Semana 2

Con la persistencia de red resuelta y verificada, y la alerta T1562 investigada y explicada, la Semana 2 (Mes 7-8: SIEM) queda cerrada. Próxima sesión: Semana 3, Nessus Essentials.
