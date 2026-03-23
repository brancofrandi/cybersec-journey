## Proceso vs Servicio

**Proceso**
Cualquier programa que esta en ejecucion en un momento dado.
Tiene un inicio, usa recursos del sistema (CPU, RAM) y termina.
Ejemplos: abrir el navegador, ejecutar un comando en terminal,
correr un script.

**Servicio**
Es un tipo especial de proceso que corre en segundo plano,
sin interfaz visible, generalmente desde que arranca el sistema.
No necesita que el usuario lo inicie manualmente.
En Linux se llaman daemons.
Ejemplos: servidor SSH, servicio de red, servidor DNS local.

**Diferencia clave**
Un servicio es un proceso, pero no todo proceso es un servicio.
La diferencia esta en que el servicio corre en segundo plano
de forma continua y automatica, sin intervencion del usuario.

**Relevancia en ciberseguridad**
Identificar que servicios corren en un sistema es uno de los
primeros pasos en un pentest. Un servicio mal configurado o
desactualizado es un vector de ataque.
