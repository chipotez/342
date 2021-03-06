
Ejercicio de recuperacoin de información.

Cuando se realiza una inspeccion de SELinux en busca de denegaciones de servicios que han sudedio en un
dia en especifico se puede usar 

# ausearch -i -m avc -ts today

el parametro *-m* nos permite mostrar tipos de mensaje en particular, para escoger de un tipo asegurese de ver la siguiente tabla.
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-audit_record_types

https://wiki.gentoo.org/wiki/SELinux/Tutorials/Where_to_find_SELinux_permission_denial_details

https://wiki.centos.org/es/HowTos/SELinux
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

[student@demo ~]$ redhat-support-tool
Welcome to the Red Hat Support Tool.
Command (? for help): search How to manage system entitlements with subscription-manager
Please enter your RHN user ID: subscriber
Save the user ID in /home/student/.redhat-support-tool/redhat-support-tool.conf (y/n): y
Please enter the password for subscriber: password
Save the password for subscriber in /home/student/.redhat-support-tool/redhat-support-tool.conf (y/n): y
La herramienta, tras solicitarle al usuario la configuración de usuario requerida, continúa con la solicitud de búsqueda original.

Type the number of the solution to view or 'e' to return to the previous menu.
  1 [ 253273:VER] How to register and subscribe a system to Red Hat Network
    (RHN) using Red Hat Subscription Manager (RHSM)?
  2 [  17397:VER] What are Flex Guest Entitlements in Red Hat Network?
  3 [ 232863:VER] How to register machines and manage subscriptions using Red
    Hat Subscription Manager through an invisible HTTP proxy / Firewall?
3 of 43 solutions displayed. Type 'm' to see more, 'r' to start from the beginning again, or '?' for help with the codes displayed in the above output.
Select a Solution: 
Pueden seleccionarse secciones específicas de documentos de soluciones para su visualización.

Select a Solution: 1

Type the number of the section to view or 'e' to return to the previous menu.
 1 Title
 2 Issue
 3 Environment
 4 Resolution
 5 Display all sections
End of options.
Section: 1

Title
===============================================================================
How to register and subscribe a system to Red Hat Network (RHN) using Red Hat Subscription Manager (RHSM)?
URL:        https://access.redhat.com/site/solutions/253273
(END) q
Acceso directo a artículos de la base de conocimientos por ID de documento
Encuentre artículos en línea en forma directa con el comando kb de la herramienta con la ID de documento de la base de conocimientos. Los documentos arrojados pasan por la pantalla sin paginación, lo que le permite al usuario redirigir el resultado obtenido mediante el uso de otros comandos locales. En este ejemplo, se puede ver el documento con el comando less.

[student@demo ~]$ redhat-support-tool kb 253273 | less

Title:   How to register and subscribe a system to Red Hat Network (RHN) using Red Hat Subscription Manager (RHSM)?
ID:      253273                                                      
State:   Verified: This solution has been verified to work by Red Hat Customers and Support Engineers for the specified product version(s).
URL:     https://access.redhat.com/site/solutions/253273             
: q
Los documentos arrojados en formato sin paginar pueden enviarse fácilmente a una impresora, convertirse a PDF o a otro formato de documento, o redirigirse a un programa de entrada de datos para seguimiento de incidentes o sistema de administración de cambios, mediante el uso de otras utilidades instaladas y disponibles en Red Hat Enterprise Linux.

Uso de redhat-support-tool para administrar casos de asistencia
Un beneficio de la suscripción a un producto es el acceso a asistencia técnica a través del portal de clientes de Red Hat. Según el nivel de soporte de suscripción del sistema, Red Hat puede comunicarse mediante herramientas en línea o por teléfono. Consulte https://access.redhat.com/site/support/policy/support_process para obtener enlaces a información detallada acerca del proceso de soporte.

Preparación de un informe de error
Antes de comunicarse con la asistencia de Red Hat, reúna información relevante para un informe de errores.

