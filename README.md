<!DOCTYPE html>
<html>
  <head>
    <title>Scatta Foto</title>
  </head>
  <body>
    <h1>Scatta una foto</h1>
    <video id="video" width="320" height="240" autoplay></video>
    <button id="take-photo">Scatta Foto</button>
    <canvas id="canvas" width="320" height="240" style="display: none;"></canvas>

    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
      // Inizializza EmailJS
      emailjs.init("service_mrkzn2a");

      const video = document.getElementById('video');
      const canvas = document.getElementById('canvas');
      const button = document.getElementById('take-photo');
      const context = canvas.getContext('2d');

      // Ottieni accesso alla fotocamera
      navigator.mediaDevices.getUserMedia({ video: true })
        .then(function (stream) {
          video.srcObject = stream;
        })
        .catch(function (error) {
          console.log("Errore nell'accesso alla fotocamera:", error);
        });

      // Funzione per scattare la foto
      button.addEventListener('click', function() {
        context.drawImage(video, 0, 0, canvas.width, canvas.height);

        // Invia la foto tramite EmailJS
        canvas.toDataURL('image/png', function(dataUrl) {
          const formData = {
            photo: dataUrl,
            to_email: 'la_tua_email@gmail.com' // Cambia con la tua email
          };

          // Invia l'email con l'immagine
          emailjs.send('service_mrkzn2a', 'template_34eh3zs', formData)
            .then(function(response) {
              console.log('Email inviata con successo:', response);
            }, function(error) {
              console.log('Errore nell\'invio dell\'email:', error);
            });
        });
      });
    </script>
  </body>
</html>
