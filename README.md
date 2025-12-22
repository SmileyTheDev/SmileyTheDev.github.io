<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Remote Control Panel</title>

    <style>
        body {
            background: #0e0e0e;
            color: #fff;
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            padding: 20px;
        }

        .container {
            width: 900px;
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
        }

        iframe {
            width: 100%;
            height: 360px;
            border-radius: 10px;
            border: none;
            background: #000;
        }

        .panel {
            background: #151515;
            padding: 15px;
            border-radius: 10px;
        }

        button {
            background: #2a2a2a;
            color: #fff;
            border: none;
            padding: 12px;
            margin: 5px 0;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }

        button:hover {
            background: #3a3a3a;
        }

        input {
            width: 100%;
            padding: 10px;
            margin-top: 8px;
            border-radius: 6px;
            border: none;
            outline: none;
        }

        .movement-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            justify-items: center;
            margin-top: 10px;
        }

        .movement-grid button {
            width: 80px;
        }

        .hidden-ui {
            display: none;
            margin-top: 10px;
        }

        .donate-link {
            display: block;
            background: #2a2a2a;
            padding: 10px;
            margin: 5px 0;
            border-radius: 6px;
            text-align: center;
            color: #fff;
            text-decoration: none;
        }

        .donate-link:hover {
            background: #3a3a3a;
        }
    </style>
</head>
<body>

<div class="container">

    <!-- VIDEO SCREEN -->
    <div class="panel">
        <h3>Live Screen</h3>
        <!-- Replace LIVE_STREAM_ID with your YouTube livestream ID -->
        <iframe src="https://www.youtube.com/embed/LIVE_STREAM_ID?autoplay=1" 
                allow="autoplay; encrypted-media" allowfullscreen>
        </iframe>
    </div>

    <!-- CONTROL PANEL -->
    <div class="panel">
        <h3>Movement Controls</h3>

        <div class="movement-grid">
            <div></div>
            <button onclick="sendMovement('Up')">↑</button>
            <div></div>

            <button onclick="sendMovement('Left')">←</button>
            <button onclick="sendMovement('Down')">↓</button>
            <button onclick="sendMovement('Right')">→</button>
        </div>

        <hr style="margin: 20px 0; border-color:#333;">

        <!-- Chat and Donate Buttons -->
        <button id="toggleChatBtn">Chat</button>
        <button id="toggleDonateBtn">Donate</button>

        <!-- Hidden Chat UI -->
        <div id="chatUI" class="hidden-ui">
            <input id="username" placeholder="Roblox Username">
            <input id="chatInput" placeholder="Type message...">
            <button onclick="sendChat()">Send Message</button>
        </div>

        <!-- Hidden Donate UI -->
        <div id="donateUI" class="hidden-ui">
            <a class="donate-link" href="https://www.roblox.com/game-pass/1639345770/Donate-To-Developer-Thank-you" target="_blank">
                Donate To Developer (Thank you,) - $670
            </a>
            <a class="donate-link" href="https://www.roblox.com/game-pass/1639315675/Donate-To-Developer-Thank-You" target="_blank">
                Donate To Developer (Thank you.) - $1500
            </a>
        </div>
    </div>

</div>

<script>
const webhookURL = "https://discord.com/api/webhooks/1452549047461220365/cYjgrH5LReOy-NO7QOssV_EuzpAYjIuYVOk9WbwKuVcnDwsIFuMxzvzzjCxYai6nU88T";

// Simple anti-swear list
const bannedWords = ["fuck", "shit", "bitch", "cunt", "nigger", "faggot", "asshole"];

function containsSwear(text) {
    const lower = text.toLowerCase();
    return bannedWords.some(word => lower.includes(word));
}

function sendToWebhook(message) {
    fetch(webhookURL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ content: message })
    }).catch(err => console.error("Webhook error:", err));
}

function sendMovement(direction) {
    sendToWebhook(`Movement: ${direction}`);
}

function sendChat() {
    const username = document.getElementById("username").value.trim();
    const message = document.getElementById("chatInput").value.trim();

    if (!username) {
        alert("You must enter a Roblox username.");
        return;
    }

    if (!message) return;

    if (containsSwear(message)) {
        alert("Message blocked by chat filter.");
        return;
    }

    sendToWebhook(`Message: ${message} By ${username}`);
    document.getElementById("chatInput").value = "";
}

// Toggle Chat UI
document.getElementById("toggleChatBtn").addEventListener("click", () => {
    const chatUI = document.getElementById("chatUI");
    chatUI.style.display = chatUI.style.display === "none" ? "block" : "none";
});

// Toggle Donate UI
document.getElementById("toggleDonateBtn").addEventListener("click", () => {
    const donateUI = document.getElementById("donateUI");
    donateUI.style.display = donateUI.style.display === "none" ? "block" : "none";
});
</script>

</body>
</html>
