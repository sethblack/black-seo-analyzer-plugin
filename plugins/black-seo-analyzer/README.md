# Black SEO Analyzer for Claude

Technical SEO audits from Claude. The audits run on Black SEO Analyzer's servers, so nothing is installed on your
computer, and the results stay with your account so you can come back to them in a later conversation.

- `.mcp.json` connects Claude to the hosted connector at `https://mcp.blackseoanalyzer.com/mcp`. The first time,
  Claude opens a sign-in page: enter your email and use the link we send, or paste a Black SEO Analyzer license
  key, then press Allow. No key or token is stored in the plugin.
- `skills/seo-audit/` teaches Claude how to run an audit well: check one page or crawl a site, read the summary
  before the detail, and finish with a fix plan grouped by issue and ordered by severity.

The connector only reads public web pages and reports what it finds. It never logs in to your site and never
changes it.

## Install

In Claude Code:

```
/plugin marketplace add sethblack/black-seo-analyzer-plugin
/plugin install black-seo-analyzer@black-seo-analyzer
```

then `/mcp` to sign in. On claude.ai, add `https://mcp.blackseoanalyzer.com/mcp` as a custom connector instead.
Setup for every host: https://www.blackseoanalyzer.com/en/mcp

You need a hosted plan or the free 14-day hosted trial, which starts the first time you sign in.

## What to ask

- "Check https://demo.blackseoanalyzer.com/contact for SEO problems."
- "Audit https://www.example.com/ up to 200 pages and tell me what to fix first."
- "Compare my last two crawls of example.com. What improved and what got worse?"
- "How will my homepage and pricing page look in Google?"
- "What does the bseoa issue key `metadata.missing_meta_description` mean?"

`demo.blackseoanalyzer.com` is a made-up bakery site with known problems, there to try the tools on.

## Help and privacy

Questions or problems: hello@blackseoanalyzer.com. What the connector keeps and for how long:
https://www.blackseoanalyzer.com/en/privacy. Terms: https://www.blackseoanalyzer.com/en/terms. Every tool, limit
and error code: https://www.blackseoanalyzer.com/en/connector-reference
