# 피카츄기르기
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>피카츄 다마고치</title>
    <style>
        body { background-color: #FFDE00; text-align: center; font-family: 'Arial', sans-serif; }
        #game-box { 
            width: 350px; background: white; margin: 50px auto; 
            padding: 20px; border-radius: 50px; border: 8px solid #3B4CCA;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        #pikachu { font-size: 80px; height: 120px; transition: 0.3s; margin: 20px 0; }
        .stat-bar { text-align: left; margin: 10px 0; font-weight: bold; }
        .buttons { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 20px; }
        button { 
            padding: 10px; border-radius: 20px; border: none; 
            background: #CC0000; color: white; font-weight: bold; cursor: pointer;
        }
        button:hover { background: #3B4CCA; }
        #log { margin-top: 15px; font-size: 14px; color: #555; height: 20px; }
        /* 몬스터볼 효과 */
        .shake { animation: shake 0.5s; }
        @keyframes shake {
            0% { transform: translate(1px, 1px) rotate(0deg); }
            20% { transform: translate(-3px, 0px) rotate(-10deg); }
            40% { transform: translate(3px, 2px) rotate(10deg); }
            100% { transform: translate(1px, -1px) rotate(0deg); }
        }
    </style>
</head>
<body>

<div id="game-box">
    <h2 style="color: #3B4CCA;">Pika-Pika! ⚡️</h2>
    <div class="stat-bar">🍎 배고픔: <span id="hunger">100</span></div>
    <div class="stat-bar">💖 행복도: <span id="happiness">100</span></div>
    <div class="stat-bar">💢 순종도: <span id="obedience">20</span></div>

    <div id="pikachu">⚡(o'ω'o)⚡</div>
    <div id="log">피카츄가 당신을 쳐다봅니다.</div>

    <div class="buttons">
        <button onclick="action('feed')">🍎 맛있는 열매</button>
        <button onclick="action('play')">🎡 같이 놀기</button>
        <button onclick="action('sleep')">💤 낮잠 자기</button>
        <button style="background: black;" onclick="action('punish')">🔴 몬스터볼로 혼내기</button>
    </div>
</div>

<script>
    let hunger = 100, happiness = 100, obedience = 20;
    const pika = document.getElementById('pikachu');
    const log = document.getElementById('log');

    function update() {
        document.getElementById('hunger').innerText = hunger;
        document.getElementById('happiness').innerText = happiness;
        document.getElementById('obedience').innerText = obedience;

        if (obedience < 30) pika.innerText = "⚡( `皿´ )⚡"; // 반항적
        else if (happiness < 30) pika.innerText = "⚡( ; ω ; )⚡"; // 슬픔
        else pika.innerText = "⚡(o'ω'o)⚡"; // 평온
    }

    function action(type) {
        pika.classList.remove('shake');
        void pika.offsetWidth; // 애니메이션 리셋용

        if (type === 'feed') {
            hunger = Math.min(hunger + 20, 100);
            log.innerText = "피카츄가 자뭉열매를 먹었습니다!";
        } else if (type === 'play') {
            happiness = Math.min(happiness + 20, 100);
            hunger -= 10;
            log.innerText = "피카츄와 즐겁게 놀았습니다!";
        } else if (type === 'sleep') {
            happiness += 5;
            log.innerText = "피카츄가 푹 자고 일어났습니다.";
        } else if (type === 'punish') {
            pika.classList.add('shake');
            obedience = Math.min(obedience + 30, 100);
            happiness = Math.max(happiness - 40, 0);
            pika.innerText = "🔴"; // 몬스터볼에 갇힘!
            log.innerText = "몬스터볼을 던져 따끔하게 혼냈습니다!";
            setTimeout(update, 1000); 
            return;
        }
        update();
    }

    // 시간이 흐르면 수치 감소
    setInterval(() => {
        hunger = Math.max(hunger - 2, 0);
        happiness = Math.max(happiness - 1, 0);
        update();
        if(hunger <= 0 && happiness <= 0) {
            alert("피카츄가 가출했습니다!");
            location.reload();
        }
    }, 4000);
</script>
</body>
</html>
