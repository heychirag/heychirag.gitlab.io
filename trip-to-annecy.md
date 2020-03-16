---
layout: page
title: Trip to Annecy
category: photos
---

# Trip to Annecy

{% google_photos https://photos.app.goo.gl/bhWukds8QVodFU246 none %}

<script src="https://cdn.jsdelivr.net/npm/publicalbum@latest/embed-ui.min.js" async></script>

<div class="pa-gallery-player-widget" style="width:100%; height:480px; display:none;"
  data-link="https://photos.app.goo.gl/bhWukds8QVodFU246"
  data-title="Trip to Annecy"
  data-description="Trip to Annecy"
  data-delay="5"
  id="MyAlbum1">
</div>

<script>
  let MyAlbum1 = document.getElementById('MyAlbum1');
  let imageWidth = '0'; 
  for(var i in googlePhotos.urls) {
    let picture = document.createElement('object');
    picture.setAttribute("data", googlePhotos.urls[i] + '=w' + imageWidth);
    MyAlbum1.appendChild(picture);
  }
</script>
