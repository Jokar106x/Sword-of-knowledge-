# Sword-of-knowledge-
<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>سيف المعرفة - ضد الكمبيوتر</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: Arial, sans-serif; background-color: #eef1f4; margin:0; padding:0; direction:rtl; text-align:center;}
    header { background:#1e3a5f; color:#fff; padding:10px; font-size:24px;}
    .map { width:100%; max-width:800px; margin:auto; background:url('https://upload.wikimedia.org/wikipedia/commons/3/37/World_map_blank_without_borders.svg') center/contain no-repeat; height:400px; position:relative;}
    .castle { position:absolute; width:32px; height:32px; background:url('https://cdn-icons-png.flaticon.com/512/69/69524.png') center/contain no-repeat; }
    .player-info { display:flex; justify-content:space-around; padding:10px; background:#f0f0f0; margin-bottom:10px;}
    .hp-bar { width:100px; height:12px; background:red;}
    .question-box { background:#fff; padding:20px; margin:20px auto; box-shadow:0 2px 5px rgba(0,0,0,0.2); max-width:400px;}
    .answers button { margin:5px; padding:10px 15px; cursor:pointer;}
    .timer { font-size:20px; margin-top:10px; color:#1e3a5f;}
  </style>
</head>
<body>
  <header>لعبة سيف المعرفة - ضد الكمبيوتر</header>
  <div class="player-info">
    <div><strong>أنت</strong><br><div class="hp-bar" id="player-hp"></div></div>
    <div><strong>الكمبيوتر</strong><br><div class="hp-bar" id="ai-hp"></div></div>
  </div>
  <div class="map">
    <div class="castle" style="top:20%; left:60%;"></div>
    <div class="castle" style="top:50%; left:30%;"></div>
  </div>
  <div class="question-box">
    <div id="question">جاري تحميل السؤال...</div>
    <div class="answers" id="answers"></div>
    <div class="timer" id="timer">15 ثانية</div>
  </div>
  <script>
    const questions = [
      { question: "ما هي عاصمة اليابان؟", options: ["طوكيو","بكين","سيول","بانكوك"], correct: "طوكيو" },
      { question: "كم عدد كواكب المجموعة الشمسية؟", options: ["7","8","9","6"], correct: "8" },
      { question: "من هو مؤسس شركة مايكروسوفت؟", options: ["إيلون ماسك","بيل غيتس","ستيف جوبز","مارك زوكربيرغ"], correct: "بيل غيتس" }
    ];
    let current = 0, playerHP = 100, aiHP = 100;
    const playerHpBar = document.getElementById("player-hp"),
          aiHpBar     = document.getElementById("ai-hp"),
          questionBox = document.getElementById("question"),
          answersBox  = document.getElementById("answers"),
          timerBox    = document.getElementById("timer");

    function loadQuestion() {
      const q = questions[current];
      questionBox.textContent = q.question;
      answersBox.innerHTML = "";
      let time = 15;
      timerBox.textContent = `${time} ثانية`;
      const interval = setInterval(() => {
        time--; timerBox.textContent = `${time} ثانية`;
        if (time === 0) {
          clearInterval(interval);
          checkAnswer(null);
        }
      }, 1000);
      q.options.forEach(opt => {
        const btn = document.createElement("button");
        btn.textContent = opt;
        btn.onclick = () => {
          clearInterval(interval);
          checkAnswer(opt);
        };
        answersBox.appendChild(btn);
      });
    }

    function checkAnswer(answer) {
      const q = questions[current];
      if (answer === q.correct) aiHP -= 30;
      else playerHP -= 30;
      updateBars();
      current = (current + 1) % questions.length;
      if (playerHP <= 0 || aiHP <= 0) endGame();
      else setTimeout(loadQuestion, 1000);
    }

    function updateBars() {
      playerHpBar.style.width = `${playerHP}px`;
      aiHpBar.style.width = `${aiHP}px`;
    }

    function endGame() {
      questionBox.textContent = playerHP > 0 ? "🎉 لقد فزت!" : "😢 خسرت!";
      answersBox.innerHTML = '<button onclick="location.reload()">إعادة اللعب</button>';
      timerBox.textContent = "";
    }

    updateBars();
    loadQuestion();
  </script>
</body>
</html>
