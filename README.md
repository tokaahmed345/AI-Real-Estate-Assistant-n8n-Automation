<div align="center">

# 🏠  AI Real Estate Assistant

🟢 **Live & tested on Telegram**

An autonomous n8n agent that chats with property leads on Telegram, scores them in real time, and books qualified viewings on Google Calendar — no human touch required.

`n8n` · `Google Gemini` · `Telegram Bot API` · `Google Sheets` · `Google Calendar API` · `Prompt Engineering`

</div>

---

## 1. The Problem

- **Manual replies are slow.** When a client messages a real estate company on Telegram, there's a delay until a human agent sees it and responds — and plenty of leads drop off before anyone replies at all.
- **No lead prioritization.** Every message gets treated the same, whether it's a serious buyer ready to close on a villa or someone just browsing. Agents waste time manually figuring out who deserves a fast follow-up.
- **Booking is a manual, error-prone chain.** Once a client agrees to a viewing, someone still has to log their info, open the calendar, create the event, and send the link — steps that get delayed or forgotten.
- **No 24/7 coverage.** A message sent outside business hours waits until morning — plenty of time for the client to go to a competitor who replies faster.

## 2. What It Does

| | |
|---|---|
| 💬 **Natural conversation** | Gathers name, phone, property type, budget and timeline conversationally — never a rigid form, never re-asks what's already known. |
| 📊 **Silent lead scoring** | Every reply is scored 0–100 behind the scenes based on property type, buy/rent, timeline and budget. |
| 📝 **Auto-logging** | Hot leads (score > 80) are appended to a Google Sheet automatically — full client profile, no manual entry. |
| 📅 **Instant booking** | Converts natural time expressions ("tomorrow at 4pm") into a real calendar event and replies with a live Google Meet link. |

## 3. How the Flow Works

1. **Telegram trigger** receives the client's message and passes it to the AI Agent node.
2. **AI Agent (Gemini)** reads full conversation memory, decides what's still missing, and asks one clear follow-up question at a time.
3. Once the essentials are collected, the agent **calculates the lead score silently** using the internal point table.
4. If the score qualifies, the agent calls `log_client_to_sheet` — writing the row directly into Google Sheets.
5. The agent then calls `book_viewing_appointment`, converting the client's stated time into a real ISO datetime for Google Calendar.
6. The Meet link from the calendar response is extracted and sent back to the client in the same conversation, in character as "Sara."

## 4. The n8n Workflow

<img width="1494" height="613" alt="n8n workflow diagram" src="https://github.com/user-attachments/assets/59ca2f70-48ed-4e8e-a775-ebaa736b21ae" />

*Telegram trigger → AI Agent (Gemini + Memory) → Sheets & Calendar tools → reply back to client*

## 5. Proof It Works End-to-End

<img width="1920" height="903" alt="Google Sheet with logged leads and scores" src="https://github.com/user-attachments/assets/49c883ea-c868-49e4-a30f-4c7f540d4dc6" />

*Only leads scoring above 80 get logged here — everything else is filtered out silently*

<img width="1920" height="903" alt="Telegram confirmation with Google Meet link" src="https://github.com/user-attachments/assets/cc624be6-2104-46a9-bd03-d3f8250b90cc" />

*Sara confirms the appointment with a live Google Meet link, in the same reply*

## 6. Lead Scoring Logic

| Factor | Values & Points |
|---|---|
| Property type | Apartment +10 · Villa +15 · Commercial +10 |
| Buy / Rent | Buy +15 · Rent +5 |
| New / Resale | New +10 · Resale +10 |
| First purchase | Yes +5 · No +10 |
| Timeline | Immediately +20 · 1mo +15 · 1–3mo +10 · 3–6mo +5 · 6mo+ +0 |
| Budget | >$1M +20 · $700K–1M +15 · $400–700K +10 · $200–400K +5 · <$200K +0 |

Score above **80** → auto-logged & fast-tracked to booking. Everything else gets a warm, low-pressure "the team will follow up."

## 7. Key Engineering Challenge

The trickiest part wasn't the tools — it was getting the agent to correctly sequence *multi-turn* tool calls (log → ask for a time → wait → book) using persistent memory keyed by the Telegram `chat.id`, and to resolve relative dates ("tomorrow", "next Sunday") against the real current date rather than the model's internal training-time assumptions.

---

<div align="center">

Built with n8n · Google Gemini · Telegram Bot API
**Toka Ahmed** — Flutter & AI Automation

</div>
