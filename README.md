<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Escape Room: Prevención de la Misoginia</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&family=Creepster&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  
  body {
    font-family: 'Poppins', sans-serif;
    overflow: hidden;
    background: #0a0a0a;
    color: white;
    cursor: default;
  }

  #gameContainer {
    position: fixed;
    width: 100vw;
    height: 100vh;
    perspective: 1000px;
  }

  .room {
    position: absolute;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    transition: all 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
  }

  #room1 {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    box-shadow: inset 0 0 100px rgba(0,0,0,0.5);
  }

  #room2 {
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    box-shadow: inset 0 0 100px rgba(0,0,0,0.5);
  }

  #room3 {
    background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
    box-shadow: inset 0 0 100px rgba(0,0,0,0.5);
  }

  #room4 {
    background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
    box-shadow: inset 0 0 100px rgba(0,0,0,0.5);
  }

  .room-title {
    position: absolute;
    top: 30px;
    left: 50%;
    transform: translateX(-50%);
    font-size: 3em;
    font-weight: 700;
    text-shadow: 0 5px 20px rgba(0,0,0,0.5);
    animation: glow 2s ease-in-out infinite alternate;
  }

  @keyframes glow {
    from { text-shadow: 0 5px 20px rgba(255,255,255,0.3); }
    to { text-shadow: 0 5px 40px rgba(255,255,255,0.8); }
  }

  .interactive-object {
    position: absolute;
    cursor: pointer;
    transition: all 0.3s;
    animation: pulse 2s ease-in-out infinite;
  }

  .interactive-object:hover {
    transform: scale(1.15) rotate(5deg);
    filter: brightness(1.3);
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
  }

  .object-icon {
    font-size: 5em;
    filter: drop-shadow(0 10px 20px rgba(0,0,0,0.4));
  }

  .status-hud {
    position: fixed;
    top: 20px;
    right: 20px;
    background: rgba(0,0,0,0.8);
    backdrop-filter: blur(20px);
    padding: 20px;
    border-radius: 15px;
    border: 2px solid rgba(255,255,255,0.2);
    z-index: 100;
    min-width: 200px;
  }

  .stat-row {
    display: flex;
    justify-content: space-between;
    margin: 10px 0;
    font-weight: 600;
    font-size: 1.1em;
  }

  .timer {
    font-size: 2em;
    color: #f56565;
    text-align: center;
    font-weight: 700;
  }

  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: linear-gradient(135deg, rgba(255,255,255,0.98), rgba(240,240,255,0.98));
    backdrop-filter: blur(30px);
    border-radius: 25px;
    padding: 40px;
    max-width: 700px;
    width: 90%;
    max-height: 80vh;
    overflow-y: auto;
    box-shadow: 0 30px 80px rgba(0,0,0,0.6);
    z-index: 1000;
    animation: modalIn 0.6s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    color: #2d3748;
  }

  @keyframes modalIn {
    from { transform: translate(-50%, -50%) scale(0.3) rotate(10deg); opacity: 0; }
    to { transform: translate(-50%, -50%) scale(1) rotate(0); opacity: 1; }
  }

  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(5px);
    z-index: 999;
  }

  .modal h2 {
    color: #2b6cb0;
    margin-bottom: 20px;
    font-size: 2em;
    text-align: center;
  }

  .modal p {
    margin: 15px 0;
    line-height: 1.8;
    font-size: 1.1em;
  }

  .modal input, .modal textarea {
    width: 100%;
    padding: 15px;
    border-radius: 12px;
    border: 2px solid #cbd5e0;
    font-family: inherit;
    font-size: 1em;
    margin: 10px 0;
    transition: border 0.3s;
  }

  .modal input:focus, .modal textarea:focus {
    outline: none;
    border-color: #667eea;
  }

  .btn {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border: none;
    padding: 16px 32px;
    border-radius: 12px;
    cursor: pointer;
    font-weight: 600;
    font-size: 1.1em;
    transition: all 0.3s;
    margin: 10px 5px;
    box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
  }

  .btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(102, 126, 234, 0.6);
  }

  .option-btn {
    display: block;
    width: 100%;
    margin: 12px 0;
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    text-align: left;
    padding: 18px 24px;
    font-size: 1em;
  }

  .option-btn.correct {
    background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%);
    animation: correctPulse 0.6s;
  }

  .option-btn.wrong {
    background: linear-gradient(135deg, #eb3349 0%, #f45c43 100%);
    animation: shake 0.5s;
  }

  @keyframes correctPulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.08); }
  }

  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    25% { transform: translateX(-15px) rotate(-5deg); }
    75% { transform: translateX(15px) rotate(5deg); }
  }

  .feedback {
    background: linear-gradient(135deg, rgba(102, 126, 234, 0.2), rgba(118, 75, 162, 0.2));
    padding: 20px;
    border-radius: 12px;
    margin: 20px 0;
    font-style: italic;
    border-left: 4px solid #667eea;
  }

  .drag-item {
    display: inline-block;
    background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
    padding: 14px 20px;
    margin: 10px;
    border-radius: 10px;
    cursor: grab;
    font-weight: 600;
    color: #333;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
  }

  .drag-item:active {
    cursor: grabbing;
  }

  .drop-zone {
    display: inline-block;
    background: rgba(255,255,255,0.3);
    border: 3px dashed #667eea;
    min-width: 220px;
    min-height: 60px;
    margin: 10px;
    border-radius: 12px;
    padding: 15px;
    vertical-align: middle;
    transition: all 0.3s;
  }

  .drop-zone:hover {
    background: rgba(255,255,255,0.5);
    border-color: #764ba2;
  }

  .diploma {
    border: 8px double #2b6cb0;
    background: linear-gradient(135deg, #fdfbfb 0%, #ebedee 100%);
    padding: 40px;
    border-radius: 15px;
    margin: 25px 0;
    text-align: center;
    box-shadow: 0 15px 40px rgba(0,0,0,0.3);
  }

  .diploma h3 {
    color: #2b6cb0;
    font-size: 2.2em;
    margin-bottom: 20px;
  }

  .diploma p {
    font-size: 1.2em;
    margin: 10px 0;
  }

  .resources {
    margin-top: 25px;
    padding: 20px;
    background: rgba(43, 108, 176, 0.1);
    border-radius: 12px;
  }

  .resources a {
    display: inline-block;
    margin: 10px;
    color: #2b6cb0;
    text-decoration: none;
    font-weight: 600;
    padding: 10px 20px;
    border-radius: 10px;
    background: rgba(255,255,255,0.5);
    transition: all 0.3s;
  }

  .resources a:hover {
    background: rgba(43, 108, 176, 0.3);
    transform: translateY(-2px);
  }

  .hidden { display: none !important; }

  .progress-bar {
    position: fixed;
    bottom: 0;
    left: 0;
    height: 8px;
    background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
    transition: width 0.5s;
    z-index: 100;
    box-shadow: 0 -2px 10px rgba(102, 126, 234, 0.5);
  }

  .room-hint {
    position: absolute;
    bottom: 40px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0,0,0,0.8);
    padding: 15px 30px;
    border-radius: 25px;
    font-size: 1.1em;
    animation: bounce 2s ease-in-out infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: translateX(-50%) translateY(0); }
    50% { transform: translateX(-50%) translateY(-10px); }
  }

  .particle-container {
    position: fixed;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;
  }

  .particle {
    position: absolute;
    width: 6px;
    height: 6px;
    background: white;
    border-radius: 50%;
    opacity: 0.7;
    animation: float-particle 4s infinite;
  }

  @keyframes float-particle {
    0%, 100% { transform: translateY(0) translateX(0); opacity: 0.3; }
    50% { transform: translateY(-30px) translateX(20px); opacity: 1; }
  }

  .code-input {
    display: flex;
    gap: 10px;
    justify-content: center;
    margin: 20px 0;
  }

  .code-digit {
    width: 60px;
    height: 60px;
    text-align: center;
    font-size: 2em;
    font-weight: bold;
    border: 3px solid #667eea;
    border-radius: 10px;
  }

  .instruction-list {
    text-align: left;
    margin: 20px 0;
    padding-left: 20px;
  }

  .instruction-list li {
    margin: 10px 0;
    font-size: 1.05em;
  }
