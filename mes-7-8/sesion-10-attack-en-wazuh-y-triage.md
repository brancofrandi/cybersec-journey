# Sesión 10 — MITRE ATT&CK en Wazuh: tácticas, técnicas y triage de una alerta T1078

**Mes 7-8 (SIEM) · Semana 4 (MITRE ATT&CK) · Práctica en el laboratorio**

## Objetivo

Consolidar la diferencia entre táctica, técnica y sub-técnica, y verla aplicada en alertas reales del dashboard de Wazuh, cerrando con el triage de una alerta etiquetada con una técnica de ATT&CK.

## 1. Repaso de conceptos (sin mirar)

- **Ejemplo propio completo:** correo con adjunto malicioso. Táctica *Initial Access*, técnica *Phishing* (T1566), sub-técnica T1566.001 (*Spearphishing Attachment*). Los IDs se verificaron en el sitio oficial: ante la posibilidad de renumeración entre versiones, siempre se confirma en la fuente.
- **Stealth vs Defense Impairment:** la diferencia se decide por **qué le hace la acción a las herramientas de defensa**. Si las toca o las rompe (desactivar los registros de un servicio), es Defense Impairment. Si solo esconde la actividad y las deja intactas (crear una cuenta de usuario oculta), es Stealth.
- Una técnica puede servir a **más de una táctica**.

## 2. Laboratorio

- Se encendieron solo Kali (servidor Wazuh) y la VM `wazuh-agent`; Metasploitable2 queda apagada por el límite de RAM. El disco externo se conecta antes de abrir VirtualBox.
- Verificación del agente: `sudo /var/ossec/bin/agent_control -l` mostró el agente 001 `wazuh-agent` como **Active** y el servidor (000) como Active/Local. El `IP: any` indica que el agente se registró sin IP fija.

## 3. ATT&CK dentro de Wazuh

- El módulo está en el grupo **Threat Detection and Response → MITRE ATT&CK**.
- El panel mostró una sola técnica, **Valid Accounts**, con 2 alertas, contadas bajo cuatro tácticas: Defense Evasion, Initial Access, Persistence y Privilege Escalation.
- **Dos hallazgos:**
  - Wazuh sigue mostrando *Defense Evasion*, táctica que el sitio oficial ya dividió en Stealth y Defense Impairment. Una etiqueta "Defense Evasion" no alcanza para saber cuál de las dos mitades es: hay que mirar la técnica y qué hace.
  - Una técnica puede servir a varios objetivos. Una cuenta válida robada sirve para entrar (Initial Access) y, si es una cuenta con más permisos que los que el atacante ya tenía (por ejemplo root o administrador), para escalar privilegios.

## 4. Triage de la alerta T1078

| Campo | Valor |
|---|---|
| Agente | wazuh-agent (ID 001) |
| Técnica | T1078 (Valid Accounts) |
| Descripción | PAM: Login session opened |
| Nivel | 3 (bajo, informativo) |
| Usuario | `branco` (uid 1000); el proceso que abrió la sesión corre como uid 0 |

- **Hora:** la alerta marcaba las 11:53 y el reloj de Kali iba 2 horas atrasado respecto de Windows. La hora real fue las 13:53, que coincide con el inicio de sesión propio en la VM. Los relojes desajustados dificultan correlacionar eventos en un SIEM.
- **`agent.ip` (192.168.100.102)** indica qué equipo reportó el evento, no desde dónde se conectó el usuario. El origen de la conexión no se verificó.
- **Conclusión redactada:** dos alertas T1078 "Login session opened", nivel 3, a la hora del inicio de sesión propio y con el usuario propio; probable falso positivo benigno. La etiqueta T1078 describe lo que podría ser una técnica de ataque, no lo que ocurrió. No se verificó el origen de la conexión.

## 5. Errores propios y aprendizajes

- Respondí con definiciones de la tabla en vez de describir un método con mis palabras; al achicar la pregunta sí lo logré.
- Apelé a "así aparece en MITRE" como justificación: es un dato, no una razón.
- Propuse la detectabilidad como criterio para clasificar Stealth vs Defense Impairment; el criterio correcto es si la acción toca las defensas.
- Expliqué Privilege Escalation como "estar dentro como usuario", que es acceso, no escalada.
- Con la alerta de Wazuh no supe identificar el triage al principio; con el método (nivel, descripción, hora y contexto) lo resolví.

## Pendientes

- Sincronizar la hora/zona horaria de Kali y de la VM del agente con el sistema real.
- Verificar el origen de la conexión de la alerta T1078 con los documentos relacionados.
- La técnica de modificar procesos de autenticación en la nube (para ubicar su táctica en el sitio).
- Volver a probar la priorización con un caso nuevo.
- Reactivar el curso de Google (máximo una hora por día) y subir las sesiones 09 y 10 a `mes-7-8/`.
