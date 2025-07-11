# HTML-clone
<!DOCTYPE html>
<html>
<head>
    <title>WhatsApp Clone</title>
    <link rel="stylesheet" href="/static/style.css">
    <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
</head>
<body>
    <div id="chat-container">
        <div id="chat-box">
            {% for msg in messages %}
                <div class="message"><strong>{{ msg.username }}:</strong> {{ msg.message }} <span>{{ msg.timestamp }}</span></div>
            {% endfor %}
        </div>
        <input type="text" id="username" placeholder="Your name" />
        <input type="text" id="message" placeholder="Type a message..." />
        <button onclick="sendMessage()">Send</button>
    </div>

    <script>
        const socket = io();

        function sendMessage() {
            const username = document.getElementById('username').value;
            const message = document.getElementById('message').value;
            if (username && message) {
                socket.emit('send_message', { username, message });
                document.getElementById('message').value = '';
            }
        }

        socket.on('receive_message', function(msg) {
            const chatBox = document.getElementById('chat-box');
            const div = document.createElement('div');
            div.classList.add('message');
            div.innerHTML = <strong>${msg.username}:</strong> ${msg.message} <span>${msg.timestamp}</span>;
            chatBox.appendChild(div);
            chatBox.scrollTop = chatBox.scrollHeight;
        });
    </script>
</body>
</html>
