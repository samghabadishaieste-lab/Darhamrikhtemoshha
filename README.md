<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>بازی کلمات درهم ریخته - کلاس خانم صفی</title>
    <style>
        body { background: linear-gradient(135deg, #FFDEE9, #B5FFFC); font-family: Tahoma, sans-serif; text-align: center; padding: 20px; }
        .card { background: white; padding: 20px; border-radius: 20px; box-shadow: 0 10px 20px rgba(0,0,0,0.1); display: inline-block; width: 80%; max-width: 500px; }
        .letter-box { font-size: 30px; font-weight: bold; margin: 10px; padding: 10px; border: 2px solid #ff6f61; border-radius: 10px; display: inline-block; background: #fff5e6; }
        button { background: #4CAF50; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-size: 16px; margin: 5px; }
        .hidden { display: none; }
    </style>
</head>
<body>

    <div class="card" id="start-screen">
        <h1>آموزگار: شایسته صفی</h1>
        <p>سلام! اسم قشنگت چیه؟</p>
        <input type="text" id="studentName" placeholder="نام دانش‌آموز">
        <button onclick="startGame()">شروع بازی 🎮</button>
    </div>

    <div id="game-screen" class="hidden">
        <div class="card">
            <h2 id="question-text">حروف رو مرتب کن!</h2>
            <div id="letters-container"></div>
            <br>
            <div id="options"></div>
        </div>
    </div>

    <audio id="bg-music" loop>
        <source src="https://dl.aleftab.ir/download/Music/Madrese%20Mooshha.mp3" type="audio/mpeg">
    </audio>

    <script>
        const words = ["فردوسی", "سنجاقک", "شاهنامه", "عظیم", "موجودات", "کتابخانه", "اصفهان", "شغال", "میکروب", "کثیف"];
        let currentIdx = 0, score = 0, studentName = "";

        function startGame() {
            studentName = document.getElementById('studentName').value;
            if(!studentName) return alert("لطفاً نامت را وارد کن!");
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('game-screen').classList.remove('hidden');
            document.getElementById('bg-music').play();
            showQuestion();
        }

        function shuffle(word) {
            let arr = word.split('');
            return arr.sort(() => Math.random() - 0.5).join(' ');
        }

        function showQuestion() {
            if(currentIdx >= words.length) return endGame();
            
            let word = words[currentIdx];
            document.getElementById('letters-container').innerHTML = `<div class="letter-box">${shuffle(word)}</div>`;
            
            let optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = "";
            
            let wrongWord = words[(currentIdx + 1) % words.length];
            let choices = [word, wrongWord].sort(() => Math.random() - 0.5);
            
            choices.forEach(c => {
                let btn = document.createElement('button');
                btn.innerText = c;
                btn.onclick = () => { if(c === word) score++; currentIdx++; showQuestion(); };
                optionsDiv.appendChild(btn);
            });
        }

        function endGame() {
            document.body.innerHTML = `
                <div class="card">
                    <h1>🎉 آفرین ${studentName} عزیز! 🎉</h1>
                    <h2>امتیاز نهایی تو: ${score} از ${words.length}</h2>
                    <p>خانم شایسته صفی بهت افتخار می‌کنه! 🧸✨</p>
                    <button onclick="location.reload()">دوباره بازی کن</button>
                </div>`;
            // افکت تشویق می‌تواند اینجا اضافه شود
        }
    </script>
</body>
</html>
