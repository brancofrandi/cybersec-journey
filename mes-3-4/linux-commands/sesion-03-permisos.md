## Sesion 03 - Permisos de archivos
**Fecha:** [08/04/26]

---

### Comandos aprendidos

| Comando | Que hace |
|---------|----------|
| touch archivo | Crea un archivo vacio o actualiza su marca de tiempo |
| chmod 755 archivo | Cambia permisos en notacion numerica |
| chmod u+x archivo | Agrega permiso de ejecucion al usuario |
| chmod o-r archivo | Quita permiso de lectura a otros |
| ls -la archivo | Muestra permisos y detalles de un archivo especifico |

---

### Notacion numerica

Cada numero es la suma de los permisos del grupo:
- r = 4
- w = 2
- x = 1

| Numero | Permisos | Simbolico |
|--------|----------|-----------|
| 7 | rwx | leer, escribir, ejecutar |
| 6 | rw- | leer y escribir |
| 5 | r-x | leer y ejecutar |
| 4 | r-- | solo leer |
| 0 | --- | sin permisos |

Ejemplos practicados:
- 755 → rwxr-xr-x (usuario todo, grupo y otros leer+ejecutar)
- 644 → rw-r--r-- (usuario leer+escribir, grupo y otros solo leer)
- 400 → r-------- (solo propietario puede leer)
- 500 → r-x------ (propietario leer y ejecutar)

---

### Notacion simbolica

| Letra | Representa |
|-------|------------|
| u | user (propietario) |
| g | group (grupo) |
| o | others (otros) |
| a | all (todos) |

| Operador | Accion |
|----------|--------|
| + | Agrega permiso |
| - | Quita permiso |
| = | Establece exactamente ese permiso |

Ejemplos:
- chmod u+x archivo → agrega ejecucion al propietario
- chmod o-r archivo → quita lectura a otros
- chmod a+r archivo → agrega lectura a todos

---

### Permisos de archivos criticos del sistema

| Archivo | Permisos | Por que |
|---------|----------|---------|
| /etc/passwd | -rw-r--r-- (644) | Todos pueden leer, solo root escribe |
| /etc/shadow | -rw-r----- (640) | Solo root y grupo shadow pueden leer |
| /etc/hosts | -rw-r--r-- (644) | Lectura general, escritura solo root |

shadow es mas restrictivo porque contiene las contrasenas cifradas.
Un atacante con acceso a shadow puede intentar crackear las contrasenas.

---

### umask

Configuracion de Ubuntu que define los permisos por defecto
al crear archivos. Por eso touch creo un archivo con 664
en vez de 644 como se esperaba.

---

### Por que 777 es peligroso

777 otorga todos los permisos a cualquier usuario, proceso
o servicio del sistema. En un servidor, cualquier proceso
comprometido puede modificar o ejecutar ese archivo sin
restriccion. Es superficie de ataque innecesaria.

---

### Relevancia en ciberseguridad

- Archivos con 777 son vectores de ataque clasicos en pentesting
- Archivos de configuracion con contrasenas deben tener 600
- Scripts ejecutables necesitan chmod +x para correr
- Permisos mal asignados son uno de los hallazgos mas comunes
  en auditorias de seguridad

---

### Lo que no sabia al arrancar

- touch y su funcion
- chmod en notacion numerica y simbolica
- Como interpretar permisos numericamente
- Por que ciertos archivos tienen permisos mas restrictivos

### Dudas pendientes

- En que situaciones reales se cambian permisos:
  cuando llegue al Mes 5-6 y Bandit lo voy a ver en practica
