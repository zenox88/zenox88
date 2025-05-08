<!-- Cozy Slot Machine: Copy this into your HTML file -->
<div id="slot-machine" style="background:#fff8f0;padding:2em;border-radius:1em;box-shadow:0 2px 8px #e0c3a0;max-width:350px;margin:2em auto;text-align:center;font-family:'Comic Sans MS',cursive;">
  <h2 style="color:#b5838d;">🎰 Cozy Slot Machine</h2>
  <div id="slots" style="font-size:2.5em;letter-spacing:1em;">🍒 🍋 🍊</div>
  <button id="spin" style="margin-top:1em;padding:0.5em 2em;background:#ffb4a2;color:#fff;border:none;border-radius:0.5em;font-size:1.2em;cursor:pointer;">Spin!</button>
  <div id="result" style="margin-top:1em;font-size:1.1em;color:#6d6875;"></div>
  <audio id="spin-sound" src="https://cdn.pixabay.com/audio/2022/07/26/audio_124bfa4c7b.mp3"></audio>
  <audio id="win-sound" src="https://cdn.pixabay.com/audio/2022/03/15/audio_115b9b7b7b.mp3"></audio>
</div>
<script>
const symbols = ['🍒','🍋','🍊','🍉','🍇','⭐'];
const slots = document.getElementById('slots');
const result = document.getElementById('result');
const spinBtn = document.getElementById('spin');
const spinSound = document.getElementById('spin-sound');
const winSound = document.getElementById('win-sound');

spinBtn.onclick = () => {
  spinSound.currentTime = 0;
  spinSound.play();
  result.textContent = '';
  let slotVals = [];
  for(let i=0;i<3;i++){
    slotVals[i] = symbols[Math.floor(Math.random()*symbols.length)];
  }
  slots.textContent = slotVals.join(' ');
  setTimeout(()=>{
    if(slotVals[0]===slotVals[1]&&slotVals[1]===slotVals[2]){
      winSound.currentTime = 0;
      winSound.play();
      result.textContent = '✨ You win! ✨';
    } else {
      result.textContent = 'Try again!';
    }
  }, 500);
};
</script>
