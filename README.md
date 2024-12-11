
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pacarku Chat</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #e5ddd5;
        }

        .chat-container {
            display: flex;
            height: 100vh;
            flex-direction: column;
        }

        .chat-header {
            background-color: #075e54;
            color: white;
            padding: 10px;
            text-align: center;
            font-size: 18px;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            padding-left: 15px;
        }

        .profile-img {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            margin-right: 10px;
            background-image: url('https://via.placeholder.com/40');
            background-size: cover;
        }

        .chat-messages {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
            background-color: #fff;
            display: flex;
            flex-direction: column;
        }

        .message {
            padding: 6px 12px;
            margin: 8px 0;
            border-radius: 8px;
            font-size: 14px;
            line-height: 1.4;
            display: inline-block;
            max-width: 75%;
            word-wrap: break-word;
            white-space: pre-wrap;
            position: relative;
        }

        .message.me {
            background-color: #dcf8c6;
            margin-left: auto;
            margin-right: 10px;
            display: flex;
            flex-direction: column;
            align-items: flex-end;
        }

        .message.other {
            background-color: #f1f1f1;
            margin-right: auto;
            margin-left: 10px;
        }

        .checkmark {
            font-size: 10px; /* Ukuran ceklis lebih kecil */
            margin-left: 10px;
        }

        .checkmark.unanswered {
            color: gray;
        }

        .checkmark.answered {
            color: blue;
        }

        .message-time {
            font-size: 10px;
            color: gray;
            margin-top: -5px; /* Mengurangi jarak waktu dengan teks menjadi sangat dekat */
            display: flex;
            align-items: center;
        }

        .chat-input {
            display: flex;
            border-top: 1px solid #ddd;
            background-color: #fff;
            padding: 10px;
        }

        .chat-input input {
            flex: 1;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 20px;
            font-size: 14px;
        }

        .chat-input button {
            background-color: #075e54;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 50%;
            cursor: pointer;
            margin-left: 10px;
        }

        .chat-input button:hover {
            background-color: #128c7e;
        }
    </style>
</head>
<body>

<div class="chat-container">
    <!-- Chat Header -->
    <div class="chat-header">
        <div class="profile-img"></div> <!-- Gambar profil -->
        Pacarku
    </div>

    <!-- Chat Messages -->
    <div class="chat-messages" id="chatMessages">
        <!-- Pesan akan ditampilkan di sini -->
    </div>

    <!-- Chat Input -->
    <div class="chat-input">
        <input type="text" id="messageInput" placeholder="Tulis pesan..." />
        <button id="sendButton">Kirim</button>
    </div>
</div>

<script>
    const messageInput = document.getElementById('messageInput');
    const sendButton = document.getElementById('sendButton');
    const chatMessages = document.getElementById('chatMessages');

    const replies = [
        "Kamu idiot!",
        "Dasar bodoh!",
        "Jangan ganggu gue!",
        "Pfft, bego!",
        "Kamu ngapain sih?",
        "Jangan banyak omong, tolol!"
    ];

    const unansweredMessages = [];

    function addMessage(content, sender, messageId) {
        const messageDiv = document.createElement('div');
        messageDiv.classList.add('message', sender);

        const messageText = document.createElement('p');
        messageText.innerHTML = processBoldText(content); // Memanggil fungsi untuk memproses teks

        messageDiv.appendChild(messageText);

        const messageTime = document.createElement('span');
        messageTime.classList.add('message-time');
        messageTime.textContent = getFormattedTime(); // Menambahkan waktu terkirim

        const checkmark = document.createElement('span');
        checkmark.classList.add('checkmark', 'unanswered');
        checkmark.textContent = '✓';

        // Menambahkan waktu dan ceklis ke dalam satu div
        const timeContainer = document.createElement('div');
        timeContainer.style.display = 'flex';
        timeContainer.style.alignItems = 'center';

        timeContainer.appendChild(messageTime);
        timeContainer.appendChild(checkmark);

        messageDiv.appendChild(timeContainer);

        if (sender === 'me') {
            unansweredMessages.push(messageId);
        }

        chatMessages.appendChild(messageDiv);
        chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function processBoldText(text) {
        return text.replace(/\*(.*?)\*/g, '<strong>$1</strong>');
    }

    function getFormattedTime() {
        const now = new Date();
        let hours = now.getHours();
        let minutes = now.getMinutes();
        let period = hours >= 12 ? 'PM' : 'AM';

        if (hours > 12) hours -= 12;
        if (hours === 0) hours = 12;

        if (minutes < 10) minutes = '0' + minutes;

        return `${hours}:${minutes} ${period}`;
    }

    function sendMessage() {
        const message = messageInput.value.trim();
        if (message !== "") {
            const messageId = Date.now();

            addMessage(message, 'me', messageId);
            messageInput.value = "";

            setRandomResponseTime(() => {
                const randomReply = replies[Math.floor(Math.random() * replies.length)];
                addMessage(randomReply, 'other', messageId);
                markAsAnswered(messageId);
            });
        }
    }

    function setRandomResponseTime(callback) {
        const randomValue = Math.random();
        let delay = 1000;

        if (randomValue < 0.7) {
            delay = 1000; // 70% untuk 1 detik
        } else if (randomValue < 0.9) {
            delay = 5000; // 20% untuk 5 detik
        } else {
            delay = 10000; // 10% untuk 10 detik
        }

        setTimeout(callback, delay);
    }

    function markAsAnswered(messageId) {
        const messages = document.querySelectorAll('.message.me');
        messages.forEach(message => {
            const checkmark = message.querySelector('.checkmark');
            if (checkmark && unansweredMessages.includes(messageId)) {
                checkmark.classList.remove('unanswered');
                checkmark.classList.add('answered');
                checkmark.textContent = '✓';
            }
        });
    }

    sendButton.addEventListener('click', sendMessage);

    messageInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            sendMessage();
        }
    });
</script>

</body>
</html>
