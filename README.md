<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cozy Slot Machine</title>
  <style>
    body {
      background: linear-gradient(120deg, #f9f6f2 0%, #ffe0b2 100%);
      font-family: 'Comic Sans MS', cursive, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      overflow: hidden;
    }
    .stars {
      position: fixed;
      top: 0; left: 0; width: 100vw; height: 100vh;
      pointer-events: none;
      z-index: 0;
    }
    h1 {
      color: #d2691e;
      margin-bottom: 10px;
      z-index: 1;
      position: relative;
      text-shadow: 0 2px 8px #fff8e1;
    }
    .slot-machine {
      background: #fff8e1cc;
      border-radius: 20px;
      box-shadow: 0 4px 24px rgba(0,0,0,0.1);
      padding: 30px 40px;
      display: flex;
      flex-direction: column;
      align-items: center;
      z-index: 1;
      position: relative;
    }
    .reels {
      display: flex;
      gap: 20px;
      margin-bottom: 20px;
    }
    .reel {
      background: #ffe0b2;
      border-radius: 10px;
      width: 60px;
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2.5rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      transition: transform 0.2s;
      animation: pop 0.3s;
    }
    @keyframes pop {
      0% { transform: scale(1); }
      50% { transform: scale(1.2); }
      100% { transform: scale(1); }
    }
    .spin-btn {
      background: #ffb74d;
      color: #fff;
      border: none;
      border-radius: 8px;
      padding: 12px 32px;
      font-size: 1.2rem;
      cursor: pointer;
      transition: background 0.2s, transform 0.1s;
      margin-bottom: 10px;
      z-index: 1;
      position: relative;
    }
    .spin-btn:hover {
      background: #ffa726;
      transform: scale(1.05);
    }
    .message {
      font-size: 1.1rem;
      color: #388e3c;
      margin-top: 10px;
      min-height: 24px;
      z-index: 1;
      position: relative;
      text-shadow: 0 2px 8px #fff8e1;
    }
  </style>
</head>
<body>
  <canvas class="stars"></canvas>
  <h1>🎰 Cozy Slot Machine</h1>
  <div class="slot-machine">
    <div class="reels">
      <div class="reel" id="reel1">🍒</div>
      <div class="reel" id="reel2">🍋</div>
      <div class="reel" id="reel3">🍊</div>
    </div>
    <button class="spin-btn" id="spinBtn">Spin</button>
    <div class="message" id="message"></div>
  </div>
  <audio id="spinSound" src="https://cdn.pixabay.com/audio/2022/07/26/audio_124bfa4c7b.mp3"></audio>
  <audio id="winSound" src="https://cdn.pixabay.com/audio/2022/07/26/audio_124bfa4c7b.mp3"></audio>
  <audio id="jackpotSound" src="https://cdn.pixabay.com/audio/2022/03/15/audio_115b9b6e7b.mp3"></audio>
  <script>
    // Animated stars background
    const canvas = document.querySelector('.stars');
    const ctx = canvas.getContext('2d');
    let stars = [];
    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    function createStars() {
      stars = [];
      for (let i = 0; i < 60; i++) {
        stars.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          r: Math.random() * 1.5 + 0.5,
          d: Math.random() * 0.5 + 0.2,
          o: Math.random() * 0.5 + 0.5
        });
      }
    }
    function drawStars() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (const s of stars) {
        ctx.globalAlpha = s.o;
        ctx.beginPath();
        ctx.arc(s.x, s.y, s.r, 0, 2 * Math.PI);
        ctx.fillStyle = '#fff9c4';
        ctx.shadowColor = '#fffde7';
        ctx.shadowBlur = 8;
        ctx.fill();
        ctx.shadowBlur = 0;
      }
      ctx.globalAlpha = 1;
    }
    function animateStars() {
      for (const s of stars) {
        s.y += s.d;
        if (s.y > canvas.height) {
          s.y = 0;
          s.x = Math.random() * canvas.width;
        }
      }
      drawStars();
      requestAnimationFrame(animateStars);
    }
    window.addEventListener('resize', () => {
      resizeCanvas();
      createStars();
    });
    resizeCanvas();
    createStars();
    animateStars();

    // Slot machine logic
    const symbols = ['🍒', '🍋', '🍊', '🍉', '🍇', '⭐'];
    const reels = [
      document.getElementById('reel1'),
      document.getElementById('reel2'),
      document.getElementById('reel3')
    ];
    const spinBtn = document.getElementById('spinBtn');
    const message = document.getElementById('message');
    const spinSound = document.getElementById('spinSound');
    const winSound = document.getElementById('winSound');
    const jackpotSound = document.getElementById('jackpotSound');

    let spinning = false;

    function randomSymbol() {
      return symbols[Math.floor(Math.random() * symbols.length)];
    }

    function animateReel(reel, duration, finalSymbol, delay) {
      return new Promise(resolve => {
        let elapsed = 0;
        let interval = 50;
        function spin() {
          if (elapsed < duration) {
            reel.textContent = randomSymbol();
            reel.style.transform = 'scale(1.15)';
            setTimeout(() => { reel.style.transform = 'scale(1)'; }, 100);
            elapsed += interval;
            setTimeout(spin, interval);
          } else {
            setTimeout(() => {
              reel.textContent = finalSymbol;
              reel.style.transform = 'scale(1.2)';
              setTimeout(() => { reel.style.transform = 'scale(1)'; }, 200);
              resolve();
            }, delay);
          }
        }
        spin();
      });
    }

    async function spinReels() {
      if (spinning) return;
      spinning = true;
      message.textContent = '';
      spinSound.currentTime = 0;
      spinSound.play();
      // Decide final symbols
      let results = [randomSymbol(), randomSymbol(), randomSymbol()];
      // Animate each reel with staggered stop
      await Promise.all([
        animateReel(reels[0], 900, results[0], 0),
        animateReel(reels[1], 1200, results[1], 0),
        animateReel(reels[2], 1500, results[2], 0)
      ]);
      setTimeout(() => {
        if (results[0] === results[1] && results[1] === results[2]) {
          jackpotSound.currentTime = 0;
          jackpotSound.play();
          message.textContent = '🎉 JACKPOT! You win! 🎉';
        } else if (results[0] === results[1] || results[1] === results[2] || results[0] === results[2]) {
          winSound.currentTime = 0;
          winSound.play();
          message.textContent = 'You got two! Nice win!';
        } else {
          message.textContent = 'Try again!';
        }
        spinning = false;
      }, 400);
    }

    spinBtn.addEventListener('click', spinReels);
  </script>
</body>
</html>
