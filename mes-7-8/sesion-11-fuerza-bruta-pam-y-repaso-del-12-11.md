# Sesión 11 — Repaso del 12/11 y práctica en Kali: fallos de login, PAM y fuerza bruta

**Mes 7-8 (SIEM) · Semana 4 (MITRE ATT&CK) · Miércoles 12/11 (repaso) y Jueves (práctica) · Sesión del 13/11**

## Objetivo

Aplicar la lectura de una alerta de autenticación (quién, cuándo, qué se hizo), provocar intentos fallidos en el laboratorio propio y ubicarlos en ATT&CK verificando en la fuente. Primera sesión con la estructura nueva: explicación, práctica guiada, evaluación y cierre.

## 1. Repaso del 12/11 (sin herramientas)

- Stealth vs Defense Impairment: el criterio es si la acción **toca las defensas**. Enmascaramiento (nombre o ubicación que parece legítima) es Stealth; modificar el firewall o borrar los registros de eventos es Defense Impairment, aunque la intención sea esconderse.
- Valid Accounts sirve a varias tácticas: persistencia (volver a entrar sin explotar de nuevo) y Stealth (el uso de una cuenta legítima parece normal).
- Se acordó una regla: solo se evalúa lo que se trabajó con tus palabras; lo que apareció de pasada se explica primero y se evalúa después de estudiarlo.

## 2. Repaso al arrancar el 13/11

Caso: un atacante renombra su malware como `svchost.exe`. Respuesta: **Stealth**, porque se disfraza como proceso legítimo sin tocar las defensas. Se cerró sin correcciones.

## 3. Explicación: el "quién" en una línea de PAM

- El nombre de un usuario es una etiqueta; el sistema trabaja con el número (`uid`). `0` es root; `1000` suele ser el primer usuario humano.
- `session opened for user branco(uid=1000) by (uid=0)`: sesión para `branco`, abierta por un proceso con uid 0 (el servicio del sistema). Es lo esperado en un login.
- `session opened for user root(uid=0) by branco(uid=1000)`: un usuario común obtuvo una sesión de root (formato de `sudo` o `su`). Es el formato de ejemplo; las líneas reales se ven en el campo `full_log` de la alerta.
- Respuesta inicial: dije que `branco` "no es administrador". Eso lo deduje del laboratorio, no de la línea. En un reporte se escribe como supuesto, no como hecho verificado.
- Lo que decide si esa línea es normal: el horario (contra la hora real, por el desfase de reloj de Kali) y el campo `COMMAND` del log de `sudo`.

## 4. Fuerza bruta y Credential Access

- Un fallo aislado es normal. Una fuerza bruta son muchos fallos seguidos, en poco tiempo y del mismo origen; los usuarios inexistentes (`invalid user`) son una señal frecuente, **no un requisito**.
- Ejercicio con tres líneas de ejemplo (`root`, `admin`, `test` desde la misma IP): sospechoso; la línea de `root` como "invalid user" estaba mal armada en mi ejemplo (root existe en todo Linux).
- Conclusión redactada sin afirmar de más: aparenta fuerza bruta, pero no se puede afirmar sin verificar el timestamp, los usuarios probados y, sobre todo, si hubo un acceso exitoso desde esa IP.
- La verificación `COMMAND` de `sudo` no corresponde a intentos rechazados; allí corresponde buscar un acceso aceptado (`Accepted password`).

## 5. Práctica en la VM `wazuh-agent`

- Se verificó el agente con `sudo /var/ossec/bin/agent_control -l` (Kali como servidor Wazuh, agente Ubuntu activo).
- Prueba: `sudo -k` y luego `sudo ls /root` con tres contraseñas incorrectas, en la VM propia.
- **Predicción previa:** técnica de fuerza bruta, táctica Initial Access (corregí después a Privilege Escalation).
- **Resultado en Wazuh:** nivel 5, **Password Guessing (T1110.001)**, táctica **Credential Access**.
- **Verificación en attack.mitre.org:** T1110.001 es Brute Force: Password Guessing; técnica madre Brute Force; táctica Credential Access. Coincidieron Wazuh y el sitio.
- Aprendizaje: la táctica es el objetivo **del paso que se ve** (conseguir la credencial), no el objetivo final del ataque. Iniciar sesión con la credencial es otro paso y otra táctica.

## 6. Evaluación (4 preguntas)

1. Intento fallido vs fuerza bruta y dato que decide escalar: bien; corregí que "usuarios inexistentes" no es un requisito.
2. Por qué una técnica sirve a varias tácticas (Valid Accounts): bien, con ejemplo propio (Initial Access y Stealth).
3. Qué verificar en dos casos distintos: bien; respuestas diferentes para cada caso.
4. Priorizar dos alertas con dos criterios que cambien: orden correcto y primer criterio sólido (acceso exitoso); el segundo (qué sistema y qué datos) lo completé con ayuda. La repriorización cuenta como pasada con ayuda.

## 7. Errores propios y aprendizajes

- Tipeo: `dstuster`, `srcurser`, `virificaría`, `agent conrol`, "Sealth". Releer antes de enviar.
- Tomar un rasgo frecuente por una definición.
- Aplicar la verificación de un caso a otro donde no corresponde.
- Elegir la táctica por el objetivo final en vez del objetivo del paso.
- Afirmar de más ("un humano no puede físicamente").

## Pendientes

- Definir qué significa el nivel de alerta de Wazuh (viste 3 y 5, sin escala definida).
- Confirmar en el campo `full_log` de la alerta real qué contiene `srcuser`, `uid` y `dstuser`.
- Viernes integradora: tres alertas de Wazuh con táctica, técnica e ID verificados, triage de cada una y priorización con dos criterios distintos.
- Sábado: bloque de fricción. Domingo: GitHub al día y los cinco criterios para pasar a la Semana 5.
- Sincronizar la hora y la zona horaria de Kali y de la VM del agente.
- Verificar el origen de la conexión de la alerta T1078 de la sesión 10.
- Subir las sesiones 09, 10 y 11 a `mes-7-8/` (sin códigos de activación ni datos sensibles).
- Reactivar el curso de Google (máximo una hora por día).
