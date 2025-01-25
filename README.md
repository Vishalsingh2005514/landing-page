# landing-page
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Time Voyager - Virtual Time Travel</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(to right, #141e30, #243b55);
            color: #ffffff;
            transition: background 0.3s, color 0.3s;
        }

        header {
            text-align: center;
            padding: 20px;
            background-color: #1e293b;
        }

        header h1 {
            margin: 0;
            font-size: 2.5rem;
        }

        main {
            padding: 20px;
            text-align: center;
        }

        .animation-container {
            margin: 20px auto;
            width: 300px;
            height: 300px;
            background: radial-gradient(circle, #ff9a9e, #fad0c4, #fbc2eb, #a18cd1, #fbc2eb);
            border-radius: 50%;
            animation: pulse 3s infinite alternate, rotate 5s linear infinite;
            position: relative;
            overflow: hidden;
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            100% {
                transform: scale(1.2);
            }
        }

        @keyframes rotate {
            from {
                transform: rotate(0deg);
            }
            to {
                transform: rotate(360deg);
            }
        }

        .theme-controls {
            margin: 20px;
        }

        .theme-controls input {
            margin: 10px;
        }

        form {
            margin: 20px auto;
            max-width: 400px;
            padding: 20px;
            background: #1e293b;
            border-radius: 8px;
        }

        form input, form button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: none;
            border-radius: 5px;
        }

        form input {
            background: #e0e0e0;
        }

        form button {
            background: #0078d7;
            color: white;
            cursor: pointer;
        }

        footer {
            text-align: center;
            padding: 10px;
            background-color: #1e293b;
        }

        footer p {
            margin: 0;
        }

        .chatbot {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: #0078d7;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .chatbot-popup {
            display: none;
            position: fixed;
            bottom: 90px;
            right: 20px;
            width: 300px;
            height: 400px;
            background: #ffffff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            flex-direction: column;
        }

        #chat-window {
            padding: 10px;
            height: 80%;
            overflow-y: auto;
            background: #f1f1f1;
        }

        #chat-input {
            width: 100%;
            padding: 10px;
            border: none;
            box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to Time Voyager</h1>
        <p>Experience Virtual Time Travel with AI Chatbot and 3D Animation</p>
    </header>

    <main>
        <div class="animation-container"></div>

        <div class="theme-controls">
            <label for="theme-color">Change Theme Color:</label>
            <input type="color" id="theme-color" value="#141e30">
            <br>
            <label for="font-color">Change Font Color:</label>
            <input type="color" id="font-color" value="#ffffff">
        </div>

        <form id="contact-form">
            <h2>Contact Us</h2>
            <input type="text" placeholder="Your Name" required>
            <input type="email" placeholder="Your Email" required>
            <textarea placeholder="Your Message" rows="5" required></textarea>
            <button type="submit">Submit</button>
            <p id="form-status" style="display: none; color: lightgreen;">Your form has been successfully saved!</p>
        </form>
    </main>

    <div class="chatbot" onclick="toggleChatbot()">💬</div>
    <div class="chatbot-popup" id="chatbot-popup">
        <div id="chat-window"></div>
        <input id="chat-input" type="text" placeholder="Type your question...">
    </div>

    <footer>
        <p>Contact: +1 288 722 7343</p>
        <p>Email: <a href="mailto:timevoyager@gmail.com" style="color: #4caf50;">timevoyager@gmail.com</a></p>
        <p>Visit us: <a href="http://www.timevoyager.com" style="color: #4caf50;">www.timevoyager.com</a></p>
    </footer>

    <script>
        // Change theme color
        document.getElementById('theme-color').addEventListener('input', (event) => {
            document.body.style.background = event.target.value;
        });

        // Change font color
        document.getElementById('font-color').addEventListener('input', (event) => {
            document.body.style.color = event.target.value;
        });

        // Form submission
        document.getElementById('contact-form').addEventListener('submit', (event) => {
            event.preventDefault();
            document.getElementById('form-status').style.display = 'block';
        });

        // Chatbot interaction
        const chatInput = document.getElementById('chat-input');
        const chatWindow = document.getElementById('chat-window');

        chatInput.addEventListener('keypress', async (event) => {
            if (event.key === 'Enter' && chatInput.value.trim()) {
                const userMessage = chatInput.value.trim();
                appendMessage('You', userMessage);

                try {
                    const response = await fetch('http://localhost:3000/chat', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ message: userMessage }),
                    });
                    const data = await response.json();
                    appendMessage('AI', data.reply);
                } catch (error) {
                    appendMessage('AI', 'Sorry, something went wrong. Please try again.');
                }

                chatInput.value = '';
            }
        });

        function appendMessage(sender, message) {
            const messageElement = document.createElement('div');
            messageElement.innerHTML = `<strong>${sender}:</strong> ${message}`;
            chatWindow.appendChild(messageElement);
            chatWindow.scrollTop = chatWindow.scrollHeight;
        }

        function toggleChatbot() {
            const chatbotPopup = document.getElementById('chatbot-popup');
            chatbotPopup.style.display = chatbotPopup.style.display === 'block' ? 'none' : 'block';
        }
    </script>
</body>
</html>
