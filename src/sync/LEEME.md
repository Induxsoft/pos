### Dónde va cada cosa en el servidor de la central

No basta con que la central sea alcanzable por HTTP: varios de estos
archivos tienen que existir físicamente en ese servidor, en la ruta que
sus propios `#include` esperan.

| Archivo | Ubicación en la central |
|---|---|
| `pos/_services/sync-v12/index.dkl` | Servicio HTTP: el punto final que la local llama (`op=baja`, `op=tx`) |
| `pos/_services/sync-v12/sync.svc.dk` | Servicio HTTP: auxiliares de `index.dkl` |
| `config.bajada.dkl` | Carpeta `sync/`, junto a los binarios de Devkron |
| `config.import.dkl` | Carpeta `sync/`, junto a los binarios de Devkron |
| `dbr.ftt.dkl` | Junto a los binarios de Devkron, sin subcarpeta |

La ruta no es una convención suelta: la fijan los propios `#include` de
los programas que corren en la central (`rmt-baja.dkl`, `sync-import.dkl`):

```dkl
#include "dbr.ftt.dkl"              // junto al intérprete, sin prefijo
#include "sync/config.bajada.dkl"   // dentro de sync/
#include "sync/config.import.dkl"   // dentro de sync/
```

`index.dkl` y `sync.svc.dk` no son parte de ese árbol de `#include`: son
el servicio en sí, el que atiende las peticiones que la local manda por
HTTP. Se instalan donde corra ese servicio (el sitio/proceso que
publica el `endpoint` de `sync.json`), no dentro de `sync/`.

**Consecuencia si no se respeta esto:** `config.bajada.dkl` y
`config.import.dkl` se editan seguido durante el desarrollo. Si el
cambio no se sube a la copia de `sync/` en la central, la central sigue
corriendo la versión vieja — un catálogo que "no baja" pese a que el
código local luce correcto casi siempre es esto. Y si el servicio HTTP
es un proceso persistente (no se relanza por cada petición), puede que
además haya que reiniciarlo para que tome el archivo nuevo.
