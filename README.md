# Chatbot
Chatbot
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Sov — AI Chat</title>
<style>
  :root {
    --pink: #ff5f9e;
    --rose: #ff8fb1;
    --gold: #ffc85c;
    --deep: #2b1a2b;
    --ink: #3b2530;
    --soft: #fff7f4;
    --card: rgba(255, 255, 255, 0.72);
    --radius: 22px;
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    min-height: 100vh;
    font-family: "Segoe UI", system-ui, -apple-system, sans-serif;
    color: var(--ink);
    background:
      radial-gradient(circle at 12% 18%, rgba(255,95,158,.35), transparent 42%),
      radial-gradient(circle at 88% 12%, rgba(255,200,92,.42), transparent 45%),
      radial-gradient(circle at 70% 88%, rgba(255,143,177,.30), transparent 48%),
      linear-gradient(135deg, #fff3f6 0%, #fff8e8 55%, #fffdf8 100%);
    display: grid;
    place-items: center;
    padding: 24px;
  }

  .app {
    width: min(920px, 100%);
    height: min(760px, calc(100vh - 48px));
    background: var(--card);
    border: 1px solid rgba(255,255,255,.65);
    border-radius: 30px;
    box-shadow: 0 24px 70px rgba(150, 60, 100, .18);
    backdrop-filter: blur(18px);
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  header {
    padding: 20px 24px;
    display: flex;
    align-items: center;
    gap: 14px;
    background: linear-gradient(90deg, rgba(255,95,158,.92), rgba(255,200,92,.92));
    color: white;
  }

  .logo {
    width: 46px;
    height: 46px;
    border-radius: 16px;
    display: grid;
    place-items: center;
    font-weight: 800;
    background: rgba(255,255,255,.22);
    border: 1px solid rgba(255,255,255,.35);
  }

  .title h1 {
    margin: 0;
    font-size: 1.25rem;
    letter-spacing: .3px;
  }

  .title p {
    margin: 3px 0 0;
    font-size: .8rem;
    opacity: .9;
  }

  .status {
    margin-left: auto;
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: .78rem;
    background: rgba(255,255,255,.18);
    padding: 8px 13px;
    border-radius: 999px;
    border: 1px solid rgba(255,255,255,.25);
  }

  .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: #d8ffe9;
    box-shadow: 0 0 0 4px rgba(216,255,233,.18);
  }

  .chat {
    flex: 1;
    overflow-y: auto;
    padding: 26px 24px 12px;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .row {
    display: flex;
    gap: 10px;
    align-items: flex-end;
  }

  .row.user {
    justify-content: flex-end;
  }

  .avatar {
    width: 34px;
    height: 34px;
    border-radius: 12px;
    display: grid;
    place-items: center;
    font-size: .8rem;
    font-weight: 800;
    flex: 0 0 auto;
  }

  .bot-avatar {
    color: white;
    background: linear-gradient(135deg, var(--pink), var(--gold));
  }

  .user-avatar {
    color: var(--deep);
    background: #ffe3ec;
  }

  .bubble {
    max-width: min(78%, 560px);
    padding: 13px 16px;
    line-height: 1.5;
    font-size: .95rem;
    white-space: pre-wrap;
    animation: pop .22s ease;
  }

  .bot .bubble {
    background: white;
    border: 1px solid rgba(255,95,158,.14);
    border-radius: 20px 20px 20px 6px;
    box-shadow: 0 8px 22px rgba(170, 70, 110, .08);
  }

  .user .bubble {
    color: white;
    background: linear-gradient(120deg, var(--pink), var(--gold));
    border-radius: 20px 20px 6px 20px;
    box-shadow: 0 10px 24px rgba(255,95,158,.20);
  }

  .typing {
    display: inline-flex;
    gap: 5px;
    align-items: center;
  }

  .typing span {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--pink);
    animation: bounce 1s infinite;
  }

  .typing span:nth-child(2) { animation-delay: .15s; }
  .typing span:nth-child(3) { animation-delay: .3s; }

  .quick {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    padding: 0 24px 12px;
  }

  .chip {
    border: 1px solid rgba(255,95,158,.28);
    background: rgba(255,255,255,.75);
    color: var(--deep);
    padding: 8px 13px;
    border-radius: 999px;
    font-size: .82rem;
    cursor: pointer;
    transition: .18s ease;
  }

  .chip:hover {
    background: linear-gradient(120deg, var(--pink), var(--gold));
    color: white;
    transform: translateY(-1px);
  }

  .composer {
    display: flex;
    gap: 10px;
    align-items: center;
    padding: 14px 18px 18px;
    background: rgba(255,255,255,.78);
    border-top: 1px solid rgba(255,95,158,.12);
  }

  input {
    flex: 1;
    border: 1px solid rgba(255,95,158,.22);
    background: white;
    border-radius: 999px;
    padding: 14px 18px;
    font-size: .95rem;
    color: var(--ink);
    outline: none;
  }

  input:focus {
    border-color: var(--pink);
    box-shadow: 0 0 0 4px rgba(255,95,158,.12);
  }

  button {
    width: 48px;
    height: 48px;
    border: 0;
    border-radius: 50%;
    color: white;
    font-size: 1.15rem;
    cursor: pointer;
    background: linear-gradient(120deg, var(--pink), var(--gold));
    box-shadow: 0 10px 22px rgba(255,95,158,.25);
    transition: .18s ease;
  }

  button:hover { transform: translateY(-2px); }

  @keyframes pop {
    from { opacity: 0; transform: translateY(8px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); opacity: .45; }
    50% { transform: translateY(-5px); opacity: 1; }
  }

  @media (max-width: 560px) {
    body { padding: 10px; }
    .app { height: calc(100vh - 20px); border-radius: 22px; }
    .bubble { max-width: 86%; }
    .status { display: none; }
  }
</style>
</head>
<body>
  <main class="app">
    <header>
      <div class="logo">S</div>
      <div class="title">
        <h1>Sov</h1>
        <p>Your friendly AI assistant</p>
      </div>
      <div class="status"><span class="dot"></span> Online</div>
    </header>

    <section class="chat" id="chat"></section>

    <div class="quick">
      <button class="chip" onclick="quickAsk('What can you do?')">What can you do?</button>
      <button class="chip" onclick="quickAsk('Give me a study tip')">Study tip</button>
      <button class="chip" onclick="quickAsk('Suggest a creative idea')">Creative idea</button>
      <button class="chip" onclick="quickAsk('Tell me a joke')">Tell me a joke</button>
    </div>

    <div class="composer">
      <input id="input" placeholder="Message Sov…" autocomplete="off" />
      <button onclick="send()" aria-label="Send message">➤</button>
    </div>
  </main>

<script>
  const chat = document.getElementById("chat");
  const input = document.getElementById("input");

  const responses = [
    { keys: ["hello", "hi", "hey"], reply: "Hey! I’m Sov. What would you like help with today?" },
    { keys: ["what can you do", "help"], reply: "I can brainstorm ideas, explain concepts simply, help plan tasks, suggest study tips, write messages, or just chat." },
    { keys: ["study", "school", "exam"], reply: "Try the 25/5 method: focus for 25 minutes, then take a 5-minute break. Reviewing material shortly after learning it also helps it stick." },
    { keys: ["idea", "creative", "project"], reply: "Make a tiny project that solves one annoyance in your day—such as a habit tracker, a mood-based playlist generator, or a simple recipe planner." },
    { keys: ["joke"], reply: "Why did the computer go to art school? Because it wanted to learn how to draw a better byte." },
    { keys: ["thank", "thanks"], reply: "You’re welcome! I’m here whenever you need me." },
    { keys: ["bye", "goodbye"], reply: "Goodbye! Come back anytime—Sov will be here." }
  ];

  function addMessage(text, who) {
    const row = document.createElement("div");
    row.className = `row ${who}`;
    row.innerHTML = `
      <div class="avatar ${who === "bot" ? "bot-avatar" : "user-avatar"}">${who === "bot" ? "S" : "You"}</div>
      <div class="bubble">${text}</div>
    `;
    chat.appendChild(row);
    chat.scrollTop = chat.scrollHeight;
  }

  function typingIndicator() {
    const row = document.createElement("div");
    row.className = "row bot";
    row.id = "typing";
    row.innerHTML = `
      <div class="avatar bot-avatar">S</div>
      <div class="bubble"><span class="typing"><span></span><span></span><span></span></span></div>
    `;
    chat.appendChild(row);
    chat.scrollTop = chat.scrollHeight;
  }

  function removeTyping() {
    document.getElementById("typing")?.remove();
  }

  function getReply(message) {
    const text = message.toLowerCase();
    const match = responses.find(item => item.keys.some(key => text.includes(key)));
    if (match) return match.reply;

    if (text.includes("?")) {
      return `Great question. Here’s a simple way to approach it: identify what you already know, break the problem into smaller steps, then test one step at a time.`;
    }

    return `I hear you. Tell me a little more about “${message}” and I’ll help you think it through.`;
  }

  function send() {
    const message = input.value.trim();
    if (!message) return;

    addMessage(message, "user");
    input.value = "";
    typingIndicator();

    setTimeout(() => {
      removeTyping();
      addMessage(getReply(message), "bot");
    }, 700 + Math.random() * 500);
  }

  function quickAsk(text) {
    input.value = text;
    send();
  }

  input.addEventListener("keydown", event => {
    if (event.key === "Enter") send();
  });

  addMessage("Hi, I’m Sov — your pink-and-gold AI companion. Ask me anything, or choose a quick prompt below.", "bot");
</script>
</body>
</html>
