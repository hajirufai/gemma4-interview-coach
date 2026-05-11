---
title: "Gemma 4's Hidden Superpower: Why Built-in Thinking Tokens Change Everything for Evaluation Tasks"
published: false
tags: ["devchallenge", "gemma4"]
---

Most coverage of Gemma 4 focuses on the headline numbers: 128K context, 31B parameters, multimodal input. But after building a production app with it, I think the most underrated feature is something barely mentioned in the docs: **native thinking tokens**.

Here's what they are, why they matter, and what they unlock.

## What Are Thinking Tokens?

When you call Gemma 4's API, the response includes parts with a `thought: true` flag:

```json
{
  "candidates": [{
    "content": {
      "parts": [
        {
          "text": "The user asked about sorting algorithms...\n- They mentioned merge sort but not quicksort...\n- Their Big-O analysis is correct...\n- They missed the space complexity point...",
          "thought": true
        },
        {
          "text": "Great explanation of merge sort! One thing to add: while your O(n log n) analysis is correct, it's worth mentioning..."
        }
      ]
    }
  }],
  "usageMetadata": {
    "promptTokenCount": 45,
    "candidatesTokenCount": 52,
    "totalTokenCount": 312,
    "thoughtsTokenCount": 215
  }
}
```

The model generates internal reasoning (215 tokens of thinking) that you, the developer, can see but that the model treats as its "inner monologue." The final output (52 tokens) is what the user sees.

This is not chain-of-thought prompting. This is **chain-of-thought architecture** — the model does it automatically, without being told to "think step by step."

## Why This Matters for Evaluation Tasks

