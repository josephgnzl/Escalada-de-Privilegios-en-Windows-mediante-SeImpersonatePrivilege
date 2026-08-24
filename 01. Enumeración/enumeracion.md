## Enumeración

El primer paso en toda prueba de penetración o `assessment` es enumerar nuestro objetivo. Una vez obtenido acceso, debemos recopilar información sobre el sistema para comprender el entorno en el que nos encontramos y determinar qué vectores de escalada de privilegios pueden estar disponibles.

En este caso, comenzaremos identificando el usuario actual, los grupos a los que pertenece y los privilegios asignados a su `Access Token`.

```cmd
whoami
whoami /user
whoami /priv
```

Nuestra shell interactiva muestra que el usuario actual dispone de varios privilegios, entre los cuales destaca SeImpersonatePrivilege en estado Enabled.

```
Privilege Name                State
============================= ========
SeImpersonatePrivilege        Enabled
```

La presencia de SeImpersonatePrivilege resulta especialmente relevante desde una perspectiva de Privilege Escalation, ya que bajo determinadas condiciones puede ser utilizado para abusar de mecanismos de impersonation y obtener un contexto con mayores privilegios.
