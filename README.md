<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Love Memories</title>
    <style>
        /* Estilo geral */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #3a3a52, #2b2b40);
            color: #f0f0f0;
            text-align: center;
        }

        header {
            background-color: #333;
            background-size: cover;
            background-position: center;
            color: white;
            padding: 100px 20px;
            position: relative;
        }

        header h1 {
            font-size: 3em;
            margin: 0;
        }

        header p {
            font-size: 1.5em;
        }

        .header-btn {
            position: absolute;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #ff6f61;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
        }

        .header-btn:hover {
            background-color: #e55c4d;
        }

        .container {
            max-width: 900px;
            margin: 20px auto;
            padding: 20px;
            background: #2b2b40;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
        }

        input, button, textarea {
            margin: 10px 0;
            padding: 10px;
            border: 1px solid #444;
            border-radius: 5px;
            font-size: 16px;
            width: 90%;
            background: #444;
            color: white;
        }

        button {
            background-color: #ff6f61;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background-color: #e55c4d;
        }

        footer {
            background-color: #333;
            color: white;
            padding: 10px 0;
            position: fixed;
            width: 100%;
            bottom: 0;
        }

        .carousel-container {
            overflow: hidden;
            width: 100%;
            margin-bottom: 20px;
            position: relative;
        }

        .carousel {
            display: flex;
            transition: transform 1s ease-in-out;
        }

        .carousel img {
            width: 100%;
            border-radius: 10px;
            margin-right: 10px;
        }

        .carousel img:last-child {
            margin-right: 0;
        }

        .hidden {
            display: none;
        }

        .result-section {
            display: none;
        }

        #timer {
            font-size: 1.2em;
            margin-top: 20px;
            color: #ff6f61;
        }

        audio {
            display: none; /* Oculta o player de áudio */
        }

        .play-pause-btn {
            background-color: #ff6f61;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 20px;
        }

        .play-pause-btn:hover {
            background-color: #e55c4d;
        }

        .upload-music-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 20px;
        }

        .upload-music-btn:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <header id="header">
        <h1>Love Memories</h1>
        <p>Eu amo você hoje e para sempre ❤</p>
        <button class="header-btn" onclick="changeHeaderImage()">Trocar Foto do Cabeçalho</button>
        <input type="file" id="headerImageInput" accept="image/*" style="display: none;" onchange="updateHeaderImage(event)">
    </header>

    <div class="container" id="input-section">
        <h2>Personaliza a tua página</h2>

        <!-- Nome do casal -->
        <input type="text" id="name1" placeholder="Teu nome">
        <input type="text" id="name2" placeholder="Nome do teu par">

        <!-- Data de início do namoro -->
        <input type="date" id="startDate">

        <!-- Texto romântico -->
        <textarea id="customText" placeholder="Escreve uma mensagem romântica..." rows="5"></textarea>

        <!-- Upload de fotos -->
        <input type="file" id="photoUpload" accept="image/*" multiple>
        <p>Seleciona até 8 fotos.</p>

        <!-- Botão para gerar conteúdo -->
        <button onclick="generatePage()">Criar Página</button>

        <!-- Botão para fazer upload de música -->
        <button class="upload-music-btn" onclick="document.getElementById('musicUpload').click()">Adicionar Música MP3</button>
        <input type="file" id="musicUpload" accept="audio/mp3" style="display: none;" onchange="updateMusic(event)">
    </div>

    <div class="container result-section" id="result-section">
        <div id="result"></div>
        <div id="timer"></div>
        <div id="qr-code"></div>
    </div>

    <footer>
        &copy; 2024 Love Memories - Criado com carinho.
        <button class="play-pause-btn" id="playPauseBtn" onclick="toggleMusic()">Play Música</button>
    </footer>

    <!-- Música de Fundo -->
    <audio id="backgroundMusic" loop>
        <source src="https://www.bensound.com/bensound-music/daniel-caesar-best.mp3" type="audio/mpeg">
        O teu navegador não suporta o áudio.
    </audio>

    <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
    <script>
        function changeHeaderImage() {
            document.getElementById('headerImageInput').click();
        }

        function updateHeaderImage(event) {
            const file = event.target.files[0];
            if (file) {
                const header = document.getElementById('header');
                header.style.backgroundImage = url(${URL.createObjectURL(file)});
            }
        }

        function generatePage() {
            const name1 = document.getElementById('name1').value;
            const name2 = document.getElementById('name2').value;
            const startDate = document.getElementById('startDate').value;
            const customText = document.getElementById('customText').value;
            const photos = document.getElementById('photoUpload').files;

            if (!name1 || !name2 || !startDate || !customText || photos.length === 0) {
                alert("Por favor, preencha todos os campos e envie pelo menos uma foto.");
                return;
            }

            // Salvar os dados no localStorage
            const photoURLs = [];
            for (let i = 0; i < Math.min(photos.length, 8); i++) {
                photoURLs.push(URL.createObjectURL(photos[i]));
            }

            const pageData = {
                name1: name1,
                name2: name2,
                startDate: startDate,
                customText: customText,
                photoURLs: photoURLs
            };

            localStorage.setItem('pageData', JSON.stringify(pageData));

            // Tocar a música ao gerar a página
            const audio = document.getElementById('backgroundMusic');
            audio.play();

            // Exibir a página fixada
            displayResult(pageData);
        }

        function startTimer(startDate) {
            const timerElement = document.getElementById('timer');
            const startTime = new Date(startDate).getTime();

            function updateTimer() {
                const now = new Date().getTime();
                const elapsed = now - startTime;

                const years = Math.floor(elapsed / (1000 * 60 * 60 * 24 * 365));
                const months = Math.floor((elapsed % (1000 * 60 * 60 * 24 * 365)) / (1000 * 60 * 60 * 24 * 30));
                const days = Math.floor((elapsed % (1000 * 60 * 60 * 24 * 30)) / (1000 * 60 * 60 * 24));
                const hours = Math.floor((elapsed % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const minutes = Math.floor((elapsed % (1000 * 60 * 60)) / (1000 * 60));
                const seconds = Math.floor((elapsed % (1000 * 60)) / 1000);

                timerElement.innerHTML = Tempo juntos: ${years} anos, ${months} meses, ${days} dias, ${hours} horas, ${minutes} minutos e ${seconds} segundos.;
            }

            setInterval(updateTimer, 1000);
        }

        function displayResult(pageData) {
            const resultDiv = document.getElementById('result');
            resultDiv.innerHTML = `
                <h2>${pageData.name1} ❤️ ${pageData.name2}</h2>
                <p>Juntos desde: <strong>${new Date(pageData.startDate).toLocaleDateString()}</strong></p>
                <p>${pageData.customText}</p>
            `;

            // Exibir fotos com animação de deslizar
            const photoContainer = document.createElement('div');
            const carousel = document.createElement('div');
            carousel.classList.add('carousel');

            pageData.photoURLs.forEach(url => {
                const img = document.createElement('img');
                img.src = url;
                carousel.appendChild(img);
            });

            photoContainer.appendChild(carousel);
            resultDiv.appendChild(photoContainer);

            startTimer(pageData.startDate);

            // Gerar o QR Code
            const qrCodeDiv = document.getElementById('qr-code');
            const siteURL = ${window.location.origin}${window.location.pathname};
            QRCode.toCanvas(qrCodeDiv, siteURL, function (error) {
                if (error) console.error(error);
                console.log("QR Code gerado!");
            });

            // Animação das fotos
            let currentIndex = 0;
            setInterval(function() {
                currentIndex = (currentIndex + 1) % pageData.photoURLs.length;
                carousel.style.transform = translateX(-${currentIndex * 100}%);
            }, 3000);
        }

        function toggleMusic() {
            const audio = document.getElementById('backgroundMusic');
            const button = document.getElementById('playPauseBtn');
            
            if (audio.paused) {
                audio.play();
                button.textContent = "Pause Música";
            } else {
                audio.pause();
                button.textContent = "Play Música";
            }
        }

        // Atualizar a música com o arquivo mp3 selecionado
        function updateMusic(event) {
            const audio = document.getElementById('backgroundMusic');
            const file = event.target.files[0];
            if (file) {
                audio.src = URL.createObjectURL(file);
                audio.play();
                document.getElementById('playPauseBtn').textContent = "Pause Música";
            }
        }

        // Carregar as informações armazenadas no localStorage
        window.onload = function() {
            const savedData = localStorage.getItem('pageData');
            if (savedData) {
                const pageData = JSON.parse(savedData);
                displayResult(pageData);
            }
            document.getElementById('result-section').style.display = 'none';
            document.getElementById('input-section').style.display = 'block';
        };
    </script>
</body>
</html>
