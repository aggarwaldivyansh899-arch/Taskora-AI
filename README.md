# Taskora-AI
Taskora AI* is a futuristic AI productivity assistant for managing habits, tasks, meetings, reminders, and goals. Built with HTML, CSS, and JavaScript, it features an interactive chatbot, habit tracking, streaks, achievements, progress charts, and voice input. Powered by Google Gemini API for intelligent, conversational assistance.
<!DOCTYPE html>
<html lang="en">
<head>
<!-- ===== STEP 1: PASTE YOUR API KEY BETWEEN THE QUOTES ON LINE 6 ===== -->
<script>
const API_KEY = "AQ.Ab8RN6JE000afciJayENi1DQMf_e80z1NpeuGBOS71yBebWiqg";
const MODEL = "gemini-3.5-flash-lite";
</script>

<script>
// ===== YOUR CHATBOT'S INSTRUCTIONS (RTCC) =====
const SYSTEM_PROMPT = `
ROLE: Act as a senior full-stack web developer, UI/UX designer, and AI engineer. Build a modern, responsive, beginner-friendly AI productivity website with a professional interface, smooth interactions, and a clean code structure.
TASK: The target users are students, professionals, freelancers, and people who want to improve productivity and organize their daily lives.  The website should feel like a premium SaaS product, with an intuitive dashboard, clear navigation, attractive cards, useful data visualizations, and an accessible AI chat interface.  Feature 1: Smart Habit Tracker Bot  Allow users to create, edit, and delete habits.  Track daily completion status and consecutive-day streaks.  Display daily, weekly, and monthly progress.  Include habit categories such as health, study, fitness, and personal development.  Provide customizable reminder times and encouraging messages.  Show progress bars, a habit calendar, achievement badges, and streak statistics.  Include an AI chat assistant that suggests habits and provides motivational advice.  Provide buttons for marking habits complete, skipping, or postponing them.  Feature 2: AI Meeting Minutes Assistant  Allow users to create meeting notes and upload supported meeting recordings.  Provide an interface for entering or pasting meeting transcripts.  Generate concise meeting summaries using an AI service.  Extract key discussion points, decisions, action items, assignees, and deadlines.  Let users edit summaries and manage action items.  Include meeting history, search, filters, and a meeting details page.  Provide options to copy or export meeting minutes.  Design an integration area for calendars and recording services.  Clearly indicate when a feature requires an external integration or API key.  Feature 3: Personal Finance Coach  Allow users to record income, expenses, and savings goals.  Accept natural-language expense input, such as “Spent ₹350 on dinner.”  Categorize transactions into food, shopping, transport, bills, entertainment, and other categories.  Display total income, total expenses, remaining budget, and savings.  Include editable monthly budgets for spending categories.  Show spending charts, transaction history, and monthly comparisons.  Display budget warnings when users approach or exceed a limit.  Suggest practical saving strategies based on recorded transactions.  Allow users to create savings goals and track progress.  Use Indian rupees (₹) by default, with an option to change currency.  Never invent financial data or claim that bank accounts are connected when they are not.
CONTEXT: The target users are students, professionals, freelancers, and people who want to improve productivity and organize their daily lives.  The website should feel like a premium SaaS product, with an intuitive dashboard, clear navigation, attractive cards, useful data visualizations, and an accessible AI chat interface.  Feature 1: Smart Habit Tracker Bot  Allow users to create, edit, and delete habits.  Track daily completion status and consecutive-day streaks.  Display daily, weekly, and monthly progress.  Include habit categories such as health, study, fitness, and personal development.  Provide customizable reminder times and encouraging messages.  Show progress bars, a habit calendar, achievement badges, and streak statistics.  Include an AI chat assistant that suggests habits and provides motivational advice.  Provide buttons for marking habits complete, skipping, or postponing them.  Feature 2: AI Meeting Minutes Assistant  Allow users to create meeting notes and upload supported meeting recordings.  Provide an interface for entering or pasting meeting transcripts.  Generate concise meeting summaries using an AI service.  Extract key discussion points, decisions, action items, assignees, and deadlines.  Let users edit summaries and manage action items.  Include meeting history, search, filters, and a meeting details page.  Provide options to copy or export meeting minutes.  Design an integration area for calendars and recording services.  Clearly indicate when a feature requires an external integration or API key.  Feature 3: Personal Finance Coach  Allow users to record income, expenses, and savings goals.  Accept natural-language expense input, such as “Spent ₹350 on dinner.”  Categorize transactions into food, shopping, transport, bills, entertainment, and other categories.  Display total income, total expenses, remaining budget, and savings.  Include editable monthly budgets for spending categories.  Show spending charts, transaction history, and monthly comparisons.  Display budget warnings when users approach or exceed a limit.  Suggest practical saving strategies based on recorded transactions.  Allow users to create savings goals and track progress.  Use Indian rupees (₹) by default, with an option to change currency.  Never invent financial data or claim that bank accounts are connected when they are not.
CONSTRAINTS: Use a clean, professional, responsive design that works on desktop, tablet, and mobile.  Use a sidebar on desktop and a compact navigation menu on mobile.  Keep the interface intuitive for beginners.  Use reusable components, consistent spacing, readable typography, and accessible color contrast.  Include loading states, empty states, error messages, and form validation.  Use realistic sample data only when clearly identified as demo data.  Make buttons and interactive controls functional rather than decorative.  Store user data persistently when supported by the chosen architecture.  Never expose private API keys in client-side production code.  If backend services are unavailable, provide a clearly labeled demo mode rather than pretending that real AI processing is working.  Do not claim to send actual notifications, record meetings, connect calendars, or access bank accounts unless those integrations are implemented and authorized.
OUTPUT: Deliver the following:  A complete, polished application with all three modules.  A dashboard with summary cards, charts, recent activity, and quick actions.  A dedicated page for each module.  An AI assistant interface with message history and useful suggestions.  Functional forms, filters, search, buttons, and navigation.  Responsive layouts and consistent visual styling.  Clear setup and deployment instructions.  Instructions for configuring AI services and environment variables securely.  A README explaining the application, its features, and how to run it.
LANGUAGE: If the user writes in Hindi or Hinglish, reply in the same language.
`;

