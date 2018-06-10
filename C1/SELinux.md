
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






#############################################
#############################################

Uso de redhat-support-tool para realizar búsquedas en la base de conocimientos
La utilidad Red Hat Support Tool redhat-support-tool proporciona una interfaz de consola de texto para los servicios Red Hat Access que se basan en suscripciones. Hay que tener acceso a Internet para poder acceder al portal de clientes de Red Hat. La herramienta redhat-support-tool se basa en texto para su uso desde cualquier terminal o conexión SSH; no se proporciona ninguna interfaz gráfica.

El comando redhat-support-tool puede utilizarse como una shell interactiva o invocarse como si fuera un comando que se ejecuta en forma individual con opciones y argumentos. La sintaxis disponible de la herramienta es idéntica para los dos métodos. De manera predeterminada, el programa se inicia en modo de shell. Utilice el subcomando help proporcionado para ver todos los comandos disponibles. El modo de shell admite la compleción con el tabulador y la capacidad de solicitar programas en la shell principal

[student@demo ~]$ redhat-support-tool
Welcome to the Red Hat Support Tool.
Command (? for help):

Cuando se invoca por primera vez, redhat-support-tool solicita la información necesaria de inicio de sesión como suscriptor a Red Hat Access. Para evitar proporcionar esta información en reiteradas ocasiones, la herramienta le pregunta si desea almacenar la información de la cuenta en el directorio de inicio del usuario (~/.redhat-support-tool/redhat-support-tool.conf). Si varios usuarios comparten una cuenta de Red Hat Access, la opción --global permite guardar la información de la cuenta en /etc/redhat-support-tool.conf, junto con la configuración de todo el sistema. El comando config modifica los valores de configuración de la herramienta.

La herramienta redhat-support-tool permite que los suscriptores busquen y muestren el mismo contenido de la base de conocimientos que se ve cuando están en el portal de clientes de Red Hat. La base de conocimientos permite realizar búsquedas por palabras claves, similar al comando man. Los usuarios pueden ingresar códigos de error, sintaxis de archivos de registro o cualquier combinación de palabras clave para producir una lista de documentos de soluciones relevantes.

A continuación se incluye una demostración de búsqueda básica y configuración inicial:





