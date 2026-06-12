# AI Time Awareness

AI agents frequently make a subtle but dangerous mistake:

They assume adjacent chat messages happened close together in time.

This skill teaches agents to anchor reasoning to the user's current date, time, and timezone before answering time-sensitive questions.

## Problem

Long-running AI conversations are not always continuous.

A user may reply two minutes later, two hours later, or two days later. But many agents still behave as if the conversation never paused. That can lead to awkward or incorrect answers:

- Saying "as we discussed earlier today" when the prior message was days ago.
- Telling the user "it is getting late" without knowing their current local time.
- Answering stock, policy, politics, economics, or news questions from stale training data.
- Treating the model's training cutoff as if it were the current date.

For time-sensitive tasks, that small assumption can quietly break the whole answer.

## Solution

AI Time Awareness gives agents a simple rule:

Before answering time-sensitive questions, establish the user's current date, time, and timezone, then verify any post-cutoff or date-sensitive facts with fresh sources.

The current time does not need to be printed in every response. It just needs to be part of the agent's reasoning before it answers.

## How It Works

```text
User message
    |
    v
Check whether the answer is time-sensitive
    |
    v
Anchor current date, time, and timezone
    |
    v
Detect conversation gap and relative-date risk
    |
    v
Verify post-cutoff or changeable facts with fresh sources
    |
    v
Answer with clear separation between fresh facts and analysis
```

AI Time Awareness is intentionally small. It does not try to replace web search, market data APIs, or source verification. It tells the agent when those things are required.

## Example

Without AI Time Awareness:

```text
User:
Should I still buy the dip?

Agent:
As we discussed earlier today...
```

The discussion actually happened two days ago.

With AI Time Awareness:

```text
Agent:
I need to verify the current price and market context first before answering.
```

## Features

- Current time anchoring
- Timezone-aware reasoning
- Conversation gap detection
- Relative date resolution
- Post-cutoff fact verification
- Fresh-source workflow for date-sensitive topics
- Clear fallback when current time or live data is unavailable

## Conversation Gap Heuristic

Agents should not infer elapsed time from message order alone. If your platform tracks message timestamps, use them as a soft signal:

| Gap | Suggested behavior |
| --- | --- |
| Short gap, such as under 30 minutes | Preserve normal conversational continuity. |
| Medium gap, such as 2-24 hours | Avoid assumptions like "earlier today", "tonight", or "good morning" unless current local time is known. |
| Long gap, such as over 24 hours | Treat previous messages as historical context, not a just-now conversation. Summarize or re-anchor when needed. |

These thresholds are intentionally heuristic. Different products can adjust them based on session length, user expectations, and task risk.

## Time Injection Modes

AI Time Awareness supports two implementation patterns.

### 1. System-Level Injection

The host application adds fresh time metadata to each user turn:

```text
[Current Time: 2026-06-11 20:08 America/Edmonton]
[Previous User Message: 2026-06-09 17:42 America/Edmonton]
```

This is the most reliable option when your agent framework supports dynamic system or developer context.

### 2. Tool-Based Injection

The agent calls a runtime tool only when it needs precise current time:

```text
get_current_time(timezone="America/Edmonton")
```

This is useful for lightweight frameworks that cannot inject dynamic metadata on every turn.

## Date-Sensitive Topics

Use this skill for questions involving:

- Stocks, crypto, prices, earnings, and markets
- Policy, law, politics, and elections
- Economics, macro data, and central banks
- News, current events, and public figures
- Product releases, launches, schedules, and availability
- Anything phrased as current, latest, today, recent, now, tomorrow, or yesterday

## Supported Agents

- Claude Code
- Codex CLI
- OpenAI Agents
- Custom agent frameworks

## Quick Start

Add the skill folder to your agent, then include a current-time anchor in your runtime context.

```text
You have access to the AI Time Awareness skill.

Current user time:
2026-06-11 20:08 America/Edmonton

Before answering time-sensitive questions, anchor reasoning to the current user time.
For post-cutoff or date-sensitive facts, search fresh authoritative sources before answering.
```

For tool-based agents, expose a current-time tool:

```python
def get_current_time(timezone: str) -> str:
    """Return the current date, time, and timezone for the user."""
    ...
```

Then instruct the agent:

```text
If current time is missing or ambiguous, call get_current_time() before answering time-sensitive questions.
```

## Install

Copy the `ai-time-awareness` folder into your agent's skills directory.

For Codex-style skills, the important file is:

```text
ai-time-awareness/SKILL.md
```

Optional UI metadata is included at:

```text
ai-time-awareness/agents/openai.yaml
```

## License

MIT
