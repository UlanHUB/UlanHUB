<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Официальная повестка на жор</title>
<style>
  * { box-sizing: border-box; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; }
  body {
    margin: 0; min-height: 100vh; display: flex; align-items: center; justify-content: center;
    background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 100%); padding: 16px; overflow: hidden;
  }
  .card {
    background: #fff; width: 100%; max-width: 420px; border-radius: 24px; padding: 28px 22px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.12); text-align: center; position: relative;
  }
  .gif { width: 180px; height: 180px; object-fit: cover; border-radius: 16px; margin-bottom: 16px; }
  h1 { font-size: 22px; margin: 0 0 10px; color: #222; }
  p { font-size: 15px; color: #555; margin: 0 0 22px; line-height: 1.4; }
  .btns { display: flex; flex-direction: column; gap: 10px; }
  .row { display: flex; gap: 12px; justify-content: center; position: relative; min-height: 50px; }
  button {
    border: none; border-radius: 14px; padding: 14px 18px; font-size: 16px; font-weight: 600;
    cursor: pointer; transition: transform 0.15s, background 0.2s;
  }
  .primary { background: #ff477e; color: #fff; }
  .primary:active { transform: scale(0.96); }
  .secondary { background: #f1f3f5; color: #333; }
  .option { background: #fff0f3; color: #c9184a; border: 2px solid #ffccd5; text-align: left; }
  .option:hover { background: #ffccd5; }
  .step { display: none; animation: fadeIn 0.3s ease; }
  .step.active { display: block; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
  .ticket {
    background: #f8f9fa; border: 2px dashed #ff477e; border-radius: 16px; padding: 16px;
    text-align: left; margin-bottom: 18px; font-size: 15px; color: #222;
  }
  .ticket div { margin-bottom: 8px; }
</style>
</head>
<body>

<div class="card">
  <!-- ЭКРАН 1: Вступление -->
  <div class="step active" id="step1">
    <img class="gif" src="https://giphy.com" alt="Кот">
    <h1>Внимание! Срочное собрание 🍔</h1>
    <p>Твой желудок подал жалобу в Верховный суд. Пойдём спасать ситуацию прямо сейчас?</p>
    <div class="row">
      <button class="primary" onclick="nextStep(2)">Да, я жрать хочу</button>
      <button class="secondary" id="runBtn" onmouseover="runAway()" ontouchstart="runAway()">Я не голодна</button>
    </div>
  </div>

  <!-- ЭКРАН 2: Выбор еды -->
  <div class="step" id="step2">
    <img class="gif" src="https://giphy.com" alt="Еда">
    <h1>Шаг 1. Чем заправляемся?</h1>
    <p>Выбирай сердцем, платит спонсор вечера:</p>
    <div class="btns">
      <button class="option" onclick="selectFood('🍣 Суши / роллы (на богатом)')">🍣 Суши / роллы (на богатом)</button>
      <button class="option" onclick="selectFood('🍔 Сочные бургеры (прощай, диета)')">🍔 Сочные бургеры (прощай, диета)</button>
      <button class="option" onclick="selectFood('🥟 Манты / лагман / плов (база)')">🥟 Манты / лагман / плов (база)</button>
      <button class="option" onclick="selectFood('🍕 Пицца и что-нибудь сладкое')">🍕 Пицца и что-нибудь сладкое</button>
      <button class="option" onclick="selectFood('🤷‍♀️ Мне пофиг, вези куда хочешь')">🤷‍♀️ «Мне пофиг» (опасный выбор)</button>
    </div>
  </div>

  <!-- ЭКРАН 3: Выбор места -->
  <div class="step" id="step3">
    <img class="gif" src="https://giphy.com" alt="Локация">
    <h1>Шаг 2. Куда держим путь?</h1>
    <p>Выбери локацию для сегодняшнего рейда:</p>
    <div class="btns">
      <button class="option" onclick="selectPlace('🏛️ В центре (красиво посидеть)')">🏛️ В центре (красиво посидеть)</button>
      <button class="option" onclick="selectPlace('🚗 В тачке с видом на город')">🚗 В тачке с видом на город</button>
      <button class="option" onclick="selectPlace('🛋️ Заказать домой и смотреть кино')">🛋️ Заказать домой под сериал</button>
      <button class="option" onclick="selectPlace('🎲 Секретное место от водителя')">🎲 Секретное место (на твой вкус)</button>
    </div>
  </div>

  <!-- ЭКРАН 4: Финал -->
  <div class="step" id="step4">
    <img class="gif" src="https://giphy.com" alt="Успех">
    <h1>Заказ принят в обработку! 🫡</h1>
    <p>Сделай скриншот этого чека и кинь мне в личку, чтобы выезд начался быстрее:</p>
    <div class="ticket">
      <div><strong>🍽️ Меню:</strong> <span id="resFood"></span></div>
      <div><strong>📍 Локация:</strong> <span id="resPlace"></span></div>
      <div><strong>💳 Оплата:</strong> Счёт закрывает лучший парень</div>
    </div>
    <button class="primary" onclick="copyResult()">Скопировать текст чека 📋</button>
  </div>
</div>

<script>
  let chosenFood = '';
  let chosenPlace = '';
  const phrases = ['Неа 😜', 'Мимо!', 'Ой, рука дрогнула?', 'Жми розовую!', 'Не вариант 😎', 'Подумай ещё раз'];
  let phraseIdx = 0;

  function runAway() {
    const btn = document.getElementById('runBtn');
    const x = Math.random() * 160 - 80;
    const y = Math.random() * 120 - 60;
    btn.style.transform = `translate(${x}px, ${y}px)`;
    btn.innerText = phrases[phraseIdx % phrases.length];
    phraseIdx++;
  }

  function nextStep(num) {
    document.querySelectorAll('.step').forEach(s => s.classList.remove('active'));
    document.getElementById('step' + num).classList.add('active');
  }

  function selectFood(food) {
    chosenFood = food;
    nextStep(3);
  }

  function selectPlace(place) {
    chosenPlace = place;
    document.getElementById('resFood').innerText = chosenFood;
    document.getElementById('resPlace').innerText = chosenPlace;
    nextStep(4);
  }

  function copyResult() {
    const text = `Я выбрала!\n🍽️ Еда: ${chosenFood}\n📍 Место: ${chosenPlace}\nВыезжай за мной!`;
    navigator.clipboard.writeText(text).then(() => {
      alert('Чек скопирован! Теперь просто вставь его мне в чат 😉');
    });
  }
</script>

</body>
</html>
