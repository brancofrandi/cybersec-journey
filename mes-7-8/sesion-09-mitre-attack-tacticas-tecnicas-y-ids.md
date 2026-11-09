# Sesión 09 — MITRE ATT&CK: tácticas, técnicas, sub-técnicas e IDs

**Mes 7-8 (SIEM) · Semana 4 (MITRE ATT&CK) · Primera sesión**

## Objetivo

Entender qué es MITRE ATT&CK y cómo se lee: diferencia entre táctica, técnica y sub-técnica, y cómo se relacionan con una alerta real (la alerta de Wazuh de la Semana 2).

## 1. Qué es ATT&CK y sus tres piezas

ATT&CK es un catálogo público de comportamientos de atacantes reales, armado a partir de ataques observados. Sirve para ponerle nombre a lo que se ve en una alerta.

- **Táctica:** el objetivo del atacante en un momento del ataque (el "para qué").
- **Técnica:** el método concreto con que logra ese objetivo (el "cómo"). Cada técnica pertenece a una táctica.
- **Sub-técnica:** una variante más específica de una técnica. Su ID es el de la técnica madre más un punto y un número (por ejemplo, `T1685.002` es la sub-técnica 002 de `T1685`).

Diferencia con la Cyber Kill Chain: la Kill Chain describe 7 fases generales y ordenadas; ATT&CK es un catálogo mucho más detallado de comportamientos, sin orden fijo.

## 2. Lista de tácticas vista en el sitio oficial

La matriz Enterprise mostraba **15 tácticas**, entre ellas Reconnaissance, Resource Development, Initial Access, Execution, Persistence, Privilege Escalation, **Stealth** (TA0005), **Defense Impairment** (TA0112), Credential Access, Discovery, Lateral Movement, Collection, Command and Control, Exfiltration e Impact.

Hallazgo de la sesión: la táctica que antes se llamaba *Defense Evasion* aparece ahora dividida en dos:

| Táctica | Qué busca el atacante |
|---|---|
| **Stealth** | Esconder y disimular sus acciones para parecer comportamiento normal, sin tocar las defensas. |
| **Defense Impairment** | Romper o debilitar las herramientas de seguridad para que los defensores no puedan ver ni confiar en lo que pasa. |

## 3. Ejemplo trabajado: detener el agente de Wazuh

- **Táctica:** Defense Impairment (el atacante rompe una herramienta de defensa; no se limita a esconderse).
- **Técnica:** *Disable or Modify Tools*, ID actual **T1685**.
- Una sub-técnica vista: `T1685.002`, que deshabilita o modifica el registro (logs) de la nube para limitar la recopilación de datos sobre la actividad del atacante.
- Detener un agente hace ruido: por eso Wazuh generó la alerta de "agent disconnected" en la Semana 2. Es una oportunidad de detección.

**Objetivo distinto: persistencia.** Mantenerse dentro aunque el servidor se reinicie (por ejemplo, un servicio propio con `systemctl enable`) es la táctica *Persistence*, no Defense Impairment.

## 4. Los IDs cambian entre versiones

La técnica que la alerta de la Semana 2 llevaba como T1562 hoy se publica como **T1685**, y el sitio redirige el ID viejo al nuevo. Consecuencias prácticas: anotar la versión del framework usada en un reporte, y tener presente que reglas de herramientas como Wazuh pueden seguir etiquetando con IDs anteriores.

## 5. Errores propios y aprendizajes

- Al clasificar el ejemplo del agente elegí primero Stealth, aunque mi propia definición de Stealth ("sin modificar las defensas") apuntaba a Defense Impairment. Aprendí a aplicar la definición, no solo a recitarla.
- Mezclé "mantenerse dentro" (persistencia) con "evadir la detección".
- Dije que las sub-técnicas "dependen de la táctica": dependen de la **técnica**.
- Al navegar el sitio me confundí de página (T1548) y dudé de un ID que en realidad era correcto. Verificar leyendo el título de la página resuelve la duda.

## Pendientes

- Volver a probar la priorización con un caso nuevo (pendiente de la Semana 3).
- Siguiente paso de la Semana 4: ubicar técnicas de ATT&CK en alertas reales de Wazuh.
- Reactivar el curso de Google (máximo una hora por día).
- Servicios de Nmap sin analizar (`vsftpd 2.3.4`, `UnrealIRCd`, `distccd`): material de la Semana 6.
