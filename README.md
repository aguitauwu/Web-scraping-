**🧠✨ Hentaila Reverse API**

> Documentación técnica no oficial
Investigación de endpoints públicos, arquitectura del reproductor, automatización y scraping educativo sobre hentaila.tv.



<div align="center">   

</div>
---

⚠️ Aviso Legal

> Este proyecto es educativo y de investigación técnica.
No está afiliado ni respaldado por hentaila.tv.
No promueve piratería, redistribución ni bypass de protecciones.
Usa esta información bajo tu propia responsabilidad y respeta las leyes locales y los Términos de Servicio.




---

📑 Tabla de Contenidos

Introducción

Arquitectura del sitio

Superficies de API descubiertas

Endpoints detallados

Player Logic & iframe

Sistema de tokens (nonces)

Parámetro data (estado real)

Metadata y episodios

Pruebas manuales (curl)

Automatización

Ejemplos de respuestas

Headers recomendados

Herramientas

Seguridad y limitaciones

Qué se logró realmente

Ideas futuras

Créditos



---

🌐 Introducción

Este repositorio documenta superficies públicas reales expuestas por un sitio WordPress moderno que sirve contenido multimedia mediante un plugin personalizado (player-logic).

La investigación se realizó:

📱 Desde dispositivo móvil

❌ Sin PC

❌ Sin herramientas avanzadas

❌ Sin acceso privado

✅ Solo observación, Network y pruebas pasivas


Objetivos

Entender el flujo real del reproductor

Identificar endpoints públicos

Reproducir llamadas con herramientas estándar

Diseñar automatización rate-limit friendly

Delimitar qué es posible y qué no, legalmente



---

🏗️ Arquitectura del Sitio

graph LR
A[Cliente / Browser] -->|GET| B[WordPress]
B -->|HTML + JS| C[player-logic plugin]
C -->|iframe| D[player.php]
D -->|JS runtime| E[admin-ajax.php]
E -->|JSON controlado| C

Capas identificadas

Frontend → WordPress + JS

Backend → admin-ajax.php

Player → plugin player-logic

Control → nonce dinámico por sesión

Stream → nunca expuesto directamente en HTML



---

🔎 Superficies de API Descubiertas

1️⃣ WordPress REST API (estándar)

https://hentaila.tv/wp-json/

Ejemplos:

/wp-json/wp/v2/posts
/wp-json/wp/v2/categories
/wp-json/wp/v2/tags

Devuelve:

IDs

títulos

slugs

fechas

metadata pública


📌 Útil para scraping de catálogo, NO para video


---

2️⃣ Player Logic API (plugin)

https://hentaila.tv/wp-json/player-logic/v1/

Existe

Parcialmente accesible

Algunos endpoints requieren nonce

Otros no devuelven nada útil directamente



---

3️⃣ AJAX Backend (clave real)

https://hentaila.tv/wp-admin/admin-ajax.php

Acciones observadas (por JS y Network):

load_episode

get_episode

fetch_player

get_video

get_sources


📌 Sin parámetros válidos → responde 0


---

🎥 Player Logic & iframe

El reproductor NO usa una URL de video directa en la página.

Ejemplo real observado:

<iframe
  src="https://hentaila.tv/wp-content/plugins/player-logic/player.php?data=BASE64_STRING"
  frameborder="0"
  allowfullscreen>
</iframe>

Flujo real

1. Página del episodio carga


2. Se inyecta iframe


3. player.php recibe data


4. JS del plugin procesa data


5. Se solicita info vía AJAX con nonce


6. El stream se carga en runtime




---

🔐 Sistema de Tokens (Nonces)

¿Qué es un nonce?

Token temporal de WordPress

Vinculado a sesión + contexto

Protege llamadas AJAX

Expira


Se inyecta vía JS, normalmente desde:

/wp-content/plugins/player-logic/assets/js/player.js

Ejemplo conceptual:

var playerLogic = {
  nonce: "abc123",
  episode_id: 4567
}

Flujo del nonce

graph TD
A[Visitar episodio] --> B[JS obtiene nonce]
B --> C[POST admin-ajax.php]
C --> D[Respuesta JSON controlada]


---

🧩 Parámetro data (estado real)

El parámetro:

player.php?data=Y3lUUk12S2paUG1j...

Estado actual

✔ Codificado / cifrado

✔ Procesado solo por JS del player

❌ No decodificado en este proyecto

❌ No documentado intencionalmente


📌 No es necesario romperlo para entender la arquitectura.


---

🧠 Metadata y Episodios

Se encontró metadata semántica completa:

<div itemscope itemtype="https://schema.org/VideoObject">
  <meta itemprop="name" content="Reika wa Karei na Boku no Joou - Episodio 1">
  <meta itemprop="thumbnailUrl" content="poster.jpg">
  <meta itemprop="uploadDate" content="2025-11-06">
</div>

Esto permite:

Listar episodios

Asociar IDs

Extraer posters

Fechas

Slugs


✔ Scraping pasivo válido


---

🧪 Pruebas Manuales

❌ Sin nonce

curl -X POST \
  -d "action=get_sources" \
  https://hentaila.tv/wp-admin/admin-ajax.php

Respuesta:

0


---

⚠️ Con nonce (teórico / observado)

curl -X POST \
  -d "action=get_sources" \
  -d "nonce=NONCE_VALIDO" \
  -d "episode_id=12345" \
  https://hentaila.tv/wp-admin/admin-ajax.php

Respuesta típica (estructura):

{
  "success": true,
  "data": {
    "sources": [
      {
        "label": "720p",
        "file": "https://cdn.example/video.m3u8"
      }
    ]
  }
}

📌 No se fuerza ni se automatiza en este repo


---

🤖 Automatización

Permitida / documentada

HTML scraping

REST API WordPress

Metadata

Posters

Slugs

Conteo de episodios


Ejemplo mínimo

curl https://hentaila.tv/ver/... | grep VideoObject

🚫 No incluida:

Descarga de video

Bypass de nonce

Requests masivos



---

🧾 Headers Recomendados

-H "User-Agent: Mozilla/5.0"
-H "Referer: https://hentaila.tv/"
-H "Accept: application/json"


---

🛠️ Herramientas Útiles

curl → pruebas rápidas

jq → parseo JSON

grep / sed → extracción

httpie → POST legibles

python → automatización ética

node → wrappers / bots metadata



---

🔒 Seguridad y Limitaciones

Nonces expiran

Cloudflare activo

Rate-limit posible

Plugin cambia con frecuencia

Streams no expuestos directamente


Buenas prácticas:

Cachear metadata

Backoff

No flood

Uso educativo



---

✅ Qué se logró realmente

✔ Identificar arquitectura completa
✔ Confirmar plugin y flujo
✔ Detectar endpoints reales
✔ Entender el rol del nonce
✔ Scraping pasivo exitoso
✔ Reproducción funcional solo vía navegador
✔ Documentación clara y legal

👉 Sí: esto cuenta como web scraping técnico válido


---

🚀 Ideas Futuras

Wrapper Node.js (solo metadata)

CLI scraper educativo

Bot de Discord (catálogo)

Swagger fake docs

Comparativa con Rule34 / sitios similares



---

🧾 Créditos

pos a mi xd
y a demasiada curiosidad


---

⭐ Si este repo te sirvió, deja una estrella y compártelo con otros devs curiosos.