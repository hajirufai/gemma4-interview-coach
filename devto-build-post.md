---
title: "I Built an AI Interview Coach with Gemma 4 — Zero Backend, 100% Free"
published: false
tags: ["devchallenge", "gemma4"]
---

## What I Built

**Interview Coach** — an AI-powered interview practice tool that uses Google Gemma 4 to conduct realistic mock interviews, evaluate your answers in real-time, and generate detailed performance reports.

It runs entirely in the browser. No backend, no server, no accounts. Just Gemma 4's brain and your ambition.

### Demo

<!-- Replace with actual video link -->
{% embed https://youtu.be/YOUR_DEMO_VIDEO %}

**Live Demo:** [gemma4-interview-coach.vercel.app](https://gemma4-interview-coach.vercel.app) *(bring your own free API key from [Google AI Studio](https://aistudio.google.com/apikey))*

**GitHub Repo:** [github.com/hajirufai/gemma4-interview-coach](https://github.com/hajirufai/gemma4-interview-coach)

---

## The Problem

91% of candidates who fail online assessments never practiced under timed conditions. Interview prep tools exist, but they're either:

- **Expensive** ($30-50/month for premium)
- **Generic** (same questions for everyone)
- **Passive** (read answers, not practice them)

What if you had a personal interview coach that:
- Adapts to YOUR experience level
- Gives feedback on YOUR specific answers
- Is completely free, forever?

That's what Gemma 4 makes possible.

## 6 Practice Modes

| Mode | What It Does |
|------|-------------|
| 🗣️ **Behavioral** | STAR-method questions on leadership, conflict, teamwork |
| 💻 **Technical** | Coding problems, algorithms, data structures |
| 🏗️ **System Design** | "Design Twitter" style architecture challenges |
| 📝 **Assessment** | Simulated OA with aptitude + coding + logic |
| 🏆 **Certification** | Exam-style questions (AWS, Azure, GCP, etc.) |
| 📊 **Case Study** | Business cases with structured frameworks |

Each mode has a unique system prompt that shapes how Gemma 4 behaves — asking follow-ups, evaluating with specific criteria, and calibrating difficulty to entry/mid/senior/lead levels.

## Why Gemma 4 Was the Right Model

This wasn't "I needed an LLM and Gemma 4 was there." Every architectural decision traces back to specific Gemma 4 capabilities:

### 1. 128K Context Window → Multi-Turn Coaching

Interview practice isn't a one-shot Q&A. It's a 15-20 turn conversation where the coach needs to:
- Remember your answer to Q1 when evaluating Q8
- Notice patterns ("You keep avoiding specifics — let me push harder")
- Generate a final report that references the entire session

Gemma 4's 128K context means the full conversation — system prompt + 15 questions + 15 answers + 15 feedback blocks — fits comfortably in a single context window. No chunking, no summarization, no lost context.

```
System prompt:     ~800 tokens
Per Q&A round:     ~500 tokens (question + answer + feedback)
15 rounds:         ~7,500 tokens
Final report:      ~2,000 tokens
Total:             ~10,300 tokens ← well within 128K
```

### 2. Native Chain-of-Thought → Better Evaluations

Gemma 4 has built-in "thinking" tokens (the API returns them with `thought: true`). When the model evaluates your answer, it first reasons internally:

```json
{
  "text": "The user's answer mentions leading a team...\n- Did they follow STAR? Partially...\n- Specificity? Low...\n- Selected response: provide feedback on adding metrics",
  "thought": true
},
{
  "text": "Good start! You mentioned leading the migration, but I'd love more specifics..."
}
```

This produces dramatically better feedback than models that generate evaluations in a single pass. The thinking tokens ensure the model actually considers what was good AND what was missing before responding.

### 3. 26B MoE Architecture → Fast Conversational UX

Interview coaching is conversational. Every second of latency breaks the "interview feel." The 26B MoE variant activates only ~4B parameters per token, delivering:

- **1-3 second response times** on Google AI Studio free tier
- Near-31B quality for reasoning tasks
- Lower compute costs if self-hosted

For comparison, the 31B Dense model takes 5-10 seconds per response — fine for deep analysis but disruptive for rapid-fire interview Q&A.

### 4. Open & Free → Accessible to Everyone

This was non-negotiable. Interview prep should not be gated behind a paywall. Gemma 4 runs on:
- Google AI Studio free tier (no credit card)
- Locally via Ollama on a decent laptop
- Hugging Face for research
- Even the 2B/4B variants run on phones and Raspberry Pi

## Architecture: Why Zero Backend?

```
Browser ──(HTTPS)──> Google AI Studio API
                          │
                    Gemma 4 26B MoE
                   or 31B Dense
```

The entire app is **one HTML file** (~560 lines). No React build, no Node.js server, no database.

**Why?**

1. **Privacy**: Your API key and interview responses never touch a third-party server. Everything stays in the browser.
2. **Cost**: $0 hosting. Put it on any CDN, GitHub Pages, or just open the file locally.
3. **Speed**: No proxy server round-trip. Browser → Gemma 4 → Browser.
4. **Simplicity**: `git clone && open index.html` — that's the full setup.

**The tradeoff**: Users need their own API key. I chose this intentionally — it keeps the tool free forever and teaches users about AI APIs in the process.

## Scoring System

At any point during a session, you can hit "Score Me" for a mid-session evaluation:

```
📊 Session Scorecard

1. Communication Clarity: 7/10
2. Technical Depth: 6/10
3. Problem-Solving Approach: 8/10
4. Self-Awareness: 7/10
5. Overall Readiness: 7/10

Top Strengths:
• Strong structured thinking
• Good use of STAR method
• Honest about knowledge gaps

Areas to Improve:
• Add specific metrics and numbers
• Reduce filler words
• Practice time management

Overall Score: 35/50
Verdict: Almost Ready — one more session should do it
```

The end-of-session report includes a personalized study plan with 3 specific actions for the coming week.

## How to Use It

1. Get a free API key from [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Open the app
3. Choose your mode, role, and experience level
4. Practice!

It works on desktop and mobile. No installation needed.

## Tech Stack

- **Frontend**: Vanilla HTML + Tailwind CSS (CDN)
- **AI**: Google Gemma 4 26B MoE / 31B Dense via Generative Language API
- **Markdown**: Custom lightweight renderer
- **State**: In-memory (browser)
- **Deploy**: Static file (anywhere)

## What I Learned

1. **Gemma 4's thinking tokens are game-changing for evaluation tasks.** The model genuinely considers multiple aspects before responding, producing feedback that feels like a real interviewer's assessment.

2. **128K context is overkill for most apps — but perfect for coaching.** The ability to reference earlier answers creates a coherent coaching experience that shorter-context models can't match.

3. **The MoE architecture is underappreciated for interactive apps.** The speed difference between 26B MoE and 31B Dense is night-and-day for conversational UX. Choose MoE for chat, Dense for analysis.

4. **Zero-backend AI apps are viable and powerful.** Browser → API → Browser eliminates 90% of infrastructure complexity. The main cost is that users bring their own key — but for free tools, that's a feature, not a bug.

## What's Next

- **Image input**: Upload screenshots of coding challenges for visual analysis (Gemma 4 supports multimodal)
- **Voice mode**: Speak your answers for a more realistic interview feel
- **Session history**: LocalStorage persistence so you can track improvement over time
- **Community question banks**: Curated questions per role/company

## Try It

**Live:** [gemma4-interview-coach.vercel.app](https://gemma4-interview-coach.vercel.app)
**Code:** [github.com/hajirufai/gemma4-interview-coach](https://github.com/hajirufai/gemma4-interview-coach)
**License:** MIT — fork it, improve it, ship it.

---

*Built by [Haji Rufai](https://github.com/hajirufai) — creator of [Interview Buddy](https://interview-buddy.com), an AI-powered interview preparation platform.*
