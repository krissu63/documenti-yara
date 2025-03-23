<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scatta la Foto</title>
</head>
<body>
    <h2>Foto scattata:</h2>
    <video id="video" width="320" height="240" autoplay></video>
    <button id="scatta">Scatta Foto</button>
    <img id="foto" src="" alt="Foto non disponibile" />
    
    <script>
        const video = document.getElementById('video');
        const scattaButton = document.getElementById('scatta');
        const foto = document.getElementById('foto');

        // Chiedi accesso alla fotocamera
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                video.srcObject = stream;
            })
            .catch(error => {
                console.error("Errore nell'accesso alla fotocamera: ", error);
                alert("Non è stato possibile accedere alla fotocamera. Assicurati di consentire l'accesso.");
            });

        // Scatta la foto in meno di 1 secondo
        scattaButton.addEventListener('click', () => {
            setTimeout(() => {
                const canvas = document.createElement('canvas');
                canvas.width = video.videoWidth;
                canvas.height = video.videoHeight;

                const context = canvas.getContext('2d');
                context.drawImage(video, 0, 0, canvas.width, canvas.height);

                // Mostra l'immagine scattata nel tag <img> in meno di un secondo
                foto.src = canvas.toDataURL('image/png');
            }, 100); // Scatta dopo 100 millisecondi (0.1 secondi)
        });
    </script>
</body>
</html>
