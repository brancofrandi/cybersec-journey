## Wazuh — Conceptos básicos
**Fecha:** 13/09/2026

---

## ¿Qué es Wazuh?

SIEM (Security Information and Event Management) open source.
Recopila logs de múltiples fuentes, correlaciona eventos,
detecta amenazas y genera alertas en tiempo real.
Es la herramienta que usa un analista SOC todos los días.

---

## Los tres componentes

**Wazuh Indexer:**
Almacena todos los eventos y logs que llegan al SIEM.
Base de datos basada en OpenSearch.
Guarda la información para que se pueda buscar y analizar.

**Wazuh Server:**
El cerebro del sistema.
Recibe los logs de los agentes instalados en los equipos.
Aplica las reglas de detección.
Genera alertas cuando encuentra algo sospechoso.

**Wazuh Dashboard:**
Interfaz gráfica que se abre en el navegador.
Muestra eventos, alertas y métricas en tiempo real.
Lo que ve un analista SOC todos los días.

---

## ¿Qué es un agente?

Software que se instala en cada equipo a monitorear.
Recopila los logs del equipo y los envía al Wazuh Server.
Sin agentes, Wazuh no tiene datos que analizar.

---

## Flujo completo

Equipo monitoreado
↓
Wazuh Agent (recopila logs)
↓
Wazuh Server (analiza y aplica reglas)
↓
Wazuh Indexer (almacena)
↓
Wazuh Dashboard (muestra al analista)


---

## Secciones del dashboard

- **Security Alerts:** alertas de seguridad en tiempo real
- **Integrity Monitoring:** cambios en archivos (permisos, contenido)
- **Vulnerability Detection:** vulnerabilidades conocidas en los equipos
- **MITRE ATT&CK:** eventos mapeados al framework de ataques
- **Compliance:** GDPR, HIPAA, NIST, PCI DSS

---

## Instalación en Kali

```bash
# Descargar instalador
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

# Dar permisos de ejecución
chmod u+x wazuh-install.sh

# Instalar todos los componentes ignorando chequeo de OS
sudo bash wazuh-install.sh -a -i

# Verificar estado
sudo systemctl status wazuh-manager

# Reiniciar si es necesario
sudo systemctl restart wazuh-manager
```

**Acceso al dashboard:** https://127.0.0.1
El certificado es self-signed: aceptar el riesgo en Firefox.

---

## Requisitos de hardware

Mínimo recomendado: 4 GB de RAM.
Con 2 GB el sistema arranca lento y puede fallar.
