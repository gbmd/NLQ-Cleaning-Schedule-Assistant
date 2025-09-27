# NLQ-Cleaning-Schedule-Assistant (AgenticBotMarley)
Slackbot that takes natural language and fills in a database of coffee machine cleaning duties.
We have two coffee machines—**Garching Forschungszentrum (FZ)** and **Hochbrück (HB)**—that need cleaning. The rotating schedule isn’t memorable, so a Slack assist eases planning and can **remind users automatically**.

**USPs**
* **Per-channel user mapping:** Bot auto-fetches channel members to resolve `@names` and Slack UIDs.
* **Zero setup for teams:** Just **invite Marley** to your channel; it listens to app mentions there.
* **Stacked query handling:** Automatically **decomposes multi-intent messages** (e.g., “Tom 3rd HB, me 3rd FZ”).

## What it does (super short)
* Parses casual Slack messages → `{date, location: fz|hb, user, update}`.
* Validates ambiguity; only writes when confident.
* Upserts to Postgres and posts a **clean table** (FZ / HB per day).

## Quick start
1. Import `AgenticBotMarley.json` into **n8n**.
2. Add credentials: Slack, Postgres, OpenAI.
3. Invite the bot to your Slack channel and mention it.

## Example messages
* `Ich übernehme morgen im FZ.`
* `Tom putzt am 3. in HB, ich am 3. in FZ.`
* Ambiguous (“vielleicht”, “nächste Woche”) → no update + friendly reason.

## Data
* Table `coffee_duty` keyed by `date` + `location` (`fz|hb`), stores `responsible_user_ai`.
* Joins `research_groups` to render names.