</style>
</head>
<body>

<div class="particle-container" id="particles"></div>

<div id="gameContainer">
  <div class="room" id="room1">
    <div class="room-title">🚪 Sala 1: Conceptos Básicos</div>
    <div class="room-hint">Haz clic en los objetos brillantes para resolver los desafíos</div>
    <div class="interactive-object" style="top: 35%; left: 25%;" onclick="showChallenge(0, 0)">
      <div class="object-icon">📚</div>
    </div>
    <div class="interactive-object" style="top: 45%; right: 25%;" onclick="showChallenge(0, 1)">
      <div class="object-icon">💭</div>
    </div>
    <div class="interactive-object" style="bottom: 25%; left: 45%;" onclick="showChallenge(0, 2)">
      <div class="object-icon">🎯</div>
    </div>
  </div>

  <div class="room hidden" id="room2">
    <div class="room-title">🔐 Sala 2: Causas y Prevención</div>
    <div class="room-hint">Descubre las raíces del problema</div>
    <div class="interactive-object" style="top: 30%; left: 20%;" onclick="showChallenge(1, 0)">
      <div class="object-icon">🌱</div>
    </div>
    <div class="interactive-object" style="top: 40%; right: 20%;" onclick="showChallenge(1, 1)">
      <div class="object-icon">🗣️</div>
    </div>
    <div class="interactive-object" style="bottom: 20%; left: 50%;" onclick="showChallenge(1, 2)">
      <div class="object-icon">🛡️</div>
    </div>
    <div class="interactive-object" style="top: 55%; left: 60%;" onclick="showChallenge(1, 3)">
      <div class="object-icon">📖</div>
    </div>
  </div>

  <div class="room hidden" id="room3">
    <div class="room-title">🧩 Sala 3: Casos Complejos</div>
    <div class="room-hint">Analiza situaciones del mundo real</div>
    <div class="interactive-object" style="top: 35%; left: 30%;" onclick="showChallenge(2, 0)">
      <div class="object-icon">🔗</div>
    </div>
    <div class="interactive-object" style="top: 45%; right: 30%;" onclick="showChallenge(2, 1)">
      <div class="object-icon">⚖️</div>
    </div>
    <div class="interactive-object" style="bottom: 25%; left: 50%;" onclick="showChallenge(2, 2)">
      <div class="object-icon">💬</div>
    </div>
    <div class="interactive-object" style="top: 30%; right: 50%;" onclick="showChallenge(2, 3)">
      <div class="object-icon">🌍</div>
    </div>
  </div>

  <div class="room hidden" id="room4">
    <div class="room-title">🏆 Sala Final: Código de Escape</div>
    <div class="room-hint">Ingresa el código para escapar</div>
    <div class="interactive-object" style="top: 45%; left: 50%; transform: translateX(-50%);" onclick="showFinalCode()">
      <div class="object-icon">🔢</div>
    </div>
  </div>
