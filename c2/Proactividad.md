Meta	
Evitar que los pequeños problemas se conviertan en grandes problemas al emplear técnicas proactivas de administración de sistemas.

Objetivos	
Monitorear las características vitales de los sistemas.
Configurar sistemas para enviar mensajes de registro a un host centralizado.
Configurar la administración de ajustes para contribuir a la administración de sistemas de gran tamaño.
Implementar el seguimiento de cambios para mantener la homogeneidad de los sistemas.

Secciones	
Monitoreo de sistemas (y ejercicio guiado)
Configuración de registros remotos (y ejercicio guiado)
Uso de administración de configuración (y ejercicio guiado)
Configuración de seguimiento de cambios (y ejercicio guiado)

Trabajo de laboratorio	
Ser proactivo


===============================================================
=                                                             =
=                         SeccionI                            =
=                   Monitoreo de Sistema                      =
=                                                             =
===============================================================

Monitoreo del sistema con cockpit
Cockpit es software desarrollado por Red Hat que proporciona una interfaz de administración Linux interactiva basada en el navegador. Su interfaz gráfica permite que los administradores de sistemas principiantes realicen tareas comunes de administración del sistema sin las habilidades requeridas en la línea de comandos. Además de facilitar la gestión de los sistemas por parte de los administradores principiantes, Cockpit también pone a disposición datos de rendimiento y configuración del sistema sin la necesidad de tener conocimientos sobre las herramientas de la línea de comandos.

Cockpit está disponible en el paquete cockpit en el repositorio de extras de Red Hat Enterprise Linux 7. La instalación de Cockpit se realiza con el siguiente comando.

[root@demo ~]# 
      
        yum install -y cockpit
      
    
Una vez instalado en el sistema, se debe iniciar cockpit antes de que se pueda acceder a través de la red.


      [root@demo ~]# 
      
        systemctl start cockpit
      
    
Se puede acceder a Cockpit remotamente a través de HTTPS utilizando un navegador web y conectándose al puerto TCP 9090. Este puerto debe abrirse en el firewall del sistema para poder acceder a Cockpit de manera remota. El puerto está preestablecido como el cockpit para firewalld, por lo que se puede conceder acceso a través del firewall, como se muestra en el siguiente ejemplo.

[root@demo ~]# firewall-cmd --add-service=cockpit --permanent
[root@demo ~]# firewall-cmd --reload
Una vez que se establece una conexión con la interfaz web de Cockpit, un usuario debe autenticarse para obtener acceso. La autenticación se realiza a través de la base de datos de la cuenta del SO local del sistema. El acceso a funciones privilegiadas de administración del sistema proporcionado por la interfaz de Cockpit, como la creación de usuarios, requerirá que el usuario inicie sesión como el usuario root.

La pantalla Dashboard en la interfaz de Cockpit proporciona un resumen de las principales métricas de rendimiento del sistema. Las métricas se informan por segundo y permiten que los administradores monitoreen la utilización de subsistemas, como CPU, memoria, red y disco.

Uso de Performance Co-Pilot
Red Hat Enterprise Linux 7 incluye un programa llamado Performance Co-Pilot, proporcionado por el paquete RPM pcp. Performance Co-Pilot, o pcp, permite que los administradores recopilen y consulten datos de varios subsistemas. Performance Co-Pilot también se implementó en Red Hat Enterprise Linux 6, y está disponible en la versión 6.6 y versiones posteriores.

Instalación de Performance Co-Pilot
Performance Co-Pilot se instala con el paquete pcp. Después de instalar pcp, la máquina tendrá el daemon pmcd necesario para la recopilación de datos del subsistema. Además, la máquina también tendrá varias herramientas de línea de comandos para consultar datos de rendimiento del sistema.


      [root@demo ~]# 
      
        yum -y install pcp
      
    
Hay varios servicios que forman parte de Performance Co-Pilot, pero el que recopila datos del sistema localmente es pmcd, el Daemon colector de métricas de rendimiento. Este servicio debe estar en ejecución para poder consultar datos de rendimiento con las utilidades de la línea de comandos de Performance Co-Pilot.

[root@demo ~]# systemctl start pmcd

[root@demo ~]# systemctl status pmcd
● pmcd.service - Performance Metrics Collector Daemon
   Loaded: loaded (/usr/lib/systemd/system/pmcd.service; enabled; vendor preset: disabled)
   Active: active (exited) since Fri 2015-12-11 08:26:38 EST; 8s ago
... Output Truncated ...

[root@demo ~]# systemctl enable pmcd
Created symlink from /etc/systemd/system/multi-user.target.wants/pmcd.service to /usr/lib/systemd/system/pmcd.service.
Uso de las utilidades de la línea de comandos pcp
El paquete pcp proporciona una variedad de utilidades de la línea de comandos para recopilar y mostrar datos en una máquina.

El comando pmstat proporciona información similar a vmstat. Como vmstat, pmstat admitirá opciones para ajustar el intervalo entre colecciones (-t) o la cantidad de muestras (-s).

