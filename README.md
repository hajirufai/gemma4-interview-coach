# 🎯 Interview Coach — Powered by Google Gemma 4

An AI-powered interview practice tool that conducts realistic mock interviews, evaluates your answers in real-time, and generates detailed performance reports. Built entirely in the browser with zero backend.

[![Built with Gemma 4](https://img.shields.io/badge/Built%20with-Gemma%204-blue?style=flat-square)](https://ai.google.dev/gemma)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![DEV.to Challenge](https://img.shields.io/badge/DEV.to-Gemma%204%20Challenge-purple?style=flat-square)](https://dev.to/challenges/google-gemma-2026-05-06)

**[🚀 Live Demo](https://hajirufai.github.io/gemma4-interview-coach)** · **[📝 Dev.to Post](https://dev.to/thyalpha001/i-built-an-ai-interview-coach-with-gemma-4-zero-backend-100-free-3ja1)** · **[☕ Buy Me a Coffee](https://buymeacoffee.com/hajirufai)**

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
- **Report download** — save your session results as a text file

### 📄 Resume + Job Description Aware (NEW)
Paste your resume and the job description — Gemma 4's 128K context window processes both to:
- **Tailor questions** to the specific role's requirements and tech stack
- **Personalize feedback** by referencing your actual experience
- **Identify gaps** between your background and the job requirements

### 🎤 Voice Mode
- **Speak your answers** — click the mic to talk, transcribed in real-time via browser Speech Recognition
- **Coach speaks back** — toggle voice to hear feedback read aloud
- Works in Chrome, Edge, Safari — zero API cost, fully private

### 📸 Multimodal Input
Upload screenshots of coding questions, whiteboard diagrams, or error messages. Gemma 4's vision capabilities analyze the image in context of your interview discussion.

### ⏱️ Session Timer
Live timer tracks your session duration. Helps build time awareness for real interviews.

### 🌙 Dark Mode
System-preference detection with manual toggle. Full dark theme across all components.

### 📋 Session History
Past sessions are saved to localStorage — track your improvement over time with duration, questions, and token usage.

### 4 AI Providers
| Provider | API Key | Notes |
|----------|---------|-------|
| 🔷 **Google AI Studio** | Free, no credit card | Best quality, direct API |
| 🌐 **OpenRouter** | Free tier | Fallback when Google is overloaded |
| 💚 **NVIDIA NIM** | 1000 free credits | Fast inference |
| 🤗 **Hugging Face** | Free tier | Open-source hub |

All providers use the same Gemma 4 models. Switch between them if one is rate-limited.

## 🏗️ Architecture

```
Browser ──(HTTPS)──> Google AI Studio / OpenRouter / NVIDIA / HuggingFace
                          │
                    Gemma 4 31B Dense  (deep reasoning)
                    or 26B MoE         (fast, ~4B active params)
```

**Zero backend.** The entire app is a single HTML file. No React, no Node.js, no database server. Your API key and interview responses never leave your browser.

### Why Gemma 4?

| Capability | How It's Used |
|-----------|---------------|
| **128K context** | Full conversation + resume + JD fits in one context window |
| **Thinking tokens** | Model reasons through evaluation criteria before responding |
| **31B Dense** | Deep reasoning for nuanced interview answer evaluation |
| **26B MoE** | ~4B params active → fast 1-3s responses for conversational UX |
| **Multimodal** | Image input for visual code review and diagram analysis |
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

## 🛠️ Tech Stack

- **Frontend:** Vanilla HTML + [Tailwind CSS](https://tailwindcss.com) (CDN)
- **AI:** Google Gemma 4 via [Generative Language API](https://ai.google.dev) + 3 fallback providers
- **Voice:** Browser-native Web Speech API (STT + TTS)
- **Fonts:** [Inter](https://rsms.me/inter/) via Google Fonts
- **State:** In-memory + localStorage for session history + API key persistence
- **Deploy:** Static file — works on any CDN, GitHub Pages, or locally

## 📱 Mobile Friendly

Fully responsive design optimized for phones and tablets. Touch-friendly controls, appropriate text sizes, and horizontal scrolling for action buttons.

## 🤝 Contributing

Contributions welcome! Some ideas:

- [x] Voice input/output for realistic interview simulation
- [x] Multi-provider fallback (Google, OpenRouter, NVIDIA, HuggingFace)
- [x] Session timer
- [x] Job Description + Resume context
- [x] Report download
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

*Built for the [DEV.to Gemma 4 Challenge](https://dev.to/challenges/google-gemma-2026-05-06) 🏆*
