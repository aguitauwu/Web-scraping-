# 🧠✨ Hentaila Reverse API
### _Investigación técnica, scraping educativo y análisis de arquitectura_

<div align="center">

![status](https://img.shields.io/badge/status-research-blueviolet)
![wordpress](https://img.shields.io/badge/platform-WordPress-21759B)
![api](https://img.shields.io/badge/type-reverse--engineering-orange)
![educational](https://img.shields.io/badge/use-educational-green)
![device](https://img.shields.io/badge/made%20from-mobile%20only-red)

</div>

---

## ⚠️ Aviso Legal

> **Este proyecto es únicamente educativo y de investigación técnica.**  
> No está afiliado, respaldado ni aprobado por **hentaila.tv**.  
> No promueve piratería, redistribución de contenido ni bypass de protecciones.  
> Toda la información documentada proviene de **endpoints públicos y observación pasiva**.

---

## 📚 Tabla de Contenidos

- 📌 Introducción  
- 🏗️ Arquitectura del sitio  
- 🔍 Superficies de API descubiertas  
- 🎥 Player Logic e iframe  
- 🔐 Sistema de tokens (nonces)  
- 🧩 Parámetro `data`  
- 🧠 Metadata y episodios  
- 🧪 Pruebas manuales  
- 🤖 Automatización  
- 📊 Tabla de endpoints  
- 🧾 Headers recomendados  
- 🛠️ Herramientas  
- 🔒 Seguridad y limitaciones  
- ✅ Qué se logró realmente  
- 🚀 Ideas futuras  
- 🧾 Créditos  

---

## 📌 Introducción

Este repositorio documenta el análisis técnico de **hentaila.tv**, un sitio basado en **WordPress** que utiliza un **plugin personalizado (`player-logic`)** para servir contenido multimedia mediante un reproductor embebido.

### Objetivos

- Comprender el **flujo real del reproductor**
- Identificar **endpoints públicos**
- Determinar el rol del **nonce**
- Diseñar scraping **ético y documentado**

---

## 🏗️ Arquitectura del Sitio

mermaid
graph LR
A[Usuario / Navegador] -->|GET| B[WordPress]
B -->|HTML + JS| C[player-logic]
C -->|iframe| D[player.php]
D -->|AJAX + nonce| E[admin-ajax.php]
E -->|JSON| C

Componentes clave

Capa	Tecnología	Rol

Frontend	WordPress + JS	Renderizado
Backend	admin-ajax.php	Control
Player	player-logic	Video
Seguridad	nonce	Validación
Stream	Runtime	Nunca directo



---

🔍 Superficies de API Descubiertas

1️⃣ WordPress REST API

https://hentaila.tv/wp-json/

Ejemplos:

/wp-json/wp/v2/posts
/wp-json/wp/v2/categories
/wp-json/wp/v2/tags

📦 Devuelve:
IDs, títulos, slugs, fechas, metadata pública.

✅ Ideal para scraping de catálogo
❌ No expone streams


---

2️⃣ Player Logic REST API

https://hentaila.tv/wp-json/player-logic/v1/

Existe

Parcialmente documentable

Algunos endpoints requieren nonce


⚠️ No usable sin contexto de sesión


---

3️⃣ AJAX Backend (núcleo real)

https://hentaila.tv/wp-admin/admin-ajax.php

Acciones observadas:

load_episode
get_episode
fetch_player
get_video
get_sources

📌 Sin parámetros válidos → respuesta: 0


---

🎥 Player Logic e iframe

El video NO está en el HTML principal.

Ejemplo real:

<iframe
  src="https://hentaila.tv/wp-content/plugins/player-logic/player.php?data=BASE64"
  frameborder="0"
  allowfullscreen>
</iframe>

Flujo real del reproductor

1. Se carga la página del episodio


2. Se inyecta el iframe


3. player.php procesa data


4. JS obtiene nonce


5. AJAX devuelve fuentes


6. Stream se reproduce en runtime




---

🔐 Sistema de Tokens (Nonce)

¿Qué es?

Token temporal de WordPress

Asociado a sesión

Protege llamadas AJAX

Expira


Ubicación típica:

/wp-content/plugins/player-logic/assets/js/player.js

Ejemplo conceptual:

playerLogic = {
  nonce: "abc123",
  episode_id: 9876
}

graph TD
A[Visitar episodio] --> B[JS obtiene nonce]
B --> C[POST admin-ajax]
C --> D[JSON controlado]


---

🧩 Parámetro data

player.php?data=Y3lUUk12S2paUG1j...

Estado real

Aspecto	Estado

Codificado	✅
Procesado por JS	✅
Decodificado aquí	❌
Necesario romper	❌


📌 No es necesario romperlo para documentar la arquitectura.


---

🧠 Metadata y Episodios

Metadata semántica encontrada:

<div itemscope itemtype="https://schema.org/VideoObject">

Incluye:

Título

Poster

Fecha

Slug

Episodio


✅ Scraping pasivo válido
✅ Sin AJAX
✅ Sin nonce


---

🧪 Pruebas Manuales

❌ Sin nonce

curl -X POST \
  -d "action=get_sources" \
  https://hentaila.tv/wp-admin/admin-ajax.php

Respuesta:

0


---

⚠️ Con nonce (estructura observada)

{
  "success": true,
  "data": {
    "sources": [
      { "label": "720p", "file": "https://cdn.example/video.m3u8" }
    ]
  }
}

🚫 No automatizado en este repo


---

🤖 Automatización (permitida)

✔ HTML scraping
✔ REST API WordPress
✔ Metadata
✔ Posters
✔ Slugs

🚫 Descarga de video
🚫 Bypass de nonce

Ejemplo:

curl https://hentaila.tv/ver/... | grep VideoObject


---

📊 Tabla Resumen de Endpoints

Endpoint	Tipo	Uso

/wp-json/wp/v2/posts	REST	Catálogo
/wp-json/player-logic/v1/	REST	Player
/admin-ajax.php	AJAX	Control
player.php	iframe	Runtime



---

🧾 Headers Recomendados

-H "User-Agent: Mozilla/5.0"
-H "Referer: https://hentaila.tv/"
-H "Accept: application/json"


---

🛠️ Herramientas

Tool	Uso

curl	Requests
jq	JSON
grep/sed	Parsing
httpie	POST
python	Scraping
node	Bots



---

🔒 Seguridad y Limitaciones

Nonce expira

Cloudflare activo

Rate-limit posible

Plugin cambia


✔ Usar backoff
✔ No flood
✔ Uso educativo


---

✅ Qué se logró realmente

✔ Arquitectura completa
✔ Plugin identificado
✔ Endpoints reales
✔ Scraping pasivo exitoso
✔ Documentación legal

🟢 Sí: esto cuenta como web scraping técnico real


---

🚀 Ideas Futuras

Wrapper Node.js (metadata)

CLI scraper

Bot Discord

Swagger fake

Comparativa Rule34



---

🧾 Créditos

pos a mi xd
curiosidad + una tarde libre


---

⭐ Si este repo te sirvió, deja una estrella.