[root@demo ~]# pmstat -s 5
@ Fri Nov  7 15:00:08 2014
 loadavg                      memory      swap        io    system         cpu
   1 min   swpd   free   buff  cache   pi   po   bi   bo   in   cs  us  sy  id
    0.02      0 817044    688 480812    0    0    0    0   30   33   0   0  99
    0.02      0 817044    688 480812    0    0    0    0    7   14   0   0 100
    0.02      0 817044    688 480812    0    0    0    0    7   13   0   0 100
    0.02      0 817044    688 480812    0    0    0    0    9   15   0   0 100
    0.02      0 817044    688 480812    0    0    0    9   12   22   0   0 100
pmatop proporciona un resultado similar a top de datos y estadísticas de la máquina. Incluye estadísticas de E/S de disco y estadísticas de E/S de red, así como información sobre CPU, memoria y procesos proporcionada por otras herramientas. Por defecto, pmatop se actualizará cada cinco segundos.

[root@demo ~]# pmatop
ATOP - Fri Nov  7 17:43:19 2014         0:00:05 elapsed

PRC | sys    0.06s | user   0.23s | #proc    214 | #zombie    0
CPU | sys	0% | user      2% | irq       0% | idle     97% | wait      0% |
cpu | sys       0% | user      2% | irq       0% | idle     47% | cpu01     0% |
CPL | avg1    0.05 | avg5    0.08 | avg15   0.07 | csw       211 | intr      390
 |
MEM | tot    1885M | free    795M | cache   480M | buff      0M | slab    267M |
SWP | tot	0G | free      0G |              | vmcom     1G | vmlim     0G |
PAG | scan	 0 | steal	0 | stall      0 | swin       0 | swout      0 |
DSK | vda          | busy      0% | read       0 | write      0 | avio    0 ms |
DSK | vdb          | busy      0% | read       0 | write      0 | avio    0 ms |
NET | transport    | tcpi      1M | tcpo      1M | udpi      0M | udpo      0M |
NET | network	   | ipi       1M | ipo       1M | ipfrw     0M | deliv     1M |
NET | eth0         | pcki      2M | pcko      2M | si    0 Kbps | so    0 Kpbs |

  PID SYSCPU USRCPU VGROW    RGROW  RUID   THR ST EXC S CPU  CMD
27541  0.00s  0.21s      0K    264K  root     1  --  -  S 72% pmatop
 9002  0.02s  0.00s	 0K	 0K  root     1  --  -  R  6% pmdaproc
    1  0.00s  0.00s      0K      0K  root     1  --  -  S  0% systemd
    2  0.00s  0.00s      0K      0K  root     0  --  -  S  0% kthreadd
    3  0.00s  0.00s      0K      0K  root     0  --  -  S  0% ksoftirqd/0
    5  0.00s  0.00s	 0K	 0K  root     0  --  -  S  0% kworker/0:0H
    6  0.00s  0.00s	 0K	 0K  root     0  --  -  S  0% kworker/u4:0
Performance Co-Pilot también tiene un mecanismo de consultas basadas en texto para interrogar métricas monitoreadas individualmente. Para obtener una lista de las métricas almacenadas en la base de datos de Performance Co-Pilot, use el comando pminfo y, luego, use el comando pmval con el nombre de métrica para recopilar datos acerca del elemento deseado.

[root@demo ~]# pminfo
... Output Truncated ...
proc.nprocs
proc.psinfo.pid
... Output Truncated ...

[root@demo ~]# pminfo -dt proc.nprocs

proc.nprocs [instantaneous number of processes]
    Data Type: 32-bit unsigned int  InDom: PM_INDOM_NULL 0xffffffff
    Semantics: instant  Units: none

[root@demo ~]# pmval -s 5 proc.nprocs

metric:    proc.nprocs
host:      server1.example.com
semantics: instantaneous value
units:     none
samples:   5
interval:  1.00 sec
        133
        133
        133
        133
        130
Recuperación de datos históricos de rendimiento
Performance Co-Pilot también tiene la capacidad de almacenar datos en un registro. Esta capacidad es proporcionada por el servicio pmlogger. Este servicio debe estar habilitado para que Performance Co-Pilot archive los datos de rendimiento del sistema.

[root@demo ~]# systemctl start pmlogger

[root@demo ~]# systemctl status pmlogger
● pmlogger.service - Performance Metrics Archive Logger
   Loaded: loaded (/usr/lib/systemd/system/pmlogger.service; enabled; vendor preset: disabled)
   Active: active (exited) since Fri 2015-12-11 08:28:38 EST; 6s ago
... Output Truncated ...

[root@demo ~]# systemctl enable pmlogger
Created symlink from /etc/systemd/system/multi-user.target.wants/pmlogger.service to /usr/lib/systemd/system/pmlogger.service.
Por defecto, pmlogger recopila datos por segundo y almacena datos registrados en el directorio /var/log/pcp/pmlogger/HOSTNAME. Después de que se recopilan los datos en un archivo pmlogger, se pueden usar herramientas, como pmval, para consultar datos.

