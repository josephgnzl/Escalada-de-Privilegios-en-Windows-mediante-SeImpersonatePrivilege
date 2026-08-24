## Mitigación 

La mitigación debe centrarse en evitar que cuentas o servicios comprometibles dispongan de `SeImpersonatePrivilege` sin una necesidad legítima.

- Ejecutar servicios con el mínimo privilegio necesario.
- Revisar periódicamente las cuentas que tienen asignado `SeImpersonatePrivilege`.
- Evitar ejecutar aplicaciones no confiables bajo cuentas de servicio privilegiadas.
- Mantener Windows actualizado para reducir la exposición a técnicas conocidas de `impersonation`.
- Monitorizar comportamientos anómalos relacionados con creación de procesos, `token impersonation` y ejecución desde directorios temporales.

No se recomienda eliminar `SeImpersonatePrivilege` de forma indiscriminada, ya que determinados servicios legítimos de Windows pueden requerirlo.

h4cktheplanet, Joseph.