</div>

<div class="status-hud">
  <div class="stat-row">
    <span>❤️ Vidas:</span>
    <span id="lives">3</span>
  </div>
  <div class="stat-row">
    <span>⭐ Puntos:</span>
    <span id="points">0</span>
  </div>
  <div class="stat-row">
    <span>🚪 Sala:</span>
    <span id="currentRoom">1</span>
  </div>
  <div class="timer" id="timer">20:00</div>
</div>

<div class="progress-bar" id="progressBar" style="width: 0%"></div>

<!-- Modal de inicio -->
<div class="modal-overlay" id="startOverlay"></div>
<div class="modal" id="startModal">
  <h2>🚪 Escape Room Educativo</h2>
  <p><strong>¡Bienvenido/a a una experiencia inmersiva!</strong></p>
  <p>Estás atrapado/a en una serie de salas misteriosas. Para escapar, debes resolver desafíos sobre la prevención de la misoginia.</p>
  <input type="text" id="playerName" placeholder="Escribe tu nombre..." />
  <button class="btn" onclick="showInstructions()">📖 Ver Instrucciones</button>
  <button class="btn" onclick="startGame()">🚀 Comenzar Directamente</button>
</div>

<!-- Modal de instrucciones -->
<div class="modal-overlay hidden" id="instructionsOverlay"></div>
<div class="modal hidden" id="instructionsModal">
  <h2>📖 Instrucciones del Escape Room</h2>
  <ul class="instruction-list">
    <li>🎯 <strong>Objetivo:</strong> Completa las 4 salas respondiendo correctamente a los desafíos</li>
    <li>❤️ <strong>Vidas:</strong> Tienes 3 vidas. Cada respuesta incorrecta te cuesta una vida</li>
    <li>⏱️ <strong>Tiempo:</strong> Tienes 20 minutos para completar todo el escape room</li>
    <li>🔑 <strong>Código Secreto:</strong> En cada sala, una pregunta aleatoria revelará un dígito del código final (¡Anótalo!)</li>
    <li>📝 <strong>Tipos de desafíos:</strong> Opción múltiple, preguntas abiertas y arrastrar-y-soltar</li>
    <li>🚪 <strong>Progresión:</strong> Debes completar TODOS los desafíos de una sala para avanzar a la siguiente</li>
    <li>🔢 <strong>Escape Final:</strong> En la Sala 4, ingresa el código de 4 dígitos que descubriste para escapar</li>
  </ul>
  <p style="text-align: center; margin-top: 20px;"><strong>💡 Consejo:</strong> Presta atención a los feedbacks después de cada respuesta correcta, ahí aparecerán los dígitos del código.</p>
  <button class="btn" onclick="startGameFromInstructions()">🚀 ¡Entendido, Comenzar!</button>
