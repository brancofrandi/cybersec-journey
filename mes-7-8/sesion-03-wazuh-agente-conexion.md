Mes 7-8 (SIEM) — Semana 2: Conectar un agente Wazuh

Fecha: 19/09/2026

Objetivo

Plan original: conectar Metasploitable2 a Wazuh como agente monitoreado. El resultado real se desvía del plan por una razón técnica documentada (ver abajo) — el objetivo de fondo (entender y ejecutar el flujo completo de registro de un agente contra un manager Wazuh) se cumplió igual.

Entorno
Manager: Kali Linux 2026.1, Wazuh 4.7.5, 4096 MB RAM
Objetivo original: Metasploitable2 (Ubuntu 8.04, i386), 772 MB RAM
Agente final: Ubuntu Server 26.04.1 LTS (amd64), 1024 MB RAM, hostname wazuh-agent
Máquina host: 7.6 GB RAM total (Intel i3-N305) — presupuesto ajustado, las VMs corren de a dos, no de a tres
Red: VirtualBox Internal Network, nombre metalab, subred 192.168.100.0/24
Kali: 192.168.100.10 (eth1, segundo adaptador; el adaptador 1 se deja en NAT para salida a internet)
Metasploitable2: 192.168.100.101 (eth0)
VM agente Ubuntu: 192.168.100.102 (enp0s3)
Problema 1 — Los adaptadores de red parecían deshabilitados

ip addr en ambas VMs mostraba solo lo, ninguna interfaz de red real. La primera lectura del checkbox "Habilitar adaptador de red" de VirtualBox como destildado fue incorrecta — el panel se ve en gris simplemente porque la VM está prendida (la configuración se bloquea mientras corre, no significa que esté deshabilitado). La causa real no tenía que ver con ese checkbox; las interfaces aparecieron correctamente una vez que ambas VMs se apagaron por completo y se reiniciaron después de agregar el adaptador de Red interna.

Problema 2 — Sin DHCP en Red interna

El modo Internal Network de VirtualBox no ofrece servicio DHCP (a diferencia de NAT Network). Las IPs estáticas se asignaron a mano:

Kali / Metasploitable2 (nombres de interfaz viejos, herramienta ip): sudo ip address add <ip>/24 dev <interfaz> — efímero, no sobrevive un reinicio. Se confirmó de la peor manera: después de reiniciar Kali, eth1 perdió su IP y se cortó la conectividad hasta reasignarla.
VM Ubuntu 26.04 (nombres de interfaz predecibles, enp0s3): configuración persistente vía /etc/netplan/00-installer-config.yaml, dhcp4: false + bloque addresses: estático, aplicado con sudo netplan apply.

La IP estática de Kali todavía no es persistente (usa NetworkManager, no netplan) — pendiente, hay que resolverlo con una configuración persistente basada en NetworkManager (nmcli) antes de la próxima sesión.

Problema 3 — Metasploitable2 no puede correr un agente Wazuh moderno

Metasploitable2 corre Ubuntu 8.04 (i386). Wazuh ya no distribuye paquetes i386 para la línea de agentes 4.x (confirmado contra el repositorio oficial de paquetes); aunque existiera el paquete, es poco probable que un agente tan reciente corra sobre un glibc/kernel del 2008. Es una limitación de la plataforma, no un error de configuración.

Decisión: se armó una VM nueva y liviana, Ubuntu Server 26.04 LTS (wazuh-agent), exclusivamente para cumplir el objetivo de esta semana. Metasploitable2 queda reservada para lo que sí puede hacer: ser escaneada (Semana 3, Nessus) y ser atacada (Semana 6).

Instalación y registro del agente

La VM agente no tiene salida a internet (un solo adaptador en Red interna, a propósito). Transferencia del paquete por la red interna:

# En Kali (tiene NAT + internet), parado en la carpeta con el .deb:
python3 -m http.server 8000

# En la VM agente:
wget http://192.168.100.10:8000/wazuh-agent_4.7.5-1_amd64.deb
sudo dpkg -i wazuh-agent_4.7.5-1_amd64.deb

Dirección del manager configurada en /var/ossec/etc/ossec.conf (<address>192.168.100.10</address> dentro de <client><server>), y luego registro contra el authd del manager (puerto 1515):

sudo /var/ossec/bin/agent-auth -m 192.168.100.10
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
Verificación
sudo /var/ossec/bin/agent_control -l en Kali: agente wazuh-agent listado como Active.
Dashboard de Wazuh → Agentes: 1 total, 1 activo, 0 desconectados — verificado cruzado contra la CLI.
Dashboard → Security events: alertas reales entrando desde wazuh-agent (niveles 7 y 3), no solo heartbeat. Categoría MITRE ATT&CK más frecuente: familia T1562 ("Disable or Modify Tools") — todavía sin investigar, queda pendiente para una próxima sesión confirmar si es un falso positivo de actividad rutinaria de servicios durante la instalación, o algo que valga la pena investigar en serio.
Pendientes para la próxima sesión
Hacer persistente la IP estática de Kali vía NetworkManager (nmcli), no solo ip address add.
Investigar el origen de la alerta T1562 en Security Events.
Semana 3: Nessus Essentials, escaneando Metasploitable2 (no necesita agente para esto).

Ve el progreso de las tareas más largas.

sesion-03-wazuh-agente-conexion.md
notas-semana2-siem-wazuh-agente.md
week02-wazuh-agent-log.md

Rastrea las herramientas y los archivos referenciados utilizados en esta tarea.
