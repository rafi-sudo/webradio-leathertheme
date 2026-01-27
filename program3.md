---
layout: default
title: Melody Memory
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

#player-melody.playing .speaker {
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

#playBtn-melody {
  padding: 8px 14px;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  background: #000;
  color: #fff;
  cursor: pointer;
}

#playBtn-melody:disabled {
  background: #333;
  cursor: not-allowed;
}
</style>

<div class="radio-controller" id="player-melody">
  <div class="speaker">
    <img src="assets\image\melodymemory.png" alt="Melody Memory">
  </div>

  <div class="info">
    <div class="status" id="status-melody">
      Checking schedule...
    </div>
    <div class="desc">
      Melody Memory<br>
      Kembali ke kenangan manis lewat deretan lagu Pop Lawas Dewasa terbaik.<br>
      Senin – Sabtu • 19:00 – 21:00 WIB
    </div>
  </div>

  <button id="playBtn-melody" disabled>PLAY</button>

  <audio id="audio-melody"
    src="https://stream.ganisrafi.my.id/backup"
    preload="none"></audio>

</div>

<script>
(function () {
  const player = document.getElementById("player-melody");
  const audio = document.getElementById("audio-melody");
  const playBtn = document.getElementById("playBtn-melody");
  const statusText = document.getElementById("status-melody");

  function getWIBTime() {
    const now = new Date();
    const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
    return new Date(utc + (7 * 60 * 60000));
  }

  function checkSchedule() {
    const wib = getWIBTime();
    const hour = wib.getHours();
    const day = wib.getDay(); // 0=Minggu

    const isWeekday = (day >= 1 && day <= 6); // Senin–Sabtu
    const isLive = isWeekday && (hour >= 19 && hour < 21);

    if (isLive) {
      statusText.textContent = "Playing – Melody Memory";
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

{% endraw %}