El nombre del archivo de registro comenzará con la fecha con formato ISO. Además del archivo de registro, se crean varios archivos para almacenar metadatos e información de índice.

Nota
Cuando exporta el registro a otro sistema, asegúrese de copiar el archivo de registro y los archivos .meta asociados. Si falta alguno de estos archivos, las herramientas de análisis no podrán utilizar los datos de registro.

Una vez que se crea el registro, las herramientas pcp de la línea de comando utilizarán la opción -a para indicar que deben ejecutarse contra un archivo de datos en lugar de datos en tiempo real. El comando pmval también tiene opciones para especificar las horas de inicio y finalización que deben utilizarse si un administrador desea limitar los datos a un margen de tiempo específico.

[root@demo ~]# pmval -a /var/log/pcp/pmlogger/serverX.example.com/20150224.00.10.0 kernel.all.load
...
03:03:46.197       0.1100        0.1700        0.1200 
03:03:47.197       0.1100        0.1700        0.1200 
03:03:48.197       0.1100        0.1700        0.1200 
03:03:49.197       0.1100        0.1700        0.1200 
...  
Si un administrador está interesado en los promedios de carga desde las 03:03:00 hasta las 03:04:00 del 24 de febrero de 2015:

[root@demo ~]# pmval -a /var/log/pcp/pmlogger/serverX.example.com/20150224.00.10.0 kernel.all.load -S '@ Tue Feb 24 03:03:00 2015' -T '@ Tue Feb 24 03:04:00 2015'
metric:    kernel.all.load
archive:   /var/log/pcp/pmlogger/server0.example.com/20150224.00.10.0
host:      server0.example.com
start:     Tue Feb 24 03:03:00 2015
end:       Tue Feb 24 03:04:00 2015
semantics: instantaneous value
units:     none
samples:   61
interval:  1.00 sec

                 1 minute      5 minute     15 minute 
03:03:00.000       0.3000        0.2100        0.1200 
03:03:01.000       0.3000        0.2100        0.1200 
03:03:02.000       0.3000        0.2100        0.1200 
... Output Truncated ...
Puede encontrar las especificaciones de tiempo alternativas en la página del manual PCPIntro(1).

Monitoreo de sistemas remotos con Nagios
Cuando trabaja con infraestructuras de TI grandes, la mayoría de las empresas medianas y grandes requiere una solución de monitoreo centralizada capaz de encuestar el rendimiento de los sistemas remotos de la red. Algunas organizaciones se basan en sistemas y servicios de monitoreo/gestión de redes de proveedores como CA, HP, IBM y otros. Otras prefieren el código abierto, opciones de GPL en esta área, como Nagios.

Nagios permite que los administradores monitoreen el rendimiento del sistema y de la red en hosts conectados desde un servidor central. Se basa en técnicas de monitoreo activas y pasivas, algunas de las cuales pueden involucrar la instalación de agentes en los sistemas monitoreados.

Aunque Nagios es una buena herramienta de monitoreo de código abierto gratuita, actualmente se proporciona a través del repositorio EPEL (Paquetes extras para Enterprise Linux) desde el proyecto Fedora. Como tal, no es compatible con Red Hat.

Nagios es un sistema modular que consta de un paquete nagios central y funcionalidades adicionales en la forma de complementos (plug-ins). Los complementos (plug-ins) pueden ejecutarse en máquinas locales para proporcionar información que no está disponible a través de la red, como uso de espacio en disco, etc. La configuración es muy flexible y permite definiciones de períodos de tiempo, grupos de administradores, grupos de sistemas e, incluso, conjuntos de comandos personalizados.

Hay una interfaz web en el servidor principal de Nagios que puede utilizarse para configurar pruebas y ajustes para Nagios o para los hosts que está monitoreando. Los administradores pueden establecer umbrales para los indicadores de rendimiento del sistema monitoreado para que Nagios genere alertas cuando estas métricas de rendimiento queden fuera de los rangos operativos normales


***************************************************************
*                                                             *
*                         Practica I                          *
*                                                             *
***************************************************************
Ejercicio guiado: Monitoreo de sistemas	
En este trabajo de laboratorio, monitoreará el rendimiento del sistema actual e histórico a través de Performance Co-Pilot.

Recursos
Archivos	/root/cpuidle
Máquinas	
servera

Resultados
Deberá poder mostrar los indicadores de rendimiento actuales e históricos en un sistema a través de Performance Co-Pilot.

Inicie sesión en servera como el usuario root.

En este ejercicio, instalará y configurará el software de Performance Co-Pilot en servera. Utilizará las utilidades proporcionadas por el software para recopilar estadísticas de rendimiento actuales del sistema. También aprovechará la función de archivo del software y recuperará estadísticas de rendimiento históricas.

Instale el software Performance Co-Pilot en servera al instalar el paquete pcp.

[root@servera ~]# yum install -y pcp
Inicie y habilite el servicio pmcd para habilitar la recopilación de métricas de rendimiento.