</div>

<!-- Modal de desafío -->
<div class="modal-overlay hidden" id="challengeOverlay"></div>
<div class="modal hidden" id="challengeModal">
  <h2 id="challengeTitle">Desafío</h2>
  <p id="challengeText"></p>
  <div id="challengeContent"></div>
  <div id="feedback" class="feedback hidden"></div>
  <button class="btn hidden" id="nextBtn" onclick="closeChallenge()">Continuar ➡️</button>
</div>

<!-- Modal de código final -->
  <div class="modal-overlay hidden" id="codeOverlay"></div>
<div class="modal hidden" id="codeModal">
  <h2>🔢 Código de Escape</h2>
  <p>Para escapar, ingresa el código de 4 dígitos que descubriste durante tu aventura.</p>
  <div class="code-input">
    <input type="text" class="code-digit" maxlength="1" id="digit1" />
    <input type="text" class="code-digit" maxlength="1" id="digit2" />
    <input type="text" class="code-digit" maxlength="1" id="digit3" />
    <input type="text" class="code-digit" maxlength="1" id="digit4" />
  </div>
  <button class="btn" onclick="checkCode()">🔓 Intentar Escape</button>
</div>

<!-- Modal final -->
<div class="modal-overlay hidden" id="endOverlay"></div>
<div class="modal hidden" id="endModal">
  <h2 id="endTitle"></h2>
  <p id="endText"></p>
  <div id="diplomaContainer"></div>
  <div class="resources">
    <p><strong>📚 Recursos para seguir aprendiendo:</strong></p>
    <a href="https://www.unwomen.org/es" target="_blank">ONU Mujeres</a>
    <a href="https://educandoenigualdad.com/" target="_blank">Educando en Igualdad</a>
    <a href="https://violenciagenero.igualdad.gob.es/" target="_blank">Ministerio de Igualdad</a>
  </div>
  <button class="btn" onclick="location.reload()">🔁 Jugar de nuevo</button>
</div>

<audio id="soundCorrect" src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg"></audio>
<audio id="soundWrong" src="https://actions.google.com/sounds/v1/cartoon/cartoon_boing.ogg"></audio>
<audio id="soundWin" src="https://actions.google.com/sounds/v1/cartoon/slide_whistle_to_drum_hit.ogg"></audio>
<audio id="soundDoor" src="https://actions.google.com/sounds/v1/doors/wood_door_close.ogg"></audio>

<script>
let playerName = "";
let lives = 3;
let score = 0;
let currentRoom = 0;
let solvedChallenges = [[], [], [], []];
let gameTimer;
let timeLeft = 1200; // 20 minutos
let codeDigits = ['2', '0', '2', '5'];
let revealedDigits = [false, false, false, false];
let secretChallenges = []; // Desafíos que revelarán los dígitos