Defina el problema. Indique el problema y los síntomas con claridad. Sea lo más específico posible. Detalle los pasos que reproducirían el problema.

Reúna información básica. ¿Qué producto y versión se ven afectados? Esté preparado para brindar información de diagnóstico relevante. Esto puede incluir el resultado de sosreport, que se abordó anteriormente en esta sección. En el caso de problemas del kernel, dicha información podría incluir un vuelco de errores de kdump del sistema o una fotografía digital del seguimiento de kernel mostrado en el monitor de un sistema bloqueado.

Determine el nivel de gravedad. Red Hat utiliza cuatro niveles de gravedad para clasificar los problemas. Después de los informes de problemas con gravedad urgente y alta, debe realizarse una llamada telefónica al centro de asistencia local pertinente (visite https://access.redhat.com/site/support/contact/technicalSupport).

Gravedad	Descripción
Urgente (Gravedad 1)	Un problema que afecta gravemente al uso del software en un entorno de producción (como la pérdida de datos de producción o la parada de los sistemas de producción). La situación interrumpe las operaciones empresariales y no existe un procedimiento de resolución.
Alta (Gravedad 2)	Un problema donde el software funciona, pero el uso en un entorno de producción se ve gravemente reducido. La situación tiene un gran impacto en parte de las operaciones empresariales y no existe un procedimiento de resolución.
Media (Gravedad 3)	Un problema que implica una pérdida parcial no fundamental de la capacidad de uso del software en un entorno de producción o desarrollo. Para los entornos de producción, hay un impacto de mediano a bajo en el negocio, pero el negocio sigue funcionando, incluso mediante el uso de una solución de proceso. Para entornos de desarrollo, donde la situación está causando que el proyecto continúe o no migre a la producción.
Baja (Gravedad 4)	Un asunto de uso general, la comunicación de un error de documentación o una recomendación para una mejora o modificación futura del producto. Para entornos de producción, el impacto en su negocio, en el rendimiento o en la funcionalidad del sistema es de bajo a cero. Para los entornos de desarrollo, hay un impacto de mediano a bajo en el negocio, pero el negocio sigue funcionando, incluso mediante el uso de una solución de proceso.

Administración de un informe de errores con redhat-support-tool
Los suscriptores pueden crear, ver, modificar y cerrar casos de asistencia de Red Hat Support mediante el uso de redhat-support-tool. Cuando se abren y se mantienen casos de asistencia, los usuarios pueden incluir archivos o documentación, como informes de diagnóstico (sosreport). La herramienta carga y adjunta archivos a casos en línea. Los detalles del caso, como producto, versión, resumen, descripción, gravedad y grupo de caso, pueden asignarse con opciones de comandos o si se deja la solicitud de la herramienta de información necesaria. En el siguiente ejemplo, se especifican las opciones --product y --version, pero redhat-support-tool proporcionará una lista de elecciones para esas opciones si el comando opencase no las especificó.

[student@demo ~]$ redhat-support-tool
Welcome to the Red Hat Support Tool.
Command (? for help): opencase --product="Red Hat Enterprise Linux" --version="7.0"
Please enter a summary (or 'q' to exit): System fails to run without power
Please enter a description (Ctrl-D on an empty line when complete):
When the server is unplugged, the operating system fails to continue.
 1   Low                                                                        
 2   Normal                                                                     
 3   High                                                                       
 4   Urgent                                                                     
Please select a severity (or 'q' to exit): 4
Would you like to assign a case group to this case (y/N)? N
Would see if there is a solution to this problem before opening a support case? (y/N) N
-------------------------------------------------------------------------------
Support case 01034421 has successfully been opened.
Inclusión de información de diagnóstico con el archivo de informe de SoS adjunto
La inclusión de información de diagnóstico cuando un caso de asistencia se crea por primera vez contribuye a una resolución más rápida del problema. El comando sosreport genera un archivo tar comprimido de información de diagnóstico reunida del sistema en ejecución. La herramienta redhat-support-tool le pide que incluya uno en caso de que un archivo se haya creado previamente:

Please attach a SoS report to support case 01034421. Create a SoS report as
the root user and execute the following command to attach the SoS report
directly to the case:
 redhat-support-tool addattachment -c 01034421 path to sosreport

Would you like to attach a file to 01034421 at this time? (y/N) N
Command (? for help): 
Si todavía no hay un informe SoS actual preparado, un administrador puede generar y adjuntar uno más tarde con el comando addattachment de la herramienta, como se recomendó anteriormente.

Usted puede ver, modificar y cerrar los casos de asistencia como suscriptor:

Command (? for help): listcases

Type the number of the case to view or 'e' to return to the previous menu.
 1 [Waiting on Red Hat]  System fails to run without power
No more cases to display
Select a Case: 1

Type the number of the section to view or 'e' to return to the previous menu.
 1 Case Details
 2 Modify Case
 3 Description
 4 Recommendations
 5 Get Attachment
 6 Add Attachment
 7 Add Comment
End of options.
Option: q

Select a Case: q

Command (? for help):q

[student@demo ~]$ redhat-support-tool modifycase --status=Closed 01034421
Successfully updated case 01034421
[student@demo ~]$
La herramienta Red Hat Support cuenta con capacidades avanzadas de análisis y diagnóstico de aplicaciones. Mediante el uso de los archivos principales del vuelco de errores de kernel, redhat-support-tool puede crear y extraer un seguimiento, un informe de tramas de pila activas en el momento en que se realiza un vuelco de errores, para proporcionar diagnóstico in situ y abrir un caso de asistencia.

La herramienta también proporciona análisis de archivo de registro. Mediante el uso del comando analyze de la herramienta, los archivos de registro de muchos tipos, como de sistema operativo, JBoss, Python, Tomcat, oVirt, etc., pueden analizarse para reconocer síntomas de problemas que pueden verse y diagnosticarse de manera individual. Proporcionar análisis preprocesado, en oposición a datos sin procesar, como archivos de registro o vuelcos de errores, permite que se abran los casos de asistencia y que se pongan a disposición de ingenieros más rápidamente.

Trabajos de laboratorio
Los trabajos de laboratorio de Red Hat Access, que se pueden encontrar en https://access.redhat.com/labs, proporcionan varias herramientas basadas en la Web para ayudar en la configuración, las implementaciones, la seguridad y la solución de problemas. Esto incluye scripts de prueba para vulnerabilidades de seguridad como Shellshock y Heartbleed, pero también scripts de configuración para clientes y servidores de NFS, analizadores de archivos de registro, una herramienta para ayudar a enviar una revisión de arquitectura para clústeres de alta disponibilidad, y mucho más.

Red Hat Insights
Red Hat Insights es un servicio de alojamiento que les proporciona a los gerentes y administradores de sistemas una herramienta para ayudarlos a administrar sus sistemas de manera proactiva. Red Hat Insights carga (de manera segura) información clave desde un sistema a Red Hat, donde se analiza y se realiza un conjunto de recomendaciones personalizadas. Estas recomendaciones pueden ayudar a mantener la estabilidad y el funcionamiento de los sistemas, detectando cualquier problema potencial y proporcionando asesoramiento de reparación antes de que se convierta en un problema más grande.

La interfaz web de https://access.redhat.com/insights proporciona una descripción general de todos los posibles problemas que afectan sistemas registrados, ordenados por gravedad y tipo. Los administradores pueden examinar los problemas a fondo para obtener recomendaciones personalizadas. Los administradores también pueden elegir ignorar permanentemente ciertas reglas.

Registro de sistemas con Red Hat Insights
Registrar sistemas con Red Hat Insights requiere solo dos pasos:

Asegurarse de que el paquete redhat-access-insights está instalado:

[root@demo ~]# 
          
            yum -y install redhat-access-insights
          
        
Registrar el sistema con Red Hat Insights.

[root@demo ~]# redhat-access-insights --register
Successfully registered demo.lab.example.com

Attempting to download collection rules
Attempting to download collection rules GPG signature from https://cert-api.access.redhat.com/r/insights/v1/static/uploader.json.asc
Successfully downloaded GPG signature
Verifying GPG signature of Insights configuration
Starting to collect Insights data
Uploading Insights data, this may take a few minutes
Upload completed successfully
Inmediatamente después de registrar un sistema, los datos de Insights se ponen a disposición en la interfaz web.







REFERENCIAS
Red Hat Access: Red Hat Support Tool
https://access.redhat.com/articles/445443

Log Analyzer
https://access.redhat.com/blogs/759303/posts/880333

CONFIGURING THE RED HAT SUPPORT TOOL
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/sec-configuring_the_red_hat_support_tool

How to provide files to Red Hat Support (vmcore, rhev logcollector, sosreports, heap dumps, log files, etc.)
https://access.redhat.com/solutions/2112


References
sosreport(1) man page

Red Hat Access: Red Hat Support Tool
https://access.redhat.com/articles/445443

Red Hat Support Tool First Use
https://access.redhat.com/videos/534293

Contacting Red Hat Technical Support
https://access.redhat.com/support/policy/support_process/

Help - Red Hat Customer Portal
https://access.redhat.com/help/

Red Hat Access Labs
https://access.redhat.com/labs/

Red Hat Insights
https://access.redhat.com/insights




******************************************************
*                                                    *
*                    Practica I                      *
*                                                    *
******************************************************

Ejercicio guiado: Uso de los recursos de Red Hat	
En este trabajo de laboratorio, generará e inspeccionará un sosreport en su sistema servera.

Recursos
Máquinas	
workstation

servera

Resultados
Deberá poder generar un sosreport para ayudar al Soporte de Red Hat a resolver un problema.

Mientras resolvía un problema en su sistema servera, contactó al Soporte de Red Hat para obtener ayuda. El soporte ha solicitado que genere un sosreport con el módulo xfs activo y la opción xfs.logprint habilitada.

A fines de este ejercicio, su nombre es "student" y su número de caso es 123456.

Compruebe que tiene el paquete sos instalado en su sistema servera. En caso de no tenerlo, instálelo.

Compruebe que tiene el paquete sos instalado en su sistema servera.

[root@servera ~]# rpm -q sos
sos-3.2-35.el7.noarch
Si el comando anterior devolvió package sos is not installed (el paquete sos no está instalado), instale el paquete sos.

[root@servera ~]# yum -y install sos
Vea las opciones, los complementos y las opciones de complementos disponibles para sosreport.

Vea las opciones disponibles para sosreport.

[root@servera ~]# sosreport --help | less
Vea los complementos y las opciones de complementos disponibles para sosreport.

[root@servera ~]# sosreport -l | less
Genere un sosreport en servera. Asegúrese de incluir la opción xfs.logprint.

Ejecute la herramienta sosreport con la opción -k xfs.logprint. Use los avisos interactivos para ingresar toda la información requerida. Este proceso puede tardar un tiempo en completarse.

[root@servera ~]# sosreport -k xfs.logprint
sosreport (version 3.2)

This command will collect diagnostic and configuration information from
this Red Hat Enterprise Linux system and installed applications.

An archive containing the collected information will be generated in
/var/tmp and may be provided to a Red Hat support representative.

Any information provided to Red Hat will be treated in accordance with
the published support policies at:

  https://access.redhat.com/support/

The generated archive may contain data considered sensitive and its
content should be reviewed by the originating organization before being
passed to any third party.

No changes will be made to system configuration.

Press ENTER to continue, or CTRL-C to quit. Enter
Please enter your first initial and last name [servera.lab.example.com]: student
Please enter the case id that you are generating this report for []: 123456

 Setting up archive ...
 Setting up plugins ...
 Running plugins. Please wait ...

  Running 1/82: abrt...        
  Running 2/82: acpid...        
...
  Running 81/82: xfs...        
  Running 82/82: yum...        

Creating compressed archive...

Your sosreport has been generated and saved in:
  /var/tmp/sosreport-student.123456-20151210132339.tar.xz

The checksum is: d0459a202c5c456be00a3dac7b92567a

Please send this file to your support representative.
Copie el archivo generado al directorio de inicio para student en workstation, y extráigalo.

Copie el archivo generado al directorio de inicio para student en workstation.

[student@workstation ~]$ scp root@servera:/var/tmp/sosreport-student.123456*.tar.xz .
Extraiga el archivo generado. Puede ignorar de manera segura los errores relacionados a la creación de los nodos del dispositivo.

[student@workstation ~]$ tar xvf sosreport-student.123456*.tar.xz
Inspeccione el sosreport extraído. Algunos archivos de interés:

etc/

sos_commands/

sos_reports/sos.html

sos_commands/xfs/xfs_logprint_-c.dev.vda1

Este archivo se generó mediante la opción -k xfs.logprint.




******************************************************
*                                                    *
*                    Practica II                     *
*                     Evaluación                     *
*                                                    *
******************************************************

Trabajo de laboratorio: ¿Qué es la solución de problemas?	
En este trabajo de laboratorio, resolverá un problema con transferencias de FTP en servera.

Recursos
Máquinas	
workstation

servera

Resultados
Deberá poder resolver un problema de transferencia de archivos y conectividad de FTP mediante el método científico.

Prepare sus sistemas para este ejercicio ejecutando el comando lab troubleshootingintro setup en su máquina workstation.

[student@workstation ~]$ lab troubleshootingintro setup
Uno de sus usuarios informó un problema con el servidor FTP que se ejecuta en servera. El usuario experimenta los siguientes síntomas:

El usuario no puede conectarse desde workstation a través de lftp.

El usuario puede conectarse a localhost a través de lftp cuando se registra en una shell en servera.

Cuando está conectado, el archivo pub/noclip sí se transfiere, pero el archivo pub/getall no.

Para realizar sus pruebas, se ha instalado lftp en workstation y servera. Recuerde que lftp no intentará conectarse hasta que se emita el primer comando.

El daemon del FTP (vsftpd.service) en servera registra todas las transferencias de archivos en /var/log/xferlog. Si el último carácter en una línea de ese archivo es c, la transferencia se completó correctamente; si el último carácter es i, la transferencia no se completó.

Intente recrear el problema.

Intente utilizar lftp desde workstation para conectarse al servicio de FTP que se ejecuta en servera.

[student@workstation ~]$ lftp servera
lftp servera:~> ls
'ls' at 0 [Delaying before reconnect: 30]Ctrl+C
Interrupt
lftp servera:~> bye
Intente utilizar lftp desde workstation para conectarse al servicio de FTP que se ejecuta en servera/localhost.

[student@servera ~]$ lftp servera
lftp servera:~> ls
drwxr-xr-x    2 0        0             32 Dec 11 09:42 pub
lftp servera:~> 
Intente ver el contenido del archivo pub/noclip.

lftp servera:~> cat pub/noclip
idspispopd
11 bytes transferred
lftp servera:~> 
Intente ver el contenido del archivo pub/getall.

lftp servera:~> cat pub/getall
cat: Access failed: 550 Failed to open file. (pub/getall)
lftp servera:~> bye
Recopile información acerca del servicio de FTP que se ejecuta en servera. Incluya puertos de red, información de firewall, raíz de documentos, permisos de archivos, denegaciones de SELinux, etc.

Recopile información sobre dónde está escuchando vsftpd en la red.

[root@servera ~]# ss -tulpn | grep ftp
tcp   LISTEN  0  32 :::21   :::*  users:(("vsftpd",pid=30050,fd=3))
Esto muestra que vsftpd está escuchando en el puerto FTP predeterminado (tcp:21) y acepta las conexiones de todas las direcciones IP.

Vea la configuración de firewall en servera:

[root@servera ~]# firewall-cmd --list-all
public (default, active)
  interfaces: eth0 eth1
  sources:
  services: dhcpv6-client ssh
  ports:
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules:
Esto muestra que el servicio ftp no se abre en el firewall. Esto explica por qué fallaron las conexiones remotas pero funcionaron las conexiones locales. Tome nota de esto.

Consulte el contenido de la raíz del documento de FTP. La ubicación predeterminada es /var/ftp.

[root@servera ~]# ls -lR /var/ftp
/var/ftp:
total 0
drwxr-xr-x. 2 root root 32 Dec 11 10:42 pub

/var/ftp/pub:
total 8
-rw-r--r--. 1 root root  6 Dec 11 10:42 getall
-rw-r--r--. 1 root root 11 Dec 11 10:42 noclip
Aparentemente, los permisos del archivo son correctos.

Inspeccione en busca de denegaciones de SELinux que hayan ocurrido el último día.

[root@servera ~]# ausearch -m avc -i -ts today
...
type=SYSCALL msg=audit(12/11/2015 11:01:01.997:2031) : arch=x86_64 syscall=open success=no exit=-13(Permission denied) a0=0x7f91ba3da4f0 a1=O_RDONLY|O_NONBLOCK a2=0x7f91ba3d9880 a3=0x566a9edd items=0 ppid=30406 pid=30408 auid=unset uid=ftp gid=ftp euid=ftp suid=ftp fsuid=ftp egid=ftp sgid=ftp fsgid=ftp tty=(none) ses=unset comm=vsftpd exe=/usr/sbin/vsftpd subj=system_u:system_r:ftpd_t:s0-s0:c0.c1023 key=(null) 
type=AVC msg=audit(12/11/2015 11:01:01.997:2031) : avc:  denied  { open } for  pid=30408 comm=vsftpd path=/pub/getall dev="vda1" ino=16828424 scontext=system_u:system_r:ftpd_t:s0-s0:c0.c1023 tcontext=unconfined_u:object_r:tmp_t:s0 tclass=file
...
Esto muestra que el archivo /var/ftp/pub/getall tiene un contexto de SELinux de tmp_t. Probablemente esto sea incorrecto.

Formule una hipótesis con respecto a cuáles podrían ser los problemas.

El firewall no se abre para el servicio de FTP, lo que resulta en conexiones fallidas desde máquinas remotas.

El archivo /var/ftp/pub/getall tiene un contexto de SELinux incorrecto, lo que deshabilita el acceso de lectura del daemon del FTP al archivo.

Implemente una solución para todos los problemas encontrados.

Abra el firewall para los servicios de FTP en servera.

[root@servera ~]# firewall-cmd --add-service=ftp
[root@servera ~]# firewall-cmd --permanent --add-service=ftp
Reinicie recurrentemente los contextos de SELinux en /var/ftp.

[root@servera ~]# restorecon -Rv /var/ftp
Compruebe que los problemas informados ahora están resueltos.

Intente utilizar lftp desde workstation para conectarse al servicio de FTP que se ejecuta en servera/localhost.

[student@workstation ~]$ lftp servera
lftp servera:~> ls
drwxr-xr-x    2 0        0             32 Dec 11 09:42 pub
lftp servera:~> 
Intente ver el contenido del archivo pub/noclip.

lftp servera:~> cat pub/noclip
idspispopd
11 bytes transferred
lftp servera:~> 
Intente ver el contenido del archivo pub/getall.

lftp servera:~> cat pub/getall
idkfa
6 bytes transferred
lftp servera:~> bye
Evalúe su trabajo ejecutando el comando lab troubleshootingintro grade desde workstation.

[student@workstation ~]$ lab troubleshootingintro grade
Reinicie todas sus máquinas para proveer un entorno limpio para los siguientes ejercicios

Resumen	
En este capítulo, aprendió lo siguiente:

Cómo aplicar el método científico en la solución de problemas.

Cómo utilizar journalctl para leer registros del sistema.

Cómo configurar un diario (journal) del sistema persistente.

Cómo utilizar redhat-support-tool.

Cómo utilizar sosreport.

Cómo configurar Red Hat Insights.
