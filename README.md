Hentaila TV — Análisis Técnico de API Implícita & Scraping (No oficial)

> Estado: Investigación técnica / educativa
Stack: WordPress + Plugins custom + HLS
Tipo: API implícita (no documentada)
Nivel: Intermedio / Avanzado




---

⚠️ Disclaimer técnico

No es una API oficial ni estable.

No se documenta bypass de DRM, cifrado fuerte ni evasión de protecciones activas.

Todo se basa en endpoints públicos, tráfico del frontend y assets accesibles.

Ideal para aprendizaje de scraping, reverse engineering web y bots privados.



---

📑 Tabla de contenidos

1. Introducción y alcance


2. Arquitectura general


3. Reconocimiento (Recon)


4. Robots.txt


5. Sitemaps (Indexación masiva)


6. REST API WordPress


7. AJAX interno (admin-ajax)


8. Plugin player-logic


9. Flujo real del reproductor


10. Tokens y contexto


11. Estrategias de scraping


12. Modelo de datos


13. Errores comunes


14. Seguridad y limitaciones


15. Casos de uso


16. Notas finales




---

1 Introducción y alcance

Este documento describe cómo Hentaila expone datos sin una API pública, usando:

WordPress REST

Sitemaps

Plugins personalizados

Player con inicialización vía JS


No se cubre automatización agresiva ni evasión de medidas anti-bot.


---

2 Arquitectura general

Usuario/Bot
   │
   ├── HTML (posts, capítulos)
   │
   ├── REST API (wp-json)
   │
   ├── XML (sitemaps)
   │
   └── Player Logic
           └── HLS (.m3u8)

Tecnologías detectadas:

WordPress

Cloudflare

HLS.js (frontend)

Plugin custom (player-logic)



---

3 Reconocimiento (Recon)

Endpoints base

/
/robots.txt
/sitemap_index.xml
/wp-json

Técnicas usadas

Inspección de Network (XHR / Fetch)

Descarga directa de JS

Curl desde terminal móvil



---

4 Robots.txt

https://hentaila.tv/robots.txt

Utilidad:

Revela rutas indexables

No bloquea sitemaps


> Nota: robots.txt no es seguridad, solo cortesía.




---

5 Sitemaps (Indexación masiva)

Índice principal

https://hentaila.tv/sitemap_index.xml

Sitemaps detectados

URL	Contenido

/page-sitemap.xml	Páginas
/wp-manga-sitemap.xml	Obras
/wp-manga-chapters-sitemap*.xml	Capítulos
/wp-manga-genre-sitemap.xml	Géneros
/wp-manga-tag-sitemap.xml	Tags
/wp-manga-author-sitemap.xml	Autores


Ventaja clave:

> Permite scraping ordenado, rápido y sin crawling agresivo.




---

6 REST API WordPress

Raíz

https://hentaila.tv/wp-json

Namespace principal

/wp-json/wp/v2/

Endpoints útiles

Endpoint	Función

/posts	Listar contenido
/posts?search=	Buscar
/posts?slug=	Obtener por slug
/categories	Categorías
/tags	Etiquetas


Características

JSON limpio

Paginación estándar

HTML embebido en campos



---

7 AJAX interno (admin-ajax)

https://hentaila.tv/wp-admin/admin-ajax.php

Observaciones

Respuesta 0 sin contexto

Requiere action válida

Muchas acciones solo funcionan desde frontend autenticado


> ⚠️ No es una API usable por sí sola.




---

8 Plugin player-logic

Endpoint crítico

/wp-content/plugins/player-logic/player.php?data=TOKEN

Qué es

Punto de entrada del reproductor

Devuelve HTML + JS


Assets

/assets/js/player.js
/assets/css/player.css


---

9 Flujo real del reproductor

Capítulo HTML
   ↓
Extraer TOKEN
   ↓
player.php?data=TOKEN
   ↓
JS inicializa HLS
   ↓
.m3u8 desde CDN

El stream nunca se expone directamente en REST.


---

🔑 10 Tokens y contexto

TOKEN:

Generado server-side

Dependiente del capítulo

Puede expirar


No reutilizable de forma genérica.


---

🕷️ 11 Estrategias de scraping

Recomendada (low profile)

Sitemap → HTML → metadatos


Controlada

Capítulo → token → player


Evitar

Fuerza bruta

Crawling sin delay



---

📦 12 Modelo de datos

Obra

id

slug

título

sinopsis

tags


Capítulo

número

url

token



---

🚨 13 Errores comunes

Error	Causa

0	AJAX sin action
403	Cloudflare
Token inválido	Expirado



---

🔐 14 Seguridad y limitaciones

No hay API pública documentada

Tokens ligados a sesión/contexto

Cloudflare activo
