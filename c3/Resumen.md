Ejercicio guiado: Tratamiento de servicios con fallas	
En este trabajo de laboratorio, resolverá un problema donde hay dos servicios que no pueden iniciarse en el arranque.

Recursos
Máquinas	
workstation

servera

Resultados
Deberá poder resolver un problema donde hay dos servicios que no pueden iniciarse en el arranque.

Configure sus sistemas para este ejercicio al ejecutar el comando lab bootservicefail setup en su sistema workstation.

[student@workstation ~#$ lab bootservicefail setup
Como parte de un proyecto más grande, su máquina servera está configurada para ejecutar contenido web y de FTP, a través de los daemons httpd y vsftpd respectivamente. Las especificaciones para este proyecto requerían que ambos daemons estén siempre en ejecución al mismo tiempo, en la misma máquina.

Uno de sus excolegas ha intentado cumplir con este requisito, pero ahora ambos servicios se niegan a iniciar en el momento del arranque. Lo han llamado para solucionar este problema. Por ahora, no necesita garantizar que los dos servicios siempre se ejecutarán simultáneamente.

Intente recrear el problema al verificar el estado de httpd y vsftpd en su máquina servera.

Verifique el estado del servicio httpd. Dado que algunas líneas de estado pueden contener información útil que supera el límite de 80 caracteres, utilice la opción -l para mostrar las líneas de estado completas.

[root@servera ~]# systemctl status -l httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/httpd.service.d
           └─10-dependencies.conf
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)

Dec 14 10:40:00 servera.lab.example.com systemd[1]: Found ordering cycle on httpd.service/start
Dec 14 10:40:00 servera.lab.example.com systemd[1]: Found dependency on vsftpd.service/start
Dec 14 10:40:00 servera.lab.example.com systemd[1]: Found dependency on httpd.service/start
Dec 14 10:40:00 servera.lab.example.com systemd[1]: Breaking ordering cycle by deleting job vsftpd.service/start
Verifique el estado del servicio vsftpd. Dado que algunas líneas de estado pueden contener información útil que supera el límite de 80 caracteres, utilice la opción -l para mostrar las líneas de estado completas.

[root@servera ~]# systemctl status -l vsftpd
● vsftpd.service - Vsftpd ftp daemon
   Loaded: loaded (/usr/lib/systemd/system/vsftpd.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/vsftpd.service.d
           └─10-dependencies.conf
   Active: inactive (dead)
....
Dec 14 10:40:00 servera.lab.example.com systemd[1]: Job vsftpd.service/start deleted to break ordering cycle starting with httpd.service/start
Parece que ambos servicios tienen una fuerte dependencia de inicio entre sí. Si observa el resultado del paso anterior, ¿qué archivos podrían ser responsables de esta dependencia? Investigue el contenido de esos archivos.

Desde el resultado de los comandos systemctl status, puede ver que se agregaron dos archivos no estándares a las especificaciones del servicio systemd:

/etc/systemd/system/httpd.service.d/10-dependencies.conf

/etc/systemd/system/vsftpd.service.d/10-dependencies.conf

Investigue el contenido de estos dos archivos.

[root@servera ~]# cat /etc/systemd/system/httpd.service.d/10-dependencies.conf
[Unit]
After=vsftpd.service
Requires=vsftpd.service
[root@servera ~]# cat /etc/systemd/system/vsftpd.service.d/10-dependencies.conf
[Unit]
After=httpd.service
Requires=httpd.service
Formule una hipótesis acerca de por qué estos dos servicios no pueden iniciarse.

Ambos servicios tienen un valor Requires para otro servicio, pero esto solo garantiza que si uno de ellos se inicia, el otro también lo hará.

Ambos servicios tienen un requisito de After en el otro servicio. Esto provocará una dependencia cíclica, que systemd interrumpirá al no iniciar estos servicios.

Describa las dos soluciones posibles.

Eliminar ambos directorios/archivos completamente, luego actualizar systemd y, por último, iniciar los servicios. Este enfoque eliminará todas las dependencias.

Cambiar una de las dos líneas After a Before garantizará que los servicios siempre se inicien juntos, sin causar una dependencia cíclica.

Intente solucionar este problema al cambiar la línea After para el servicio httpd.service a Before. Actualice systemd e inicie ambos servicios para comprobar que todo funcione.

Cambie la línea After para el servicio httpd.service a Before. Abra /etc/systemd/system/httpd.service.d/10-dependencies.conf en un editor de texto y edítelo para que tenga el siguiente texto:

[Unit]
Before=vsftpd.service
Requires=vsftpd.service
Indíquele a systemd que vuelva a cargar todos los archivos de configuración del disco.

[root@servera ~]# systemctl daemon-reload
Inicie los servicios httpd.service y vsftpd.service.

[root@servera ~]# systemctl start httpd.service vsftpd.service
Reinicie su sistema servera para comprobar que ambos servicios se iniciarán durante el arranque.

Cuando finalice, verifique su trabajo ejecutando el comando lab bootservicefail grade desde su máquina workstation.

[student@workstation ~]$ lab bootservicefail grade
Después de calificar sus sistemas, restablezca correctamente sus máquinas al estado que tenían antes de iniciar el trabajo de laboratorio, al restablecer sus máquinas virtuales o al ejecutar el siguiente comando en su sistema workstation.

[student@workstation ~]$ lab bootservicefail reset





##########################################
# Ejercicio final
##########################################

Trabajo de laboratorio: Solución de problemas de arranque	
En este trabajo de laboratorio, solucionará un problema donde un sistema no puede arrancar después de la pantalla del cargador de arranque.

Recursos
Máquinas	
workstation

servera

Resultados
Deberá poder solucionar problemas donde un sistema no puede arrancar después del cargador de arranque.

Prepare sus sistemas para este ejercicio ejecutando el comando lab bootissues setup en su sistema workstation. Esto modificará la capacidad de su sistema servera para arrancar y, luego, lo reiniciará.

[student@workstation ~]$ lab bootissues setup
Alguien decidió “agilizar” el proceso de arranque de su máquina servera. Al hacerlo, puso fin a la capacidad que dicho sistema tenía para arrancar su objetivo (target) predeterminado.

Recibirá la tarea de lograr que servera pueda arrancar nuevamente.

Abra una consulta en su máquina servera y evalúe el problema.

Arranque temporalmente su sistema servera en una configuración que funciona.

Solucione el problema de manera permanente.

Evalúe su trabajo ejecutando el comando lab bootissues grade de su máquina workstation.

[student@workstation ~]$ lab bootissues grade
Reinicie todas sus máquinas para proveer un entorno limpio para los siguientes ejercicios.







