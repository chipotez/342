Información con sosreport.

[student@demo ~]$ redhat-support-tool
Welcome to the Red Hat Support Tool.
Command (? for help):

la información de la cuenta en el directorio de inicio del usuario (~/.redhat-support-tool/redhat-support-tool.conf).
Si varios usuarios comparten una cuenta de Red Hat Access, la opción --global permite guardar la información de la cuenta en
/etc/redhat-support-tool.conf


Preparación de un informe de error
Defina el problema
Reúna información básica
 - ¿Qué producto y versión se ven afectados?
 - el resultado de sosreport
 - vuelco de errores de kdump 
 -fotografía digital del seguimiento de kernel mostrado en el monitor de un sistema bloqueado.
Determine el nivel de gravedad
 
Resultados
Deberá poder generar un sosreport para ayudar al Soporte de Red Hat a resolver un problema.

Mientras resolvía un problema en su sistema servera, contactó al Soporte de Red Hat para obtener ayuda. El soporte ha solicitado que genere un sosreport con el módulo xfs activo y la opción xfs.logprint habilitada.

A fines de este ejercicio, su nombre es "student" y su número de caso es 123456.

Compruebe que tiene el paquete sos instalado en su sistema servera. En caso de no tenerlo, instálelo.
[root@servera ~]# rpm -q sos
[root@servera ~]# yum -y install sos

Vea las opciones, los complementos y las opciones de complementos disponibles para sosreport.
[root@servera ~]# sosreport --help | less
[root@servera ~]# sosreport -l | less

Genere un sosreport en servera. Asegúrese de incluir la opción xfs.logprint.
[root@servera ~]# sosreport -k xfs.logprint

Copie el archivo generado al directorio de inicio para student en workstation, y extráigalo.
[student@workstation ~]$ scp root@servera:/var/tmp/sosreport-student.123456*.tar.xz .
[student@workstation ~]$ tar xvf sosreport-student.123456*.tar.xz

Inspeccione el sosreport extraído. Algunos archivos de interés:
etc/
sos_commands/
sos_reports/sos.html
sos_commands/xfs/xfs_logprint_-c.dev.vda1
Este archivo se generó mediante la opción -k xfs.logprint.
