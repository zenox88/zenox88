<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cozy Slot Machine</title>
  <style>
    body {
      background: #f9f6f2;
      font-family: 'Comic Sans MS', cursive, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    h1 {
      color: #d2691e;
      margin-bottom: 10px;
    }
    .slot-machine {
      background: #fff8e1;
      border-radius: 20px;
      box-shadow: 0 4px 24px rgba(0,0,0,0.1);
      padding: 30px 40px;
      display: flex;
      flex-direction: column;
      align-items: center;
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
    }
    .spin-btn {
      background: #ffb74d;
      color: #fff;
      border: none;
      border-radius: 8px;
      padding: 12px 32px;
      font-size: 1.2rem;
      cursor: pointer;
      transition: background 0.2s;
      margin-bottom: 10px;
    }
    .spin-btn:hover {
      background: #ffa726;
    }
    .message {
      font-size: 1.1rem;
      color: #388e3c;
      margin-top: 10px;
      min-height: 24px;
    }
  </style>
</head>
<body>
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

    function randomSymbol() {
      return symbols[Math.floor(Math.random() * symbols.length)];
    }

    function spinReels() {
      spinSound.currentTime = 0;
      spinSound.play();
      let results = [];
      for (let i = 0; i < reels.length; i++) {
        results[i] = randomSymbol();
        reels[i].textContent = results[i];
      }
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
      }, 700);
    }

    spinBtn.addEventListener('click', spinReels);
  </script>
</body>
</html> 
