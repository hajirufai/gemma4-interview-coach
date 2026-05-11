# 🎯 Interview Coach — Powered by Google Gemma 4

An AI-powered interview practice tool that conducts realistic mock interviews, evaluates your answers in real-time, and generates detailed performance reports. Built entirely in the browser with zero backend.

[![Built with Gemma 4](https://img.shields.io/badge/Built%20with-Gemma%204-blue?style=flat-square)](https://ai.google.dev/gemma)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![DEV.to Challenge](https://img.shields.io/badge/DEV.to-Gemma%204%20Challenge-purple?style=flat-square)](https://dev.to/challenges/gemma)

**[🚀 Live Demo](https://hajirufai.github.io/gemma4-interview-coach)** · **[📝 Dev.to Post](https://dev.to)** · **[☕ Buy Me a Coffee](https://buymeacoffee.com/hajirufai)**

## ✨ Features

### 6 Practice Modes
| Mode | Description |
|------|-------------|
| 🗣️ **Behavioral** | STAR-method questions on leadership, conflict, teamwork |
| 💻 **Technical** | Coding problems, algorithms, data structures |
| 🏗️ **System Design** | "Design Twitter" style architecture challenges |
| 📝 **Assessment** | Simulated OA with aptitude + coding + logic |
| 🏆 **Certification** | Exam-style questions (AWS, Azure, GCP, etc.) |
| 📊 **Case Study** | Business cases with structured frameworks |

### AI-Powered Evaluation
- **Real-time feedback** after every answer
- **Mid-session scorecard** — score on 5 dimensions (Communication, Technical Depth, Problem-Solving, Self-Awareness, Readiness)
- **Final performance report** with 7-day personalized study plan
- **Pattern recognition** — notices if you keep making the same mistakes

### Multimodal Input 📸
Upload screenshots of coding questions, whiteboard diagrams, or error messages. Gemma 4's vision capabilities analyze the image in context of your interview discussion.

### Dark Mode 🌙
System-preference detection with manual toggle. Full dark theme across all components.

### Session History 📋
Past sessions are saved to localStorage — track your improvement over time.

## 🏗️ Architecture

```
Browser ──(HTTPS)──> Google AI Studio API (Generative Language)
                          │
                    Gemma 4 26B MoE  (fast, conversational)
                    or 31B Dense     (deep reasoning)
```

**Zero backend.** The entire app is a single HTML file (~790 lines). No React, no Node.js, no database server. Your API key and interview responses never leave your browser.

### Why Gemma 4?

| Capability | How It's Used |
|-----------|---------------|
| **128K context** | Full conversation (15+ Q&A rounds + feedback) fits in one context window |
| **Thinking tokens** | Model reasons through evaluation criteria before responding |
| **26B MoE** | Only ~4B params active per token → fast 1-3s responses for interactive chat |
| **Multimodal** | Accepts image input for visual code review and diagram analysis |
| **Free & open** | Google AI Studio free tier, no credit card required |

## 🚀 Quick Start

### Option 1: Use the live demo
→ [hajirufai.github.io/gemma4-interview-coach](https://hajirufai.github.io/gemma4-interview-coach)

### Option 2: Run locally
```bash
git clone https://github.com/hajirufai/gemma4-interview-coach.git
cd gemma4-interview-coach
open index.html  # or just double-click the file
```

### Get your free API key
1. Go to [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
2. Click "Create API Key" (no credit card needed)
3. Paste it into the app

## 📸 Screenshots

<details>
<summary>Landing Page (Dark Mode)</summary>

<!-- Add screenshot -->
</details>

<details>
<summary>Chat Session</summary>

<!-- Add screenshot -->
</details>

<details>
<summary>Performance Scorecard</summary>

<!-- Add screenshot -->
</details>

## 🛠️ Tech Stack

- **Frontend:** Vanilla HTML + [Tailwind CSS](https://tailwindcss.com) (CDN)
- **AI:** Google Gemma 4 via [Generative Language API](https://ai.google.dev)
- **Fonts:** [Inter](https://rsms.me/inter/) via Google Fonts
- **State:** In-memory + localStorage for session history
- **Deploy:** Static file — works on any CDN, GitHub Pages, or locally

## 🤝 Contributing

Contributions welcome! Some ideas:

- [ ] Voice input/output for realistic interview simulation
- [ ] Company-specific question banks (FAANG, startups, etc.)
- [ ] Export session transcripts as PDF
- [ ] Chrome extension for in-tab practice
- [ ] Ollama support for fully offline use

## 📄 License

MIT — fork it, improve it, ship it.

## 👤 Author

**Haji Rufai** — [@hajirufai](https://github.com/hajirufai)

Creator of [Interview Buddy](https://interview-buddy.com), an AI-powered interview preparation platform.

- GitHub: [@hajirufai](https://github.com/hajirufai)
- X/Twitter: [@HajiRufai](https://twitter.com/HajiRufai)
- LinkedIn: [hajirufai](https://linkedin.com/in/hajirufai)
- Buy Me a Coffee: [hajirufai](https://buymeacoffee.com/hajirufai)

---

*Built for the [DEV.to Gemma 4 Challenge](https://dev.to/challenges/gemma) 🏆*
