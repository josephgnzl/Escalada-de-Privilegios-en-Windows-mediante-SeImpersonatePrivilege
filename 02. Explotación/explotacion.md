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

