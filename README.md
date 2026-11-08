<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Тест: Химические свойства</title>
<style>
  * { box-sizing: border-box; }
  body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea, #764ba2);
    min-height: 100vh;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
  }
  .card {
    background: #fff;
    width: 100%;
    max-width: 650px;
    padding: 30px;
    border-radius: 16px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  }
  h1 { margin: 0 0 10px; font-size: 22px; color: #333; text-align: center; }
  .progress { font-size: 14px; color: #888; margin-bottom: 20px; text-align: center; font-weight: bold;}
  .question { font-size: 17px; font-weight: bold; color: #222; margin-bottom: 20px; line-height: 1.4; }
  .options { display: flex; flex-direction: column; gap: 12px; }
  .option {
    padding: 14px 16px;
    border: 2px solid #ddd;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.2s;
    background: #fafafa;
    font-size: 15px;
  }
  .option:hover { border-color: #667eea; background: #f0f3ff; }
  .option.selected { border-color: #667eea; background: #e8ecff; }
  .option.correct { border-color: #2ecc71; background: #e8f8ef; }
  .option.wrong { border-color: #e74c3c; background: #fdecea; }
  button {
    margin-top: 25px;
    width: 100%;
    padding: 14px;
    background: #667eea;
    color: #fff;
    border: none;
    border-radius: 10px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: 0.2s;
  }
  button:hover { background: #5563c1; }
  button:disabled { background: #ccc; cursor: not-allowed; }
  .feedback { margin-top: 15px; font-weight: bold; font-size: 16px; text-align: center;}
  .feedback.ok { color: #2ecc71; }
  .feedback.bad { color: #e74c3c; }
  .final { text-align: center; }
  .final h2 { color: #333; margin-bottom: 10px;}
  .final p { font-size: 18px; color: #555;}
  .restart-btn { background: #2ecc71; margin-top: 15px;}
  .restart-btn:hover { background: #27ae60;}
</style>
</head>
<body>
<div class="card" id="app"></div>

<script>
// База вопросов сформирована на основе ваших фотографий и ключей ответов
const questions = [
  {
    question: "Этиламин может взаимодействовать с",
    options: ["пропаном", "хлорметаном", "гидроксидом натрия", "хлоридом калия"],
    correct: 1 // Ответ 2
  },
  {
    question: "Окисления глюкозы не происходит при действии на нее",
    options: ["уксусной кислоты", "оксида серебра(I) (аммиачный раствор)", "кислорода", "гидроксида меди(II) (аммиачный раствор)"],
    correct: 0 // Ответ 1
  },
  {
    question: "Верны ли следующие суждения о свойствах глюкозы?<br>А. Глюкоза окисляется соединениями серебра(I).<br>Б. Глюкоза является дисахаридом.",
    options: ["верно только А", "верно только Б", "верны оба суждения", "оба суждения неверны"],
    correct: 0 // Ответ 1
  },
  {
    question: "Метиламин способен реагировать с",
    options: ["серной кислотой", "гидроксидом натрия", "крахмалом", "ацетоном"],
    correct: 0 // Ответ 1
  },
  {
    question: "И с аммиачным раствором оксида серебра(I), и с гидроксидом меди(II) может реагировать",
    options: ["глюкоза", "сахароза", "крахмал", "этанол"],
    correct: 0 // Ответ 1
  },
  {
    question: "Верны ли суждения?<br>А. Жиры — это сложные эфиры глицерина и карбоновых кислот.<br>Б. Омыление жиров происходит в щелочной среде.",
    options: ["Оба суждения верны", "Верно только А", "Верно только Б", "Оба суждения не верны"],
    correct: 0 // Ответ 1
  },
  {
    question: "Хлороводородная кислота взаимодействует с веществом:",
    options: ["аминоуксусная кислота", "глюкоза", "фруктоза", "целлюлоза"],
    correct: 0 // Ответ 1
  },
  {
    question: "Основные свойства проявляют",
    options: ["сложные эфиры", "альдегиды", "аминокислоты", "карбоновые кислоты"],
    correct: 2 // Ответ 3
  },
  {
    question: "С гидроксидом натрия взаимодействует:",
    options: ["метиэтиламин", "глицин", "триметиламин", "фениламин"],
    correct: 1 // Ответ 2
  },
  {
    question: "Аланин НЕ взаимодействует со следующим веществом:",
    options: ["кислород", "гидроксид натрия", "хлороводородная кислота", "сульфат натрия"],
    correct: 3 // Ответ 4
  },
  {
    question: "И анилин, и аланин могут реагировать со следующим веществом:",
    options: ["кислород", "водный раствор гидроксида натрия", "карбонат натрия", "пропен"],
    correct: 0 // Ответ 1
  },
  {
    question: "Полисахаридом является",
    options: ["глюкоза", "сахароза", "целлюлоза", "фруктоза"],
    correct: 2 // Ответ 3
  },
  {
    question: "С кислотами реагирует",
    options: ["CH₃NH₂", "CH₂(ONO₂)CH(ONO₂)CH₂(ONO₂)", "C₆H₃(NO₂)₃", "C₆H₅NO₂"],
    correct: 0 // Ответ 1
  },
  {
    question: "2-Аминопропановая кислота НЕ реагирует с",
    options: ["HCl", "NaOH", "C₂H₅OH", "CH₄"],
    correct: 3 // Ответ 4
  },
  {
    question: "Моносахаридами являются",
    options: ["глюкоза и мальтоза", "фруктоза и глюкоза", "крахмал и фруктоза", "сахароза и целлюлоза"],
    correct: 1 // Ответ 2
  },
  {
    question: "Анилин взаимодействует с",
    options: ["бромной водой", "аммиаком", "этанолом", "оксидом серебра"],
    correct: 0 // Ответ 1
  },
  {
    question: "При щелочном гидролизе жиров образуются",
    options: ["карбоновые кислоты и вода", "глицерин и вода", "глицерин и карбоновые кислоты", "глицерин и мыла"],
    correct: 3 // Ответ 4
  },
  {
    question: "Метиламин взаимодействует",
    options: ["с соляной кислотой", "с гидроксидом натрия", "с медью", "с метаном"],
    correct: 0 // Ответ 1
  },
  {
    question: "Верны ли следующие суждения о свойствах жиров?<br>А. Жиры подвергаются гидролизу.<br>Б. Жиры окисляются кислородом.",
    options: ["верно только А", "верно только Б", "верны оба суждения", "оба суждения неверны"],
    correct: 2 // Ответ 3
  },
  {
    question: "Белки НЕ",
    options: ["построены из остатков аминокислот", "содержат в своем составе только углерод, водород и кислород", "гидролизуются в кислотной и щелочной среде", "способны к денатурации"],
    correct: 1 // Ответ 2
  },
  {
    question: "Жиры",
    options: ["построены из остатков глюкозы", "содержат в своем составе только углерод, водород и кислород", "не гидролизуются в кислотной и щелочной среде", "способны к денатурации"],
    correct: 1 // Ответ 2
  },
  {
    question: "Анилин взаимодействует с",
    options: ["Br₂(р)", "NaOH(р)", "Na₂CO₃(р)", "Na₂SO₄(р)"],
    correct: 0 // Ответ 1
  },
  {
    question: "Реакция «серебряного зеркала» возможна для следующего углевода:",
    options: ["глюкоза", "сахароза", "фруктоза", "крахмал"],
    correct: 0 // Ответ 1
  },
  {
    question: "Разбавленная хлороводородная кислота будет реагировать с веществом:",
    options: ["аминоуксусная кислота", "глюкоза", "фруктоза", "целлюлоза"],
    correct: 0 // Ответ 1
  },
  {
    question: "Анилин НЕ",
    options: ["содержит аминогруппу", "является более слабым основанием, чем аммиак", "реагирует с кислотами с образованием солей", "построен из цепочек, образованных остатками аминокислот"],
    correct: 3 // Ответ 4
  },
  {
    question: "И анилин, и аланин",
    options: ["содержат аминогруппу", "являются более сильными основаниями, чем аммиак", "являются ароматическими соединениями", "содержат карбоксильную группу"],
    correct: 0 // Ответ 1
  },
  {
    question: "Аминокислоты НЕ",
    options: ["проявляют кислотные свойства", "проявляют основные свойства", "вступают в реакции со спиртами", "образуют жиры"],
    correct: 3 // Ответ 4
  },
  {
    question: "Амфотерные свойства аминокислот обусловлены наличием в них",
    options: ["карбоксильной группы и аминогруппы", "гидроксильной группы и аминогруппы", "карбонильной и карбоксильной групп", "карбоксильной группы и нитрогруппы"],
    correct: 0 // Ответ 1
  },
  {
    question: "Аминоуксусная кислота НЕ реагирует с",
    options: ["HCl", "NaOH", "C₂H₅OH", "CH₄"],
    correct: 3 // Ответ 4
  },
  {
    question: "Моносахаридом является",
    options: ["глюкоза", "сахароза", "крахмал", "лактоза"],
    correct: 0 // Ответ 1
  },
  {
    question: "Метиламин реагирует с",
    options: ["Br₂(р)", "NaOH(р)", "HCl(р)", "Na₂CO₃(р)"],
    correct: 2 // Ответ 3
  },
  {
    question: "Глицин может взаимодействовать с",
    options: ["хлороводородом", "медью", "нитратом калия", "этином"],
    correct: 0 // Ответ 1
  }
];

let current = 0;
let selected = null;
let score = 0;
const app = document.getElementById("app");

function renderQuestion() {
  const q = questions[current];
  selected = null;
  app.innerHTML = `
    <h1>Тест по химии</h1>
    <div class="progress">Вопрос ${current + 1} из ${questions.length}</div>
    <div class="question">${current + 1}. ${q.question}</div>
    <div class="options" id="options"></div>
    <button id="checkBtn" disabled>Ответить</button>
    <div class="feedback" id="feedback"></div>
  `;

  const optionsEl = document.getElementById("options");
  q.options.forEach((opt, i) => {
    const div = document.createElement("div");
    div.className = "option";
    div.innerHTML = `<b>${i + 1})</b> ${opt}`;
    div.onclick = () => {
      if (document.getElementById("feedback").textContent) return;
      selected = i;
      document.querySelectorAll(".option").forEach(el => el.classList.remove("selected"));
      div.classList.add("selected");
      document.getElementById("checkBtn").disabled = false;
    };
    optionsEl.appendChild(div);
  });

  document.getElementById("checkBtn").onclick = checkAnswer;
}

function checkAnswer() {
  const q = questions[current];
  const feedback = document.getElementById("feedback");
  const optionsEls = document.querySelectorAll(".option");

  optionsEls.forEach((el, i) => {
    if (i === q.correct) el.classList.add("correct");
    if (i === selected && i !== q.correct) el.classList.add("wrong");
  });

  if (selected === q.correct) {
    feedback.textContent = "✅ Правильно!";
    feedback.className = "feedback ok";
    score++;
  } else {
    feedback.textContent = "❌ Неправильно. Правильный ответ: " + (q.correct + 1);
    feedback.className = "feedback bad";
  }

  const btn = document.getElementById("checkBtn");
  btn.disabled = false;
  if (current === questions.length - 1) {
    btn.textContent = "Показать результат";
    btn.onclick = showResult;
  } else {
    btn.textContent = "Следующий вопрос";
    btn.onclick = nextQuestion;
  }
}

function nextQuestion() {
  current++;
  renderQuestion();
}

function showResult() {
  const percent = Math.round((score / questions.length) * 100);
  let mark = "";
  if (percent >= 90) mark = "Отлично! 5";
  else if (percent >= 75) mark = "Хорошо! 4";
  else if (percent >= 50) mark = "Удовлетворительно. 3";
  else mark = "Нужно повторить материал. 2";

  app.innerHTML = `
    <div class="final">
      <h2>Тест завершён!</h2>
      <p>Правильных ответов: <b>${score} из ${questions.length}</b></p>
      <p>Ваш результат: <b>${percent}%</b></p>
      <p>Оценка: <b>${mark}</b></p>
      <button class="restart-btn" onclick="location.reload()">Пройти заново</button>
    </div>
  `;
}

renderQuestion();
</script>
</body>
</html>
