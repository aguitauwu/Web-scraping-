<div align="center"><h1>🧠✨ Hentaila Reverse Engineering & Scraping Research</h1><p><i>Análisis técnico completo del ecosistema WordPress, player propietario y flujo real de datos</i></p><img src="https://img.shields.io/badge/scope-research-blueviolet?style=for-the-badge">
<img src="https://img.shields.io/badge/platform-wordpress-blue?style=for-the-badge">
<img src="https://img.shields.io/badge/method-passive_analysis-green?style=for-the-badge">
<img src="https://img.shields.io/badge/legal-educational_only-success?style=for-the-badge"></div>


<div class="section warn"><h2>⚠️ Aviso Legal y Ético</h2>Este repositorio NO distribuye contenido, NO rompe cifrado, NO evade protecciones
y NO automatiza descargas.

Todo el trabajo se basa en:

• HTML público
• APIs expuestas
• Metadatos visibles
• Análisis de red pasivo
• Ingeniería inversa observacional

El objetivo es entender cómo funciona, no explotarlo.

</div>
---

📑 Tabla de Contenidos

1. Contexto de la investigación


2. Alcance real


3. robots.txt


4. Superficies públicas detectadas


5. WordPress REST API


6. Estructura de un episodio


7. Schema.org VideoObject


8. Player Logic (plugin)


9. iframe y parámetro data


10. player.php vs api.php


11. Network: por qué a veces “no hay requests”


12. admin-ajax.php


13. Sistema de nonce


14. Qué se puede scrapear legalmente


15. Qué NO se puede scrapear


16. Automatización posible


17. Limitaciones reales


18. Resultado final (qué significa “100%”)




---

<div class="section"><h2>🧠 Contexto de la Investigación</h2>La investigación se realizó:

• Sin PC
• Desde móvil
• Sin herramientas avanzadas
• Sin experiencia previa en scraping
• Usando únicamente observación, lógica y análisis

El objetivo era responder:

<b>¿Hentaila tiene una API real y es posible obtener episodios?</b>

</div>
---

<div class="section"><h2>🎯 Alcance Real</h2><table>
<tr><th>Elemento</th><th>Resultado</th></tr>
<tr><td>Listado de episodios</td><td>✔</td></tr>
<tr><td>Metadata completa</td><td>✔</td></tr>
<tr><td>Player interno</td><td>✔</td></tr>
<tr><td>Flujo de video</td><td>✔</td></tr>
<tr><td>URLs directas de video</td><td>✖ (protegidas)</td></tr>
<tr><td>Descarga de streams</td><td>✖</td></tr>
</table></div>
---

<div class="section"><h2>🤖 robots.txt</h2>robots.txt NO bloquea:

• /wp-json/
• /wp-content/
• /wp-admin/admin-ajax.php

Esto indica indexación permitida, no protección anti-scraping.

</div>
---

<div class="section"><h2>🌐 Superficies Públicas Detectadas</h2><ul>
<li class="mono">/wp-json/</li>
<li class="mono">/wp-json/wp/v2/</li>
<li class="mono">/wp-json/player-logic/v1/</li>
<li class="mono">/wp-admin/admin-ajax.php</li>
<li class="mono">/wp-content/plugins/player-logic/</li>
</ul>Todas accesibles sin login.

</div>
---

<div class="section"><h2>📦 WordPress REST API</h2>Endpoints funcionales:

<table>
<tr><th>Endpoint</th><th>Devuelve</th></tr>
<tr><td>/posts</td><td>IDs, títulos, slugs</td></tr>
<tr><td>/categories</td><td>Clasificación</td></tr>
<tr><td>/tags</td><td>Metadata</td></tr>
</table>Esto permite scraping estructural completo del catálogo.

</div>
---

<div class="section"><h2>📄 Estructura de un Episodio</h2>Cada episodio es un post WordPress con:

• HTML
• iframe del player
• metadata
• schema.org

No contiene el video directamente.

</div>
---

<div class="section"><h2>🎞️ Schema.org VideoObject</h2>Incluido en el HTML:

