<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Tajwid Lengkap</title>
<style>
* { box-sizing: border-box; }
body {
  font-family: Arial, sans-serif;
  background: #f1f5f9;
  margin: 0;
  padding: 20px;
  color: #1e293b;
}
.container {
  max-width: 650px;
  margin: 25px auto;
  padding: 25px;
  background: white;
  border-radius: 18px;
  box-shadow: 0 5px 20px #00000012;
}
h1 { text-align: center; margin-bottom: 5px; }
.subtitle { text-align: center; color: #64748b; }
.progress {
  height: 9px;
  background: #e2e8f0;
  border-radius: 20px;
  margin: 20px 0;
  overflow: hidden;
}
#bar {
  height: 100%;
  width: 0%;
  background: #16a34a;
  transition: .3s;
}
#number { color: #64748b; font-weight: bold; }
#category {
  display: inline-block;
  padding: 6px 10px;
  border-radius: 20px;
  background: #dcfce7;
  color: #166534;
  font-size: 12px;
  margin: 5px 0;
}
#question { line-height: 1.6; }
.ayat {
  font-size: 32px;
  text-align: center;
  direction: rtl;
  line-height: 2;
  margin: 20px 0;
}
.option {
  width: 100%;
  padding: 14px;
  margin: 7px 0;
  text-align: left;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  background: white;
  font-size: 15px;
  cursor: pointer;
}
.option:hover:not(:disabled) {
  border-color: #16a34a;
  background: #f0fdf4;
}
.option:disabled { cursor: default; opacity: 1; }
.correct { background: #dcfce7; border-color: #16a34a; }
.wrong { background: #fee2e2; border-color: #dc2626; }
#feedback {
  padding: 12px;
  border-radius: 10px;
  line-height: 1.5;
  margin: 15px 0;
}
.next {
  width: 100%;
  padding: 14px;
  background: #166534;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
  margin-top: 10px;
}
.hidden { display: none; }
#score { color: #166534; font-size: 42px; }
.center { text-align: center; }
</style>
</head>

<body>
<div class="container">
  <h1>📖 Kuis Tajwid Lengkap</h1>
  <p class="subtitle">20 Soal • Semua Materi</p>

  <div id="quiz">
    <p id="number"></p>
    <div class="progress"><div id="bar"></div></div>
    <span id="category"></span>
    <div class="ayat" id="ayat"></div>
    <h3 id="question"></h3>
    <div id="options"></div>
    <div id="feedback" class="hidden"></div>
    <button class="next hidden" id="next">Soal Berikutnya</button>
  </div>

  <div id="result" class="hidden center">
    <h2>Kuis Selesai!</h2>
    <h1 id="score"></h1>
    <p id="detail"></p>
    <h3 id="grade"></h3>
    <button class="next" onclick="restart()">Ulangi Kuis</button>
  </div>
</div>

<script>
const questions = [
{
 category:"Makharijul Huruf",
 ayat:"ء ه ع ح غ خ",
 question:"Huruf-huruf tersebut keluar dari makhraj mana?",
 options:["Al-Lisan (lidah)","Al-Halq (tenggorokan)","Asy-Syafatain (dua bibir)","Al-Khaisyum (rongga hidung)"],
 answer:1,
 explanation:"Keenam huruf tersebut merupakan huruf halqiyah yang keluar dari tenggorokan."
},
{
 category:"Shifatul Huruf",
 ayat:"فَحَثَّهُ شَخْصٌ سَكَتْ",
 question:"Apa nama sifat huruf yang dikumpulkan dalam kalimat tersebut?",
 options:["Jahr","Rakhawah","Hams","Syiddah"],
 answer:2,
 explanation:"فَحَثَّهُ شَخْصٌ سَكَتْ merupakan rangkaian huruf-huruf Hams."
},
{
 category:"Nun Mati dan Tanwin",
 ayat:"مِنْ بَعْدِ",
 question:"Apa hukum nun mati bertemu huruf ب?",
 options:["Iqlab","Ikhfa Haqiqi","Izhar Halqi","Idgham Bighunnah"],
 answer:0,
 explanation:"Nun mati bertemu ب dibaca Iqlab, dengan mengubah bunyi nun menjadi mim samar disertai ghunnah."
},
{
 category:"Ghunnah",
 ayat:"إِنَّ",
 question:"Berapa panjang bacaan ghunnah pada nun bertasydid?",
 options:["1 harakat","2 harakat","4 harakat","6 harakat"],
 answer:1,
 explanation:"Nun dan mim bertasydid dibaca ghunnah selama 2 harakat."
},
{
 category:"Mim Mati",
 ayat:"تَرْمِيهِمْ بِحِجَارَةٍ",
 question:"Apa hukum mim mati bertemu huruf ب?",
 options:["Idgham Mimi","Izhar Syafawi","Ikhfa Syafawi","Iqlab"],
 answer:2,
 explanation:"Mim mati bertemu ب merupakan Ikhfa Syafawi."
},
{
 category:"Jenis-jenis Idgham",
 ayat:"قَدْ دَخَلُوا",
 question:"Pertemuan دْ dengan د pada contoh tersebut termasuk jenis idgham apa?",
 options:["Idgham Mutajanisain","Idgham Mutamatsilain","Idgham Mutaqaribain","Idgham Bighunnah"],
 answer:1,
 explanation:"Dua huruf yang sama makhraj dan sifatnya bertemu, yaitu دْ dan د, disebut Mutamatsilain."
},
{
 category:"Jenis-jenis Idgham",
 question:"Idgham Mutajanisain terjadi ketika dua huruf memiliki ...",
 options:["Makhraj dan sifat yang sama","Makhraj berdekatan","Makhraj sama tetapi sifat berbeda","Sifat yang sama tetapi makhraj berbeda"],
 answer:2,
 explanation:"Mutajanisain adalah pertemuan dua huruf yang sama makhrajnya tetapi berbeda sifatnya."
},
{
 category:"Waqaf dan Ibtida",
 ayat:"م",
 question:"Apa fungsi tanda waqaf م dalam mushaf Al-Qur'an?",
 options:["Boleh berhenti atau lanjut","Harus berhenti","Dilarang berhenti","Lebih utama melanjutkan"],
 answer:1,
 explanation:"Tanda م menunjukkan waqaf lazim, yaitu pembaca harus berhenti."
},
{
 category:"Lafzhul Jalalah",
 ayat:"بِسْمِ اللَّهِ",
 question:"Bagaimana cara membaca lafzhul jalalah pada contoh tersebut?",
 options:["Tafkhim","Tarqiq","Ikhfa","Qalqalah"],
 answer:1,
 explanation:"Lafzhul jalalah didahului kasrah sehingga dibaca tipis atau tarqiq."
},
{
 category:"Ra Tafkhim dan Tarqiq",
 ayat:"رَحْمَةٌ",
 question:"Bagaimana hukum membaca huruf ر pada awal kata tersebut?",
 options:["Tarqiq","Tafkhim","Ikhfa","Izhar"],
 answer:1,
 explanation:"Ra berharakat fathah dibaca tebal atau tafkhim."
},
{
 category:"Al Ta'rif",
 ayat:"الْقَمَرُ",
 question:"Apa hukum bacaan Alif Lam pada kata tersebut?",
 options:["Alif Lam Syamsiyah","Alif Lam Qamariyah","Idgham Mimi","Ikhfa Haqiqi"],
 answer:1,
 explanation:"Lam pada الْقَمَرُ dibaca jelas karena bertemu huruf ق."
},
{
 category:"Mad Ashli",
 ayat:"قَالَ",
 question:"Berapa panjang bacaan mad ashli atau mad thabi'i?",
 options:["1 harakat","2 harakat","4 harakat","6 harakat"],
 answer:1,
 explanation:"Mad ashli dibaca sepanjang 2 harakat."
},
{
 category:"Mad Far'i Hamzah",
 ayat:"جَاءَ",
 question:"Apa hukum mad pada kata tersebut?",
 options:["Mad Jaiz Munfashil","Mad Wajib Muttashil","Mad Badal","Mad Thabi'i"],
 answer:1,
 explanation:"Huruf mad bertemu hamzah dalam satu kata, sehingga termasuk Mad Wajib Muttashil."
},
{
 category:"Mad Far'i Hamzah",
 ayat:"يَا أَيُّهَا",
 question:"Apa hukum mad pada يَا jika dibaca bersambung dengan kata berikutnya?",
 options:["Mad Wajib Muttashil","Mad Badal","Mad Jaiz Munfashil","Mad Thabi'i"],
 answer:2,
 explanation:"Huruf mad berada di akhir kata dan bertemu hamzah pada awal kata berikutnya."
},
{
 category:"Mad Far'i Sukun dan Tasydid",
 ayat:"الضَّالِّينَ",
 question:"Apa hukum mad pada bagian ضَآلِّينَ?",
 options:["Mad Lazim Kalimi Mutsaqqal","Mad Aridh Lissukun","Mad Lin","Mad Wajib Muttashil"],
 answer:0,
 explanation:"Mad bertemu huruf bertasydid dalam satu kata, sehingga termasuk Mad Lazim Kalimi Mutsaqqal."
},
{
 category:"Mad Far'i Sukun dan Tasydid",
 ayat:"الْعَالَمِينَ",
 question:"Jika berhenti pada akhir kata tersebut, hukum madnya adalah ...",
 options:["Mad Lin","Mad Aridh Lissukun","Mad Badal","Mad Wajib Muttashil"],
 answer:1,
 explanation:"Ketika waqaf, huruf terakhir menjadi sukun setelah huruf mad, sehingga termasuk Mad Aridh Lissukun."
},
{
 category:"Ayat-ayat Gharibah",
 question:"Apa yang dimaksud dengan bacaan Saktah?",
 options:["Berhenti membaca dengan mengambil napas","Berhenti sejenak tanpa mengambil napas","Memanjangkan suara enam harakat","Mengubah bunyi huruf menjadi mim"],
 answer:1,
 explanation:"Saktah adalah berhenti sejenak tanpa mengambil napas, kemudian melanjutkan bacaan."
},
{
 category:"Ayat-ayat Gharibah",
 question:"Apa yang dimaksud dengan Isymam dalam bacaan Al-Qur'an?",
 options:["Memiringkan bunyi fathah ke kasrah","Membaca dengan dengung","Membulatkan kedua bibir sebagai isyarat dhammah tanpa suara","Mengganti hamzah menjadi huruf mad"],
 answer:2,
 explanation:"Isymam merupakan isyarat dengan membulatkan kedua bibir untuk menunjukkan harakat dhammah tanpa menghasilkan suara dhammah."
},
{
 category:"Matan Jazari",
 question:"Siapakah penyusun Matan Al-Muqaddimah Al-Jazariyyah?",
 options:["Imam Asy-Syathibi","Imam Ibnul Jazari","Imam Al-Khalil","Imam Sibawaih"],
 answer:1,
 explanation:"Matan Al-Muqaddimah Al-Jazariyyah disusun oleh Imam Muhammad bin Muhammad Ibnul Jazari."
},
{
 category:"Matan Jazari dan Shifatul Huruf",
 ayat:"قُطْبُ جَدٍّ",
 question:"Huruf-huruf dalam rangkaian tersebut merupakan huruf sifat apa?",
 options:["Hams","Lin","Qalqalah","Istithalah"],
 answer:2,
 explanation:"ق ط ب ج د adalah lima huruf Qalqalah."
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

  document.getElementById("category").textContent = q.category;
  document.getElementById("ayat").textContent = q.ayat || "";
  document.getElementById("question").textContent = q.question;

  const feedback = document.getElementById("feedback");
  feedback.classList.add("hidden");
  feedback.textContent = "";

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
    if (i === q.answer) btn.classList.add("correct");
  });

  const feedback = document.getElementById("feedback");
  feedback.classList.remove("hidden");

  if (index === q.answer) {
    score++;
    feedback.style.background = "#dcfce7";
    feedback.style.color = "#166534";
    feedback.textContent = "Benar! " + q.explanation;
  } else {
    selectedButton.classList.add("wrong");
    feedback.style.background = "#fee2e2";
    feedback.style.color = "#991b1b";
    feedback.textContent = "Belum tepat. " + q.explanation;
  }

  document.getElementById("bar").style.width =
    `${((current + 1) / questions.length) * 100}%`;

  const next = document.getElementById("next");
  next.classList.remove("hidden");
  next.textContent =
    current === questions.length - 1 ? "Lihat Hasil" : "Soal Berikutnya";
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

  const percentage = score * 5;

  document.getElementById("score").textContent =
    `${score} / 20`;

  document.getElementById("detail").textContent =
    `Benar: ${score} | Salah: ${20 - score} | Nilai: ${percentage}`;

  let grade;
  if (score >= 18) grade = "Luar biasa! Penguasaan materi antum kuat.";
  else if (score >= 15) grade = "Bagus! Tinggal perkuat beberapa materi.";
  else if (score >= 10) grade = "Lumayan, masih ada materi yang perlu diulang.";
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
