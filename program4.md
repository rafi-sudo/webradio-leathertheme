---
layout: default
title: Lorem Ipsum News
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
    src="https://stream2.ganisrafi.my.id/backup"
    preload="none"></audio>
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
{$ endraw %}
