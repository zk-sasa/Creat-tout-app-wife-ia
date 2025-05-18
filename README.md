<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="theme-color" content="#111" />
  <title>CodePlay.AI</title>
  <style>
    body {
      margin: 0; font-family: sans-serif;
      background: #1e1e1e; color: #eee; display: flex; flex-direction: column; height: 100vh;
    }
    header {
      background: #111; padding: 10px 20px; font-size: 1.2em;
      display: flex; justify-content: space-between; align-items: center;
    }
    main {
      display: flex; flex: 1;
    }
    #editor {
      flex: 2; background: #262626; padding: 10px; overflow: auto;
    }
    #editor textarea {
      width: 100%; height: 100%; background: #1e1e1e; color: #eee; border: none; font-family: monospace; font-size: 14px;
    }
    #chat {
      flex: 1; border-left: 1px solid #444; display: flex; flex-direction: column;
    }
    #chat-messages {
      flex: 1; overflow: auto; padding: 10px;
    }
    #chat-input {
      display: flex;
    }
    #chat-input input {
      flex: 1; padding: 10px; border: none; background: #333; color: #fff;
    }
    #chat-input button {
      padding: 10px; background: #444; border: none; color: #fff;
    }
    #audio-controls {
      margin-left: 20px;
    }
  </style>
</head>
<body>
  <header>
    <div><strong>CodePlay.AI</strong></div>
    <div id="audio-controls">
      <button onclick="playSound()">🔊 Play Sound</button>
    </div>
  </header>
  <main>
    <div id="editor">
      <textarea id="code" placeholder="Écris ton code ici..."></textarea>
    </div>
    <div id="chat">
      <div id="chat-messages"></div>
      <div id="chat-input">
        <input type="text" id="chat-text" placeholder="Parle avec l'IA..." />
        <button onclick="sendMessage()">Envoyer</button>
      </div>
    </div>
  </main>

  <script>
    // Chat infini basique
    const messages = document.getElementById('chat-messages');
    const input = document.getElementById('chat-text');

    function appendMessage(text, from = 'user') {
      const div = document.createElement('div');
      div.style.margin = '5px 0';
      div.innerHTML = `<strong>${from === 'user' ? '👤' : '🤖'}:</strong> ${text}`;
      messages.appendChild(div);
      messages.scrollTop = messages.scrollHeight;
    }

    function sendMessage() {
      const text = input.value.trim();
      if (!text) return;
      appendMessage(text, 'user');
      input.value = '';
      // Simulation IA
      setTimeout(() => {
        const reply = "Je suis une IA hors ligne. Connecte-moi plus tard pour une vraie réponse.";
        appendMessage(reply, 'ai');
      }, 500);
    }

    // Web Audio API (test simple)
    function playSound() {
      const context = new (window.AudioContext || window.webkitAudioContext)();
      const o = context.createOscillator();
      const g = context.createGain();
      o.type = 'sine';
      o.frequency.setValueAtTime(440, context.currentTime);
      o.connect(g);
      g.connect(context.destination);
      o.start();
      o.stop(context.currentTime + 0.5);
    }

    // PWA install (manifeste simplifié)
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('sw.js').catch(console.error);
    }
  </script>
</body>
</html>