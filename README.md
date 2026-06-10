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
