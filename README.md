<div align="center"><h1>🧠✨ Hentaila Reverse Engineering & Scraping Research</h1><p><i>Análisis técnico completo del ecosistema WordPress, player propietario y flujo real de datos</i></p><img src="https://img.shields.io/badge/scope-research-blueviolet?style=for-the-badge">
<img src="https://img.shields.io/badge/platform-wordpress-blue?style=for-the-badge">
<img src="https://img.shields.io/badge/method-passive_analysis-green?style=for-the-badge">
<img src="https://img.shields.io/badge/legal-educational_only-success?style=for-the-badge"></div>
---

<style>
.section{border-left:4px solid #7c3aed;padding:14px;margin:24px 0;background:rgba(124,58,237,.06);border-radius:10px}
.warn{border-left-color:#f59e0b;background:rgba(245,158,11,.08)}
.ok{border-left-color:#22c55e;background:rgba(34,197,94,.08)}
.bad{border-left-color:#ef4444;background:rgba(239,68,68,.08)}
.mono{font-family:monospace;background:rgba(0,0,0,.05);padding:2px 6px;border-radius:6px}
table{width:100%;border-collapse:collapse}
th,td{border:1px solid #444;padding:8px}
th{background:rgba(124,58,237,.18)}
</style>
---

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
