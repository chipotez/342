
Ejercicio de recuperacoin de información.

Cuando se realiza una inspeccion de SELinux en busca de denegaciones de servicios que han sudedio en un
dia en especifico se puede usar 

# ausearch -i -m avc -ts today

el parametro *-m* nos permite mostrar tipos de mensaje en particular, para escoger de un tipo asegurese de ver la siguiente tabla.
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-audit_record_types


