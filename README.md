# Sonora · Fuentes

Sonora consulta [sources.json](sources.json) desde Android. El archivo indica qué catálogos integrados puede consultar la app. Tras un error de red se usan los últimos ajustes válidos, o los valores incluidos en el APK si es la primera búsqueda.

| Identificador | Catálogo |
| --- | --- |
| `live-music` | Live Music Archive (FLAC) |
| `netlabels` | Netlabels de Internet Archive (FLAC) |
| `librivox` | Audiolibros de LibriVox (MP3) |

Las fuentes responden directamente al móvil. El formato actual es `version: 1`. Quitar un identificador desactiva ese catálogo en las búsquedas posteriores. Los cambios pueden tardar unos minutos en llegar. `catalogs: []` desactiva los tres catálogos.

## Prowlarr opcional

[deploy](deploy) contiene un despliegue para un servidor con Docker Compose, un dominio que apunte al servidor y los puertos 80 y 443 disponibles. La configuración de Prowlarr se guarda en un volumen persistente.

1. En el servidor, entra en `deploy`, define `INDEXER_HOST` en el entorno o en un archivo `.env` local y ejecuta `docker compose up -d prowlarr`.
2. Abre un túnel SSH con `ssh -L 9696:127.0.0.1:9696 usuario@servidor`. En tu navegador, abre `http://127.0.0.1:9696` y configura la autenticación **Forms**, con **Authentication Required** activado para todas las direcciones. Añade tus indexadores de audio y prueba una búsqueda.
3. Ejecuta `docker compose up -d`. Caddy publica por HTTPS solo los endpoints GET de búsqueda, estado y descarga que usa Sonora. La interfaz de administración queda en el acceso local o en el túnel SSH.
4. En Sonora, abre **Ajustes → Fuentes → Indexador de audio** e introduce la URL HTTPS y la clave de API de Prowlarr. Desde **Buscar → Addons**, toca un resultado y elige Real-Debrid o TorBox.

El repositorio es público. Las claves de Prowlarr y Debrid se introducen en Sonora y se guardan cifradas en Android. Los archivos de Docker son una plantilla. Para consultar Prowlarr hace falta un despliegue activo y un indexador de audio configurado.
