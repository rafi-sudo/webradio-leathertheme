---
layout: default
title: Morning Vibes
---


```
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
```

- - -

 [](https://wa.me/6282298765432?text=Halo%20Kakak%20Penyiar)