• name
• description
• thumbnailUrl
• uploadDate
• contentURL (no funcional real)

Esto es scraping-friendly y totalmente legal.

</div>
---

<div class="section"><h2>🧩 Player Logic (Plugin)</h2>Ubicación:

<span class="mono">/wp-content/plugins/player-logic/</span>

Componentes:

• player.php
• api.php (vacío / no público)
• JS
• CSS

El plugin controla todo el flujo del video.

</div>
---

<div class="section"><h2>🧬 iframe y parámetro data</h2>El iframe apunta a:

<span class="mono">player.php?data=ENCODED</span>

El parámetro data:

• No es base64 simple
• No contiene la URL directa
• Se usa como input interno

No es necesario romperlo para investigación.

</div>
---

<div class="section"><h2>📡 Network: ¿por qué a veces no aparece nada?</h2>Porque:

• El iframe es un contexto aislado
• El JS interno maneja el flujo
• Algunas llamadas ocurren antes del inspector
• El video puede cargarse desde cache

Esto NO significa que no exista backend.

</div>
---

<div class="section"><h2>⚙️ admin-ajax.php</h2>Es el backend real.

Acciones observadas:

<table>
<tr><th>Action</th><th>Función</th></tr>
<tr><td>get_episode</td><td>Datos del episodio</td></tr>
<tr><td>get_sources</td><td>Fuentes del player</td></tr>
</table>Sin nonce → devuelve <b>0</b>

</div>
---

<div class="section"><h2>🔐 Sistema de Nonce</h2>Propiedades:

• Token WordPress
• Temporal
• Por sesión
• Anti-CSRF

Generado vía JS del plugin.

No romperlo = comportamiento correcto.

</div>
---

<div class="section ok"><h2>✅ Scraping Legal y Ético</h2>Permitido:

• Catálogo
• Episodios
• Posters
• Metadata
• Slugs
• Fechas
• Relaciones

</div>
---

<div class="section bad"><h2>🚫 No permitido</h2>• Descargar streams
• Redistribuir contenido
• Evadir nonce
• Automatizar video

</div>
---

<div class="section"><h2>🤖 Automatización Posible</h2>Casos válidos:

• Indexador
• Buscador
• Analytics
• Comparadores
• Investigación académica

</div>
---

<div class="section ok"><h2>🏁 Resultado Final</h2><b>¿Se logró el 100%?</b>

✔ Sí, el 100% del entendimiento técnico
✖ No, el 100% del contenido multimedia (y eso es correcto)

Esto es exactamente lo que significa una investigación bien hecha.

</div>
---

<div align="center"><h3>🧠 Hecho por curiosidad, no por abuso</h3><p>Reverse engineering ≠ piratería</p></div>


---

🔬 APÉNDICE TÉCNICO — INVESTIGACIÓN HENTAILA


---

1️⃣ Comandos curl utilizados (y qué demostraron)

🔹 Test base: comprobar backend AJAX

Comando <span class="mono">curl -X POST https://hentaila.tv/wp-admin/admin-ajax.php</span>

Resultado <span class="mono">0</span>

Conclusión

El endpoint existe

WordPress responde

Falta el parámetro action

Confirma backend activo



---

🔹 Probar acción inexistente

<span class="mono">curl -X POST -d "action=test" https://hentaila.tv/wp-admin/admin-ajax.php</span>

Resultado <span class="mono">0</span>

Conclusión

WordPress ignora acciones no registradas

No hay error HTTP → handler interno

Clásico comportamiento de admin-ajax.php



---

🔹 WordPress REST API base

<span class="mono">curl https://hentaila.tv/wp-json</span>

Resultado

JSON válido

Namespaces registrados


Conclusión

REST API activa

No bloqueada

Indexable



---

🔹 Posts (episodios)

<span class="mono">curl https://hentaila.tv/wp-json/wp/v2/posts</span>

Devuelve

id

date

slug

title.rendered

content.rendered (HTML del episodio)


Conclusión 👉 Scraping estructural COMPLETO del catálogo


---

