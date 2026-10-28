# Black SEO Analyzer plugin for Claude

A Claude plugin marketplace with one plugin, [`black-seo-analyzer`](plugins/black-seo-analyzer/): technical SEO
audits from Claude, run on Black SEO Analyzer's servers. It connects Claude to the hosted connector at
`https://mcp.blackseoanalyzer.com/mcp` and adds a skill for running audits and turning them into a fix plan.

```
/plugin marketplace add sethblack/black-seo-analyzer-plugin
/plugin install black-seo-analyzer@black-seo-analyzer
```

Then run `/mcp` and sign in with your email or a Black SEO Analyzer license key. The plugin's
[README](plugins/black-seo-analyzer/README.md) covers what it does, example prompts, help and privacy.

This repository is generated from the Black SEO Analyzer monorepo; changes made here directly are overwritten
by the next release.
