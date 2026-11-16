# David Wooten Jr

Enterprise sales leader, AI and cloud infrastructure.

I pick up new tech early, and in March 2026 that meant building my own agents. Meeting prep was the first thing that stuck: a brief waiting before each customer call, replacing about 30 minutes of digging through LinkedIn, CRM notes and old emails. That brief grew into an 18-agent fleet.

## The fleet

The agents run on a Mac Mini, on [OpenClaw](https://github.com/openclaw/openclaw), the open-source agent gateway. A chief-of-staff agent routes each request to 1 of 6 domain leads (sales prep, security, health, personal admin, and 2 businesses), and each lead hands the actual work to its own sub-agents. 59 scheduled jobs and 93 skills run on it daily, with model fallback across 4 providers so one outage doesn't stop the morning.

## Taste preferences

Drafts I reject teach the system. Each correction I capture becomes a row in a Supabase table: what to avoid, what to do instead, and when the opposite is right. Agents read those rules before they write anything a customer or the public will see, and they check their draft against them before handing it back. The general writing domain alone holds 50 rules, so a correction I made in August still shapes an email written today.

## What broke

Most of what I know about agents in production I learned from this system breaking.

Cost is conversation size times number of calls. One chat burned 8.3M tokens in an hour because the agent spawned sub-agents one at a time, and every child that finished resent the whole conversation. Now one question gets one delegation, and a breaker stops any session that passes 3M tokens an hour.

Background heartbeats once made up 78% of token spend, because each one replayed an agent's entire main conversation. Running them in their own session, on 1 agent instead of 17, cut each prompt from about 180K tokens to under 25K.

An agent told to "run the report" will sometimes report success it never had. Deterministic work now lives in scripts the agent executes, and the model only gets the parts that need judgment.

## Pinned

- [openclaw-fleet](https://github.com/RealDavidArt/openclaw-fleet): how the system is built and what broke, so you can borrow the patterns without my config.
- [taste-preferences](https://github.com/RealDavidArt/taste-preferences): the schema, the read-before-drafting rule and a stdlib Python file that checks drafts, small enough to add to any agent in an afternoon.
- Coming soon: meeting prep and deal review skills on made-up accounts, and a LangGraph agent.

Off the keyboard I ride and cook. [LinkedIn](https://www.linkedin.com/in/davidwootenjr)