🔹 Categorías y tags

<span class="mono">curl https://hentaila.tv/wp-json/wp/v2/categories</span>
<span class="mono">curl https://hentaila.tv/wp-json/wp/v2/tags</span>

Conclusión

Clasificación accesible

Permite filtros

Ideal para bots / buscadores



---

2️⃣ URLs descubiertas (todas) y su función

🌐 Núcleo WordPress

URL	Función

/wp-json/	Índice REST
/wp-json/wp/v2/posts	Episodios
/wp-json/wp/v2/categories	Categorías
/wp-json/wp/v2/tags	Tags
/wp-admin/admin-ajax.php	Backend dinámico



---

🎞️ Player Logic (plugin)

URL	Función

/wp-content/plugins/player-logic/	Plugin
/player.php	Reproductor embebido
/api.php	Endpoint interno (no público)
/assets/js/player.js	Lógica JS
/assets/css	Estilos



---

🔌 API del plugin

URL	Estado

/wp-json/player-logic/v1/	Namespace registrado
Endpoints internos	Protegidos por nonce



---

3️⃣ Qué nos dio view-source: (clave)

🔍 HTML del episodio

Desde: <span class="mono">view-source:https://hentaila.tv/ver/SLUG/</span>

Se obtuvo:

iframe del reproductor

Schema.org VideoObject

ID del episodio

Poster

Título real

Fecha


👉 Todo sin JS ni Network


---

🧠 Schema.org detectado

Campos reales:

itemtype="https://schema.org/VideoObject"

name

description

thumbnailUrl

uploadDate

contentURL (placeholder)


Conclusión

Metadata rica

Indexación SEO

Scraping 100% legal



---

4️⃣ iframe: análisis técnico

Código observado

El iframe apunta a:

<span class="mono">/wp-content/plugins/player-logic/player.php?data=XXXXXXXX</span>

Observaciones sobre data

No es base64 simple

No es URL directa

No cambia sin cambiar episodio

Se genera en backend


👉 No es necesario romperlo para la investigación


---

5️⃣ Network: QUÉ SÍ y QUÉ NO apareció

❌ Lo que NO apareció

No XHR visibles al cargar el video

No llamadas REST claras

No URLs MP4 visibles


✅ Lo que SÍ se dedujo

El iframe es un sandbox

El JS interno maneja el flujo

Algunas llamadas ocurren antes de abrir DevTools

Otras se resuelven por backend + nonce


👉 No ver requests ≠ no hay backend


---

6️⃣ Análisis de player.js

Desde:

<span class="mono">curl -s https://hentaila.tv/wp-content/plugins/player-logic/assets/js/player.js</span>

Se encontró:

Enumeración de errores HLS

Eventos hls.js

Manejo de sources

Control total del player


Conclusión

Usa hls.js

No expone URLs directamente

Todo pasa por lógica del plugin



---

7️⃣ admin-ajax.php: acciones inferidas

Por análisis de JS y comportamiento:

Acción	Uso

get_episode	Metadata
get_sources	Fuentes de video
fetch_player	Configuración
load_episode	Inicialización


⚠️ Todas requieren:

nonce válido

sesión activa

headers correctos



---

8️⃣ Sistema de nonce (confirmado)

Características:

Generado por WordPress

Inyectado vía JS

Por sesión

Expira

Anti-CSRF


Comportamiento probado

Sin nonce → 0

Con nonce incorrecto → 0

Con nonce válido → JSON


👉 No romperlo = correcto


---

9️⃣ Headers observados / necesarios

Recomendados para cualquier prueba:

User-Agent: navegador real

Referer: hentaila.tv

Accept: application/json


Sin estos:

respuestas incompletas

bloqueos silenciosos



---

🔟 Qué se logró técnicamente (sin humo)

✔️ Logrado

Scraping completo del catálogo

Indexación de episodios

Metadata total

Arquitectura entendida

Flujo del player documentado

Backend identificado

Tokens comprendidos


❌ No (y no se intentó)

Descarga de streams

Decriptar video

Evadir nonce

Saltar protecciones