[root@servera ~]# systemctl start pmcd
[root@servera ~]# systemctl enable pmcd
Created symlink from /etc/systemd/system/multi-user.target.wants/pmcd.service to /usr/lib/systemd/system/pmcd.service.
[root@servera ~]# systemctl status pmcd
● pmcd.service - Performance Metrics Collector Daemon
   Loaded: loaded (/usr/lib/systemd/system/pmcd.service; enabled; vendor preset: disabled)
   Active: active (exited) since Fri 2015-12-11 08:26:38 EST; 8s ago
     Docs: man:pmcd(8)
 Main PID: 1896 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/pmcd.service
           ├─1942 /usr/libexec/pcp/bin/pmcd
           ├─1945 /var/lib/pcp/pmdas/root/pmdaroot
           ├─1946 /var/lib/pcp/pmdas/proc/pmdaproc -d 3
           ├─1947 /var/lib/pcp/pmdas/xfs/pmdaxfs -d 11
           └─1948 /var/lib/pcp/pmdas/linux/pmdalinux

Dec 14 08:26:38 servera.lab.example.com systemd[1]: Starting Performance Metrics Collector Daemon...
Dec 14 08:26:38 servera.lab.example.com pmcd[1896]: Starting pmcd ...
Dec 14 08:26:38 servera.lab.example.com systemd[1]: Started Performance Metrics Collector Daemon.
Inicie y habilite el servicio pmlogger para configurar el registro persistente de métricas.

[root@servera ~]# systemctl start pmlogger
[root@servera ~]# systemctl enable pmlogger
Created symlink from /etc/systemd/system/multi-user.target.wants/pmlogger.service to /usr/lib/systemd/system/pmlogger.service.
[root@servera ~]# systemctl status pmlogger
● pmlogger.service - Performance Metrics Archive Logger
   Loaded: loaded (/usr/lib/systemd/system/pmlogger.service; enabled; vendor preset: disabled)
   Active: active (exited) since Fri 2015-12-11 08:28:38 EST; 6s ago
     Docs: man:pmlogger(1)
 Main PID: 2535 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/pmlogger.service
           └─6489 /usr/libexec/pcp/bin/pmlogger -P -r -T24h10m -c config.default -m pmlogger_check 20151211.08.28

Dec 14 08:28:38 servera.lab.example.com systemd[1]: Starting Performance Metrics Archive Logger...
Dec 14 08:28:38 servera.lab.example.com pmlogger[2535]: /usr/share/pcp/lib/pmlogger: Warning: Performance Co-Pilot archive ...bled.
Dec 14 08:28:38 servera.lab.example.com pmlogger[2535]: To enable pmlogger, run the following as root:
Dec 14 08:28:38 servera.lab.example.com pmlogger[2535]: # /usr/bin/systemctl enable pmlogger.service
Dec 14 08:28:38 servera.lab.example.com pmlogger[2535]: Starting pmlogger ...
Dec 14 08:28:38 servera.lab.example.com systemd[1]: Started Performance Metrics Archive Logger.
Hint: Some lines were ellipsized, use -l to show in full.
Consulte el daemon pmcd para mostrar el tiempo de inactividad por CPU para un minuto.

[root@servera ~]# pmval -T 1minute kernel.percpu.cpu.idle
metric:    kernel.percpu.cpu.idle
host:      servera.lab.example.com
semantics: cumulative counter (converting to rate)
units:     millisec (converting to time utilization)
samples:   61
interval:  1.00 sec

              cpu0                  cpu1    
               0.9997                0.9997 
               0.9997                0.9997 
... Output omitted ...
Recupere la ubicación del registro de archivos en el que el proceso pmlogger principal está escribiendo.

[root@servera ~]# pcp | grep 'primary logger'
 pmlogger: primary logger: /var/log/pcp/pmlogger/servera.lab.example.com/20151211.08.28
A través del resultado del comando anterior, determine la iteración más reciente del registro de archivos al examinar los datos de fecha y hora contenidos en el nombre del archivo.

[root@servera ~]# ls -1 /var/log/pcp/pmlogger/servera.lab.example.com/20151211.08.28.[0-9]* | tail -1
/var/log/pcp/pmlogger/servera.lab.example.com/20151211.08.28.0
Use el comando pmval para obtener estadísticas históricas del tiempo de inactividad por CPU en intervalos de un minuto desde el registro de archivo más reciente. Guarde el resultado en el archivo /root/cpuidle.

[root@servera ~]# pmval -t1minute kernel.percpu.cpu.idle -a /var/log/pcp/pmlogger/servera.lab.example.com/20151211.08.28.0 > /root/cpuidle

pmval: pmFetch: End of PCP archive log
Evalúe su trabajo y, luego, realice una limpieza en sus sistemas para el próximo ejercicio.

Evalúe su trabajo.

[student@workstation ~]$ lab pcp grade
Realice una limpieza en sus sistemas para el próximo ejercicio.

[student@workstation ~]$ lab pcp reset


===============================================================
=                                                             =
=                         SeccionII                           =
=             Configuración de registros remotos              =
=                                                             =
===============================================================

