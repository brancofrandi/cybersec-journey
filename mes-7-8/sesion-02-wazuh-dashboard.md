## Sesión 02 — Wazuh: exploración del dashboard y primeras alertas
**Fecha:** 17/09/2026

---

## Objetivo

Explorar el dashboard de Wazuh y analizar las primeras alertas
generadas por el propio servidor.

---

## Intento de instalación del agente

Se intentó instalar el agente de Wazuh en la misma VM donde
está el servidor. El instalador rechazó la instalación:

wazuh-agent conflicts with wazuh-manager


El agente y el servidor no pueden coexistir en el mismo equipo.
**Solución:** instalar el agente en Metasploitable en la Semana 2.

---

## Exploración del dashboard — Security Events

Al entrar a Security Events aparecieron 11 alertas generadas
por el propio servidor Wazuh.

**Columnas del dashboard:**
- Time: fecha y hora del evento
- Agent: ID del agente (000 = el propio servidor)
- Agent name: nombre del equipo
- Description: descripción del evento
- Level: severidad del 0 al 15
- Rule ID: identificador de la regla que disparó la alerta

---

## Análisis de alerta — Rule 533

**Descripción:** Listened ports status (netstat) changed

**Qué detectó:** Wazuh comparó el estado anterior de los puertos
con el nuevo y encontró diferencias.

**Puerto anterior:**
- 1514 → wazuh-remoted
- 1515 → wazuh-authd
- 55000 → python3 (API)

**Puerto nuevo (aparecieron):**
- 443 → node (dashboard de Wazuh)
- 9200 y 9300 → java (Wazuh Indexer / OpenSearch)

Wazuh detectó sus propios puertos al arrancar.
Es el propio sistema detectándose a sí mismo.

---

## Conceptos aprendidos

**Level en Wazuh:**
Severidad del 0 al 15.
- Level 0-6: informativo
- Level 7-11: advertencia
- Level 12-15: crítico, requiere atención inmediata

**Rootcheck:**
Módulo de Wazuh que analiza el sistema buscando configuraciones
inseguras, archivos sospechosos y anomalías a nivel del SO.
No es un log de root, es una verificación de integridad.

**Compliance en Wazuh:**
Wazuh mapea automáticamente cada alerta a los artículos
de los estándares de seguridad correspondientes:
- GDPR: protección de datos en Europa
- HIPAA: privacidad de datos médicos en EEUU
- PCI DSS: seguridad en procesamiento de tarjetas de crédito
- NIST 800-53: marco de seguridad del gobierno de EEUU

Permite al analista SOC filtrar alertas por estándar de compliance
sin revisar manualmente miles de eventos.

---

## Próximo paso

Instalar Metasploitable como segunda VM y conectarla
como agente a Wazuh para monitorear una máquina víctima real.

---

## Security Alerts — conceptos básicos

- **Total alerts:** cantidad total de eventos registrados
- **Level 12 or above:** alertas críticas que requieren atención inmediata
- **Authentication failure:** intentos de login fallidos
- **Authentication success:** logins exitosos

Estos contadores aparecen en la parte superior del dashboard
y dan una vista rápida del estado de seguridad del sistema.

