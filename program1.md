---
layout: default
title: Wayang Golek
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

#player-wayang.playing .speaker {
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

#playBtn-wayang {
  padding: 8px 14px;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  background: #000;
  color: #fff;
  cursor: pointer;
}

#playBtn-wayang:disabled {
  background: #333;
  cursor: not-allowed;
}
</style>

<div class="radio-controller" id="player-wayang">
  <div class="speaker">
    <img src="assets\image\wayanggolek.jpg" alt="Wayang Golek">
  </div>

  <div class="info">
    <div class="status" id="status-wayang">
      Checking schedule...
    </div>
    <div class="desc">
      Wayang Golek<br>
      Midangkeun Wayang Golek salami 3 jam dina wengi Salasa sareng jumat<br>
      Selasa & Jumat • 21:00 – 00:00 WIB
    </div>
  </div>

  <button id="playBtn-wayang" disabled>PLAY</button>

  <audio id="audio-wayang"
    src="https://stream.ganisrafi.my.id/rcabackup"
    preload="none"></audio>

</div>

<script>
(function () {
  const player = document.getElementById("player-wayang");
  const audio = document.getElementById("audio-wayang");
  const playBtn = document.getElementById("playBtn-wayang");
  const statusText = document.getElementById("status-wayang");

  function getWIBTime() {
    const now = new Date();
    const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
    return new Date(utc + (7 * 60 * 60000));
  }

  function checkSchedule() {
    const wib = getWIBTime();
    const hour = wib.getHours();
    const day = wib.getDay(); // 0=Minggu, 1=Senin, 2=Selasa, ..., 5=Jumat, 6=Sabtu

    let isLive = false;

    // Selasa 21–24
    if (day === 2 && hour >= 21) {
      isLive = true;
    }

    // Jumat 21–24
    if (day === 5 && hour >= 21) {
      isLive = true;
    }

    if (isLive) {
      statusText.textContent = "Playing – Wayang Golek";
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