I discovered this while building an [AI interview coach](https://github.com/hajirufai/gemma4-interview-coach). The app asks interview questions, the user answers, and Gemma 4 evaluates the response.

Here's the thing: **evaluation is harder than generation**. Generating a plausible interview question is easy. Evaluating whether someone's answer is good — considering structure, specificity, depth, relevance, and experience level — requires the model to hold multiple criteria in working memory simultaneously.

### Without thinking tokens (typical LLM behavior):

```
User: "Tell me about a time you led a team through a difficult project."
Answer: "I led my team to deliver the Q3 release on time."

Model output: "Good job mentioning leadership! Try to add more details next time."
```

The model races to produce output without deeply considering what was missing.

### With thinking tokens (Gemma 4):

```
Thinking: "The candidate claims they led a team through a difficult project.
  - STAR analysis:
    - Situation: Not specified (what project? what was difficult?)
    - Task: Vague ('deliver Q3 release')
    - Action: Missing entirely (what did THEY specifically do?)
    - Result: 'on time' — but no metrics, no impact
  - Experience level: Mid-level, so I should expect specific examples
  - Pattern: This is the third vague answer — I should push harder this time
  - Selected approach: Acknowledge the topic, then probe for specifics"

Output: "You picked a relevant example, but I need more. What specifically made
this project difficult? What decisions did YOU make as the lead? And what
was the measurable outcome? Give me numbers — team size, timeline, impact."
```

The thinking tokens let the model systematically work through evaluation criteria before committing to a response. The result feels like feedback from an experienced interviewer, not a chatbot.

## Three Patterns Where Thinking Tokens Excel

### 1. Multi-Criteria Evaluation

When your task has multiple evaluation dimensions (like grading an essay, reviewing code, or scoring an interview answer), thinking tokens let the model address each criterion before synthesizing a final judgment.

```python
# The model naturally breaks down scoring without explicit prompting
prompt = """
Score this answer for: clarity, depth, and relevance.
Answer: "Machine learning is when computers learn from data."
"""
# Gemma 4's thinking will evaluate each dimension separately,
# then produce a balanced score. Other models might just say "7/10."
```

### 2. Pattern Recognition Across Context

With 128K context AND thinking tokens, Gemma 4 can notice patterns across a long conversation and reason about them:

```
Thinking: "Looking at answers 1-7:
  - Answers 1, 3, and 5 all lack specific metrics
  - Answer 4 was the strongest — they used concrete numbers
  - This is a pattern: the candidate is good at storytelling
    but weak at quantification
  - I should focus my feedback on this specific gap"
```

This combination — long context for memory + thinking for analysis — is genuinely new.

### 3. Calibrated Difficulty Adjustment

The model can reason about whether to make the next question easier or harder:

```
Thinking: "The candidate nailed the last two system design questions.
  - They correctly identified the bottleneck
  - Their caching strategy was solid
  - But they haven't been tested on distributed consensus yet
  - Since they're interviewing for a senior role, I should
    push into harder territory now"
```

## 26B MoE vs 31B Dense: When to Use Each

After testing both extensively, here's my take:

| | 26B MoE | 31B Dense |
|---|---------|-----------|
| **Speed** | 1-3s per response | 5-10s per response |
| **Thinking quality** | Good — covers main points | Excellent — catches edge cases |
| **Best for** | Interactive chat, quick feedback | Deep analysis, final reports |
| **Thinking tokens used** | ~100-200 per response | ~200-400 per response |

**My recommendation**: Use 26B MoE for the conversational back-and-forth, and 31B Dense for summary/evaluation tasks where speed matters less.

In my interview coach app, I default to 26B MoE because conversational latency matters more than marginal evaluation quality. But if you're building a code review tool or essay grader where the user can wait 10 seconds, go with 31B Dense.

## Practical Tips for Building with Thinking Tokens

### 1. Filter them in your UI

```javascript
const parts = response.candidates[0].content.parts;
const visibleText = parts.filter(p => !p.thought).map(p => p.text).join('');
const thinkingText = parts.filter(p => p.thought).map(p => p.text).join('');
```

Users should see the polished output, not the internal reasoning.

### 2. Log the thinking for debugging

The thinking tokens are incredibly useful for understanding why the model gave a particular response. I log them during development:

```javascript
if (process.env.NODE_ENV === 'development') {
    console.log('🧠 Model thinking:', thinkingText);
}
```

### 3. Don't fight the thinking — design around it

If you prompt Gemma 4 to "just give me a one-word answer," it'll still think internally. That's fine — the thinking tokens don't appear in the output. But they DO count toward your token usage.

For simple tasks (classification, yes/no), the thinking overhead might not be worth it. For complex tasks (evaluation, planning, multi-step reasoning), it's exactly what you want.

### 4. Temperature affects thinking quality

At low temperature (0.1-0.3), thinking tokens are more systematic and thorough. At high temperature (0.8+), they're more creative but occasionally tangential. For evaluation tasks, I recommend 0.5-0.7.

## The Bigger Picture

Thinking tokens represent a shift from "prompt engineering" to "reasoning architecture." Instead of crafting elaborate prompts that force step-by-step reasoning, the model does it natively.

This matters because:

1. **Simpler prompts, better results** — You don't need "Let's think step by step." The model already does.
2. **More reliable evaluation** — The model is less likely to give snap judgments on complex tasks.
3. **Transparent reasoning** — You can inspect the thinking to understand (and debug) the model's logic.

Combined with 128K context and the efficiency of the MoE architecture, Gemma 4 is uniquely positioned for applications that need to **reason over long interactions** — tutoring, coaching, mentoring, code review, and any task where shallow responses aren't good enough.

## Try It Yourself

The best way to see thinking tokens in action is to build something that requires evaluation. Here's a minimal example:

```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemma-4-26b-a4b-it:generateContent?key=YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts":[{"text":"Evaluate this Python code for bugs and style issues:\n\ndef fibonacci(n):\n  if n <= 1: return n\n  return fibonacci(n-1) + fibonacci(n-2)"}]}]
  }'
```

Look at the response. You'll see the thinking tokens breaking down the code analysis before the final review appears. That's Gemma 4's hidden superpower at work.

---

*This post was inspired by building [Interview Coach](https://github.com/hajirufai/gemma4-interview-coach), an open-source AI interview practice tool powered by Gemma 4. The code is MIT licensed — fork it and build something better.*
