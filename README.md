<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>QUIZZ LOVE</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
/* ——— AQUÍ VA TODO TU CSS ——— */

:root{
  --vinotinto: #6f1022;
  --rosado: #ffb6d1;
  --fondo: #fff5f8;
  --card: #ffffff;
  --accent: #c81b46;
}
*{box-sizing:border-box}
body{
  margin:0;
  font-family: "Helvetica Neue", Arial, sans-serif;
  background: linear-gradient(180deg,var(--fondo), #fff0f5 60%);
  color: var(--vinotinto);
  display:flex;
  align-items:center;
  justify-content:center;
  min-height:100vh;
  padding:20px;
}
.app { width:100%; max-width:760px; background:var(--card); border-radius:18px; box-shadow:0 8px 30px rgba(111,16,34,0.12); overflow:hidden; }
header{
  background: linear-gradient(90deg,var(--vinotinto), #9b1630);
  color:white;
  padding:18px 22px;
  display:flex;
  gap:12px;
  align-items:center;
}
.logo {
  width:56px;height:56px; background: radial-gradient(circle at 20% 20%, #ffdeea, #ffd2e6 40%, rgba(255,255,255,0) 60%);
  border-radius:50%; display:flex; align-items:center; justify-content:center; font-weight:700; font-size:20px;
  color:var(--vinotinto); box-shadow:0 4px 12px rgba(0,0,0,0.08); border:3px solid rgba(255,255,255,0.12);
}

header h1{font-size:20px;margin:0}
header p{margin:0;font-size:12px;opacity:0.95}
.main { padding:22px; }

.card {
  background: linear-gradient(180deg, #fff, #fff7fb);
  border-radius:14px;
  padding:18px;
  box-shadow: 0 6px 18px rgba(111,16,34,0.04);
}

.center{text-align:center}
.menu{display:flex;gap:12px;justify-content:center;margin-bottom:16px}
.btn{
  background:var(--rosado);color:var(--vinotinto);border:none;padding:10px 16px;
  border-radius:12px;font-weight:600;cursor:pointer;box-shadow:0 6px 18px rgba(200,27,70,0.12);
}
.btn.primary{background:var(--accent);color:white}

.row{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:12px}

.vidas{font-weight:700;display:flex;gap:8px;align-items:center}
.hearts{display:flex;gap:6px}
.heart{width:18px;height:18px;border-radius:4px;background:var(--rosado);transition:.25s}
.heart.empty{opacity:0.18;background:#ffdfe8}

.progress{height:10px;background:#0001;border-radius:8px;overflow:hidden;flex:1;margin-left:12px}
.progress>i{display:block;height:100%;background:linear-gradient(90deg,var(--accent), #ff6b93);width:0%;transition:width .45s ease}

.question-title{font-size:18px;font-weight:700;margin-bottom:10px}

.options{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.option{
  border-radius:12px;padding:12px;background:#fff;border:2px solid #0002;
  cursor:pointer;font-weight:600;transition:.12s;
}
.option:hover{transform:translateY(-4px);box-shadow:0 12px 30px #0001}
.option.correct{background:#ecffef;border-color:#42b883}
.option.wrong{background:#fff0f0;border-color:#ff6b6b}

.small{font-size:13px;opacity:0.9}
.result{text-align:center;padding:18px}
.result h2{margin:6px 0;font-size:22px}

</style>
</head>

<body>

<!-- ——— AQUÍ VA TODO TU HTML ——— -->

<div class="app">
  <header>
    <div class="logo">QL</div>
    <div>
      <h1>QUIZZ LOVE</h1>
      <p>¿Cuánto me conoces? — 20 preguntas · 5 vidas</p>
    </div>
  </header>

  <div class="main">
    <div id="pantalla-inicio" class="card center fadeIn">
      <h2>Bienvenido a QUIZZ LOVE 💕</h2>
      <p class="small">Tienes <strong>5 vidas</strong>. Cada error resta 1 vida.</p>
      <div class="menu">
        <button class="btn primary" id="btn-jugar">Jugar</button>
        <button class="btn" id="btn-instrucciones">Instrucciones</button>
      </div>
    </div>

    <div id="pantalla-instrucciones" class="card" style="display:none">
      <h3>Instrucciones</h3>
      <ul style="text-align:left; margin:12px 0 16px 20px; line-height:1.5">
        <li>Responde las 20 preguntas.</li>
        <li>Cada pregunta tiene 4 opciones.</li>
        <li>Empiezas con 5 vidas.</li>
      </ul>
      <div style="text-align:center">
        <button class="btn primary" id="btn-comenzar">Comenzar</button>
        <button class="btn" id="btn-volver-menu">Volver</button>
      </div>
    </div>

    <div id="pantalla-juego" style="display:none">
      <div class="row">
        <div class="vidas small">
          <strong>Vidas</strong>
          <div class="hearts" id="hearts"></div>
        </div>
        <div class="progress"><i id="progressBar"></i></div>
        <div class="small" id="contador">Pregunta 1 / 20</div>
      </div>

      <div class="card fadeIn">
        <div class="question-title" id="pregunta-text"></div>
        <div class="options" id="opciones"></div>
        <div class="meta"><div class="small" id="feedback"></div></div>
      </div>
    </div>

    <div id="pantalla-resultado" class="card" style="display:none">
      <div class="result" id="resultado-contenido"></div>
      <div style="text-align:center;margin-top:12px">
        <button class="btn primary" id="btn-reintentar">Volver a jugar</button>
        <button class="btn" id="btn-ir-menu">Ir al menú</button>
      </div>
    </div>
  </div>

  <footer>Proyecto QUIZZ LOVE • Diseño: Valeria Machado</footer>
</div>

<script>
/* ——— AQUÍ VA TODO TU JAVASCRIPT ——— */

/* (PEGUÉ TODO TU JS AQUÍ) */
const preguntas = [
  {p:"¿Qué es lo que más me gusta que tú hagas por mí?", opciones:[
    {t:"Que me abraces"}, {t:"Que me des mucha atención", correcta:true}, {t:"Que me ignores"}, {t:"Que no me escribas"}
  ]},
  {p:"¿Cuál es mi comida favorita?", opciones:[
    {t:"Sushi"}, {t:"Pizza", correcta:true}, {t:"Ensalada"}, {t:"Hamburguesa"}
  ]},
  {p:"¿Qué cosa siempre me alegra el día?", opciones:[
    {t:"Estar a tu lado", correcta:true}, {t:"Dormir"}, {t:"Escuchar música"}, {t:"Comer algo rico"}
  ]},
  {p:"¿Cuál es mi color favorito?", opciones:[
    {t:"Azul"}, {t:"Rosado"}, {t:"Vinotinto", correcta:true}, {t:"Negro"}
  ]},
  {p:"¿Qué frase digo mucho?", opciones:[
    {t:"Ay ya"}, {t:"Q fuerte", correcta:true}, {t:"Literal"}, {t:"No sé"}
  ]},
  {p:"¿Qué me molesta demasiado?", opciones:[
    {t:"Que digas tonterías", correcta:true}, {t:"Que no me hables"}, {t:"Que llegues tarde"}, {t:"Que no me mires"}
  ]},
  {p:"¿Qué pensaste de mí al inicio?", opciones:[
    {t:"Que parecía interesante", correcta:true}, {t:"Que era aburrida"}, {t:"Que no te caía bien"}, {t:"Que no te importaba"}
  ]},
  {p:"¿Qué es lo que más amas de mí?", opciones:[
    {t:"Mi sonrisa"}, {t:"Mi forma de hablar"}, {t:"Mi personalidad", correcta:true}, {t:"Mi ropa"}
  ]},
  {p:"¿Qué parte de mí resalta más?", opciones:[
    {t:"Mis ojos"}, {t:"Mi cabello"}, {t:"Mi personalidad", correcta:true}, {t:"Mi voz"}
  ]},
  {p:"¿Cuál ha sido tu momento favorito conmigo?", opciones:[
    {t:"Cuando nos reímos juntos", correcta:true}, {t:"Cuando peleamos"}, {t:"Cuando no hablábamos"}, {t:"Cuando discutimos"}
  ]},
  {p:"¿Qué sientes cuando me ves sonreír?", opciones:[
    {t:"Felicidad", correcta:true}, {t:"Indiferencia"}, {t:"Molestia"}, {t:"Curiosidad"}
  ]},
  {p:"¿Por qué crees que te elegí a ti?", opciones:[
    {t:"Porque vi algo en ti que no vi en más nadie", correcta:true}, {t:"Por casualidad"}, {t:"Por aburrimiento"}, {t:"Porque no había más"}
  ]},
  {p:"¿Qué aprendiste desde que estamos juntos?", opciones:[
    {t:"A amar bonito", correcta:true}, {t:"A discutir más"}, {t:"A ignorar"}, {t:"A ser más frío"}
  ]},
  {p:"¿Qué necesito cuando estoy triste?", opciones:[
    {t:"Tu compañía", correcta:true}, {t:"Estar sola"}, {t:"Dormir"}, {t:"Que me dejes en paz"}
  ]},
  {p:"¿Cómo describirías mi forma de amar?", opciones:[
    {t:"Tierna e intensa", correcta:true}, {t:"Fría"}, {t:"Lejana"}, {t:"Confusa"}
  ]},
  {p:"¿Cómo soy emocionalmente?", opciones:[
    {t:"Tranquila"}, {t:"Intensa", correcta:true}, {t:"Cambiante"}, {t:"Reservada"}
  ]},
  {p:"¿Qué quiero que nunca dudes?", opciones:[
    {t:"Que confías en mí", correcta:true}, {t:"Que no me importas"}, {t:"Que no quieres nada"}, {t:"Que eres temporal"}
  ]},
  {p:"¿Qué parte de mí es más única?", opciones:[
    {t:"Mi forma de amar", correcta:true}, {t:"Mi forma de vestir"}, {t:"Mi forma de hablar"}, {t:"Mi forma de pensar"}
  ]},
  {p:"¿Qué siempre te da risa?", opciones:[
    {t:"Mis frases de la nada", correcta:true}, {t:"Mis enojos"}, {t:"Mis historias"}, {t:"Mis bromas"}
  ]},
  {p:"¿La situación más graciosa juntos?", opciones:[
    {t:"Cuando nos encontraron", correcta:true}, {t:"Cuando discutimos"}, {t:"Cuando nos perdimos"}, {t:"Cuando vimos una película"}
  ]}
];

let vidas = 5;
let indice = 0;
const total = preguntas.length;

const inicio = document.getElementById("pantalla-inicio");
const instrucciones = document.getElementById("pantalla-instrucciones");
const juego = document.getElementById("pantalla-juego");
const resultado = document.getElementById("pantalla-resultado");
const heartsEl = document.getElementById("hearts");
const progressBar = document.getElementById("progressBar");
const contador = document.getElementById("contador");
const preguntaText = document.getElementById("pregunta-text");
const opcionesEl = document.getElementById("opciones");
const feedback = document.getElementById("feedback");
const btnJugar = document.getElementById("btn-jugar");
const btnInstrucciones = document.getElementById("btn-instrucciones");
const btnComenzar = document.getElementById("btn-comenzar");
const btnVolver = document.getElementById("btn-volver-menu");
const btnReintentar = document.getElementById("btn-reintentar");
const btnIrMenu = document.getElementById("btn-ir-menu");

btnJugar.addEventListener("click", ()=>{ inicio.style.display="none"; juego.style.display="block"; startGame(); });
btnInstrucciones.addEventListener("click", ()=>{ inicio.style.display="none"; instrucciones.style.display="block"; });
btnComenzar.addEventListener("click", ()=>{ instrucciones.style.display="none"; juego.style.display="block"; startGame(); });
btnVolver.addEventListener("click", ()=>{ instrucciones.style.display="none"; inicio.style.display="block"; });
btnReintentar.addEventListener("click", ()=>{ resultado.style.display="none"; juego.style.display="block"; startGame(); });
btnIrMenu.addEventListener("click", ()=>{ resultado.style.display="none"; inicio.style.display="block"; });

function startGame(){
  vidas = 5;
  indice = 0;
  updateHearts();
  updateProgress();
  showQuestion();
  feedback.innerText = "";
}

function updateHearts(){
  heartsEl.innerHTML = "";
  for(let i=0;i<5;i++){
    const div = document.createElement("div");
    div.className = "heart" + (i < vidas ? "" : " empty");
    heartsEl.appendChild(div);
  }
}

function updateProgress(){
  const pct = Math.round((indice/total)*100);
  progressBar.style.width = pct + "%";
  contador.innerText = `Pregunta ${Math.min(indice+1,total)} / ${total}`;
}

function shuffleArray(arr){
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

function showQuestion(){
  if(indice >= total){
    showResult(true);
    return;
  }
  updateHearts();
  updateProgress();
  const q = preguntas[indice];
  preguntaText.innerText = q.p;

  let opciones = q.opciones.map(o => ({...o}));
  opciones = shuffleArray(opciones);
  opcionesEl.innerHTML = "";
  opciones.forEach(opt => {
    const btn = document.createElement("button");
    btn.className = "option";
    btn.innerText = opt.t;
    btn.onclick = ()=> selectOption(btn, opt, q);
    opcionesEl.appendChild(btn);
  });
  feedback.innerText = "";
}

function selectOption(btn, opt, q){
  const botones = opcionesEl.querySelectorAll("button");
  botones.forEach(b=>b.style.pointerEvents="none");

  if(opt.correcta){
    btn.classList.add("correct");
    feedback.innerText = "¡Correcto! 💖";
    setTimeout(()=>{
      indice++;
      showQuestion();
    }, 700);
  } else {
    btn.classList.add("wrong");
    feedback.innerText = "Oh no, esa no era 💔";
    botones.forEach(b=>{
      if(b.innerText === q.opciones.find(o=>o.correcta)?.t){
        b.classList.add("correct");
      }
    });
    vidas--;
    updateHearts();
    if(vidas <= 0){
      setTimeout(()=> showResult(false), 900);
      return;
    }
    setTimeout(()=>{
      indice++;
      showQuestion();
    }, 900);
  }
}

function showResult(victoria){
  juego.style.display = "none";
  resultado.style.display = "block";
  const cont = document.getElementById("resultado-contenido");

  if(victoria){
    cont.innerHTML = `
      <div style="font-size:44px">🎉</div>
      <h2>¡Victoria! Sabes mucho sobre mí 💕</h2>
      <p>Terminaste el quiz con <strong>${vidas}</strong> vidas.</p>
    `;
  } else {
    cont.innerHTML = `
      <div style="font-size:44px">💔</div>
      <h2>Perdiste, mi amor</h2>
      <p>Te quedaste sin vidas… pero puedes intentarlo otra vez 💗</p>
    `;
  }
}

inicio.style.display = "block";
instrucciones.style.display = "none";
juego.style.display = "none";
resultado.style.display = "none";

</script>

</body>
</html>
