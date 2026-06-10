# AI Time Awareness Skill

A small AI agent skill for time-sensitive conversations.

It helps prevent two common failures:

- Treating adjacent chat messages as if they happened minutes apart, even when the user returned hours or days later.
- Treating the model's training cutoff as if it were the current date, especially for policy, stocks, politics, economics, news, prices, releases, and other date-sensitive topics.

## What It Does

When a conversation depends on current time or facts that may have changed, the agent should:

1. Anchor the answer to the user's current date, time, and timezone.
2. Avoid inferring elapsed time from transcript order alone.
3. Search fresh authoritative sources for post-cutoff or date-sensitive facts.
4. Clearly say when current time or live data is unavailable.

## Install

Copy the `realtime-context` folder into your agent's skills directory.

For Codex-style skills, the important file is:

```text
realtime-context/SKILL.md
```

Optional UI metadata is included at:

```text
realtime-context/agents/openai.yaml
```

## Example Use Cases

- A user resumes a conversation two days later and asks for advice that depends on the current time.
- A user asks about current stock prices, market events, policy changes, economic data, or breaking news.
- A user asks about something that happened after the model's training cutoff.

## License

MIT
