---
name: realtime-context
description: Use when answering time-sensitive conversations involving current time, time zones, elapsed time between messages, current/latest/today/recent facts, stocks, policy, politics, economics, news, releases, events, prices, or any question where stale information could change the answer. The skill makes the agent anchor answers to the user's current time and verify post-cutoff or date-sensitive facts with fresh sources.
---

# Realtime Context

Use this skill whenever the user's request depends on current time or facts that may have changed.

## Core Rule

Do not answer time-sensitive questions from memory alone. Before answering, establish:

- The user's current date, time, and timezone.
- Whether the answer depends on facts after the model's training cutoff or facts that may have changed.

The current time anchor may be internal context; it does not need to be printed unless it affects the answer. If current time or fresh sources are unavailable, say so clearly.

## Workflow

1. Anchor user time.
   Determine the user's current date, time, and timezone from system context or a time tool. Do not assume the conversation is continuous just because messages are adjacent in the transcript; the user may have returned hours or days later.

2. Check date sensitivity.
   Treat these as date-sensitive by default: current/latest/today/recent questions, stocks and prices, policy, politics, economics, news, laws, regulations, public figures, company facts, releases, events, schedules, product availability, and anything after the model's training cutoff.

3. Verify before answering.
   The model's training cutoff is not the current date. For post-cutoff or date-sensitive facts, search the web or use fresh authoritative sources before analyzing or answering. Prefer primary sources for policy, finance, politics, economics, legal, medical, and technical topics.

4. Answer with freshness.
   Include the source timestamp or current-time context when it materially affects the answer. Keep it compact:

   ```text
   Time context: 2026-06-10 14:35 America/Edmonton. Sources checked as of 2026-06-10.
   ```

5. Separate facts from analysis.
   Clearly distinguish observed live data, sourced news, historical background, and model inference.

## Conversation Gap Guardrail

Do not infer elapsed time from message order or transcript proximity. If the answer depends on what time it is for the user, establish the user's current time first.

## Knowledge Cutoff Guardrail

The model's training cutoff is not current context. If the user asks about facts beyond the cutoff, or the answer could have changed since the cutoff, verify with fresh sources before answering. If verification is unavailable, say so directly:

```text
I cannot verify the current status from live sources here, so I should not present this as up to date.
```

## Failure Mode

If current time or fresh data cannot be obtained, use this form:

```text
I do not have access to current time or live data in this environment, so I should not present this as up to date. I can analyze data you provide or explain the general framework.
```
