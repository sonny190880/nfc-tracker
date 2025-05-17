<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Трекер NFC → Google Form</title>
  <style>
    body { font-family: sans-serif; text-align: center; margin-top: 50px; }
  </style>
</head>
<body>
  <h3 id="status">Отправляю координаты…</h3>

  <form id="sendForm"
        action="https://docs.google.com/forms/d/e/1FAIpQLSf4617SugNzH5BI_xeVOgc-c-9wQlsDUDU3XgVk--zPVk8d_Q/formResponse"
        method="POST"
        target="hidden_iframe" style="display:none;">
    <input name="entry.1660310348" id="lat">
    <input name="entry.101014652" id="lon">
  </form>
  <iframe name="hidden_iframe" style="display:none;"></iframe>

  <script>
    const form      = document.getElementById('sendForm');
    const inputLat  = document.getElementById('lat');
    const inputLon  = document.getElementById('lon');
    const status    = document.getElementById('status');

    function show(msg){ status.textContent = msg; }

    if (!navigator.geolocation) {
      show('Геолокация не поддерживается.');
    } else {
      navigator.geolocation.getCurrentPosition(
        pos => {
          inputLat.value = pos.coords.latitude;
          inputLon.value = pos.coords.longitude;
          form.submit();
          show('Координаты отправлены!');
        },
        err => { show('Ошибка: ' + err.message); },
        { enableHighAccuracy: true, timeout: 10000 }
      );
    }
  </script>
</body>
</html>