Registro remoto
La configuración estándar de la gestión de registros del sistema rota archivos de registro todas las semanas y los conserva durante cuatro rotaciones. A veces se desea mantener registros durante más de cuatro semanas (que es el valor predeterminado), especialmente cuando establece tendencias de rendimiento del sistema relacionadas con tareas, como cierres financieros al final del mes, que se ejecutan solo una vez por mes. Al enviar mensajes de registro a un host de registro remoto con un almacenamiento masivo especializado, los administradores pueden conservar grandes archivos de registros del sistema para sus sistemas sin cambiar la configuración predeterminada de rotación de registros, que tiene como intención evitar que los registros consuman almacenamiento en disco de manera excesiva.

La recopilación central de los mensajes de registro del sistema también puede ser muy útil para monitorear el estado de los sistemas e identificar rápidamente los problemas. Además, proporciona una ubicación de copia de seguridad para los mensajes de registro en caso de que el sistema tenga un error grave en el disco duro u otros problemas, que hacen que los registros locales no estén disponibles. En estas situaciones, la copia de mensajes de registro que residen en el host de registro central puede utilizarse para ayudar a diagnosticar el error que causó el problema.

El registro estandarizado de sistemas es implementado en Red Hat Enterprise Linux 7 por el servicio rsyslog. Los programas del sistema pueden enviar mensajes de syslog al servicio rsyslogd local que, a su vez, redireccionará esos mensajes a los archivos que están en /var/log, servidores de registro remotos u otras bases de datos, según los parámetros que estén en su archivo de configuración, /etc/rsyslog.conf.

Los mensajes de registro tienen dos características que se usan para clasificarlos. La utilidad (facility) de un mensaje de registro indica qué tipo de mensaje es. La prioridad (priority), por otra parte, indica la importancia del evento registrado en el mensaje.

Tabla 2.1. Niveles de prioridad de syslog

Prioridad (Priority)	Significado
emerg	El sistema no se puede usar.
alert	Se requiere acción inmediata.
crit	Condiciones críticas.
err	Condiciones de error.
warning	Advertencia.
notice	Condiciones normales, pero importantes.
info	Mensajes informativos
debug	Mensajes de depuración.
Configuración de un host de registro central
La implementación de un host de registro central requiere la configuración del servicio rsyslog en dos tipos de sistemas: los sistemas remotos desde donde se originan los mensajes de registro y el host de registro central que recibe los mensajes. En el host de registro central, el servicio rsyslog debe configurarse para que se acepten los mensajes de registro de hosts remotos.

Para configurar el servicio rsyslog en el host de registro central para que acepte registros remotos, quite los comentarios de las líneas de recepción TCP o UDP en la sección de módulos en el archivo /etc/rsyslog.conf.

Para la recepción UDP:

# Provides UDP syslog reception
$ModLoad imudp.so
$UDPServerRun 514
o para la recepción TCP:

# Provides TCP syslog reception
$ModLoad imtcp.so  
$InputTCPServerRun 514
TCP proporciona una entrega más confiable de los mensajes de registro remoto, pero UDP es compatible con una variedad más amplia de sistemas operativos y dispositivos de red.

Importante
El transporte TCP sin formato de los mensajes de syslog está bastante más implementado, aunque todavía no está estandarizado. La mayoría de las implementaciones actualmente usan el puerto TCP 514, que es el puerto rshd heredado. Si el sistema tiene el paquete rsh-server instalado y está usando el servicio rshd inseguro y antiguo, se producirá un conflicto con el puerto TCP 514 para la recepción de syslog de TCP sin formato. Configure el servidor de registro para que use un puerto diferente; para hacerlo, cambie la configuración de $InputTCPServerRun.

Las reglas contenidas en /etc/rsyslog.conf están configuradas por defecto para admitir el registro de mensajes en un único host. Por lo tanto, ordena y agrupa los mensajes por instalación. Por ejemplo, los mensajes de correo se envían a /var/log/maillog mientras que los mensajes generados por el daemon crond se consolidan en /var/log/cron para facilitar la búsqueda de cada tipo de mensaje.

Aunque el orden de los mensajes por instalación (facility) es ideal en un único host, produce un resultado inapropiado en un host de registro central debido a que mezcla los mensajes de diferentes hosts remotos. Por lo general, en un host de registro central es mejor que los mensajes de registro de sistemas remotos permanezcan por separado. Esta separación puede alcanzarse al definir nombres de archivos de registro dinámicos a través de la función de plantilla de rsyslog.

Las plantillas se definen en /etc/rsyslog.conf y pueden utilizarse para generar reglas con nombres de archivos de registro dinámicos. Una definición de plantilla consta de la directiva $template, seguida del nombre de una plantilla y, luego, una cadena que representa el texto de la plantilla. El texto de la plantilla puede ser dinámico a través del uso de valores sustituidos de las propiedades de un mensaje de registro. Por ejemplo, para direccionar mensajes de syslog cron de diferentes sistemas a un host de registro central, utilice la siguiente plantilla para generar nombres de archivos de registro dinámicos en la propiedad HOSTNAME de cada mensaje:

