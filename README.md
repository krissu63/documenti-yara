<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scatta la Foto</title>
</head>
<body>
  <h1>Scatta la tua foto!</h1>

  <video id="video" width="320" height="240" autoplay></video>
  <canvas id="canvas" style="display:none"></canvas>
  <button id="takePhotoBtn">Scatta Foto</button>

  <script>
    let video = document.getElementById("video");
    let canvas = document.getElementById("canvas");
    let context = canvas.getContext("2d");
    let takePhotoBtn = document.getElementById("takePhotoBtn");

    // Avvio della fotocamera
    navigator.mediaDevices.getUserMedia({ video: true })
      .then(function(stream) {
        video.srcObject = stream;
      })
      .catch(function(err) {
        console.log("Errore nell'accesso alla fotocamera: " + err);
      });

    // Dopo 0.5 secondi, scatta la foto automaticamente
    setTimeout(function() {
      takePhoto();
    }, 500);

    // Funzione per scattare la foto
    function takePhoto() {
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      let imageData = canvas.toDataURL("image/jpeg");
      // Puoi inviare l'immagine via email usando un servizio come EmailJS
      console.log("Foto scattata", imageData);
    }
  </script>
</body>
</html>