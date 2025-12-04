<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>پورتال آموزشی آکادمی راتب</title>
  <style>
    body {
      font-family: 'Tahoma', sans-serif;
      background: #f5f5f5;
      color: #000;
      margin: 0;
      padding: 0;
      direction: rtl;
    }
    header {
      background: #556b2f;
      color: #fff;
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 15px;
    }
    header img { height: 50px; }
    header h1 { margin: 0; font-size: 24px; }
    main { max-width: 900px; margin: 20px auto; padding: 10px; }
    .button {
      background: #556b2f;
      color: #fff;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      margin-top: 10px;
      border-radius: 5px;
    }
    .button:hover { background: #6b8e23; }
    .question { margin-bottom: 20px; background: #fff; padding: 15px; border-radius: 5px; box-shadow: 0 0 5px rgba(0,0,0,0.1);}
    .option { margin: 5px 0; }
    #result { margin-top: 20px; font-weight: bold; font-size: 18px; }
    #wrong-answers { margin-top: 10px; color: red; }
    #timer { font-weight: bold; color: #556b2f; }
  </style>
</head>
<body>

  <header>
    <img src="assets/logo.png" alt="لوگوی آکادمی راتب">
    <h1>پورتال آموزشی آکادمی راتب</h1>
  </header>

  <main>
    <!-- ثبت نام دانش آموز -->
    <div id="signup-section">
      <h2>ثبت نام دانش آموز</h2>
      <input type="text" id="student-name" placeholder="نام خود را وارد کنید">
      <button class="button" onclick="registerStudent()">ثبت نام</button>
    </div>

    <!-- بخش آزمون -->
    <div id="test-section" style="display:none;">
      <h2 id="welcome"></h2>
      <p>زمان باقی مانده: <span id="timer">05:00</span></p>
      <div id="questions-container"></div>
      <button class="button" onclick="submitTest()">ثبت پاسخ‌ها</button>
      <button class="button" onclick="resetTest()">شروع مجدد</button>
      <div id="result"></div>
      <div id="wrong-answers"></div>
    </div>
  </main>

  <script>
    const questions = [
      {
        question: "کدام اندام خون را در سراسر بدن پمپاژ می‌کند؟",
        options: ["کبد", "قلب", "ریه", "کلیه"],
        answer: "قلب"
      },
      {
        question: "کدام ویتامین هنگام تابش نور خورشید به پوست تولید می‌شود؟",
        options: ["ویتامین A", "ویتامین B", "ویتامین C", "ویتامین D"],
        answer: "ویتامین D"
      },
      {
        question: "بزرگترین اندام بدن انسان کدام است؟",
        options: ["پوست", "کبد", "قلب", "مغز"],
        answer: "پوست"
      },
      {
        question: "کدام سلول مسئول حمل اکسیژن در خون است؟",
        options: ["گلبول سفید", "گلبول قرمز", "پلاکت", "سلول‌های عصبی"],
        answer: "گلبول قرمز"
      },
      {
        question: "عضوی که مسئول فیلتر کردن مواد زائد خون است، کدام است؟",
        options: ["کبد", "ریه", "کلیه", "قلب"],
        answer: "کلیه"
      }
    ];

    let timerInterval;

    function registerStudent() {
      const name = document.getElementById('student-name').value;
      if(name) {
        document.getElementById('signup-section').style.display = 'none';
        document.getElementById('test-section').style.display = 'block';
        document.getElementById('welcome').innerText = `سلام ${name}! آزمون خود را شروع کنید.`;
        loadQuestions();
        startTimer(300); // 5 دقیقه
      } else {
        alert('لطفاً نام خود را وارد کنید.');
      }
    }

    function loadQuestions() {
      const container = document.getElementById('questions-container');
      container.innerHTML = "";
      questions.forEach((q, index) => {
        const div = document.createElement('div');
        div.classList.add('question');
        div.innerHTML = `<p>${index + 1}. ${q.question}</p>` + q.options.map(opt => `
          <div class="option">
            <input type="radio" name="q${index}" value="${opt}" id="q${index}-${opt}">
            <label for="q${index}-${opt}">${opt}</label>
          </div>`).join("");
        container.appendChild(div);
      });
    }

    function submitTest() {
      clearInterval(timerInterval);
      let score = 0;
      let wrong = [];
      questions.forEach((q, index) => {
        const selected = document.querySelector(`input[name="q${index}"]:checked`);
        if(selected && selected.value === q.answer) {
          score++;
        } else {
          wrong.push(`${index + 1}. ${q.question} (پاسخ صحیح: ${q.answer})`);
        }
      });
      document.getElementById('result').innerText = `امتیاز شما: ${score} / ${questions.length} (${Math.round(score/questions.length*100)}%)`;
      document.getElementById('wrong-answers').innerHTML = wrong.length ? 
        "پاسخ‌های اشتباه:<br>" + wrong.join("<br>") : "";
    }

    function resetTest() {
      document.getElementById('signup-section').style.display = 'block';
      document.getElementById('test-section').style.display = 'none';
      document.getElementById('student-name').value = '';
      document.getElementById('result').innerText = '';
      document.getElementById('wrong-answers').innerText = '';
      clearInterval(timerInterval);
      document.getElementById('timer').innerText = "05:00";
    }

    function startTimer(seconds) {
      let remaining = seconds;
      const timerDisplay = document.getElementById('timer');
      timerInterval = setInterval(() => {
        const mins = Math.floor(remaining / 60).toString().padStart(2, '0');
        const secs = (remaining % 60).toString().padStart(2, '0');
        timerDisplay.innerText = `${mins}:${secs}`;
        remaining--;
        if(remaining < 0) {
          clearInterval(timerInterval);
          submitTest();
          alert('زمان آزمون به پایان رسید!');
        }
      }, 1000);
    }
  </script>

</body>
</html>
