## Sesión 23 — Python: detector de RST mejorado con alertas Telegram
**Fecha:** 31/08/2026

---

## Objetivo

Mejorar el detector de RST anómalo agregando:
- Guardado de anomalías en archivo con timestamp
- Alerta automática por Telegram al finalizar el escaneo

---

## Script completo

```python
import asyncio
from telegram import Bot
from datetime import datetime

TOKEN = "tu_token_aqui"  # nunca subir a GitHub
CHAT_ID = tu_chat_id

async def enviar_alerta():
    bot = Bot(TOKEN)
    await bot.send_message(chat_id=CHAT_ID, text="Alerta desde Kali: ANOMALIA")

captura = open("captura.csv", "r")
anomalia = open("anomalias.txt", "w")
i = 0

for linea in captura:
    if "RST" in linea and "80" not in linea and "443" not in linea:
        print(linea.strip())
        ahora = datetime.now()
        texto_fecha = ahora.strftime("%Y-%m-%d %H:%M:%S")
        anomalia.write("[" + texto_fecha + "] " + linea.strip() + "\n")
        i += 1

if i != 0:
    asyncio.run(enviar_alerta())

print("Los puertos a tener en cuenta por anomalias son: " + str(i))
captura.close()
anomalia.close()
```

---

## Resultado en anomalias.txt

[2026-08-31 17:59:31] "999","1.0","10.0.2.15","192.168.1.1","TCP","60","4444 → 1234 [RST]"


---

## Conceptos nuevos

**asyncio:**
Librería de Python para ejecutar código asíncrono.
Se usa cuando una operación puede tardar (llamadas a internet).
Sin async el script quedaría bloqueado esperando la respuesta.

**python-telegram-bot:**
Librería que conecta Python con la API de Telegram.
Permite enviar mensajes, fotos y archivos a un chat o grupo.

**async def:**
Define una función asíncrona. Debe llamarse con `asyncio.run()`.

**¿Por qué la alerta va después del loop?**
Para enviar una sola alerta con el resumen final.
Si estuviera dentro del loop enviaría una alerta por cada anomalía.

---

## Seguridad

El TOKEN es una clave privada del bot.
Nunca debe subirse a GitHub ni compartirse.
Si se expone, revocar inmediatamente en BotFather:
/mybots → seleccionar bot → API Token → Revoke current token

Agregar al .gitignore los archivos con credenciales:

puertos_ex.py
test_telegram.py

---

## Instalación de dependencias

```bash
pip install python-telegram-bot --break-system-packages
```

---

## Flujo completo del proyecto

1. Capturar tráfico con Wireshark
2. Exportar como CSV: File → Export Packet Dissections → As CSV
3. Ejecutar el script: python3 puertos_ex.py
4. Revisar anomalias.txt
5. Si hay anomalías, llega alerta a Telegram
