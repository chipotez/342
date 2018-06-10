
Ejercicio de recuperacoin de información.

Cuando se realiza una inspeccion de SELinux en busca de denegaciones de servicios que han sudedio en un
dia en especifico se puede usar 

# ausearch -i -m avc -ts today

el parametro *-m* nos permite mostrar tipos de mensaje en particular, para escoger de un tipo asegurese de ver la siguiente tabla.
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-audit_record_types

**Recopilación de información con sosreport

Red Hat incluye una herramienta llamada sosreport en el paquete sos. Esta herramienta puede recopilar varios fragmentos de información del sistema, incluidos archivos de registro y archivos de configuración, y agruparlos en un tarball listo para enviarse al Soporte de Red Hat.


Tarball es un término de jerga para un archivo tar : un grupo de archivos reunidos como uno solo. El término sugiere una bola de alquitrán, el derivado de carbón pegajoso utilizado como adherente y sellador en techos y otros trabajos de construcción. Tar (para Tape ARchive) es un comando de UNIX que crea un único archivo llamado archivo desde varios archivos especificados o extrae (separa) los archivos de dicho archivo. Un archivo tar tiene el sufijo de archivo .tar. Los archivos en un archivo tar no se comprimen, simplemente se juntan en un archivo. 

Los administradores de sistema también pueden usar esta herramienta para recopilar rápidamente información sobre sus propios sistemas.

Cuando se queda sin opciones, sosreport le pedirá al usuario sus iniciales, un número de caso y, luego, recopilará un conjunto predeterminado de archivos y ajustes en un tarball. Las opciones de la línea de comandos pueden utilizarse para habilitar o deshabilitar complementos (plug-ins), configurar opciones de complementos (plug-ins) y hacer que el proceso no sea interactivo.

Con sosreport -l puede solicitar una descripción completa de todos los complementos (plug-ins), y si están habilitados o deshabilitados actualmente, junto con las opciones de complementos (plug-ins). Cuando ejecuta un informe, puede usar la opción -e PLUGINS para habilitar complementos adicionales, y puede usar la opción -k PLUGOPTS para configurar opciones de complementos. Se puede usar la opción -n NOPLUGINS para deshabilitar complementos no deseados 