$template DynamicFile,"/var/log/loghost/%HOSTNAME%/cron.log"
El nombre de plantilla puede hacer referencia al archivo dinámico creado a través de la definición de plantilla en una regla como la siguiente:

cron.*   ?DynamicFile
En sistemas que realizan un registro extremadamente detallado, es recomendable desactivar la sincronización del archivo de registro después de cada operación de lectura para mejorar el rendimiento. La sincronización de un archivo de registro después de cada registro puede omitirse al colocar el signo menos (-) como prefijo en el nombre del archivo de registro en una regla de registro. Sin embargo, la compensación del rendimiento mejorado crea la posibilidad de pérdida de datos de registro si el sistema colapsa inmediatamente después de un intento de escritura.

A continuación se muestra otro ejemplo del uso de plantillas para generar nombres de archivos de registro dinámicos. En este ejemplo, los mensajes de registro remotos se ordenarán por su nombre de host y valores de instalación (facility) al hacer referencia a las propiedades HOSTNAME y syslogfacility-test. Los mensajes de registro se escribirán en los nombres de archivos de registro generados de manera dinámica, y no se realizará una sincronización después de la operación de escritura.

$template DynamicFile,"/var/log/loghost/%HOSTNAME%/%syslogfacility-text%.log"
*.*   -?DynamicFile
Nota
Puede encontrar una lista completa de propiedades de mensajes de syslog facilitada por rsyslog en la sección Propiedades disponibles de la página del manual rsyslog.conf(5).

Una vez que se haya activado la recepción de syslog y el host haya creado las reglas deseadas para la separación de registros, reinicie el servicio rsyslog para que se apliquen los cambios de configuración. Además, agregue las reglas de firewall UDP o TCP para permitir el tráfico de syslog entrante y, luego, actualice firewalld.

[root@loghost ~]# systemctl restart rsyslog
[root@loghost ~]# firewall-cmd --add-port=514/udp --permanent
[root@loghost ~]# firewall-cmd --add-port=514/tcp --permanent
[root@loghost ~]# firewall-cmd --reload
Cuando se crean nuevos archivos de registro, es posible que no se incluyan en el programa de rotación de registros existente del host de registro. Esto debe solucionarse para garantizar que los nuevos archivos de registro no alcancen tamaños incontrolables. Por ejemplo, para incluir nuevos archivos de registro de ejemplos anteriores en la rotación de registros, agregue la siguiente entrada a la lista de archivos de registro en el archivo de configuración /etc/logrotate.d/syslog.

