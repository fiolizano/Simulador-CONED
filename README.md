<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Práctica de Preguntas – Laboratorio Remoto de Microscopía · CONED</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #0f2027 0%, #203a43 50%, #2c5364 100%);
    min-height: 100vh; padding: 20px; color: #333;
  }
  .container { max-width: 860px; margin: 0 auto; }

  /* HEADER */
  header {
    background: linear-gradient(135deg, #1a6b5c 0%, #0d4f42 100%);
    color: white; border-radius: 16px; padding: 30px;
    text-align: center; margin-bottom: 24px;
    box-shadow: 0 8px 32px rgba(0,0,0,0.3);
  }
  header .icon { font-size: 3rem; margin-bottom: 10px; }
  header h1 { font-size: 1.55rem; font-weight: 700; margin-bottom: 8px; line-height: 1.3; }
  header p  { font-size: 0.95rem; opacity: .88; max-width: 600px; margin: 0 auto; }

  /* CARDS */
  .card {
    background: white; border-radius: 14px; padding: 28px;
    margin-bottom: 20px; box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  }
  .card h2 {
    font-size: 1.1rem; font-weight: 700; margin-bottom: 16px;
    color: #1a6b5c; display: flex; align-items: center; gap: 8px;
  }

  /* TEXTAREA */
  textarea {
    width: 100%; min-height: 140px; padding: 16px;
    border: 2px solid #d1d5db; border-radius: 10px;
    font-size: 0.97rem; font-family: inherit; line-height: 1.6;
    resize: vertical; transition: border-color .25s; color: #333;
  }
  textarea:focus { outline: none; border-color: #1a6b5c; box-shadow: 0 0 0 3px rgba(26,107,92,.15); }
  .char-counter { text-align: right; font-size: .82rem; color: #9ca3af; margin-top: 6px; }
  .char-counter.warn  { color: #f59e0b; }
  .char-counter.limit { color: #ef4444; }

  /* BUTTONS */
  .btn-group { display: flex; gap: 10px; flex-wrap: wrap; margin-top: 16px; }
  button {
    padding: 11px 22px; border: none; border-radius: 8px;
    font-size: .92rem; font-weight: 600; cursor: pointer;
    transition: all .2s; display: flex; align-items: center; gap: 6px;
  }
  .btn-primary { background: linear-gradient(135deg, #1a6b5c, #0d4f42); color: white; flex: 1; justify-content: center; min-width: 160px; }
  .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 6px 18px rgba(26,107,92,.4); }
  .btn-secondary { background: #f3f4f6; color: #374151; border: 1.5px solid #e5e7eb; }
  .btn-secondary:hover { background: #e5e7eb; }
  .btn-danger { background: #fef2f2; color: #dc2626; border: 1.5px solid #fecaca; }
  .btn-danger:hover { background: #fee2e2; }

  /* EXAMPLES */
  .examples-panel { display: none; background: #f0fdf4; border: 1.5px solid #bbf7d0; border-radius: 10px; padding: 18px; margin-top: 14px; }
  .examples-panel.show { display: block; }
  .examples-panel h3 { font-size: .92rem; font-weight: 700; color: #166534; margin-bottom: 12px; }
  .example-tabs { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 14px; }
  .ex-tab { padding: 6px 14px; border-radius: 20px; font-size: .8rem; font-weight: 600; cursor: pointer; border: 1.5px solid #bbf7d0; background: white; color: #166534; transition: all .15s; }
  .ex-tab.active, .ex-tab:hover { background: #16a34a; color: white; border-color: #16a34a; }
  .example-item { background: white; border-radius: 8px; padding: 12px 14px; margin-bottom: 8px; border-left: 4px solid #22c55e; font-size: .88rem; line-height: 1.55; cursor: pointer; transition: background .15s; display: none; }
  .example-item.active-cat { display: block; }
  .example-item:hover { background: #f0fdf4; }
  .example-label { font-weight: 700; color: #166534; font-size: .78rem; text-transform: uppercase; letter-spacing: .05em; margin-bottom: 4px; }
  .q-bad  { border-left-color: #ef4444; }
  .q-ok   { border-left-color: #f59e0b; }
  .q-good { border-left-color: #22c55e; }

  /* RESULTS */
  #results { display: none; }
  #typeBadge {
    background: #f0fdf4; border: 1.5px solid #86efac; border-radius: 10px;
    padding: 12px 16px; margin-bottom: 18px; font-size: .9rem;
    color: #166534; font-weight: 600; display: none; align-items: center; gap: 8px;
  }

  /* SCORE */
  .score-badge { text-align: center; margin-bottom: 24px; }
  .score-circle {
    width: 110px; height: 110px; border-radius: 50%;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    margin: 0 auto 12px; border: 6px solid; transition: all .5s;
  }
  .score-circle .score-num   { font-size: 2rem; font-weight: 800; line-height: 1; }
  .score-circle .score-denom { font-size: .82rem; opacity: .7; }
  .score-level { font-size: 1.15rem; font-weight: 700; }
  .score-desc  { font-size: .88rem; color: #6b7280; margin-top: 4px; }

  .level-deficiente .score-circle { color: #dc2626; border-color: #dc2626; background: #fef2f2; }
  .level-deficiente .score-level  { color: #dc2626; }
  .level-basico .score-circle     { color: #f97316; border-color: #f97316; background: #fff7ed; }
  .level-basico .score-level      { color: #f97316; }
  .level-moderado .score-circle   { color: #f59e0b; border-color: #f59e0b; background: #fffbeb; }
  .level-moderado .score-level    { color: #f59e0b; }
  .level-bueno .score-circle      { color: #3b82f6; border-color: #3b82f6; background: #eff6ff; }
  .level-bueno .score-level       { color: #3b82f6; }
  .level-excelente .score-circle  { color: #16a34a; border-color: #16a34a; background: #f0fdf4; }
  .level-excelente .score-level   { color: #16a34a; }

  /* CRITERIA BARS */
  .criteria-grid { display: grid; gap: 14px; margin-bottom: 20px; }
  .criterion { background: #f9fafb; border-radius: 10px; padding: 14px 16px; }
  .criterion-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
  .criterion-name   { font-weight: 700; font-size: .9rem; color: #374151; }
  .criterion-weight { font-size: .78rem; color: #9ca3af; background: #e5e7eb; padding: 2px 7px; border-radius: 20px; }
  .criterion-score-row { display: flex; align-items: center; gap: 10px; }
  .bar-track { flex: 1; height: 10px; background: #e5e7eb; border-radius: 10px; overflow: hidden; }
  .bar-fill  { height: 100%; border-radius: 10px; transition: width .6s ease; width: 0; }
  .criterion-points { font-size: .85rem; font-weight: 700; min-width: 42px; text-align: right; }
  .stars { font-size: 1.05rem; letter-spacing: 1px; margin-top: 6px; }

  /* FEEDBACK */
  .feedback-section { margin-top: 6px; }
  .feedback-section h3 { font-size: .95rem; font-weight: 700; margin-bottom: 12px; color: #374151; }
  .feedback-item { display: flex; gap: 10px; align-items: flex-start; padding: 10px 14px; border-radius: 8px; margin-bottom: 8px; font-size: .88rem; line-height: 1.5; }
  .feedback-item.positive { background: #f0fdf4; border-left: 3px solid #22c55e; }
  .feedback-item.warning  { background: #fffbeb; border-left: 3px solid #f59e0b; }
  .feedback-item.error    { background: #fef2f2; border-left: 3px solid #ef4444; }
  .feedback-icon { font-size: 1rem; flex-shrink: 0; margin-top: 1px; }

  /* PE TIPS INSIDE RESULTS */
  .pe-tips { background: #eff6ff; border: 1.5px solid #bfdbfe; border-radius: 10px; padding: 16px; margin-top: 16px; }
  .pe-tips h3 { font-size: .9rem; font-weight: 700; color: #1d4ed8; margin-bottom: 10px; }
  .pe-tip-item { font-size: .86rem; color: #374151; line-height: 1.55; margin-bottom: 7px; display: flex; gap: 8px; }

  /* IMPROVED PROMPT */
  .improved-box { background: linear-gradient(135deg, #f0fdf4, #ecfdf5); border: 1.5px solid #86efac; border-radius: 10px; padding: 18px; margin-top: 16px; }
  .improved-box h3 { font-size: .92rem; font-weight: 700; color: #166534; margin-bottom: 10px; display: flex; align-items: center; gap: 6px; }
  .improved-text { background: white; border-radius: 8px; padding: 14px; font-size: .9rem; line-height: 1.65; color: #374151; border: 1px solid #d1fae5; font-style: italic; }
  .btn-use-improved { margin-top: 10px; background: #16a34a; color: white; border: none; padding: 9px 18px; border-radius: 7px; font-size: .85rem; font-weight: 600; cursor: pointer; }
  .btn-use-improved:hover { background: #15803d; }

  /* TIPS CARD */
  .tips-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 12px; }
  .tip-card { background: linear-gradient(135deg, #f0fdf4, #ecfdf5); border-radius: 10px; padding: 16px; border: 1px solid #bbf7d0; }
  .tip-card .tip-icon  { font-size: 1.5rem; margin-bottom: 8px; }
  .tip-card .tip-title { font-weight: 700; font-size: .88rem; color: #166534; margin-bottom: 5px; }
  .tip-card .tip-body  { font-size: .82rem; color: #374151; line-height: 1.5; }

  /* HISTORY */
  .history-list { max-height: 260px; overflow-y: auto; }
  .history-item { background: #f9fafb; border-radius: 8px; padding: 12px 14px; margin-bottom: 8px; cursor: pointer; transition: background .15s; border: 1px solid #e5e7eb; }
  .history-item:hover { background: #f0fdf4; border-color: #bbf7d0; }
  .history-meta { display: flex; justify-content: space-between; margin-bottom: 4px; }
  .history-score  { font-weight: 700; font-size: .85rem; }
  .history-time   { font-size: .78rem; color: #9ca3af; }
  .history-prompt { font-size: .85rem; color: #6b7280; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .empty-history  { text-align: center; color: #9ca3af; font-size: .88rem; padding: 20px; }

  footer { text-align: center; color: rgba(255,255,255,.55); font-size: .8rem; margin-top: 20px; padding-bottom: 10px; }

  @keyframes fadeIn { from { opacity:0; transform: translateY(12px); } to { opacity:1; transform: translateY(0); } }
  .fade-in { animation: fadeIn .4s ease; }

  @media (max-width: 600px) {
    header h1 { font-size: 1.25rem; }
    .card { padding: 20px; }
    .btn-group { flex-direction: column; }
    .btn-primary { min-width: unset; }
  }
</style>
</head>
<body>
<div class="container">

  <!-- HEADER -->
  <header>
    <div class="icon">🔬</div>
    <h1>Práctica para hacer buenas preguntas<br>al asistente del Laboratorio de Microscopía</h1>
    <p>Antes de usar el asistente de inteligencia artificial en el laboratorio, practique aquí cómo escribir su pregunta. El sistema le va a decir qué tan completa es y cómo mejorarla.</p>
  </header>

  <!-- INPUT -->
  <div class="card">
    <h2>✏️ Escriba su pregunta aquí</h2>
    <p style="font-size:.88rem;color:#6b7280;margin-bottom:12px;">
      💡 <strong>¿Qué puede preguntar?</strong> Lo que necesite: qué es una célula, cómo funciona el microscopio, qué pasos seguir para preparar una muestra, por qué ve borroso en la pantalla… <em>No hay preguntas malas.</em> La idea es practicar cómo escribirlas mejor.
    </p>
    <textarea id="promptInput"
      placeholder="Escriba aquí su pregunta tal como se la haría al asistente. Por ejemplo: &#10;'No entiendo para qué sirve el Lugol cuando preparo la muestra de cebolla.'&#10;'¿Por qué la imagen se ve borrosa aunque ya moví el tornillo?'"></textarea>
    <div class="char-counter" id="charCounter">0 / 600 caracteres</div>

    <div class="btn-group">
      <button class="btn-primary"   id="btnAnalyze">🔍 Revisar mi pregunta</button>
      <button class="btn-secondary" id="btnExample">💡 Ver ejemplos</button>
      <button class="btn-danger"    id="btnClear"  >🗑️ Borrar todo</button>
    </div>

    <!-- EXAMPLES PANEL -->
    <div class="examples-panel" id="examplesPanel">
      <h3>📌 Estos son ejemplos de preguntas reales. Haga clic en cualquiera para cargarlo y ver cómo lo califica el sistema:</h3>

      <div class="example-tabs">
        <div class="ex-tab active" data-cat="conceptos">🧫 Conceptos</div>
        <div class="ex-tab" data-cat="equipo">🔭 El equipo</div>
        <div class="ex-tab" data-cat="montaje">🧪 Preparación</div>
        <div class="ex-tab" data-cat="observacion">👁️ Lo que veo</div>
      </div>

      <!-- CONCEPTOS -->
      <div class="example-item q-bad  active-cat" data-cat="conceptos" data-prompt="que es la elodea">
        <div class="example-label">❌ Muy corta — el asistente no sabe qué explicar ni para qué lo necesita</div>
        que es la elodea
      </div>
      <div class="example-item q-ok active-cat" data-cat="conceptos" data-prompt="¿Qué es la Elodea y para qué se usa en microscopía?">
        <div class="example-label">⚠️ Mejor, pero falta decir qué ya sabe y por qué le genera duda</div>
        ¿Qué es la Elodea y para qué se usa en microscopía?
      </div>
      <div class="example-item q-good active-cat" data-cat="conceptos" data-prompt="Estoy estudiando células vegetales en el laboratorio remoto de microscopía. Sé que la Elodea es una planta de agua, pero no entiendo por qué la usan para ver los cloroplastos en vez de otra planta. ¿Qué tiene de especial para eso?">
        <div class="example-label">✅ Muy buena — dice dónde está, qué ya sabe y hace una pregunta específica</div>
        Estoy estudiando células vegetales en el laboratorio remoto de microscopía. Sé que la Elodea es una planta de agua, pero no entiendo por qué la usan para ver los cloroplastos en vez de otra planta. ¿Qué tiene de especial para eso?
      </div>

      <!-- EQUIPO -->
      <div class="example-item q-bad" data-cat="equipo" data-prompt="como funciona el microscopio">
        <div class="example-label">❌ Muy vaga — el microscopio tiene muchas partes, no queda claro qué necesita</div>
        como funciona el microscopio
      </div>
      <div class="example-item q-ok" data-cat="equipo" data-prompt="¿Para qué sirve el tornillo micrométrico del microscopio?">
        <div class="example-label">⚠️ Mejor porque nombra la parte, pero falta decir qué está intentando hacer</div>
        ¿Para qué sirve el tornillo micrométrico del microscopio?
      </div>
      <div class="example-item q-good" data-cat="equipo" data-prompt="Estoy en el laboratorio remoto de microscopía viendo las partes del equipo. Entiendo que el tornillo macrométrico mueve la lente rápido para un enfoque grueso, pero no me queda claro cuándo debo cambiarme al tornillo micrométrico. ¿Me puede explicar la diferencia entre los dos y cuándo se usa cada uno?">
        <div class="example-label">✅ Muy buena — explica lo que ya sabe, dónde está su duda y pregunta de forma clara</div>
        Estoy en el laboratorio remoto de microscopía viendo las partes del equipo. Entiendo que el tornillo macrométrico mueve la lente rápido para un enfoque grueso, pero no me queda claro cuándo debo cambiarme al tornillo micrométrico. ¿Me puede explicar la diferencia entre los dos y cuándo se usa cada uno?
      </div>

      <!-- MONTAJE -->
      <div class="example-item q-bad" data-cat="montaje" data-prompt="como preparo la muestra">
        <div class="example-label">❌ No dice qué muestra ni en qué paso está — el asistente no puede ayudar bien</div>
        como preparo la muestra
      </div>
      <div class="example-item q-ok" data-cat="montaje" data-prompt="¿Cuáles son los pasos para preparar la muestra de cebolla en el laboratorio?">
        <div class="example-label">⚠️ Menciona la muestra, pero no dice en qué paso tiene la duda</div>
        ¿Cuáles son los pasos para preparar la muestra de cebolla en el laboratorio?
      </div>
      <div class="example-item q-good" data-cat="montaje" data-prompt="Estoy en la sección de preparación del laboratorio remoto de microscopía con la muestra de cebolla. Ya hice el corte y lo puse sobre el portaobjetos, pero no entiendo para qué le tengo que agregar el Lugol. ¿Para qué sirve ese paso y qué pasaría si no lo hago?">
        <div class="example-label">✅ Muy buena — dice la sección, la muestra, qué ya hizo y pregunta el porqué del paso</div>
        Estoy en la sección de preparación del laboratorio remoto de microscopía con la muestra de cebolla. Ya hice el corte y lo puse sobre el portaobjetos, pero no entiendo para qué le tengo que agregar el Lugol. ¿Para qué sirve ese paso y qué pasaría si no lo hago?
      </div>

      <!-- OBSERVACIÓN -->
      <div class="example-item q-bad" data-cat="observacion" data-prompt="no veo nada">
        <div class="example-label">❌ Sin ningún dato — el asistente no sabe qué muestra usa ni qué intentó</div>
        no veo nada
      </div>
      <div class="example-item q-ok" data-cat="observacion" data-prompt="¿Por qué hay que empezar siempre con el lente de 4x antes de pasar al de 40x?">
        <div class="example-label">⚠️ Buena pregunta, pero no dice qué situación le generó esa duda</div>
        ¿Por qué hay que empezar siempre con el lente de 4x antes de pasar al de 40x?
      </div>
      <div class="example-item q-good" data-cat="observacion" data-prompt="Estoy en el laboratorio remoto de microscopía observando la muestra de glóbulos rojos. Con el lente de 40x logré ver las células, pero cuando pasé al de 100x la imagen quedó muy oscura aunque ya le agregué el aceite de inmersión. ¿Qué debo ajustar en la luz o en el condensador para que se vea mejor?">
        <div class="example-label">✅ Muy buena — dice la muestra, el lente, el problema exacto y qué ya intentó</div>
        Estoy en el laboratorio remoto de microscopía observando la muestra de glóbulos rojos. Con el lente de 40x logré ver las células, pero cuando pasé al de 100x la imagen quedó muy oscura aunque ya le agregué el aceite de inmersión. ¿Qué debo ajustar en la luz o en el condensador para que se vea mejor?
      </div>
    </div>
  </div>

  <!-- RESULTS -->
  <div class="card fade-in" id="results">
    <div id="typeBadge"></div>

    <div class="score-badge" id="scoreBadge">
      <div class="score-circle" id="scoreCircle">
        <span class="score-num"   id="scoreNum">0</span>
        <span class="score-denom">/100</span>
      </div>
      <div class="score-level" id="scoreLevel">–</div>
      <div class="score-desc"  id="scoreDesc">–</div>
    </div>

    <div class="criteria-grid" id="criteriaGrid"></div>

    <div class="feedback-section">
      <h3>💬 ¿Qué tiene de bueno y qué puede mejorar?</h3>
      <div id="feedbackList"></div>
    </div>

    <div class="pe-tips" id="peTips" style="display:none;">
      <h3>💡 Consejos para mejorar este tipo de pregunta</h3>
      <div id="peTipsList"></div>
    </div>

    <div class="improved-box" id="improvedBox" style="display:none;">
      <h3>✨ Así podría quedar su pregunta — lista para usar</h3>
      <p style="font-size:.85rem;color:#166534;margin-bottom:10px;">Le ofrecemos una versión mejorada de su pregunta. Puede usarla tal como está o cambiarle los detalles según lo que usted está haciendo en el laboratorio.</p>
      <div class="improved-text" id="improvedText"></div>
      <div style="display:flex;gap:10px;flex-wrap:wrap;margin-top:10px;">
        <button class="btn-use-improved" id="btnUseImproved">📋 Cargar en el área de escritura</button>
        <button class="btn-use-improved" id="btnCopyImproved" style="background:#0d9488;">📄 Copiar al portapapeles</button>
      </div>
    </div>
  </div>

  <!-- TIPS CARD -->
  <div class="card">
    <h2>📖 ¿Cómo se escribe una buena pregunta para el asistente?</h2>
    <div class="tips-grid">
      <div class="tip-card">
        <div class="tip-icon">🎯</div>
        <div class="tip-title">1. Una sola duda por pregunta</div>
        <div class="tip-body">Si tiene varias dudas, es mejor hacerlas por separado. Así el asistente puede responder cada una bien, sin confundirse.</div>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🌍</div>
        <div class="tip-title">2. Diga dónde está y qué está haciendo</div>
        <div class="tip-body">El asistente no sabe en qué parte del laboratorio está. Diga, por ejemplo: "Estoy en la sección de preparación con la muestra de cebolla".</div>
      </div>
      <div class="tip-card">
        <div class="tip-icon">📚</div>
        <div class="tip-title">3. Diga qué ya sabe</div>
        <div class="tip-body">Si escribe "Sé que el Lugol es un colorante, pero no entiendo por qué lo uso aquí", el asistente le explica justo lo que le falta, sin repetirle lo que ya sabe.</div>
      </div>
      <div class="tip-card">
        <div class="tip-icon">❓</div>
        <div class="tip-title">4. Termine con una pregunta directa</div>
        <div class="tip-body">Use los signos de interrogación (¿ ?) y sea directo: ¿para qué sirve esto?, ¿cómo lo hago?, ¿por qué pasa eso?. Así el asistente sabe exactamente qué responder.</div>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🔍</div>
        <div class="tip-title">5. Sea específico/a</div>
        <div class="tip-body">"No veo bien" es difícil de responder. Mejor: "Cuando paso al lente de 40x, la imagen se pone borrosa aunque ya ajusté el tornillo". Con más detalle, mejor respuesta.</div>
      </div>
      <div class="tip-card">
        <div class="tip-icon">📏</div>
        <div class="tip-title">6. Ni muy corta ni muy larga</div>
        <div class="tip-body">Una pregunta de 2 palabras no tiene suficiente información. Una de 200 palabras puede confundir. Lo ideal es entre 3 y 6 oraciones completas.</div>
      </div>
    </div>
  </div>

  <!-- HISTORY -->
  <div class="card">
    <h2>📋 Sus intentos anteriores</h2>
    <p style="font-size:.85rem;color:#6b7280;margin-bottom:12px;">Haga clic en cualquier intento para volver a cargarlo y seguir mejorándolo.</p>
    <div class="history-list" id="historyList">
      <div class="empty-history">Todavía no ha revisado ninguna pregunta. ¡Escriba una arriba y presione "Revisar mi pregunta"!</div>
    </div>
  </div>

</div>
<footer>Laboratorio Remoto de Microscopía · CONED · LER-UNED Costa Rica</footer>

<script>
/* ═══════════════════════════════════════════════════
   TOPIC DETECTION
   Detect what kind of question this is so feedback
   can be tailored – not every question needs a sample
   or objective lens mention.
═══════════════════════════════════════════════════ */
const TOPICS = {
  concepto: {
    label: '🧫 Pregunta sobre un concepto o tema del laboratorio',
    keys: ['qué es','que es','qué son','que son','define','definición','concepto',
           'para qué sirve','función de','importancia de','diferencia entre',
           'qué diferencia','comparar','por qué se usa','cómo funciona',
           'elodea','cebolla','cucaracha','estoma','cloroplasto','epitelial','espermatozoide',
           'sanguínea','glóbulo','núcleo','membrana','pared celular','vacuola',
           'célula vegetal','célula animal','microscopía','tejido','organelo',
           'eucariota','procariota','histología','lugol','eosina','resolución óptica',
           'poder de resolución'],
    tips: [
      { i:'📚', t:'<strong>Diga qué ya sabe:</strong> Empiece con lo que conoce y explique dónde se le hace difícil. Por ejemplo: "Sé que la Elodea es una planta de agua, pero no entiendo por qué la usan en el microscopio..."' },
      { i:'🎯', t:'<strong>Enfóquese en una sola cosa:</strong> Si el tema es muy amplio, escoja el aspecto específico que no le queda claro. Así la respuesta será más útil.' },
      { i:'🔗', t:'<strong>Conecte con el laboratorio:</strong> Aunque sea una pregunta de concepto, mencionar que está en el laboratorio de microscopía ayuda al asistente a darle ejemplos más concretos.' },
    ]
  },
  equipo: {
    label: '🔭 Pregunta sobre el microscopio o sus partes',
    keys: ['tornillo','macrométrico','micrométrico','platina','condensador','revólver',
           'fuente de luz','ajuste de luz','ocular','objetivo','lente','brazo','cabeza',
           'pinzas','carro','diafragma','cabezal','portaobjetos','cubreobjetos',
           'partes del microscopio','componente','estructura del microscopio',
           'modelo 3d','aceite de inmersión'],
    tips: [
      { i:'⚙️', t:'<strong>Nombre la parte que le genera duda:</strong> En vez de decir "el microscopio", diga exactamente qué parte no entiende: "el tornillo micrométrico", "el condensador", etc.' },
      { i:'📍', t:'<strong>Diga qué está intentando hacer:</strong> Por ejemplo: "Estoy tratando de enfocar la imagen y no sé cuándo usar el tornillo grueso y cuándo el fino".' },
      { i:'🔗', t:'<strong>Pregunte también para qué sirve:</strong> Entender la función de cada parte le ayuda a usarla bien, no solo a saber su nombre.' },
    ]
  },
  montaje: {
    label: '🧪 Pregunta sobre cómo preparar o montar una muestra',
    keys: ['preparar','preparación','montaje','montar','corte','bisturí','lugol','eosina',
           'colorante','agua','fijación','calor','punción','extensión','extendido',
           'frotis','colocar','paso','pasos','procedimiento','cómo preparo','secuencia',
           'orden de pasos','cómo hago','portaobjetos','cubreobjetos'],
    tips: [
      { i:'📋', t:'<strong>Diga en qué paso está:</strong> Cuente qué pasos ya hizo y en cuál se le presentó la duda. Así el asistente puede orientarle exactamente ahí.' },
      { i:'🧫', t:'<strong>Mencione qué muestra está usando:</strong> Los pasos cambian según la muestra (cebolla, epitelial bucal, sanguínea). Decirlo mejora mucho la respuesta.' },
      { i:'❓', t:'<strong>Pregunte también por qué:</strong> No solo "¿cómo lo hago?" sino "¿por qué se hace así?". Entender el porqué le ayuda a no olvidarlo.' },
    ]
  },
  observacion: {
    label: '👁️ Pregunta sobre lo que ve en la pantalla del microscopio',
    keys: ['observar','observación','imagen','ver','veo','enfoque','enfocar','borroso',
           'nítido','nitidez','iluminación','objetivo 4','objetivo 10','objetivo 40',
           'objetivo 100','4x','10x','40x','100x','aceite','contraste','color','viraje',
           'pantalla completa','descargar','aumento','cambio de objetivo'],
    tips: [
      { i:'🔭', t:'<strong>Diga qué lente está usando:</strong> Si el problema le pasa con un lente específico (4x, 10x, 40x, 100x), menciónelo. El problema puede ser diferente según el aumento.' },
      { i:'🧫', t:'<strong>Describa lo que está viendo:</strong> Por ejemplo: "la imagen se ve borrosa" o "todo está muy oscuro". Cuanto más describe, mejor le pueden ayudar.' },
      { i:'🔧', t:'<strong>Diga qué ya intentó:</strong> Si ya movió el tornillo o ajustó la luz, cuéntelo. Así el asistente no le va a sugerir lo mismo que ya hizo.' },
    ]
  },
  general: {
    label: '💬 Pregunta general sobre el laboratorio',
    keys: [],
    tips: [
      { i:'🌍', t:'<strong>Mencione el laboratorio:</strong> Diga que está en el laboratorio remoto de microscopía. Eso le da al asistente la información básica para ayudarle.' },
      { i:'🎯', t:'<strong>Haga una pregunta directa:</strong> Use ¿ y ? para que quede claro qué quiere que le expliquen.' },
      { i:'📚', t:'<strong>Cuente qué ya sabe:</strong> Así el asistente le explica justo lo que le falta, sin empezar desde cero.' },
    ]
  }
};

function detectTopic(t) {
  let best = 'general', bestN = 0;
  for (const [topic, data] of Object.entries(TOPICS)) {
    if (topic === 'general') continue;
    const n = data.keys.filter(k => t.includes(k)).length;
    if (n > bestN) { bestN = n; best = topic; }
  }
  return best;
}

/* ═══════════════════════════════════════════════════
   SCORING ENGINE — 5 Prompt Engineering criteria
═══════════════════════════════════════════════════ */
function analyze(text) {
  const t  = text.toLowerCase().trim();
  const wc = t.split(/\s+/).length;
  const topic = detectTopic(t);

  /* 1. CLARITY (25 pts) */
  let clarity = 0;
  const hasQ   = /[?¿]/.test(t);
  const hasVerb = /\b(cómo|como|por qué|porque|qué|que|cuál|cual|cuándo|cuando|explica|explícame|describe|ayuda|ayúdame|diferencia|compara|identifica)\b/.test(t);
  if (wc >= 10 && wc <= 100) clarity += 10;
  else if (wc >= 5)           clarity += 5;
  else if (wc > 100)          clarity += 7;
  if (hasQ)    clarity += 9;
  if (hasVerb) clarity += 6;
  clarity = Math.min(clarity, 25);

  /* 2. CONTEXT (25 pts) */
  let context = 0;
  const hasLabRef     = /laboratorio|microsco|lab remoto|lr\b/.test(t);
  const hasSectionRef = /\b(configuración|preparación|observación|introducción|sección)\b/.test(t);
  const hasActivityRef= /\b(estoy|estaba|estuve|estamos|durante|en la práctica|en la actividad)\b/.test(t);
  if (hasLabRef)      context += 10;
  if (hasSectionRef)  context += 8;
  if (hasActivityRef) context += 7;
  // For pure conceptual questions, lab ref is optional – give partial credit
  if (topic === 'concepto' && !hasLabRef) context += 4;
  context = Math.min(context, 25);

  /* 3. SPECIFICITY (20 pts)
     Only evaluate topic-relevant details, not always sample+objective */
  let spec = 0;
  const topicKeys = TOPICS[topic].keys;
  const topicMatches = topicKeys.filter(k => t.includes(k)).length;
  if (topicMatches >= 3) spec += 12;
  else if (topicMatches === 2) spec += 8;
  else if (topicMatches === 1) spec += 4;
  // Bonus for named specific entities relevant to microscopía
  const hasSpecific = /\b(4x|10x|40x|100x|lugol|eosina|elodea|cebolla|epitelial|sanguínea|espermatozoide|cloroplasto|estoma|macrométrico|micrométrico|condensador|portaobjetos|cubreobjetos|aceite de inmersión|glóbulo)\b/.test(t);
  if (hasSpecific) spec += 8;
  spec = Math.min(spec, 20);

  /* 4. PRIOR KNOWLEDGE SHOWN (15 pts) */
  let prior = 0;
  const showsKnown   = /\b(sé que|ya sé|entiendo que|comprendo que|aprendí que|leí que|según|ya realicé|ya hice|ya apliqué|intenté|probé|noté que|observé que)\b/.test(t);
  const showsStruggle= /\b(pero no|sin embargo no|aunque no|no entiendo|no comprendo|no sé|no logro|no puedo|me confunde|tengo duda|me queda la duda|no me queda claro)\b/.test(t);
  const hasComparison= /\b(diferencia|comparar|vs\b|versus|a diferencia de|en contraste|mientras que)\b/.test(t);
  if (showsKnown)    prior += 7;
  if (showsStruggle) prior += 5;
  if (hasComparison) prior += 3;
  prior = Math.min(prior, 15);

  /* 5. COMPLETENESS (15 pts) */
  let complete = 0;
  const sentences = t.split(/[.!?¿¡]+/).filter(s => s.trim().length > 2);
  if (wc >= 5)  complete += 3;
  if (wc >= 15) complete += 4;
  if (wc >= 30) complete += 4;
  if (sentences.length >= 2) complete += 4;
  complete = Math.min(complete, 15);

  const total = clarity + context + spec + prior + complete;

  /* LEVEL */
  let level, levelClass, desc;
  if      (total >= 85) { level = '⭐ ¡Excelente!';      levelClass = 'level-excelente'; desc = '¡Pura vida! Su pregunta está muy bien escrita. El asistente le puede dar una respuesta completa y útil.'; }
  else if (total >= 68) { level = '👍 Buena pregunta';   levelClass = 'level-bueno';    desc = 'Va muy bien. Con uno o dos ajustes pequeños, su pregunta puede quedar excelente.'; }
  else if (total >= 48) { level = '⚠️ Va por buen camino'; levelClass = 'level-moderado'; desc = 'Su pregunta tiene cosas buenas, pero le falta un poco más de información para que el asistente le ayude bien.'; }
  else if (total >= 28) { level = '🔸 Necesita más detalle'; levelClass = 'level-basico'; desc = 'El asistente tendría que adivinar muchas cosas. Agregue más información sobre lo que está haciendo y lo que necesita.'; }
  else                  { level = '❌ Muy corta o vaga'; levelClass = 'level-deficiente'; desc = 'Con tan poca información es difícil obtener una ayuda útil. ¡Cuéntele más al asistente!'; }

  /* FEEDBACK — contextual, not one-size-fits-all */
  const fb = [];

  // Clarity
  if (hasQ)    fb.push({ type:'positive', icon:'✅', text:'Usó los signos de pregunta (¿?). Así el asistente sabe exactamente qué tiene que responder. ¡Bien hecho!' });
  else         fb.push({ type:'error',    icon:'❌', text:'No hay una pregunta clara. Escriba su duda usando ¿ al inicio y ? al final. Por ejemplo: ¿Para qué sirve el Lugol?' });

  if (hasVerb) fb.push({ type:'positive', icon:'✅', text:'Usó una palabra de pregunta clara (cómo, por qué, qué, explique…). Eso le indica al asistente qué tipo de respuesta necesita.' });
  else         fb.push({ type:'warning',  icon:'⚠️', text:'Agregue una palabra que indique qué quiere saber: ¿cómo?, ¿por qué?, ¿qué diferencia hay?, ¿me puede explicar?' });

  if (wc < 5)        fb.push({ type:'error',   icon:'❌', text:'Su pregunta tiene muy pocas palabras. El asistente necesita más información para poder ayudarle bien.' });
  else if (wc > 120) fb.push({ type:'warning', icon:'⚠️', text:'Su pregunta es bastante larga. Intente enfocarse en una sola duda para que la respuesta sea más clara.' });
  else if (wc >= 15) fb.push({ type:'positive',icon:'✅', text:'La longitud de su pregunta es buena. Tiene suficiente información para obtener una respuesta completa.' });

  // Context
  if (hasLabRef)      fb.push({ type:'positive', icon:'✅', text:'Mencionó el laboratorio de microscopía. Eso le da al asistente el contexto necesario para orientarle mejor.' });
  else if (topic !== 'concepto')
                      fb.push({ type:'warning',  icon:'⚠️', text:'Considere mencionar que está en el laboratorio remoto de microscopía. Con eso el asistente entiende mejor su situación.' });

  if (hasSectionRef)  fb.push({ type:'positive', icon:'✅', text:'Indicó en qué sección del laboratorio está (configuración, preparación u observación). ¡Eso ayuda mucho!' });

  // Specificity — only suggest sample/objective when the topic makes it relevant
  if (topic === 'observacion') {
    const hasSample  = /\b(cebolla|elodea|epitelial|sanguínea|cucaracha|espermatozoide|glóbulo)\b/.test(t);
    const hasObjLens = /\b(4x|10x|40x|100x|objetivo)\b/.test(t);
    if (hasSample)   fb.push({ type:'positive', icon:'✅', text:'Mencionó la muestra que está observando. Eso permite que la ayuda sea mucho más específica para su caso.' });
    else             fb.push({ type:'warning',  icon:'⚠️', text:'Si aplica, diga qué muestra está viendo (por ejemplo: cebolla, glóbulos rojos, epitelial bucal). Eso cambia la respuesta.' });
    if (hasObjLens)  fb.push({ type:'positive', icon:'✅', text:'Indicó el lente o aumento que está usando. Con eso el asistente puede identificar el problema con más precisión.' });
    else             fb.push({ type:'warning',  icon:'⚠️', text:'Diga con qué lente está trabajando (4x, 10x, 40x o 100x). El problema puede ser diferente según el aumento.' });
  }
  if (topic === 'montaje') {
    const hasSample = /\b(cebolla|elodea|epitelial|sanguínea|cucaracha|espermatozoide|glóbulo)\b/.test(t);
    if (hasSample)  fb.push({ type:'positive', icon:'✅', text:'Mencionó qué muestra está preparando. Los pasos cambian según la muestra, así que eso es importante.' });
    else            fb.push({ type:'warning',  icon:'⚠️', text:'Si aplica, diga qué muestra está preparando. El procedimiento es diferente para cada una (cebolla, sanguínea, epitelial bucal, etc.).' });
  }

  // Prior knowledge
  if (showsKnown)     fb.push({ type:'positive', icon:'✅', text:'Explicó qué ya sabe sobre el tema. Así el asistente puede ir directo a lo que le falta sin repetirle lo que ya conoce.' });
  else                fb.push({ type:'warning',  icon:'⚠️', text:'Cuente un poco qué ya sabe sobre el tema. Así el asistente le explica justo lo que le falta, sin empezar desde cero.' });
  if (showsStruggle)  fb.push({ type:'positive', icon:'✅', text:'Describió con claridad en qué parte tiene la duda. ¡Eso hace que la respuesta sea mucho más útil!' });

  // Completeness
  if (sentences.length >= 2) fb.push({ type:'positive', icon:'✅', text:'Su pregunta tiene varias oraciones. Eso le da al asistente suficiente información para responderle bien.' });

  /* ── IMPROVED PROMPT — smart, ready-to-use generation ── */
  let improved = null;
  if (total < 80) {

    // Extract the core of what the student wrote (strip punctuation noise)
    const core = text.trim().replace(/^[\s¿?!¡]+|[\s¿?!¡]+$/g, '');

    // Detect if specific sample/objective already mentioned
    const sampleMatch = t.match(/\b(cebolla|elodea|epitelial bucal|epitelial|sanguínea|cucaracha|espermatozoide|glóbulos? rojos?|glóbulos?)\b/);
    const lensMatch   = t.match(/\b(4x|10x|40x|100x)\b/);
    const sectionMatch= t.match(/\b(configuración|preparación|observación)\b/);

    const sample  = sampleMatch ? sampleMatch[0] : null;
    const lens    = lensMatch   ? lensMatch[0]   : null;
    const section = sectionMatch? sectionMatch[0]: null;

    // Build each missing block with sensible defaults
    const labCtx  = hasLabRef    ? 'el laboratorio remoto de microscopía'
                                 : 'el laboratorio remoto de microscopía';
    const secCtx  = section      ? `la sección de ${section} de ${labCtx}`
                                 : labCtx;

    // Topic-specific improved prompts
    if (topic === 'concepto') {
      // For conceptual questions: context + what they know + specific question
      const knownBlock = showsKnown
        ? ''
        : 'He leído la introducción del laboratorio, pero aún no tengo claro el concepto. ';
      const qBlock = hasQ
        ? core.replace(/.*?\?/, '').trim() || core
        : core;
      improved =
        `Estoy estudiando en ${labCtx}. ${knownBlock}` +
        `${showsKnown ? core : qBlock} ` +
        `¿Puedes explicarme esto con un ejemplo concreto relacionado con las muestras del laboratorio?`;

    } else if (topic === 'equipo') {
      const partMatch = t.match(/\b(tornillo macrométrico|tornillo micrométrico|macrométrico|micrométrico|platina|condensador|revólver|ocular|objetivos?|fuente de luz|brazo|cabeza|pinzas|carro|diafragma)\b/);
      const part = partMatch ? partMatch[0] : 'el componente';
      const knownBlock = showsKnown
        ? ''
        : `Sé que ${part} forma parte del microscopio, pero no entiendo bien su función. `;
      improved =
        `Estoy en ${secCtx} y estoy revisando las partes del microscopio. ` +
        `${knownBlock}` +
        `${hasQ ? core : `Mi duda es sobre ${part}.`} ` +
        `¿Puedes explicarme cómo se usa correctamente y qué ocurre si no se ajusta bien?`;

    } else if (topic === 'montaje') {
      const sampleCtx = sample ? `la muestra de ${sample}` : 'la muestra seleccionada';
      const stepMatch = t.match(/\b(corte|lugol|eosina|portaobjetos|cubreobjetos|colorante|fijación|extensión|frotis|agua)\b/);
      const step = stepMatch ? stepMatch[0] : null;
      const knownBlock = showsKnown
        ? ''
        : `Ya realicé los primeros pasos de la preparación, pero tengo una duda en el proceso. `;
      const stepCtx = step
        ? `Llegué al paso de agregar ${step} y no entiendo para qué sirve en este caso.`
        : `Tengo dudas sobre el orden correcto de los pasos.`;
      improved =
        `Estoy en la sección de preparación de ${labCtx} trabajando con ${sampleCtx}. ` +
        `${knownBlock}${stepCtx} ` +
        `¿Puedes explicarme qué función cumple ese paso y qué pasaría si lo omito?`;

    } else if (topic === 'observacion') {
      const sampleCtx = sample ? `la muestra de ${sample}` : 'la muestra seleccionada';
      const lensCtx   = lens   ? `el objetivo ${lens}`     : 'uno de los objetivos';
      const problemMatch = t.match(/\b(borroso|oscuro|no veo|no se ve|desenfocado|imagen mala|sin imagen|sin luz)\b/);
      const problem = problemMatch ? problemMatch[0] : 'la imagen no se ve con claridad';
      const knownBlock = showsKnown
        ? ''
        : `Seguí el procedimiento recomendado de comenzar con 4x y avanzar progresivamente, pero encontré un problema. `;
      improved =
        `Estoy en la sección de observación de ${labCtx} con ${sampleCtx}. ` +
        `${knownBlock}` +
        `Al usar ${lensCtx}, noto que ${problem}. Ya intenté ajustar el tornillo micrométrico y el nivel de iluminación. ` +
        `¿Qué otros ajustes debo revisar para obtener una imagen nítida?`;

    } else {
      // General fallback — still more specific than before
      const knownBlock = showsKnown ? '' : 'He revisado la sección de introducción del laboratorio, pero me quedó una duda. ';
      improved =
        `Estoy en ${labCtx}. ` +
        `${knownBlock}` +
        `${core}` +
        `${hasQ ? '' : ' ¿Puedes explicarme esto con detalle?'}`;
    }
  }

  return { total, clarity, context, spec, prior, complete,
           level, levelClass, desc, fb, improved, topic,
           hasQ, hasVerb, wc };
}

/* ═══════════════════════════════════════════════════
   RENDER
═══════════════════════════════════════════════════ */
function stars(pct) {
  const f = Math.round(pct / 20);
  return '⭐'.repeat(f) + '☆'.repeat(5 - f);
}

function renderResults(r) {
  const el = document.getElementById('results');
  el.style.display = 'block';
  el.classList.remove('fade-in'); void el.offsetWidth; el.classList.add('fade-in');

  // Type badge
  const badge = document.getElementById('typeBadge');
  badge.style.display = 'flex';
  badge.innerHTML = '🏷️ Tipo de pregunta identificado: <strong style="margin-left:4px">' + TOPICS[r.topic].label + '</strong>';

  // Score
  document.getElementById('scoreBadge').className = `score-badge ${r.levelClass}`;
  document.getElementById('scoreNum').textContent   = r.total;
  document.getElementById('scoreLevel').textContent = r.level;
  document.getElementById('scoreDesc').textContent  = r.desc;

  // Criteria bars
  const criteria = [
    { label:'💬 ¿La pregunta es clara?',          weight:'25%', score: r.clarity,  max:25, color:'#2563eb' },
    { label:'🌍 ¿Dice dónde está y qué hace?',    weight:'25%', score: r.context,  max:25, color:'#0d9488' },
    { label:'🎯 ¿Es suficientemente específica?', weight:'20%', score: r.spec,     max:20, color:'#7c3aed' },
    { label:'📚 ¿Muestra lo que ya sabe?',        weight:'15%', score: r.prior,    max:15, color:'#d97706' },
    { label:'📋 ¿Está completa?',                 weight:'15%', score: r.complete, max:15, color:'#dc2626' },
  ];
  const grid = document.getElementById('criteriaGrid');
  grid.innerHTML = '';
  criteria.forEach(c => {
    const pct = Math.round((c.score / c.max) * 100);
    const div = document.createElement('div');
    div.className = 'criterion';
    div.innerHTML = `
      <div class="criterion-header">
        <span class="criterion-name">${c.label}</span>
        <span class="criterion-weight">${c.weight}</span>
      </div>
      <div class="criterion-score-row">
        <div class="bar-track"><div class="bar-fill" style="background:${c.color};width:0" data-w="${pct}%"></div></div>
        <span class="criterion-points" style="color:${c.color}">${c.score}/${c.max}</span>
      </div>
      <div class="stars">${stars(pct)}</div>
    `;
    grid.appendChild(div);
  });
  requestAnimationFrame(() => document.querySelectorAll('.bar-fill').forEach(b => b.style.width = b.dataset.w));

  // Feedback
  const fbList = document.getElementById('feedbackList');
  fbList.innerHTML = '';
  r.fb.forEach(f => {
    const d = document.createElement('div');
    d.className = `feedback-item ${f.type}`;
    d.innerHTML = `<span class="feedback-icon">${f.icon}</span><span>${f.text}</span>`;
    fbList.appendChild(d);
  });

  // PE Tips
  const peTips = document.getElementById('peTips');
  const tips = TOPICS[r.topic].tips;
  if (tips && tips.length) {
    peTips.style.display = 'block';
    document.getElementById('peTipsList').innerHTML =
      tips.map(t => `<div class="pe-tip-item"><span>${t.i}</span><span>${t.t}</span></div>`).join('');
  } else { peTips.style.display = 'none'; }
  // Improved prompt
  const improvedBox = document.getElementById('improvedBox');
  if (r.improved) {
    improvedBox.style.display = 'block';
    document.getElementById('improvedText').textContent = r.improved;
  } else { improvedBox.style.display = 'none'; }

  setTimeout(() => el.scrollIntoView({ behavior:'smooth', block:'start' }), 100);
}

/* ═══════════════════════════════════════════════════
   HISTORY
═══════════════════════════════════════════════════ */
const history = [];
function addHistory(prompt, r) {
  history.unshift({ full: prompt, prompt: prompt.length > 80 ? prompt.slice(0,80)+'…' : prompt,
    score: r.total, level: r.level, time: new Date().toLocaleTimeString('es-CR',{hour:'2-digit',minute:'2-digit'}) });
  if (history.length > 8) history.pop();
  renderHistory();
}
function renderHistory() {
  const list = document.getElementById('historyList');
  if (!history.length) { list.innerHTML = '<div class="empty-history">Todavía no ha revisado ninguna pregunta. ¡Escriba una arriba y presione "Revisar mi pregunta"!</div>'; return; }
  list.innerHTML = '';
  history.forEach(h => {
    const col = h.score >= 85 ? '#16a34a' : h.score >= 68 ? '#2563eb' : h.score >= 48 ? '#d97706' : h.score >= 28 ? '#f97316' : '#dc2626';
    const d = document.createElement('div');
    d.className = 'history-item';
    d.innerHTML = `<div class="history-meta"><span class="history-score" style="color:${col}">${h.score}/100 · ${h.level}</span><span class="history-time">${h.time}</span></div><div class="history-prompt">${h.prompt}</div>`;
    d.addEventListener('click', () => { document.getElementById('promptInput').value = h.full; updateCC(); document.getElementById('promptInput').scrollIntoView({behavior:'smooth'}); });
    list.appendChild(d);
  });
}

/* ═══════════════════════════════════════════════════
   EVENTS
═══════════════════════════════════════════════════ */
function updateCC() {
  const len = document.getElementById('promptInput').value.length;
  const c   = document.getElementById('charCounter');
  c.textContent = `${len} / 600 caracteres`;
  c.className = 'char-counter' + (len > 480 ? ' warn' : '') + (len > 560 ? ' limit' : '');
}
document.getElementById('promptInput').addEventListener('input', updateCC);

document.getElementById('btnAnalyze').addEventListener('click', () => {
  const text = document.getElementById('promptInput').value.trim();
  if (!text) { alert('Por favor escribe un prompt antes de analizar.'); return; }
  const r = analyze(text);
  renderResults(r);
  addHistory(text, r);
});

document.getElementById('btnExample').addEventListener('click', () =>
  document.getElementById('examplesPanel').classList.toggle('show'));

document.getElementById('btnClear').addEventListener('click', () => {
  document.getElementById('promptInput').value = '';
  updateCC();
  document.getElementById('results').style.display = 'none';
  document.getElementById('examplesPanel').classList.remove('show');
});

document.getElementById('btnUseImproved').addEventListener('click', () => {
  document.getElementById('promptInput').value = document.getElementById('improvedText').textContent;
  updateCC();
  document.getElementById('promptInput').scrollIntoView({ behavior:'smooth' });
});

document.getElementById('btnCopyImproved').addEventListener('click', () => {
  const text = document.getElementById('improvedText').textContent;
  navigator.clipboard.writeText(text).then(() => {
    const btn = document.getElementById('btnCopyImproved');
    const original = btn.textContent;
    btn.textContent = '✅ ¡Copiado!';
    btn.style.background = '#16a34a';
    setTimeout(() => { btn.textContent = original; btn.style.background = '#0d9488'; }, 2000);
  });
});
// Example tabs
document.querySelectorAll('.ex-tab').forEach(tab => {
  tab.addEventListener('click', () => {
    document.querySelectorAll('.ex-tab').forEach(t => t.classList.remove('active'));
    tab.classList.add('active');
    const cat = tab.dataset.cat;
    document.querySelectorAll('.example-item').forEach(item =>
      item.classList.toggle('active-cat', item.dataset.cat === cat));
  });
});

// Load example on click
document.querySelectorAll('.example-item').forEach(item => {
  item.addEventListener('click', () => {
    document.getElementById('promptInput').value = item.dataset.prompt;
    updateCC();
    document.getElementById('examplesPanel').classList.remove('show');
    document.getElementById('promptInput').focus();
  });
});
</script>
</body>
</html>
