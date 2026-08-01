## Sesión 12 — Bandit nivel 15: conexión SSL con OpenSSL
**Fecha:** [01/08/26]

---

## Qué se aprendió

Conectarse a un puerto con cifrado SSL/TLS usando OpenSSL.
nc no soporta SSL, por eso se necesita una herramienta diferente.

---

## Diferencia entre nc y openssl s_client

| Herramienta | Cifrado | Uso |
|-------------|---------|-----|
| nc | No, texto plano | Conexión a puertos sin cifrado |
| openssl s_client | Sí, SSL/TLS | Conexión a puertos con cifrado |

---

## Comando usado

```bash
openssl s_client -connect localhost:30001
```

**Desglose:**
- `openssl` → herramienta de criptografía
- `s_client` → modo cliente SSL/TLS
- `-connect` → flag que especifica host y puerto
- `localhost:30001` → destino de la conexión

---

## Cómo encontrar el comando

Se investigó con `man openssl` y se identificó `s_client`
como el subcomando para establecer conexiones SSL como cliente.

---
**Desglose:**
- `openssl` → herramienta de criptografía
- `s_client` → modo cliente SSL/TLS
- `-connect` → flag que especifica host y puerto
- `localhost:30001` → destino de la conexión

---

## Cómo encontrar el comando

Se investigó con `man openssl` y se identificó `s_client`
como el subcomando para establecer conexiones SSL como cliente.

---

---

## Certificado self-signed

Un certificado firmado por el mismo servidor que lo emite,
no por una autoridad de certificación externa.

El cliente no puede verificar su legitimidad porque no hay
una CA de confianza que lo respalde.
Por eso aparece la advertencia `self-signed certificate`.

En producción real sería una señal de alerta.
En Bandit es intencional para el laboratorio.

---

## read R BLOCK

Mensaje que indica que la conexión SSL está establecida
y el servidor espera que el cliente envíe datos.
Se escribió la contraseña de bandit14 y el servidor
respondió con la contraseña del nivel siguiente.

---

## Proceso completo del nivel

1. Leer el enunciado: puerto 30001 con SSL
2. Investigar con `man openssl` → encontrar `s_client`
3. Ejecutar: `openssl s_client -connect localhost:30001`
4. Esperar `read R BLOCK`
5. Escribir la contraseña de bandit14
6. Recibir la contraseña del nivel 16


## Resultado de la conexión
