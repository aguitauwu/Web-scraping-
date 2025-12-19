🧠✨ Hentaila Reverse API

> Documentación técnica no oficial — investigación de endpoints públicos, automatización y scraping educativo sobre hentaila.tv.



<div align="center">  

</div>
---

⚠️ Aviso Legal

> Este proyecto es educativo y de investigación. No está afiliado ni respaldado por hentaila.tv. Usa esta información bajo tu propia responsabilidad y respeta las leyes locales y los Términos de Servicio de los sitios.




---

📑 Tabla de Contenidos

• Introducción
• Arquitectura del sitio
• Superficies de API descubiertas
• Endpoints detallados
• Sistema de tokens (nonces)
• Pruebas manuales (curl)
• Automatización
• Ejemplos de respuestas
• Headers recomendados
• Herramientas
• Seguridad y limitaciones
• Ideas futuras
• Créditos


---

🌐 Introducción

Este repositorio documenta endpoints expuestos públicamente (WordPress REST, AJAX y plugins) identificados durante una sesión de investigación desde un dispositivo móvil, sin PC ni herramientas avanzadas.

Objetivos:

Entender el flujo real de datos del reproductor

Reproducir llamadas con herramientas estándar

Diseñar automatización segura (rate‑limit friendly)



---

🏗️ Arquitectura del Sitio

graph LR

 A[Cliente] -->|GET| B[WordPress]

 B -->|JS| C[player-logic]

 C -->|nonce| D[admin-ajax.php]

 D -->|JSON| A

Frontend  → WordPress + JS

Backend   → admin-ajax.php

Player    → plugin player-logic

Control   → Nonce dinámico por sesión


---

🔎 Superficies de API Descubiertas

1️⃣ WordPress REST API (estándar)

https://hentaila.tv/wp-json/

Ejemplos:

https://hentaila.tv/wp-json/wp/v2/posts
https://hentaila.tv/wp-json/wp/v2/categories
https://hentaila.tv/wp-json/wp/v2/tags

Devuelve: JSON estándar (IDs, títulos, fechas, slugs, metadata)


---

2️⃣ Player Logic API (plugin)

https://hentaila.tv/wp-json/player-logic/v1/

> ⚠️ Algunos endpoints requieren nonce válido generado por JS.




---

3️⃣ AJAX Backend (clave)

https://hentaila.tv/wp-admin/admin-ajax.php

Acciones observadas:

load_episode
get_episode
fetch_player
get_video
get_sources

> Sin parámetros correctos → responde 0




---

🔐 Sistema de Tokens (Nonces)

¿Qué es un nonce?

• Token temporal
• Generado por WordPress
• Valida llamadas AJAX
• Expira por sesión

Se inyecta vía JS, normalmente en:

/wp-content/plugins/player-logic/assets/js/player.js

Ejemplo (simplificado):

var playerLogic = { nonce: "abc123" }

Flujo del nonce

graph TD
A[Visitar episodio] --> B[JS carga nonce]
B --> C[POST admin-ajax]
C --> D[JSON con sources]


---

🧪 Pruebas Manuales

❌ Llamada sin nonce

curl -X POST \
  -d "action=get_sources" \
  https://hentaila.tv/wp-admin/admin-ajax.php

Respuesta:

0


---

✅ Llamada con nonce

curl -X POST \
  -d "action=get_sources" \
  -d "nonce=NONCE_AQUI" \
  -d "episode_id=12345" \
  https://hentaila.tv/wp-admin/admin-ajax.php

Respuesta típica:

{
  "success": true,
  "data": {
    "sources": [
      { "label": "720p", "file": "https://cdn.example/video.mp4" }
    ]
  }
}


---

🤖 Automatización

Bash (mínimo)

#!/usr/bin/env bash
URL="https://hentaila.tv/wp-admin/admin-ajax.php"
NONCE="xxxx"
EP="12345"

curl -s -X POST \
  -H "User-Agent: Mozilla/5.0" \
  -H "Referer: https://hentaila.tv/" \
  -d "action=get_sources" \
  -d "nonce=$NONCE" \
  -d "episode_id=$EP" \
  "$URL" | jq

Extracción automática del nonce (idea)

nonce["']\s*:\s*["']([a-zA-Z0-9]+)["']


---

📦 Ejemplos de Respuestas

Endpoint	Contenido

/wp/v2/posts	IDs, títulos, fechas
admin-ajax	URLs reales del video
player-logic	Config del reproductor
JS player	nonce + episode_id



---

🧾 Headers Recomendados

-H "User-Agent: Mozilla/5.0"
-H "Referer: https://hentaila.tv/"
-H "Accept: application/json"


---

🛠️ Herramientas Útiles

curl      → pruebas rápidas
httpie   → POST legibles
jq        → parseo JSON
grep/sed → extracción tokens
python   → automatización
node     → bots / wrappers


---

🔒 Seguridad y Limitaciones

• Nonces expiran
• Rate‑limit posible
• Cloudflare activo
• Cambios frecuentes del plugin

Buenas prácticas:

Cachear nonces

Reintentos con backoff

No saturar endpoints



---

🚀 Ideas Futuras

• Wrapper Node.js
• Bot de Discord
• CLI scraper
• Docs Swagger (fake)
• Comparativa con Rule34 API


---

🧾 Créditos

pos a mi xd



---

⭐ Si este repo te sirvió, deja una estrella y compártelo con otros devs curiosos.