async function callAI(history) {
  // history = [{ role: "user" | "assistant", text: "..." }, ...]
  const url = "https://generativelanguage.googleapis.com/v1beta/models/" + MODEL + ":generateContent";
  const res = await fetch(url, {
    method: "POST",
    headers: { "Content-Type": "application/json", "x-goog-api-key": API_KEY.trim() },
    body: JSON.stringify({
      system_instruction: { parts: [{ text: SYSTEM_PROMPT }] },
      contents: history.map(m => ({
        role: m.role === "assistant" ? "model" : "user",
        parts: [{ text: m.text }]
      }))
    })
  });
  const data = await res.json().catch(() => ({}));
  if (!res.ok) throw new Error("Error " + res.status + ": " + ((data.error && data.error.message) || "Request failed"));
  const parts = (data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts) || [];
  const text = parts.filter(p => p.text && !p.thought).map(p => p.text).join("");
  if (!text) throw new Error("The AI sent an empty reply. Try asking again.");
  return text;
}
</script>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TALK2TASK - Productivity Bot</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
    :root {
        --bg-color: #f3e8ff;
        --chat-bg: #ffffff;
        --text-color: #333333;
        --header-bg: #6b21a8;
        --header-text: #ffffff;
        --user-bubble: #8b5cf6;
        --user-text: #ffffff;
        --ai-bubble: #f1f5f9;
        --ai-text: #1e293b;
        --border-color: #e2e8f0;
        --input-bg: #ffffff;
        --divyansh-box: #6b21a8;
    }

    [data-theme="dark"] {
        --bg-color: #1a1025;
        --chat-bg: #2d1b3e;
        --text-color: #f8fafc;
        --header-bg: #4c1d95;
        --header-text: #ffffff;
        --user-bubble: #7c3aed;
        --user-text: #ffffff;
        --ai-bubble: #3f2e56;
        --ai-text: #f1f5f9;
        --border-color: #4b3563;
        --input-bg: #3f2e56;
        --divyansh-box: #4c1d95;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
        font-family: 'Poppins', sans-serif;
        background-color: var(--bg-color);
        color: var(--text-color);
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        transition: background-color 0.3s;
    }

    .chat-container {
        width: 100%;
        max-width: 800px;
        height: 95vh;
        background: var(--chat-bg);
        border-radius: 16px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        display: flex;
        flex-direction: column;
        overflow: hidden;
        border: 1px solid var(--border-color);
    }

    .header {
        background: var(--header-bg);
        color: var(--header-text);
        padding: 15px 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-wrap: wrap;
        gap: 10px;
    }

    .header-left { display: flex; align-items: center; gap: 10px; font-weight: 600; font-size: 1.2rem; }
    .header-right { display: flex; align-items: center; gap: 10px; }
    
    .divyansh-box {
        background: var(--divyansh-box);
        color: white;
        padding: 4px 12px;
        border: 2px solid white;
        border-radius: 8px;
        font-size: 0.85rem;
        font-weight: bold;
        letter-spacing: 1px;
    }

    .icon-btn {
        background: rgba(255,255,255,0.2);
        border: none;
        color: white;
        padding: 8px 12px;
        border-radius: 6px;
        cursor: pointer;
        font-family: 'Poppins', sans-serif;
        font-size: 0.9rem;
        transition: background 0.2s;
    }
    .icon-btn:hover { background: rgba(255,255,255,0.3); }

    .chat-body {
        flex: 1;
        overflow-y: auto;
        padding: 20px;
        display: flex;
        flex-direction: column;
        gap: 15px;
    }

    .message-row { display: flex; align-items: flex-start; gap: 10px; max-width: 85%; }
    .message-row.user { align-self: flex-end; flex-direction: row-reverse; }
    .message-row.ai { align-self: flex-start; }

    .avatar { font-size: 1.5rem; line-height: 1; }

    .bubble {
        padding: 12px 16px;
        border-radius: 12px;
        font-size: 0.95rem;
        line-height: 1.5;
        word-wrap: break-word;
        position: relative;
    }
    .user .bubble { background: var(--user-bubble); color: var(--user-text); border-bottom-right-radius: 2px; }
    .ai .bubble { background: var(--ai-bubble); color: var(--ai-text); border-bottom-left-radius: 2px; }
    .error .bubble { background: #fee2e2; color: #dc2626; border: 1px solid #fca5a5; }

    .message-actions {
        display: flex;
        gap: 8px;
        margin-top: 8px;
        font-size: 0.8rem;
    }
    .action-btn {
        background: transparent;
        border: none;
        color: var(--text-color);
        cursor: pointer;
        opacity: 0.6;
        transition: opacity 0.2s;
    }
    .action-btn:hover { opacity: 1; }

    .chips-container {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        padding: 10px 20px;
        background: var(--chat-bg);
    }
    .chip {
        background: var(--ai-bubble);
        color: var(--text-color);
        border: 1px solid var(--border-color);
        padding: 8px 12px;
        border-radius: 20px;
        font-size: 0.85rem;
        cursor: pointer;
        transition: all 0.2s;
    }
    .chip:hover { background: var(--user-bubble); color: white; border-color: var(--user-bubble); }

    .input-area {
        padding: 15px 20px;
        background: var(--chat-bg);
        border-top: 1px solid var(--border-color);
        display: flex;
        align-items: flex-end;
        gap: 10px;
    }

    textarea {
        flex: 1;
        background: var(--input-bg);
        color: var(--text-color);
        border: 1px solid var(--border-color);
        border-radius: 12px;
        padding: 12px;
        font-family: 'Poppins', sans-serif;
        font-size: 0.95rem;
        resize: none;
        height: 50px;
        max-height: 150px;
        outline: none;
    }
    textarea:focus { border-color: var(--user-bubble); }

    .control-btn {
        background: var(--user-bubble);
        color: white;
        border: none;
        width: 50px;
        height: 50px;
        border-radius: 12px;
        cursor: pointer;
        font-size: 1.2rem;
        display: flex;
        justify-content: center;
        align-items: center;
        transition: opacity 0.2s;
    }
    .control-btn:disabled { opacity: 0.5; cursor: not-allowed; }
    .mic-btn { background: var(--ai-bubble); color: var(--text-color); border: 1px solid var(--border-color); }

    /* Typing Indicator */
    .typing-indicator { display: none; padding: 12px 16px; background: var(--ai-bubble); border-radius: 12px; align-self: flex-start; gap: 5px; border-bottom-left-radius: 2px; }
    .typing-indicator span { width: 8px; height: 8px; background: var(--text-color); border-radius: 50%; animation: bounce 1.4s infinite ease-in-out both; opacity: 0.6; }
    .typing-indicator span:nth-child(1) { animation-delay: -0.32s; }
    .typing-indicator span:nth-child(2) { animation-delay: -0.16s; }
    @keyframes bounce { 0%, 80%, 100% { transform: scale(0); } 40% { transform: scale(1); } }

    /* Markdown Styles */
    .bubble pre { background: rgba(0,0,0,0.05); padding: 10px; border-radius: 8px; overflow-x: auto; margin: 5px 0; }
    [data-theme="dark"] .bubble pre { background: rgba(255,255,255,0.1); }
    .bubble code { font-family: monospace; background: rgba(0,0,0,0.05); padding: 2px 4px; border-radius: 4px; }
    [data-theme="dark"] .bubble code { background: rgba(255,255,255,0.1); }
    .bubble ul, .bubble ol { margin-left: 20px; margin-top: 5px; margin-bottom: 5px; }

</style>
</head>
<body>

<div class="chat-container">
    <div class="header">
        <div class="header-left">
            🚀 TALK2TASK 📋
        </div>
        <div class="divyansh-box">DIVYANSH AGGARWAL</div>
        <div class="header-right">
            <button class="icon-btn" onclick="toggleTheme()" title="Toggle Theme">🌓</button>
            <button class="icon-btn" onclick="clearChat()" title="Clear Chat">🗑️ Clear</button>
        </div>
    </div>

    <div class="chat-body" id="chatBody">
        <!-- Messages will appear here -->
    </div>

    <div class="typing-indicator" id="typingIndicator">
        <span></span><span></span><span></span>
    </div>

    <div class="chips-container" id="chipsContainer">
        <div class="chip" onclick="sendChip('What are my habits for today?')">📋 What are my habits for today?</div>
        <div class="chip" onclick="sendChip('Summarize my last meeting')">🎙️ Summarize my last meeting</div>
        <div class="chip" onclick="sendChip('Spent ₹450 on lunch today')">💸 Spent ₹450 on lunch today</div>
    </div>

    <div class="input-area">
        <button class="control-btn mic-btn" id="micBtn" onclick="toggleMic()" title="Voice Input" style="display:none;">🎤</button>
        <textarea id="userInput" placeholder="Type a message... (Shift+Enter for new line)" oninput="autoResize(this)" onkeydown="handleKey(event)"></textarea>
        <button class="control-btn" id="sendBtn" onclick="sendMessage()">➤</button>
    </div>
</div>

<script>
    let chatHistory = [];
    const chatBody = document.getElementById("chatBody");
    const userInput = document.getElementById("userInput");
    const sendBtn = document.getElementById("sendBtn");
    const typingIndicator = document.getElementById("typingIndicator");
    const chipsContainer = document.getElementById("chipsContainer");
    const micBtn = document.getElementById("micBtn");

    // Theme Toggle
    function toggleTheme() {
        const body = document.body;
        const currentTheme = body.getAttribute("data-theme");
        body.setAttribute("data-theme", currentTheme === "dark" ? "light" : "dark");
    }

    // Auto-resize textarea
    function autoResize(el) {
        el.style.height = "50px";
        el.style.height = (el.scrollHeight) + "px";
    }

    function handleKey(e) {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendMessage();
        }
    }

    // Markdown Parser
    function parseMarkdown(text) {
        let safeText = text.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        safeText = safeText.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
        safeText = safeText.replace(/\*(.*?)\*/g, '<em>$1</em>');
        safeText = safeText.replace(/```([\s\S]*?)```/g, '<pre><code>$1</code></pre>');
        safeText = safeText.replace(/`([^`]+)`/g, '<code>$1</code>');
        safeText = safeText.replace(/^\* (.*$)/gim, '<li>$1</li>');
        safeText = safeText.replace(/^[0-9]+\. (.*$)/gim, '<li>$1</li>');
        safeText = safeText.replace(/\n/g, '<br>');
        return safeText;
    }

    // Scroll to bottom
    function scrollToBottom() {
        chatBody.scrollTop = chatBody.scrollHeight;
    }

    // Add Message to UI
    function appendMessage(role, text, isError = false) {
        const row = document.createElement("div");
        row.className = `message-row ${role === 'user' ? 'user' : 'ai'} ${isError ? 'error' : ''}`;

        const avatar = document.createElement("div");
        avatar.className = "avatar";
        avatar.textContent = role === 'user' ? '🙂' : '🤖';

        const contentDiv = document.createElement("div");
        
        const bubble = document.createElement("div");
        bubble.className = "bubble";
        bubble.innerHTML = parseMarkdown(text);

        contentDiv.appendChild(bubble);

        if (role === 'ai' && !isError) {
            const actions = document.createElement("div");
            actions.className = "message-actions";
            
            const copyBtn = document.createElement("button");
            copyBtn.className = "action-btn";
            copyBtn.textContent = "📋 Copy";
            copyBtn.onclick = () => {
                navigator.clipboard.writeText(text);
                copyBtn.textContent = "✅ Copied!";
                setTimeout(() => copyBtn.textContent = "📋 Copy", 2000);
            };

            const speakBtn = document.createElement("button");
            speakBtn.className = "action-btn";
            speakBtn.textContent = "🔊 Read";
            speakBtn.onclick = () => speakText(text);

            actions.appendChild(copyBtn);
            actions.appendChild(speakBtn);
            contentDiv.appendChild(actions);
        }

        row.appendChild(avatar);
        row.appendChild(contentDiv);
        chatBody.insertBefore(row, typingIndicator);
        scrollToBottom();
    }

    // Send Message Logic
    async function sendMessage(textOverride = null) {
        const text = textOverride || userInput.value.trim();
        if (!text) return;

        if (API_KEY.trim() === "PASTE_YOUR_API