const rooms = [
  {
    challenges: [
      {
        type: "mcq",
        text: "¿Qué es la misoginia?",
        options: [
          { text: "Respeto hacia las mujeres", correct: false },
          { text: "Odio o prejuicio hacia las mujeres", correct: true },
          { text: "Educación en igualdad", correct: false },
          { text: "Admiración por las mujeres", correct: false }
        ],
        feedback: "La misoginia implica actitudes de desprecio o discriminación hacia las mujeres."
      },
      {
        type: "mcq",
        text: "¿Cuál de estas acciones es un ejemplo de misoginia?",
        options: [
          { text: "Escuchar y valorar las ideas de una compañera", correct: false },
          { text: "Decir que las mujeres no son buenas en matemáticas", correct: true },
          { text: "Compartir tareas del hogar", correct: false },
          { text: "Apoyar la igualdad salarial", correct: false }
        ],
        feedback: "Los estereotipos negativos son una forma de misoginia cotidiana."
      },
      {
        type: "dragdrop",
        text: "Arrastra el ejemplo al concepto correcto:",
        pairs: [
          { cause: "Ignorar ideas de una ingeniera", effect: "Estereotipo laboral" },
          { cause: "Reírse de una alumna por ser mujer", effect: "Misoginia escolar" }
        ],
        feedback: "Estos ejemplos reflejan cómo la misoginia se manifiesta en distintos espacios."
      }
    ]
  },
  {
    challenges: [
      {
        type: "mcq",
        text: "¿Cuál es una causa importante de la misoginia?",
        options: [
          { text: "Roles de género tradicionales", correct: true },
          { text: "Educación en igualdad de género", correct: false },
          { text: "Empoderamiento femenino", correct: false },
          { text: "Leyes contra la violencia", correct: false }
        ],
        feedback: "Los roles tradicionales perpetúan prejuicios e injusticias."
      },
      {
        type: "open",
        text: "¿Qué podrías hacer si escuchas un chiste que ridiculiza a las mujeres?",
        feedback: "Puedes decir que no es gracioso, explicar por qué es dañino o cambiar el tema hacia algo positivo."
      },
      {
        type: "mcq",
        text: "¿Cuál de las siguientes opciones ayuda a prevenir la misoginia?",
        options: [
          { text: "Fomentar la educación en igualdad", correct: true },
          { text: "Evitar hablar del tema", correct: false },
          { text: "Reforzar los estereotipos", correct: false }
        ],
        feedback: "La educación y el diálogo son claves para prevenir la misoginia."
      },
      {
        type: "mcq",
        text: "¿Qué es el 'techo de cristal'?",
        options: [
          { text: "Una barrera invisible que impide a las mujeres ascender profesionalmente", correct: true },
          { text: "Un tipo de arquitectura moderna", correct: false },
          { text: "Una ley de protección laboral", correct: false },
          { text: "Un programa educativo", correct: false }
        ],
        feedback: "El 'techo de cristal' representa las barreras invisibles que limitan el avance de las mujeres en sus carreras."
      }
    ]
  },
  {
    challenges: [
      {
        type: "dragdrop",
        text: "Une causa con su consecuencia:",
        pairs: [
          { cause: "Publicidad muestra solo a mujeres cocinando", effect: "Refuerza estereotipos domésticos" },
          { cause: "Creencia de que los hombres no deben llorar", effect: "Reprime emociones masculinas" }
        ],
        feedback: "Los estereotipos afectan a toda la sociedad."
      },
      {
        type: "mcq",
        text: "Caso real: una comunidad ignora denuncias de acoso. ¿Qué representa esto?",
        options: [
          { text: "Cultura del silencio", correct: true },
          { text: "Empoderamiento comunitario", correct: false },
          { text: "Educación en igualdad", correct: false }
        ],
        feedback: "La cultura del silencio protege a agresores y perpetúa la misoginia."
      },
      {
        type: "open",
        text: "¿Qué mensaje darías a otras personas para promover el respeto entre géneros?",
        feedback: "Promover el respeto, la empatía y la igualdad ayuda a construir una sociedad más justa."
      },
      {
        type: "mcq",
        text: "¿Qué es la 'brecha salarial de género'?",
        options: [
          { text: "La diferencia de salario entre hombres y mujeres por el mismo trabajo", correct: true },
          { text: "Un programa de incentivos laborales", correct: false },
          { text: "Una política de recursos humanos", correct: false },
          { text: "Un impuesto específico", correct: false }
        ],
        feedback: "La brecha salarial refleja la discriminación económica que enfrentan las mujeres en el mundo laboral."
      }
    ]
  },
  {
    challenges: []
  }
];

// Crear partículas
const particlesContainer = document.getElementById('particles');
for (let i = 0; i < 40; i++) {
  const p = document.createElement('div');
  p.className = 'particle';
  p.style.left = Math.random() * 100 + '%';
  p.style.top = Math.random() * 100 + '%';
  p.style.animationDelay = Math.random() * 4 + 's';
  particlesContainer.appendChild(p);
}