/var/log/loghost/*/*.log
Redireccionamiento de registros al host de registro central
Una vez que se configura el host de registro central para aceptar registros remotos, el servicio rsyslog puede configurarse en sistemas remotos para enviar registros al host de registro central. Para configurar una máquina a fin de que envíe registros a un servidor rsyslog remoto, agregue una línea a la sección de reglas en el archivo /etc/rsyslog.conf. En lugar del nombre del archivo, use la dirección IP del servidor rsyslog remoto. Para usar UDP, anexe la dirección IP con un solo signo @. Para usar TCP, anéxela con dos signos @ (@@).

Por ejemplo, para que todos los mensajes de prioridad info o de prioridad más alta se envíen a loghost.example.com por medio de UDP, use la siguiente línea:

*.info    @loghost.example.com
Para que todos los mensajes se envíen a loghost.example.com por medio de TCP, use la siguiente línea:

*.*    @@loghost.example.com
Otra opción es que el nombre de host de registro se anexe con :PORT, donde PORT es el puerto que está usando el servidor remoto rsyslog. Si no se indica un puerto, se asume el puerto 514 de forma predeterminada.

Una vez que haya agregado las reglas, reinicie el servicio rsyslog y envíe un mensaje de prueba con el comando logger:


    [root@logclient ~]# 
    
      logger "Test from logclient"
    
  
Verifique los registros en el servidor remoto para asegurarse de que se haya recibido el mensaje.

References
Para obtener más información, consulte las páginas del manual rsyslog.conf(5), rsyslogd(8) y logger(1).




========================================================
=                                                      =
=                  SeccionIII                          =
=           administración de configuración            =
=                                                      =
========================================================

Soluciones de gestión de configuraciones
A medida que crece la infraestructura de TI de una organización, muchos administradores descubren que el mantenimiento de las configuraciones del sistema se vuelven incontrolables. A menudo, este es el resultado de los administradores que gestionan la configuración de sus sistemas manualmente. A medida que crece la población del servidor de una organización, un enfoque manual de la configuración del sistema no solo no puede escalarse, sino que también se vuelve el principal punto de entrada de errores humanos, que impactan negativamente en la estabilidad y el funcionamiento del sistema.

Una mejor alternativa al mantenimiento manual de la configuración del sistema es utilizar una herramienta de gestión de configuraciones. Incluso las herramientas de gestión de configuraciones simples pueden mejorar considerablemente la eficiencia y estabilidad operativas. Estas herramientas pueden acelerar enormemente la implementación de cambios de configuración en una gran cantidad de sistemas. Debido a que los cambios se realizan mediante programación y de modo automatizado, también se garantiza la precisión de los cambios.

Un ejemplo de una herramienta de gestión de configuraciones es la función de Canal de configuración que se ofrece en Red Hat Satellite 5. Los canales de configuración de la red de Red Hat proporcionan una manera fácil de implementar archivos de configuración en un entorno empresarial. En lugar de realizar cambios del archivo de configuración en sistemas individuales, los administradores pueden realizar cambios en un archivo de configuración que se conserva en un Canal de configuración alojado en los servidores de Red Hat Satellite 5. Los cambios pueden implementarse en sistemas cliente suscritos al canal.

En los últimos años, las herramientas simples para gestionar archivos de configuración fueron sustituidas por sistemas de gestión de configuraciones. Estos sistemas han crecido considerablemente en los últimos años y ahora hay muchas soluciones de código abierto disponibles para satisfacer las necesidades de gestión de configuraciones de una organización. Las siguientes son opciones de herramientas de gestión de configuraciones que están disponibles para los administradores.

Ansible puede ser adecuado para las organizaciones con necesidades de gestión de configuraciones simples. Ansible está basado en el lenguaje de programación de Python. No tiene agente y se basa en SSH para implementar configuraciones en sistemas remotos.

Chef y Puppet son soluciones de gestión de configuraciones pesadas que pueden adaptarse mejor a las empresas con requisitos de gestión de configuraciones exigentes. Ambas se basan en el lenguaje de programación de Ruby. Además, Chef y Puppet utilizan arquitectura del servidor cliente, por lo que las implementaciones típicas requieren la instalación de agentes en sistemas gestionados.

Las última versión de Red Hat Satellite, Satellite 6, ofrece mucho más que la gestión de archivos de configuración. Incorpora el uso de Puppet y proporciona un sistema de gestión de configuraciones totalmente desarrollado para entornos empresariales.

Uso de Puppet para la gestión de configuraciones
Puppet permite que los administradores del sistema escriban infraestructura como código a través de lenguaje descriptivo para configurar máquinas, en lugar de utilizar scripts personalizados e individualizados para hacerlo. El lenguaje específico del dominio (DSL) de Puppet se utiliza para describir el estado de la máquina, y Puppet puede imponer este estado. Esto significa que si un administrador cambia un componente de la máquina por error, Puppet puede imponer el estado y devolver la máquina al estado deseado. Por lo tanto, el código de Puppet no solo puede utilizarse para configurar un sistema originalmente, sino que también puede utilizarse para mantener el estado del sistema acorde con la configuración deseada.

El administrador de un sistema tradicional necesitaría registrarse en cada máquina para realizar tareas de administración en el sistema, pero esto no se escala bien. Quizás, el administrador del sistema crearía un script de configuración inicial (p. ej., un archivo kickstart) para una máquina, pero una vez que se haya ejecutado el script inicial, la máquina puede comenzar (y comenzará) a apartarse de esa configuración de script inicial. En el mejor de los casos, el administrador del sistema necesitaría revisar la máquina diariamente para verificar la configuración. En el peor de los casos, el administrador del sistema necesitaría descubrir por qué la máquina ya no funciona o volver a desarrollar la máquina y volver a implementar la configuración.

Puppet se puede utilizar para establecer un estado deseado y hacer que la máquina coincida con ese estado. El administrador del sistema ya no necesita un script para la configuración inicial y un script separado (o peor aún, interacción humana) para la verificación del estado. Puppet administra la configuración del sistema y comprueba que el sistema esté en el estado deseado. Esto puede escalarse mucho mejor que el administrador de sistema tradicional que utiliza scripts individuales para cada sistema.

Arquitectura de Puppet
Puppet utiliza un modelo servidor/cliente. Al servidor se lo llama Puppet maestro. El Puppet maestro almacena instrucciones o manifiestos (código que contiene recursos y estados deseados) para los clientes. A los clientes se los llama nodos de Puppet y ejecutan el software del agente de Puppet. Por lo general, estos nodos ejecutan un daemon de Puppet (agent) que se utiliza para conectarse al Puppet maestro. Los nodos descargarán la instrucción asignada al nodo del Puppet maestro y aplicarán la configuración, en caso de ser necesaria.

Una ejecución de Puppet comienza con el nodo de Puppet (no el Puppet maestro). Por defecto, el agente de Puppet inicia una ejecución de Puppet cada 30 minutos. Esta ejecución utiliza servicios de transmisión segura (SSL) para pasar datos de un lado a otro entre el nodo de Puppet y el Puppet maestro. El nodo se inicia al recopilar datos acerca del sistema a través del comando facter. El comando facter incluye información sobre dispositivos de bloque, sistemas de archivos, interfaz de la red, direcciones MAC, direcciones IP, memoria, sistema operativo, CPU, virtualización, etc. Estos datos se envían del nodo al Puppet maestro.

Una vez que el Puppet maestro recibe los datos del nodo de Puppet, el Puppet maestro compila un catalog, que describe el estado deseado de cada recurso configurado para el nodo. El Puppet maestro verifica el nombre de host del nodo y lo vincula con la configuración específica del nodo (llamada clasificación de nodos) o usa la configuración predeterminada si el nodo no coincide. Este catálogo puede incluir información de dependencia para los recursos (p. ej., ¿Puppet debería instalar el paquete primero o iniciar el servicio primero?).

Una vez que se compila el catálogo, el Puppet maestro envía el catálogo al nodo. El Puppet aplicará el catálogo en el nodo de Puppet y configurará todos los recursos definidos en el catálogo. Puppet es idempotentes; Puppet puede aplicar el catálogo a un nodo varias veces sin afectar el estado resultante.

Una vez que el agente de Puppet aplica el catálogo al nodo, el nodo le informará al Puppet maestro los detalles de la ejecución. Este informe incluye información sobre los cambios que se realizaron en el nodo (si corresponde) y si la ejecución se completó correctamente. La infraestructura de informes de Puppet tiene una API, para que otras aplicaciones puedan descargar informes del Puppet maestro para almacenamiento o análisis adicional.

Configuración de un cliente de Puppet
Aunque Puppet puede ejecutarse en el modo independiente, donde todos los clientes de Puppet tienen módulos de Puppet localmente que se aplican al sistema, la mayoría de los administradores de sistemas descubren que Puppet funciona mejor con un Puppet maestro centralizado. El primer paso en la implementación de un cliente de Puppet es instalar el paquete puppet:


    [root@demo ~]# 
    
      yum install -y puppet
    
  
Una vez que se instala el paquete puppet, el cliente de Puppet debe configurarse con el nombre de host del Puppet maestro. El nombre de host del Puppet maestro debe colocarse en el archivo /etc/puppet/puppet.conf, en la sección [agent]. El siguiente fragmento muestra un ejemplo donde el Puppet maestro es denominado puppet.lab.example.com:

[agent]
    server = puppet.lab.example.com
Nota
Cuando no se define un Puppet maestro explícito, Puppet utiliza un nombre de host predeterminado de puppet. Si la ruta de búsqueda DNS incluye un host denominado puppet, este host se utilizará automáticamente.

El paso final que se debe tomar en el cliente de Puppet es comenzar el servicio del agente de Puppet y configurarlo para que se ejecute en el momento del inicio.

[root@demo ~]# systemctl start puppet.service
[root@demo ~]# systemctl enable puppet.service
ln -s '/usr/lib/systemd/system/puppet.service'
 '/etc/systemd/system/multi-user.target.wants/puppet.service'
 
Cuando el servicio del agente de Puppet se inicia por primera vez, generará un certificado de host y enviará una solicitud de firma de certificado al Puppet maestro. Una vez que se haya enviado la solicitud de certificado cliente, la solicitud para firmar el certificado cliente se puede ver en el Puppet maestro. Por defecto, la firma de certificados cliente se realiza manualmente en el Puppet maestro. Sin embargo, el Puppet maestro también puede configurarse para firmar automáticamente certificados cliente para que no se necesite una intervención humana para nuevos registros de clientes. Una vez que el Puppet maestro haya firmado el certificado, el agente de Puppet recibirá un catálogo y aplicará los cambios necesarios.

Nota
El paquete puppet viene con el repositorio de Satellite Tools 6.1.

References
Para obtener más información acerca de Puppet, consulte Introducción a Puppet

Para obtener más información acerca de facter, consulte la página del manual facter(8).

Para obtener más información acerca de la configuración de un cliente de Puppet, consulte Instalación de Puppet: Tareas posteriores a la instalación



===============================================================
=                                                             =
=                         SeccionIV                           =
=        Configuración de seguimiento de cambios              =
=                                                             =
===============================================================


===============================================================
=                                                             =
=                         Referencias                         =
=                                                             =
=                                                             =
===============================================================

Seccion I

Para obtener más información acerca de Cockpit, consulte Proyecto de Cockpit
https://cockpit-project.org/

Para obtener más información acerca de Performance Co-Pilot, consulte las páginas del manual pmstat(1), pmatop(1), pminfo(1), pmval(1) y PCPIntro(1).

Para obtener más información acerca de Nagios, consulte Proyecto de Nagios
https://www.nagios.org/

Performance Analysis with Performance Co-Pilot (PCP)
https://www.redhat.com/en/blog/performance-analysis-performance-co-pilot-pcp

Getting Started: Using Performance Co-Pilot and Vector for Browser Based Metric Visualizations
https://rhelblog.redhat.com/2015/12/18/getting-started-using-performance-co-pilot-and-vector-for-browser-based-metric-visualizations/

Performance Co-Pilot
https://pcp.io/

Side-by-side comparison of PCP tools with legacy tools
https://access.redhat.com/articles/2372811

Performance Analysis with PCP 
https://events.static.linuxfound.org/sites/events/files/slides/Paul_Evans_PCP_R.pdf

User's and Administrator's Guide
https://pcp.io/books/PCP_UAG/html-single/




Seccion III
Introducción a Puppet
https://docs.puppet.com/puppet/3.6/index.html

configuración de un cliente de Puppet
https://docs.puppet.com/puppet/3.8/post_install.html


