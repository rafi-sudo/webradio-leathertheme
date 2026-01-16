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
{% endraw %}
{% raw %}
<!-- _includes/karaoke-widget.html -->
<div class="karaoke-widget" style="
  max-width:400px;
  margin:1rem auto;
  padding:1.5rem;
  border-radius:1rem;
  box-shadow:0 4px 12px rgba(0,0,0,0.15);
  font-family:sans-serif;
">
  <h3 style="text-align:center;color:#333;margin-bottom:1rem;">🎤 Bantu penyiar temukan video karaoke-nya di Youtube. </h3>
  <form id="karaokeForm" style="display:flex;flex-direction:column;gap:0.75rem;">
    <input type="text" id="youtubeUrl" placeholder="Dengan tempel URL YouTube Karaoke" required
           style="padding:0.5rem;border-radius:0.5rem;border:1px solid #ccc;font-size:0.95rem;">
    
    <div id="captchaBox" style="padding:0.5rem;background:#fff;border-radius:0.5rem;text-align:center;color:#666;">
      Isi captcha di sini (beta)
    </div>
    <input type="number" id="captchaAnswer" placeholder="isi angka bebas." required
           style="padding:0.5rem;border-radius:0.5rem;border:1px solid #ccc;font-size:0.95rem;">
    <button type="submit" style="
      padding:0.6rem;
      background:#ff6f61;
      color:#fff;
      font-weight:bold;
      border:none;
      border-radius:0.5rem;
      cursor:pointer;
      transition:0.2s;
    ">
      Kirim
    </button>
<p>Sulit kirim link? coba refresh kembali browser.
  </form>

  <div id="status" style="margin-top:1rem;text-align:center;font-weight:bold;color:#333;"></div>
</div>
<!-- Script fetch original tetap sama -->
<script>
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycby2bjG6wnmxnndEgqWdhnh_sWKWQq4qA8JIaBNQ0u7orGP9XiI_kDoWo_6nV_wiA__XbA/exec";

document.getElementById("karaokeForm").addEventListener("submit", function(e) {
  e.preventDefault();

  const url = document.getElementById("youtubeUrl").value.trim();

  const fullUrl =
    SCRIPT_URL +
    "?youtube_url=" + encodeURIComponent(url) +
    "&ua=" + encodeURIComponent(navigator.userAgent);

  fetch(fullUrl, { mode: "no-cors" })
    .then(() => {
      document.getElementById("status").innerText =
        "✅ Request karaoke terkirim!, saran : dengarkan melalui FM konvensional!";
    })
    .catch(() => {
      document.getElementById("status").innerText =
        "❌ Gagal kirim.";
    });
    fetch(GAS_URL, {
  method: "POST",
  mode: "no-cors",
  body: JSON.stringify({
    secret: "karaoke-cempaka-2026",
    url: ytUrl
  })
});
});
</script>
{% endraw %}
