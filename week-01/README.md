# Week 01: LLM API fundamentals (TypeScript)

**Watch:** Anthropic Academy → "Building with the Claude API" (anthropic.skilljar.com), ~20-25 min per day.
**Done when:** you can write a working tool-calling loop from scratch, without a framework.

## Day 0: Setup (30 min, before Day 1)
- [ ] Personal Anthropic Console account + API key (console.anthropic.com), set a monthly spend limit
- [ ] Personal AWS account: root MFA, Budgets alerts at $10 and $25 (account is needed in week 4, but create alarms now)
- [ ] Public GitHub repo `ai-engineer-journey`, push this skeleton
- [ ] Quick look at the side-work / IP clause in your employment contract (before week 3)

## Day 1: First call
- [ ] Watch: course intro + first API request
- [ ] `week-01/mini-agent`: `npm init`, TypeScript, `@anthropic-ai/sdk`, key from `.env` (add `.env` to `.gitignore`)
- [ ] Send one message, print the answer plus `usage` (input/output tokens) and the cost of the call
- [ ] Cheatsheet: Messages API, tokens & cost
- [ ] LOG + commit

## Day 2: Streaming chat
- [ ] Watch: multi-turn conversations, system prompts, streaming
- [ ] CLI chat with readline: keep the `messages` array yourself (this is what "memory" is)
- [ ] Stream tokens to the terminal; add a system prompt flag
- [ ] Cheatsheet: system prompt, streaming
- [ ] LOG + commit

## Day 3: Tool use by hand (most important day)
- [ ] Watch: tool use
- [ ] Two tools: `get_weather` (fake data is fine) and `calculator`
- [ ] Loop: send → `stop_reason === "tool_use"` → run tool → send `tool_result` → repeat until done
- [ ] Draw the loop in `notes/week-01/agent-loop.md` (ASCII/mermaid)
- [ ] Cheatsheet: tool use loop
- [ ] LOG + commit

## Day 4: Structured output + caching
- [ ] Watch: structured output / prompt caching
- [ ] Extractor: email text → `{ customer, request, urgency }`, validate with Zod, retry on invalid output
- [ ] Enable prompt caching on a long system prompt, compare `usage` before/after
- [ ] Cheatsheet: structured output, prompt caching
- [ ] LOG + commit

## Day 5: Ship
- [ ] Clean up `mini-agent` into one CLI (chat + tools + extractor commands)
- [ ] README with the agent-loop diagram and a short GIF
- [ ] LinkedIn/X post: "This week I built an agent loop without a framework, here's what I learned"
- [ ] Radar: 3 items in `notes/radar.md`
- [ ] Self-check: can you rewrite the tool loop from scratch in 30 minutes?
