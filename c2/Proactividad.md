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
=                   Monitoreo de Sistema                      =
=                                                             =
===============================================================


===============================================================
=                                                             =
=                         SeccionIII                          =
=                   Monitoreo de Sistema                      =
=                                                             =
===============================================================



===============================================================
=                                                             =
=                         SeccionIV                           =
=                   Monitoreo de Sistema                      =
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
