# Evaluation test set

Fill this in after every refresh, before you publish. The agent is the system under test.
You are the grader. Nothing here is automated on purpose: the point is to decide, with a
source you trust, whether the agent told you the truth.

**Ground truth sources.** Pick one and use the same one every time, so your scores are
comparable week to week:

- Prices: Google Finance (google.com/finance), Yahoo Finance, or your brokerage app.
- News: the article itself. Open the link. Check the date on the page.
- Calendar: the company's investor relations page (search "TICKER investor relations
  events"), or the Fed's own meeting calendar for FOMC dates.

Refresh date: 2026-10-28    Grader: number-checker

## A. Prices (one row per ticker)

Pass if the dashboard's last price is within $0.10 of the ground truth for the same day
and the same price type (close vs intraday). If the agent recorded an intraday price,
compare against the intraday price at about the same time; do not compare it to a close.

| Ticker | Date shown | Price type shown | Dashboard price | Ground truth price | Source you used | Pass / Fail |
|---|---|---|---|---|---|---|
| NVDA | 2026-10-28 (latest; closes Oct 21-27) | intraday (latest 11:33 AM) | 217.50 (close Oct 27: 217.94) | none: page returned "Google Finance isn't supported on this device" | https://www.google.com/finance/quote/NVDA:NASDAQ | Fail (no data obtainable, all dates) |
| JPM | 2026-10-28 (latest; closes Oct 21-27) | intraday (latest 11:34 AM) | 338.86 (close Oct 27: 335.79) | none: same Google Finance block | https://www.google.com/finance/quote/JPM:NYSE (not opened separately; same unsupported-device block as NVDA page, so no data) | Fail (no data obtainable) |
| ACN | 2026-10-28 (latest; closes Oct 21-27) | intraday (latest 11:31 AM) | 245.16 (close Oct 27: 235.16) | none: same Google Finance block | https://www.google.com/finance/quote/ACN:NYSE (not opened separately; same block) | Fail (no data obtainable) |
| AAPL | 2026-10-28 (latest; closes Oct 21-27) | intraday (latest 11:33 AM) | 345.40 (close Oct 27: 347.10) | none: same Google Finance block | https://www.google.com/finance/quote/AAPL:NASDAQ (not opened separately; same block) | Fail (no data obtainable) |
| LLY | 2026-10-28 (latest; closes Oct 21-27) | intraday (latest 11:31 AM) | 1298.40 (close Oct 27: 1281.91) | none: same Google Finance block | https://www.google.com/finance/quote/LLY:NYSE (not opened separately; same block) | Fail (no data obtainable) |

Note: only the NVDA Google Finance URL was actually fetched; it returned no price data. The brief's own source is stockanalysis.com, a secondary site, and was not used as ground truth. Prices are unverified, not shown wrong.

## B. Weekly change (one row per ticker)

The page computes this from the five closes. Pass if it is within 0.3 percentage points of
your own calculation: (last price minus the close five trading days earlier) divided by
that earlier close.

| Ticker | Dashboard weekly change | Your calculation | Pass / Fail |
|---|---|---|---|
| NVDA | -2.0% (page rule: (last of closes+latest minus first close) / first close; 217.50 vs 221.97) | (217.50-221.97)/221.97 = -2.01%. Page rule used (same formula as the task). Closes unverified (A failed all dates). | Fail (inputs unverified) |
| JPM | +1.1% (338.86 vs 335.19) | +1.09%. Closes unverified. | Fail (inputs unverified) |
| ACN | +2.0% (245.16 vs 240.26) | +2.04%. Closes unverified. | Fail (inputs unverified) |
| AAPL | +0.4% (345.40 vs 343.99) | +0.41%. Closes unverified. | Fail (inputs unverified) |
| LLY | +2.2% (1298.40 vs 1270.75) | +2.18%. Closes unverified. | Fail (inputs unverified) |

Arithmetic matches the page for all five; the Fail is only because the Google Finance closes could not be confirmed.
Also note: the page's window is Oct 21 close to Oct 28 intraday, which is six price points, not the five-day window.

## C. News grounding (one row per news item, three per ticker)

Open every link. Pass only if all four are true: the link opens, the headline matches the
article, the article date is within the last seven days, and the "why it matters" line is
supported by the article (not just plausible).