function showInstructions() {
  playerName = document.getElementById('playerName').value.trim() || "Jugador/a";
  document.getElementById('startModal').classList.add('hidden');
  document.getElementById('startOverlay').classList.add('hidden');
  document.getElementById('instructionsModal').classList.remove('hidden');
  document.getElementById('instructionsOverlay').classList.remove('hidden');
}

function startGameFromInstructions() {
  document.getElementById('instructionsModal').classList.add('hidden');
  document.getElementById('instructionsOverlay').classList.add('hidden');
  initializeGame();
}

function startGame() {
  playerName = document.getElementById('playerName').value.trim() || "Jugador/a";
  document.getElementById('startModal').classList.add('hidden');
  document.getElementById('startOverlay').classList.add('hidden');
  initializeGame();
}

function initializeGame() {
  // Seleccionar desafíos aleatorios para revelar dígitos
  rooms.forEach((room, roomIndex) => {
    if (roomIndex < 3) { // Solo las primeras 3 salas
      const randomChallenge = Math.floor(Math.random() * room.challenges.length);
      secretChallenges.push({ room: roomIndex, challenge: randomChallenge });
    }
  });
  // Para la sala 4, el último dígito se revela automáticamente al entrar
  secretChallenges.push({ room: 3, challenge: 0 });
  
  startTimer();
}

