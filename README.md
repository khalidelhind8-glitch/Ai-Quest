<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI Quest Bot</title>
  <style>
    body {
      background: #f4f6fb;
      font-family: "Tajawal", sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
      margin: 0;
    }

    header {
      background: #4b9ce2;
      color: white;
      text-align: center;
      padding: 15px;
      font-size: 22px;
      font-weight: bold;
    }

    #chatBox {
      flex: 1;
      overflow-y: auto;
      padding: 10px;
      background: #ffffff;
      display: flex;
      flex-direction: column;
    }

    .user, .bot {
      margin: 8px 0;
      padding: 10px;
      border-radius: 12px;
      max-width: 80%;
      word-wrap: break-word;
    }

    .user {
      background: #e0f7fa;
      align-self: flex-end;
    }

    .bot {
      background: #f1f0e9;
      align-self: flex-start;
    }

    footer {
      display: flex;
      padding: 10px;
      background: #fff;
      border-top: 1px solid #ccc;
    }

    input {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 10px;
      outline: none;
    }

    button {
      margin-left: 10px;
      padding: 10px 15px;
      border: none;
      border-radius: 10px;
      background: #4b9ce2;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      background: #3b8bc9;
    }
  </style>
</head>
<body>
  <header>AI Quest Bot 🤖</header>
  <div id="chatBox"></div>
  <footer>
    <input type="text" id="userInput" placeholder="اكتب سؤالك هنا..." />
    <button onclick="sendMessage()">إرسال</button>
  </footer>

  <script>
    const API_KEY = "AIzaSyDuRg-DrM9L64ScgAD80l1m5ACNy28smVE"; // ← ضع مفتاحك هنا
    const chatBox = document.getElementById("chatBox");
    const userInput = document.getElementById("userInput");

    async function sendMessage() {
      const text = userInput.value.trim();
      if (!text) return;

      addMessage("user", text);
      userInput.value = "";

      addMessage("bot", "جارٍ التفكير... 🤔");

      try {
        const res = await fetch(
          "https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=" + API_KEY,
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ contents: [{ parts: [{ text }] }] }),
          }
        );

        const data = await res.json();
        const reply = data?.candidates?.[0]?.content?.parts?.[0]?.text || "عذرًا، لم أفهم 😅";
        updateLastBotMessage(reply);
      } catch (e) {
        updateLastBotMessage("حدث خطأ أثناء الاتصال بالخادم ❌");
      }
    }

    function addMessage(sender, text) {
      const msg = document.createElement("div");
      msg.className = sender;
      msg.textContent = text;
      chatBox.appendChild(msg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    function updateLastBotMessage(text) {
      const messages = document.getElementsByClassName("bot");
      messages[messages.length - 1].textContent = text;
      chatBox.scrollTop = chatBox.scrollHeight;
    }
  </script>
</body>
</html>
