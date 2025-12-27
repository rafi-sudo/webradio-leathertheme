---
layout: default
title: Morning Vibes
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

#player-info-sehat.playing .speaker {
  animation: vibrate 0.15s infinite;
}

@keyframes vibrate {
  0% { transform: translate(0); }
  25% { transform: translate(1px,-1px); }
  50% { transform: translate(-1px,1px); }
  75% { transform: translate(1px,1px); }
  100% { transform: translate(0); }
}

.info {
  flex: 1;
}

.status {
  font-weight: bold;
  font-size: 14px;
  color: #000;
}

.time {
  font-size: 12px;
  color: #222;
}

#playBtn-info-sehat {
  padding: 8px 14px;
  border: none;
  border-radius: 10px;
  font-weight: bold;
  background: #000;
  color: #fff;
  cursor: pointer;
}

#playBtn-info-sehat:disabled {
  background: #333;
  cursor: not-allowed;
}
</style>

<div class="radio-controller" id="player-info-sehat">
  <div class="speaker">
    <img src="assets\image\infosehat.png" alt="Info Sehat">
  </div>

  <div class="info">
    <div class="status" id="status-info-sehat">
      Checking schedule...
    </div>
    <div class="time">
      Info Sehat • 08:00 – 09:00 WIB
    </div>
  </div>

  <button id="playBtn-info-sehat" disabled>PLAY</button>

  <audio id="audio-info-sehat"
    src="https://stream2.ganisrafi.my.id/backup"
    preload="none"></audio>
</div>
  ...

<script>
  (function () {
  const player = document.getElementById("player-info-sehat");
  const audio = document.getElementById("audio-info-sehat");
  const playBtn = document.getElementById("playBtn-info-sehat");
  const statusText = document.getElementById("status-info-sehat");

  function getWIBTime() {
    const now = new Date();
    const utc = now.getTime() + (now.getTimezoneOffset() * 60000);
    return new Date(utc + (7 * 60 * 60000));
  }

  function checkSchedule() {
    const wib = getWIBTime();
    const hour = wib.getHours();

    // LIVE hanya 08:00–08:59 WIB
    if (hour === 8) {
      statusText.textContent = "Playing – Program Info Sehat";
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