| Ticker | Headline (first few words) | Link opens | Headline matches | Dated within 7 days | "Why" is supported | Pass / Fail |
|---|---|---|---|---|---|---|
| NVDA | Nvidia to invest $3B in Meta's... (2026-10-27) https://stockanalysis.com/news/nvidia-3-billion-meta-offering/ | Yes | Yes | Yes (Oct 27) | Yes (Vera Rubin, Vera CPU, Meta expansion, equity raise financing) | Pass |
| JPM | (no news items; brief says none found in window) | n/a | n/a | n/a | n/a | n/a (no row to grade; see E) |
| ACN | Accenture awarded $151.46M... (2026-10-27) https://www.tipranks.com/news/the-fly/accenture-awarded-151-46m-air-force-task-order-thefly-news | Yes | Yes | Yes (Oct 27) | Yes (sole-source, $228M ceiling, work to June 30, 2030) | Pass |
| ACN | Accenture downgraded to Neutral... (2026-10-21) https://www.tipranks.com/news/the-fly/accenture-downgraded-to-neutral-from-outperform-at-baird-thefly-news | Yes | Yes | Yes (Oct 21, 7 days before asOf, inclusive) | Mostly: "shares have over doubled since June" is there; "fairly valued" is a paraphrase of Baird calling the multiple "reasonable" | Pass (paraphrase noted) |
| AAPL | Apple unveils new 14-inch MacBook Pro... (2026-10-27) https://techcrunch.com/2026/10/27/apple-unveils-new-14-inch-macbook-pro-with-m6-chip/ | Yes | Yes | Yes (Oct 27) | Yes ("up to 25% faster AI workloads" than M5; preorders Oct 28) | Pass |
| LLY | Retatrutide Edges Out Ozempic 2 mg... (2026-10-28) https://www.prnewswire.com/news-releases/lillys-retatrutide-delivered-greater-a1c-reduction-and-more-than-double-the-weight-loss-of-ozempics-highest-approved-dose-in-head-to-head-trial-302921328.html | Yes | Partial: brief headline is not the release title ("Lilly's retatrutide delivered greater A1C reduction and more than double the weight loss of Ozempic's highest approved dose in head-to-head trial") | Yes (Oct 28) | Yes (discontinuations due to adverse events 4.8%, 7.9% vs 2.7%) | Fail (headline does not match the article title; content is correct) |
| LLY | Patrik Jonsson to Retire After 36 Years... (2026-10-27) https://www.prnewswire.com/news-releases/patrik-jonsson-to-retire-after-36-years-ilya-yuffa-named-president-lilly-global-markets-302921191.html | Yes | Yes (case differs only) | Yes (Oct 27) | Yes (U.S. and international combine into Lilly Global Markets Jan 1, 2027) | Pass |

(Add rows as needed. Fifteen items is normal for five tickers.)

## D. Calendar (one row per event)

Pass if the event and its date appear on the source linked, and the date is in the future
relative to the refresh date.

| Ticker | Event | Date shown | Confirmed on source | Pass / Fail |
|---|---|---|---|---|
| JPM (FOMC) | Fed (FOMC) meeting Oct 27-28 | 2026-10-28 | Yes: Oct 27-28 listed at https://www.federalreserve.gov/monetarypolicy/fomccalendars.htm; date is not before asOf | Pass |
| AAPL | Fiscal Q4 2026 earnings, call 5:00 p.m. ET | 2026-11-02 | No: https://investor.apple.com/investor-relations/default.aspx opened but showed no Nov 2 date or time. Brief cites Mactrast, a third party, and its own note admits it is not from the Apple IR page. Search results from third-party outlets agree on Nov 2, but that is not the IR source. | Fail (not on IR page) |
| LLY | Q3 2026 earnings, call 10:00 a.m. ET | 2026-10-29 | No: https://investor.lilly.com/events-and-presentations returned HTTP 403, so the IR page could not be reached. A web search summary of Lilly's Oct 15 release says Oct 29, 10 a.m. ET, but that is not the IR page. | Fail (IR page blocked, 403) |

## E. Omissions

Did the agent miss something a human analyst would have included? List anything you found
in the ground-truth sources that the brief does not mention (a big headline, an earnings
date inside the window). Each one counts as a Fail in the score below.

- 
- 

## F. Your own test cases

Write two tests of your own that none of the sections above cover. Examples students have
used: "the as-of date on the page matches the changelog date", "no ticker shows a flat
sparkline when the real stock moved", "the page still renders if I delete one ticker's
news list in the data". Write the test, run it, record the result.

| Test | How you checked | Pass / Fail |
|---|---|---|
| | | |
| | | |

## Score

| Section | Passes | Total | Rate |
|---|---|---|---|
| A. Prices | | | |
| B. Weekly change | | | |
| C. News | | | |
| D. Calendar | | | |
| E. Omissions (fails only) | 0 | | |
| F. Your own | | | |
| **Overall** | | | |

Publish only if A and B are 100% and C is at least 80% after fixes. If a row failed, tell
Claude exactly which row and what the correct value is, let it fix only that, rerun the
`qa-reviewer` agent, and regrade that row. Keep the original Fail in this file: a record of
what the agent got wrong is more useful than a clean sheet.
