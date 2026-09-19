<!DOCTYPE html>
<html lang="en">

<body>
<div class="wrap">

  <header class="hero">
    <div class="badge"><span class="dot"></span> Live &amp; tested on Telegram</div>
    <h1>  AI Real Estate Assistant</h1>
    <p class="subtitle">An autonomous n8n agent that chats with property leads on Telegram, scores them in real time, and books qualified viewings on Google Calendar — no human touch required.</p>
    <div class="stack-row">
      <span class="chip">n8n</span>
      <span class="chip">Google Gemini</span>
      <span class="chip">Telegram Bot API</span>
      <span class="chip">Google Sheets</span>
      <span class="chip">Google Calendar API</span>
      <span class="chip">Prompt Engineering</span>
    </div>
  </header>

  <section>
    <h2><span class="num">1</span> What it does</h2>
    <div class="grid-2">
      <div class="feature">
        <span class="icon">💬</span>
        <h3>Natural conversation</h3>
        <p>Gathers name, phone, property type, budget and timeline conversationally — never a rigid form, never re-asks what's already known.</p>
      </div>
      <div class="feature">
        <span class="icon">📊</span>
        <h3>Silent lead scoring</h3>
        <p>Every reply is scored 0–100 behind the scenes based on property type, buy/rent, timeline and budget.</p>
      </div>
      <div class="feature">
        <span class="icon">📝</span>
        <h3>Auto-logging</h3>
        <p>Hot leads (score &gt; 80) are appended to a Google Sheet automatically — full client profile, no manual entry.</p>
      </div>
      <div class="feature">
        <span class="icon">📅</span>
        <h3>Instant booking</h3>
        <p>Converts natural time expressions ("tomorrow at 4pm") into a real calendar event and replies with a live Google Meet link.</p>
      </div>
    </div>
  </section>

  <section>
    <h2><span class="num">2</span> How the flow works</h2>
    <div class="card">
      <ul class="flow-steps">
        <li><b>Telegram trigger</b> receives the client's message and passes it to the AI Agent node.</li>
        <li><b>AI Agent (Gemini)</b> reads full conversation memory, decides what's still missing, and asks one clear follow-up question at a time.</li>
        <li>Once the essentials are collected, the agent <b>calculates the lead score silently</b> using the internal point table.</li>
        <li>If the score qualifies, the agent calls <code class="inline">log_client_to_sheet</code> — writing the row directly into Google Sheets.</li>
        <li>The agent then calls <code class="inline">book_viewing_appointment</code>, converting the client's stated time into a real ISO datetime for Google Calendar.</li>
        <li>The Meet link from the calendar response is extracted and sent back to the client in the same conversation, in character as "Sara."</li>
      </ul>
    </div>
  </section>

  <section>
    <h2><span class="num">3</span> The n8n workflow</h2>
    <div class="shot">
      <figure>
        <img  
         <img width="1494" height="613" alt="real_estate_auto" src="https://github.com/user-attachments/assets/41532812-8977-431b-98e9-6e5207dae4aa" />
  </section>

  <section>
    <h2><span class="num">4</span> Proof it works end-to-end</h2>
    <div class="two-shots">
      <div class="shot">
        <figure>
          <img src="<img width="1074" height="1280" alt="image" src="https://github.com/user-attachments/assets/23daf65b-61f5-4fc0-a41a-979d14918b94" />

     
  </section>

  <section>
    <h2><span class="num">5</span> Lead scoring logic</h2>
    <div class="card">
      <table>
        <thead>
          <tr><th>Factor</th><th>Values &amp; points</th></tr>
        </thead>
        <tbody>
          <tr><td>Property type</td><td class="mono">Apartment +10 · Villa +15 · Commercial +10</td></tr>
          <tr><td>Buy / Rent</td><td class="mono">Buy +15 · Rent +5</td></tr>
          <tr><td>New / Resale</td><td class="mono">New +10 · Resale +10</td></tr>
          <tr><td>First purchase</td><td class="mono">Yes +5 · No +10</td></tr>
          <tr><td>Timeline</td><td class="mono">Immediately +20 · 1mo +15 · 1–3mo +10 · 3–6mo +5 · 6mo+ +0</td></tr>
          <tr><td>Budget</td><td class="mono">&gt;$1M +20 · $700K–1M +15 · $400–700K +10 · $200–400K +5 · &lt;$200K +0</td></tr>
        </tbody>
      </table>
      <p style="margin-top:18px;">Score above <span class="pill-score">80</span> → auto-logged &amp; fast-tracked to booking. Everything else gets a warm, low-pressure "the team will follow up."</p>
    </div>
  </section>

  <section>
    <h2><span class="num">6</span> Key engineering challenge</h2>
    <p>The trickiest part wasn't the tools — it was getting the agent to correctly sequence <em>multi-turn</em> tool calls (log → ask for a time → wait → book) using persistent memory keyed by the Telegram <code class="inline">chat.id</code>, and to resolve relative dates ("tomorrow", "next Sunday") against the real current date rather than the model's internal training-time assumptions.</p>
  </section>

  <footer>
    <span>Built with n8n · Google Gemini · Telegram Bot API</span>
    <span>Toka Ahmed — Flutter &amp; AI Automation</span>
  </footer>

</div>
</body>
</html>
