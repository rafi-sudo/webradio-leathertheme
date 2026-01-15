---
layout: default
title: Karaoke Asoooy
---
{% raw %}
<style>
.radio-controller {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px;
  border-radius: 14px;
  background: linear-gradient(135deg, #ffd200, #ff4d6d);
  box-shadow: 0 8px 22px rgba(0,0,0,.18);
  width: 300px;
}

.speaker {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  overflow: hidden;
  flex-shrink: 0;
}

.speaker img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

#player-karaoke.playing .speaker {
  animation: vibrate 0.15s infinite;
}

@keyframes vibrate {
  0% { transform: translate(0); }
  25% { transform: translate(1px,-1px); }
  50% { transform: translate(-1px,1px); }
  75% { transform: translate(1px,1px); }
  100% { transform: translate(0); }
}

.info { flex: 1; }

.status {
  font-weight: bold;
  font-size: 14px;
  color: #000;
}

.desc {
  font-size: 12px;
  color: #222;
  line-height: 1.3;
}

#playBtn-karaoke {
  padding: 8px 14px;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  background: #000;
  color: #fff;
  cursor: pointer;
}

#playBtn-karaoke:disabled {
  background: #333;
  cursor: not-allowed;
}
</style>

<div class="radio-controller" id="player-karaoke">
  <div class="speaker">
    <img src="assets\image\lev.png" alt="Karaoke AsooOy">
  </div>

  <div class="info">
    <div class="status" id="status-karaoke">
      Checking schedule...
    </div>
    <div class="desc">
      Karaoke AsooOy<br>
      Karaokean Mitraa!! bersama RCA 102.1 FM Karaoke AsooOy!<br>
      Sabtu 20:00–22:00 WIB<br>
      Minggu 10:00–11:00 & 14:00–17:00 WIB
    </div>
  </div>

  <button id="playBtn-karaoke" disabled>PLAY</button>

  <audio id="audio-karaoke"
    src="https://stream01.ganisrafi.my.id/rcabackup"
    preload="none"></audio>
</div>
<!-- File: karaoke-widget-highlight.html -->
<div id="karaoke-widget" class="p-4 border rounded bg-gray-50 max-w-xl mx-auto">
  <h2 class="text-2xl font-bold mb-3 text-center">🎤 Karaoke AsoOy!</h2>

  <!-- Thumbnail / YouTube iframe muted -->
  <div class="mb-4 text-center">
    <iframe width="360" height="203"
      src="https://www.youtube.com/embed/VIDEO_ID?rel=0&autoplay=0&mute=1"
      title="Karaoke Video" frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen>
    </iframe>
  </div>

  <!-- Lirik -->
  <div id="lyrics" class="mt-2 p-3 border rounded bg-white min-h-[120px]">
    <div data-time="0">[Intro] Lorem ipsum dolor sit amet...</div>
    <div data-time="5">[Verse 1] Consectetur adipiscing elit...</div>
    <div data-time="10">[Chorus] Karaoke time! Sing along...</div>
    <div data-time="15">[Verse 2] Sed do eiusmod tempor...</div>
    <div data-time="20">[Chorus] Karaoke time again!</div>
  </div>

  <!-- Link untuk penyiar -->
  <div class="mt-3 text-center text-blue-600 underline">
    <a href="https://www.youtube.com/watch?v=VIDEO_ID" target="_blank">Link YouTube (Penyiar)</a>
  </div>
</div>

<script>
(function () {
  const player = document.getElementById("player-karaoke");
  const audio = document.getElementById("audio-karaoke");
  const playBtn = document.getElementById("playBtn-karaoke");
  const statusText = document.getElementById("status-karaoke");

  function getWIBTime() {
    const now = new Date();
    const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
    return new Date(utc + (7 * 60 * 60000));
  }

  function checkSchedule() {
    const wib = getWIBTime();
    const hour = wib.getHours();
    const day = wib.getDay(); // 0=Minggu, 6=Sabtu

    let isLive = false;

    // Sabtu 20–22
    if (day === 6 && hour >= 20 && hour < 22) {
      isLive = true;
    }

    // Minggu 10–11
    if (day === 0 && hour >= 10 && hour < 11) {
      isLive = true;
    }

    // Minggu 14–17
    if (day === 0 && hour >= 14 && hour < 17) {
      isLive = true;
    }

    if (isLive) {
      statusText.textContent = "Playing – Karaoke AsooOy";
      playBtn.disabled = false;
    } else {
      statusText.textContent = "Not Playing – outside of schedule";
      playBtn.disabled = true;
      audio.pause();
      player.classList.remove("playing");
      playBtn.textContent = "PLAY";
    }
  }

  playBtn.addEventListener("click", function () {
    if (audio.paused) {
      audio.play();
      player.classList.add("playing");
      playBtn.textContent = "STOP";
    } else {
      audio.pause();
      player.classList.remove("playing");
      playBtn.textContent = "PLAY";
    }
  });

  checkSchedule();
  setInterval(checkSchedule, 30000);
})();
</script>
<script>
const lyricsDiv = document.getElementById('lyrics');
const lines = Array.from(lyricsDiv.children);

// waktu tiap baris dalam detik
let currentTime = 0;
const interval = 1000; // cek tiap 1 detik

// highlight otomatis
function highlightLine() {
    lines.forEach(line => line.style.backgroundColor = ''); // reset
    for(let i = lines.length-1; i>=0; i--){
        const t = parseInt(lines[i].getAttribute('data-time'));
        if(currentTime >= t){
            lines[i].style.backgroundColor = '#fffa91'; // highlight
            break;
        }
    }
}

setInterval(() => {
    highlightLine();
    currentTime++;
}, interval);
</script>
{% endraw %}
