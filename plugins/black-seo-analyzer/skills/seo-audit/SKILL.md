---
name: seo-audit
description: Run a technical SEO audit with the hosted Black SEO Analyzer connector and turn the results into a prioritized fix plan. Use when asked to audit, check, or crawl a site or page for SEO problems, to find out what changed since an earlier crawl, to see how a page looks in Google, or to answer a question about Black SEO Analyzer (bseoa) itself.
---

# SEO audit

Audits run on Black SEO Analyzer's servers for the signed-in account, and the
results are kept with the account, so a crawl from an earlier conversation can
be read again. Never invent issues: everything you report must come from a
tool result.

## Pick the right starting point

**One URL**: call `check_page(url)`. It fetches a single page, never follows
links, returns in seconds and does not count against the page budget. Use it
whenever the question is about one page, including a site's homepage, unless
the user asked for a crawl, a site audit or a number of pages.

**A whole site**: call `start_crawl(url, max_pages)` only when the user asks to
crawl or audit a site or names a number of pages; pass the number they named.
It returns a `job_id` immediately. Poll `crawl_status(job_id)` every few seconds
until `state` is `"finished"`; do not read results before then. If `state`
becomes `"failed"`, report the `error` text instead of retrying blindly. One
crawl per account runs at a time, a crawl stops at 2,000 pages, and its pages
count against the account's rolling 30-day budget. JavaScript is not rendered.

**An earlier crawl**: call `list_crawls()` to find the `session_id` behind "the
last crawl" or "yesterday's audit".

## Read the results

Call `issues_summary(session_id)` **before** `query_issues`. The summary is
small and names the issue keys worth drilling into; `query_issues` returns at
most 100 issues per call.

Then, only where you need the detail:

- `query_issues(session_id, ...)`: narrow with `min_severity`, `key`,
  `category` or `url_prefix` from the summary. If `next_offset` is not null,
  call again with `offset` set to it.
- `page_report(session_id, url)`: everything recorded about one URL. It never
  returns page HTML.
- `serp_preview(urls)`: title and description truncation, for "how does this
  look in Google?".

## Compare two crawls

`compare_crawls(base_session, compare_session)` answers "did my changes help?"
and "what got worse?". `base_session` is the earlier crawl. Only `new_issues`
are regressions; issues that were already there are not.

## Issue shape

Issues follow the `bseoa.issue/v1` schema: a stable `key`, a `category`, a
`severity` of `low`, `medium`, `high` or `critical`, the affected `url`, a
`message`, a `fix_hint` and a `docs_url`. Severity ranks SEO impact.

## Finish with a prioritized fix plan

Group the fix plan **by issue key**, not by URL: one bad template produces many
rows of the same key, and fixing the template fixes them all. Order the groups
by severity, then by the number of pages affected (the `by_key` list in
`issues_summary`). For each group give the key and what it means, its severity
and page count, what to change for this site from `fix_hint`, two or three
example URLs and the `docs_url`. Present `low` findings as optional hints, not
as problems. Then say which groups to fix first and why.

## Questions about Black SEO Analyzer itself

When the user asks how the product works (what a tool does, limits, pricing,
plans, licenses, what an issue key means), call `search_docs(query=...)`, or
`search_docs(issue_key=...)` for one issue, and answer from the sections it
returns, citing their `url`. That is the product's own documentation; do not
search the web for it. For the user's plan, pages left and retention, call
`account_status`.

## When a tool returns an error

- `trial_expired` or `no_hosted_addon`: hosted access is not active. Relay the
  tool's `message` and its `checkout_url` (or `website_url`). After the user
  says they have paid, call `account_status` and retry.
- `quota_exceeded`: the rolling 30-day page budget is used up. Say when it
  resets; `check_page` still works.
- `too_many_running_crawls`: a crawl is already running for this account. Poll
  it with `crawl_status` instead of starting another.
- `rate_limited`: wait a minute before calling again.
- `blocked_target`: the connector won't fetch that URL, because it is not a
  public website or its owner asked not to be crawled. Relay the `message`.
- `not_available_hosted`: JavaScript rendering is only in the desktop app and
  command-line tool.

Do not retry an entitlement error with other tools; they return the same error.