function startTimer() {
  gameTimer = setInterval(() => {
    timeLeft--;
    const minutes = Math.floor(timeLeft / 60);
    const seconds = timeLeft % 60;
    document.getElementById('timer').textContent = 
      `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
    
    if (timeLeft <= 0) {
      clearInterval(gameTimer);
      endGame(false, "Se acabó el tiempo");
    }
  }, 1000);
}

function showChallenge(roomIndex, challengeIndex) {
  if (solvedChallenges[roomIndex].includes(challengeIndex)) {
    alert('Ya resolviste este desafío. ¡Busca otro!');
    return;
  }

  const challenge = rooms[roomIndex].challenges[challengeIndex];
  const modal = document.getElementById('challengeModal');
  const overlay = document.getElementById('challengeOverlay');
  const title = document.getElementById('challengeTitle');
  const text = document.getElementById('challengeText');
  const content = document.getElementById('challengeContent');
  const feedback = document.getElementById('feedback');
  const nextBtn = document.getElementById('nextBtn');

  modal.classList.remove('hidden');
  overlay.classList.remove('hidden');
  title.textContent = `Desafío ${challengeIndex + 1}`;
  text.textContent = challenge.text;
  content.innerHTML = '';
  feedback.classList.add('hidden');
  nextBtn.classList.add('hidden');

  modal.dataset.room = roomIndex;
  modal.dataset.challenge = challengeIndex;

  if (challenge.type === 'mcq') {
    challenge.options.forEach(opt => {
      const btn = document.createElement('button');
      btn.className = 'btn option-btn';
      btn.textContent = opt.text;
      btn.onclick = () => selectAnswer(opt.correct, challenge.feedback, btn);
      content.appendChild(btn);
    });
  } else if (challenge.type === 'open') {
    const textarea = document.createElement('textarea');
    textarea.placeholder = 'Escribe tu respuesta aquí...';
    textarea.rows = 5;
    content.appendChild(textarea);
    
    const submitBtn = document.createElement('button');
    submitBtn.className = 'btn';
    submitBtn.textContent = 'Enviar Respuesta';
    submitBtn.onclick = () => submitOpen(challenge.feedback, textarea);
    content.appendChild(submitBtn);
  } else if (challenge.type === 'dragdrop') {
    const dragContainer = document.createElement('div');
    dragContainer.style.marginTop = '20px';
    
    challenge.pairs.forEach((p, i) => {
      const cause = document.createElement('div');
      cause.className = 'drag-item';
      cause.textContent = p.cause;
      cause.draggable = true;
      cause.dataset.match = i;
      dragContainer.appendChild(cause);
    });
    
    dragContainer.appendChild(document.createElement('br'));
    dragContainer.appendChild(document.createElement('br'));
    
    challenge.pairs.forEach((p, i) => {
      const zone = document.createElement('div');
      zone.className = 'drop-zone';
      zone.textContent = p.effect;
      zone.dataset.match = i;
      dragContainer.appendChild(zone);
    });
    
    content.appendChild(dragContainer);
    enableDragDrop(challenge);
  }
}

function selectAnswer(correct, fb, btn) {
  const buttons = document.querySelectorAll('.option-btn');
  buttons.forEach(b => b.disabled = true);
  
  if (correct) {
    btn.classList.add('correct');
    score++;
    document.getElementById('soundCorrect').play();
    markChallengeComplete();
  } else {
    btn.classList.add('wrong');
    loseLife();
    document.getElementById('soundWrong').play();
  }
  
  showFeedback(fb);
  updateStatus();
}

function submitOpen(fb, textarea) {
  const answer = textarea.value.trim();
  
  if (answer.length > 15) {
    score++;
    document.getElementById('soundCorrect').play();
    markChallengeComplete();
  } else {
    showFeedback('Intenta dar una respuesta más completa y reflexiva.');
    return;
  }
  
  showFeedback(fb);
  updateStatus();
}

function enableDragDrop(challenge) {
  let correct = 0;
  let attempts = 0;
  const draggables = document.querySelectorAll('.drag-item');
  const zones = document.querySelectorAll('.drop-zone');
  
  draggables.forEach(d => {
    d.addEventListener('dragstart', ev => {
      ev.dataTransfer.setData('match', d.dataset.match);
    });
  });
  
  zones.forEach(z => {
    z.addEventListener('dragover', ev => ev.preventDefault());
    z.addEventListener('drop', ev => {
      ev.preventDefault();
      const match = ev.dataTransfer.getData('match');
      attempts++;
      
      if (match === z.dataset.match) {
        correct++;
        z.style.background = 'linear-gradient(135deg, #11998e 0%, #38ef7d 100%)';
        z.style.color = 'white';
        z.style.borderColor = '#11998e';
      } else {
        z.style.background = 'linear-gradient(135deg, #eb3349 0%, #f45c43 100%)';
        z.style.color = 'white';
        z.style.borderColor = '#eb3349';
        loseLife();
      }
      
      if (attempts === challenge.pairs.length) {
        if (correct === challenge.pairs.length) {
          score++;
          document.getElementById('soundCorrect').play();
          markChallengeComplete();
        } else {
          document.getElementById('soundWrong').play();
        }
        showFeedback(challenge.feedback);
        updateStatus();
      }
    });
  });
}

function showFeedback(text) {
  const feedback = document.getElementById('feedback');
  const modal = document.getElementById('challengeModal');
  const roomIndex = parseInt(modal.dataset.room);
  const challengeIndex = parseInt(modal.dataset.challenge);
  
  // Verificar si este desafío revela un dígito
  const secretIndex = secretChallenges.findIndex(
    s => s.room === roomIndex && s.challenge === challengeIndex
  );
  
  if (secretIndex !== -1 && !revealedDigits[secretIndex]) {
    revealedDigits[secretIndex] = true;
    const digitNumber = secretIndex + 1;
    text += `\n\n🔑 ¡Has desbloqueado el dígito ${digitNumber} del código de escape: <strong style="color:#667eea; font-size:1.5em">${codeDigits[secretIndex]}</strong>!`;
  }
  
  feedback.innerHTML = text;
  feedback.classList.remove('hidden');
  document.getElementById('nextBtn').classList.remove('hidden');
}

function markChallengeComplete() {
  const modal = document.getElementById('challengeModal');
  const roomIndex = parseInt(modal.dataset.room);
  const challengeIndex = parseInt(modal.dataset.challenge);
  
  solvedChallenges[roomIndex].push(challengeIndex);
  
  const totalChallenges = rooms[roomIndex].challenges.length;
  const progress = (solvedChallenges.flat().length / 
    (rooms[0].challenges.length + rooms[1].challenges.length + rooms[2].challenges.length)) * 100;
  document.getElementById('progressBar').style.width = progress + '%';
  
  if (solvedChallenges[roomIndex].length === totalChallenges) {
    setTimeout(() => {
      alert(`¡Completaste la Sala ${roomIndex + 1}!`);
      nextRoom();
    }, 1500);
  }
}

function closeChallenge() {
  document.getElementById('challengeModal').classList.add('hidden');
  document.getElementById('challengeOverlay').classList.add('hidden');
}

function loseLife() {
  lives--;
  if (lives <= 0) {
    clearInterval(gameTimer);
    endGame(false, "Te quedaste sin vidas");
  }
}

function updateStatus() {
  document.getElementById('lives').textContent = lives;
  document.getElementById('points').textContent = score;
}

function nextRoom() {
  document.getElementById(`room${currentRoom + 1}`).classList.add('hidden');
  currentRoom++;
  document.getElementById('currentRoom').textContent = currentRoom + 1;
  
  if (currentRoom < 4) {
    document.getElementById('soundDoor').play();
    document.getElementById(`room${currentRoom + 1}`).classList.remove('hidden');
  }
}

function showFinalCode() {
  // Revelar el último dígito automáticamente
  if (!revealedDigits[3]) {
    revealedDigits[3] = true;
  }
  
  document.getElementById('codeModal').classList.remove('hidden');
  document.getElementById('codeOverlay').classList.remove('hidden');
  
  // Mostrar el último dígito como pista final
  const codeModalText = document.querySelector('#codeModal p:nth-child(2)');
  codeModalText.innerHTML = `<strong>🔑 Último dígito revelado:</strong> El 4to dígito del código es <span style="color:#667eea; font-size:1.5em">${codeDigits[3]}</span><br><br>Ahora ingresa el código completo de 4 dígitos para escapar.`;
  
  // Auto-focus y navegación entre dígitos
  const digits = ['digit1', 'digit2', 'digit3', 'digit4'];
  digits.forEach((id, index) => {
    const input = document.getElementById(id);
    input.addEventListener('input', (e) => {
      if (e.target.value.length === 1 && index < 3) {
        document.getElementById(digits[index + 1]).focus();
      }
    });
    input.addEventListener('keydown', (e) => {
      if (e.key === 'Backspace' && e.target.value === '' && index > 0) {
        document.getElementById(digits[index - 1]).focus();
      }
    });
  });
  
  document.getElementById('digit1').focus();
}

function checkCode() {
  const code = 
    document.getElementById('digit1').value +
    document.getElementById('digit2').value +
    document.getElementById('digit3').value +
    document.getElementById('digit4').value;
  
  if (code === '2025') {
    document.getElementById('soundWin').play();
    clearInterval(gameTimer);
    endGame(true);
  } else {
    alert('❌ Código incorrecto. Intenta de nuevo.');
    loseLife();
    updateStatus();
    ['digit1', 'digit2', 'digit3', 'digit4'].forEach(id => {
      document.getElementById(id).value = '';
    });
    document.getElementById('digit1').focus();
  }
}

function endGame(won, reason = '') {
  document.getElementById('codeModal').classList.add('hidden');
  document.getElementById('codeOverlay').classList.add('hidden');
  
  const endModal = document.getElementById('endModal');
  const endOverlay = document.getElementById('endOverlay');
  const endTitle = document.getElementById('endTitle');
  const endText = document.getElementById('endText');
  const diplomaContainer = document.getElementById('diplomaContainer');
  
  endModal.classList.remove('hidden');
  endOverlay.classList.remove('hidden');
  
  if (won && lives > 0) {
    endTitle.textContent = '🏆 ¡Escapaste con Éxito!';
    endText.innerHTML = `<strong>${playerName}</strong>, lograste escapar del Escape Room con <strong>${score} puntos</strong> y <strong>${lives} vidas</strong> restantes.<br>Tu tiempo fue: <strong>${formatTime(1200 - timeLeft)}</strong>`;
    diplomaContainer.innerHTML = `
      <div class="diploma">
        <h3>🎓 Diploma de Escape</h3>
        <p>Otorgado a <strong>${playerName}</strong></p>
        <p>Por haber completado exitosamente el <strong>Escape Room Educativo sobre la Prevención de la Misoginia</strong>.</p>
        <p><em>Reconocido por su compromiso con la igualdad, el respeto y la justicia social.</em></p>
        <p style="margin-top: 20px; font-size: 0.9em;">Puntuación: ${score} puntos | Vidas: ${lives} | Tiempo: ${formatTime(1200 - timeLeft)}</p>
      </div>
    `;
  } else {
    endTitle.textContent = '😢 No lograste escapar';
    endText.textContent = reason || 'Pero puedes intentarlo de nuevo y aprender más.';
    diplomaContainer.innerHTML = '';
  }
}

function formatTime(seconds) {
  const mins = Math.floor(seconds / 60);
  const secs = seconds % 60;
  return `${mins}:${secs.toString().padStart(2, '0')}`;
}
</script>
</body>
</html>
}

showQuestion();
</script>
</body>
</html>
