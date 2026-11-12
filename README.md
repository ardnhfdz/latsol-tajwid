<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Tajwid - 20 Soal</title>
<style>
* { box-sizing: border-box; }
body {
  font-family: Arial, sans-serif;
  background: #f3f4f6;
  margin: 0;
  padding: 20px;
  color: #222;
}
.container {
  max-width: 620px;
  margin: 30px auto;
  background: white;
  padding: 25px;
  border-radius: 18px;
  box-shadow: 0 4px 15px #0001;
}
h1 { text-align: center; }
.info { color: #666; text-align: center; }
.progress {
  height: 8px;
  background: #eee;
  border-radius: 10px;
  margin: 20px 0;
  overflow: hidden;
}
#bar {
  height: 8px;
  width: 0%;
  background: #16a34a;
  transition: .3s;
}
.ayat {
  font-size: 32px;
  text-align: center;
  direction: rtl;
  line-height: 1.9;
  margin: 25px 0;
}
.option {
  display: block;
  width: 100%;
  text-align: left;
  padding: 15px;
  margin: 10px 0;
  border: 2px solid #ddd;
  background: white;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: .2s;
}
.option:hover {
  background: #f0fdf4;
  border-color: #16a34a;
}
.option:disabled { cursor: default; opacity: 1; }
.correct { background: #dcfce7; border-color: #16a34a; }
.wrong { background: #fee2e2; border-color: #dc2626; }
#feedback { font-weight: bold; margin: 15px 0; }
button.next {
  width: 100%;
  padding: 14px;
  background: #166534;
  color: white;
  border: 0;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
}
.hidden { display: none; }
#score { color: #166534; }
</style>
</head>

<body>
<div class="container">

  <h1>📖 Kuis Tajwid</h1>
  <p class="info">Identifikasi Hukum Tajwid — 20 Soal</p>

  <div id="quiz">
    <p id="number"></p>
    <div class="progress"><div id="bar"></div></div>
    <div class="ayat" id="ayat"></div>
    <h3 id="question"></h3>
    <div id="options"></div>
    <p id="feedback"></p>
    <button class="next hidden" id="next">Soal Berikutnya</button>
  </div>

  <div id="result" class="hidden">
    <h2>Hasil Kuis 🎉</h2>
    <h1 id="score"></h1>
    <p id="detail"></p>
    <p id="grade"></p>
    <button class="next" onclick="restart()">Ulangi Kuis</button>
  </div>

</div>

<script>
const questions = [
{
 ayat:"أَنْعَمْتَ",
 question:"Apa hukum nun mati pada أَنْعَمْتَ?",
 options:["Ikhfa Haqiqi","Izhar Halqi","Iqlab","Idgham Bighunnah"],
 answer:1
},
{
 ayat:"مِنْ وَالٍ",
 question:"Apa hukum nun mati bertemu و?",
 options:["Idgham Bighunnah","Idgham Bilaghunnah","Izhar Halqi","Iqlab"],
 answer:0
},
{
 ayat:"سَمِيعٌ بَصِيرٌ",
 question:"Apa hukum tanwin bertemu ب?",
 options:["Iqlab","Ikhfa Haqiqi","Izhar Halqi","Idgham Bilaghunnah"],
 answer:0
},
{
 ayat:"لَهُمْ مَا يَشَاءُونَ",
 question:"Apa hukum mim mati bertemu م?",
 options:["Ikhfa Syafawi","Idgham Mimi","Izhar Syafawi","Idgham Bighunnah"],
 answer:1
},
{
 ayat:"مِنْ لَدُنْهُ",
 question:"Apa hukum nun mati bertemu ل?",
 options:["Idgham Bighunnah","Idgham Bilaghunnah","Ikhfa Haqiqi","Iqlab"],
 answer:1
},
{
 ayat:"فِي أَنْفُسِكُمْ",
 question:"Apa hukum mad pada فِي jika disambung ke kata berikutnya?",
 options:["Mad Wajib Muttashil","Mad Jaiz Munfashil","Mad Thabi'i","Mad Badal"],
 answer:1
},
{
 ayat:"إِنَّ اللَّهَ",
 question:"Bagaimana hukum lafaz Allah pada اللَّهَ?",
 options:["Tafkhim","Tarqiq","Ikhfa","Idgham"],
 answer:0
},
{
 ayat:"بِسْمِ اللَّهِ",
 question:"Bagaimana hukum lafaz Allah pada اللَّهِ?",
 options:["Tafkhim","Tarqiq","Ikhfa","Qalqalah"],
 answer:1
},
{
 ayat:"قُلْ هُوَ اللَّهُ أَحَدٌ",
 question:"Apa hukum huruf د pada أَحَدٌ jika berhenti di situ?",
 options:["Qalqalah Kubra","Mad Thabi'i","Mad Lin","Ikhfa"],
 answer:0
},
{
 ayat:"وَالشَّمْسِ",
 question:"Apa hukum alif lam pada kata tersebut?",
 options:["Alif Lam Qamariyah","Alif Lam Syamsiyah","Idgham Mimi","Izhar Halqi"],
 answer:1
},
{
 ayat:"مِنْ قَبْلُ",
 question:"Apa hukum nun mati bertemu ق?",
 options:["Izhar Halqi","Iqlab","Ikhfa Haqiqi","Idgham Bighunnah"],
 answer:2
},
{
 ayat:"مِنْ عِلْمٍ",
 question:"Apa hukum nun mati bertemu ع?",
 options:["Ikhfa Haqiqi","Izhar Halqi","Iqlab","Idgham Bilaghunnah"],
 answer:1
},
{
 ayat:"هُمْ فِيهَا",
 question:"Apa hukum mim mati bertemu ف?",
 options:["Idgham Mimi","Ikhfa Syafawi","Izhar Syafawi","Iqlab"],
 answer:2
},
{
 ayat:"تَرْمِيهِمْ بِحِجَارَةٍ",
 question:"Apa hukum mim mati bertemu ب?",
 options:["Izhar Syafawi","Ikhfa Syafawi","Idgham Mimi","Ikhfa Haqiqi"],
 answer:1
},
{
 ayat:"الْقَمَرُ",
 question:"Apa hukum alif lam pada kata tersebut?",
 options:["Alif Lam Syamsiyah","Alif Lam Qamariyah","Idgham Bilaghunnah","Izhar Halqi"],
 answer:1
},
{
 ayat:"وَلَا الضَّالِّينَ",
 question:"Apa hukum mad pada الضَّالِّينَ?",
 options:["Mad Aridh Lissukun","Mad Lin","Mad Lazim Kalimi Mutsaqqal","Mad Wajib Muttashil"],
 answer:2
},
{
 ayat:"جَاءَ",
 question:"Apa hukum mad pada kata tersebut?",
 options:["Mad Thabi'i","Mad Wajib Muttashil","Mad Jaiz Munfashil","Mad Badal"],
 answer:1
},
{
 ayat:"يَا أَيُّهَا",
 question:"Apa hukum mad pada يَا jika disambung ke أَيُّهَا?",
 options:["Mad Thabi'i","Mad Badal","Mad Jaiz Munfashil","Mad Wajib Muttashil"],
 answer:2
},
{
 ayat:"قَالُوا",
 question:"Apa hukum mad pada قَا?",
 options:["Mad Thabi'i","Mad Wajib Muttashil","Mad Lazim","Mad Aridh Lissukun"],
 answer:0
},
{
 ayat:"خَوْفٌ",
 question:"Jika berhenti pada kata tersebut, apa hukum madnya?",
 options:["Mad Thabi'i","Mad Lin","Mad Badal","Mad Wajib Muttashil"],
 answer:1
}
];

let current = 0;
let score = 0;
let locked = false;

function showQuestion() {
  locked = false;
  const q = questions[current];

  document.getElementById("number").textContent =
    `Soal ${current + 1} dari ${questions.length}`;

  document.getElementById("bar").style.width =
    `${(current / questions.length) * 100}%`;

  document.getElementById("ayat").textContent = q.ayat;
  document.getElementById("question").textContent = q.question;
  document.getElementById("feedback").textContent = "";
  document.getElementById("next").classList.add("hidden");

  const options = document.getElementById("options");
  options.innerHTML = "";

  q.options.forEach((option, index) => {
    const btn = document.createElement("button");
    btn.className = "option";
    btn.textContent = `${String.fromCharCode(65 + index)}. ${option}`;
    btn.onclick = () => chooseAnswer(index, btn);
    options.appendChild(btn);
  });
}

function chooseAnswer(index, selectedButton) {
  if (locked) return;
  locked = true;

  const q = questions[current];
  const buttons = document.querySelectorAll(".option");

  buttons.forEach((btn, i) => {
    btn.disabled = true;
    if (i === q.answer) {
      btn.classList.add("correct");
    }
  });

  const feedback = document.getElementById("feedback");

  if (index === q.answer) {
    score++;
    feedback.textContent = "Benar! 🎉";
    feedback.style.color = "green";
  } else {
    selectedButton.classList.add("wrong");
    feedback.textContent = "Belum tepat!";
    feedback.style.color = "red";
  }

  document.getElementById("bar").style.width =
    `${((current + 1) / questions.length) * 100}%`;

  document.getElementById("next").classList.remove("hidden");
  document.getElementById("next").textContent =
    current === questions.length - 1
      ? "Lihat Hasil"
      : "Soal Berikutnya";
}

document.getElementById("next").onclick = () => {
  current++;

  if (current < questions.length) {
    showQuestion();
  } else {
    showResult();
  }
};

function showResult() {
  document.getElementById("quiz").classList.add("hidden");
  document.getElementById("result").classList.remove("hidden");

  const percentage = (score / questions.length) * 100;

  document.getElementById("score").textContent =
    `${score} / ${questions.length}`;

  document.getElementById("detail").textContent =
    `Benar: ${score} | Salah: ${questions.length - score} | Nilai: ${percentage}`;

  let grade = "";
  if (score >= 18) grade = "Luar biasa! Penguasaan tajwid lu mantap.";
  else if (score >= 15) grade = "Bagus! Tinggal sedikit lagi.";
  else if (score >= 10) grade = "Lumayan! Terus latihan.";
  else grade = "Yuk, pelajari lagi materi tajwidnya.";

  document.getElementById("grade").textContent = grade;
}

function restart() {
  current = 0;
  score = 0;

  document.getElementById("result").classList.add("hidden");
  document.getElementById("quiz").classList.remove("hidden");

  showQuestion();
}

showQuestion();
</script>
</body>
</html>
