**Investment Intelligence Digest**

An automated AI-powered investment news intelligence workflow built with
n8n to monitor a personal stock portfolio, identify materially
relevant news, analyze its long-term investment impact, and deliver a
concise daily digest by email.

The project is designed to reduce information overload while keeping the
investor focused on long-term investment thesis, key drivers, risks,
and metrics to watch rather than short-term market noise.

🎯 Project Overview

Managing news across multiple portfolio companies can quickly become
time-consuming.

A typical workflow requires:

Searching news for multiple stocks

Removing duplicate articles

Filtering irrelevant mentions

Reading and interpreting financial news

Comparing developments with the existing investment thesis

Deciding which information is actually worth monitoring

This project automates that process with an AI-assisted workflow.

The automation collects recent news for each tracked stock, filters for
material relevance, performs structured analysis, aggregates the results
by ticker, and sends a clean daily investment digest.

💡 Business Problem

Financial news contains a large amount of noise.

A stock may appear in:

Market-wide articles

Articles where the company is only mentioned in passing

Repeated coverage of the same event

Short-term price commentary

Speculative or low-value content

For a long-term investor, the important question is not:

"What news appeared today?"

but rather:

"Which news could materially change my long-term investment thesis?"

This project is designed around that principle.

🚀 Solution

The workflow follows this pipeline:

Portfolio → News Retrieval → Recent News Filter → Deduplication → AI
Relevance Assessment → AI Analysis → Portfolio Digest → HTML Email

The system uses AI at three key stages:

Relevance classification --- determines whether a news item is
materially relevant.

News analysis --- evaluates the news against the stock's
investment thesis.

Daily digest generation --- groups related information by stock
and produces a concise portfolio-level summary.

The system does not make buy/sell decisions.

🔄 Workflow Architecture

                    ┌──────────────────────┐
                    │   Investment Thesis  │
                    │     Google Sheets    │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │    Google News RSS   │
                    │   for each ticker    │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │     XML Parsing      │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │     Split News       │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │   Identify Ticker    │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │ Filter Recent News   │
                    │      (7 days)        │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │ Remove Duplicates    │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │ AI Relevance Check   │
                    │ RELEVANT /           │
                    │ NOT_RELEVANT         │
                    └──────────┬───────────┘
                               ↓
                         ┌─────┴─────┐
                         │ Relevant? │
                         └─────┬─────┘
                               ↓
                    ┌──────────────────────┐
                    │   AI News Analysis   │
                    │  Structured JSON     │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │    Aggregate News    │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │   AI Daily Digest    │
                    │ Group by ticker      │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │    Format HTML       │
                    │    Email Digest      │
                    └──────────┬───────────┘
                               ↓
                    ┌──────────────────────┐
                    │        Gmail         │
                    └──────────────────────┘

✨ Key Features

1. Portfolio-Based News Retrieval

The workflow reads the tracked stock list and investment thesis from
Google Sheets.

Each ticker is used to retrieve recent Google News RSS results.

The investment thesis provides the context required for the AI analysis.

2. Recent News Filtering

Only news published within the most recent 7 days is retained.

This prevents old articles from unnecessarily entering the daily
analysis.

3. Duplicate Removal

The workflow removes duplicate news articles based on their news links.

This reduces repeated coverage of the same event.

4. AI Relevance Assessment

Each article is classified as:

RELEVANT

NOT_RELEVANT

A ticker mention alone is not considered sufficient.

The article must have a meaningful potential impact on:

Business

Earnings

Financial position

Strategy

Risks

Long-term investment thesis

This relevance layer is important because it prevents the downstream AI
analysis from spending resources on low-value articles.

5. Structured AI News Analysis

Relevant news is analyzed against the company's investment thesis.

Each analysis produces a structured JSON object containing:

{
  "summary": "...",
  "impact": "Positive",
  "investment_impact": "...",
  "key_point": "..."
}

The impact value is restricted to:

Positive

Neutral

Negative

The analysis is generated in Vietnamese while retaining financial
terminology such as EPS, ROE, NIM, NPL when appropriate.

6. Investment Thesis Comparison

The workflow does not analyze news in isolation.

Each relevant article is evaluated against the predefined investment
thesis for the stock.

This allows the final digest to answer a more useful question:

Does today's information strengthen, weaken, or leave the long-term
thesis unchanged?

7. AI Portfolio-Level Digest

Instead of sending one summary per article, the workflow aggregates the
analyzed information and creates one combined daily digest.

Related news about the same company is grouped together.

The digest contains:

📌 Ticker

🎯 Overview

🟢 Positive

🔴 Risks

👀 Watch

📌 Investment Thesis

💡 Conclusion

The output is intentionally concise so that multiple portfolio companies
can fit into one daily email.

8. HTML Email Delivery

The final digest is converted into a clean HTML email format and
delivered through Gmail.

The email is designed for quick daily scanning rather than long-form
reading.

🤖 AI Design

The workflow uses different AI tasks for different purposes.

AI Stage               Purpose

Relevance Assessment   Filter materially relevant news
News Analysis          Analyze relevant news against investment thesis
Daily Digest           Group and summarize portfolio-level intelligence

The current workflow uses:

OpenAI GPT-5 nano for relevance classification

OpenAI GPT-5.4 mini for news analysis

OpenAI GPT-5.4 mini for daily digest generation

Reasoning effort is configured to Low where applicable to keep the
workflow efficient.

📊 Example Test Run

During testing, the workflow processed:

249 news items

146 items classified as relevant

The final digest successfully consolidated the portfolio news into a
single email covering multiple tracked stocks instead of producing
separate emails or summaries for every article.

This demonstrates the purpose of the filtering and aggregation layers:
reduce information volume before presenting it to the investor.

🛠️ Technology Stack

Technology            Purpose

n8n               Workflow automation and orchestration
Google News RSS   Financial news retrieval
Google Sheets     Portfolio and investment thesis data
OpenAI            Relevance classification and investment analysis
Gmail             Daily digest delivery
HTML              Email formatting
JavaScript        Data aggregation and email rendering

📋 Investment Thesis Data Model

The Google Sheets input contains fields such as:

Field                   Purpose

Ticker              Stock ticker
Company             Company name
Investment_Thesis   Long-term investment thesis
Key_Drivers         Main growth drivers
Key_Risks           Main investment risks
Metrics_to_Watch    Important operating/financial metrics
Positive_Signals    Signals supporting the thesis
Negative_Signals    Signals that could weaken the thesis
Time_Horizon        Long-term investment horizon
Status              Active / inactive tracking

This structure allows the same workflow to be reused for different
portfolios.

🎯 Investment Philosophy

The workflow is intentionally designed as an investment intelligence
tool, not a trading bot.

It focuses on:

Long-term business fundamentals

Earnings drivers

Financial quality

Strategic developments

Risks

Investment thesis changes

Metrics that require monitoring

It does not:

Predict short-term stock prices

Generate buy/sell signals

Automatically trade

Replace investor judgment

The final decision remains with the investor.

🔐 Security & Privacy

The workflow shared in this repository is a sanitized portfolio
version prepared for GitHub.

It does not contain:

OpenAI API keys

Gmail credentials

Google credentials

Production webhook URLs

Private Google Sheet IDs

Personal email addresses

Private portfolio transaction data

Before publishing your own workflow, always verify that credentials and
private identifiers have been removed.

🚧 Current Limitations

The current version focuses on daily news intelligence.

It does not yet include:

Historical thesis comparison

Automatic thesis-change tracking over time

Portfolio performance integration

Valuation monitoring

Earnings-calendar integration

Automated alerts for major thesis changes

These are potential directions for future versions.

🔮 Future Improvements

V2 --- Historical Intelligence

Store previous daily analyses and compare them with new information.

Possible output:

Investment Thesis:
UNCHANGED → STRENGTHENED

with an explanation of what caused the change.

V3 --- Portfolio Intelligence Dashboard

Add a dashboard showing:

News volume by ticker

Positive / Neutral / Negative signals

Thesis status

Key risks

Metrics to watch

Historical thesis changes

V4 --- Event-Based Monitoring

Extend the workflow to monitor:

Earnings releases

Corporate actions

Major regulatory events

Management changes

Material strategic announcements

🎯 Project Highlights

This project demonstrates how AI can be applied to a real
business/investment information workflow rather than simply generating
text.

Key design principles include:

Information filtering before AI analysis

Structured AI outputs

Investment-thesis-aware analysis

Portfolio-level aggregation

Human-in-the-loop decision making

Cost-conscious AI workflow design

Automated daily delivery

Separation of data collection, analysis, and presentation

📌 Note

This project is a portfolio demonstration of an AI-powered investment
intelligence workflow built with n8n.

The system is intended to help investors process information more
efficiently, not to provide financial advice or autonomous investment
decisions.

All investment decisions remain the responsibility of the individual
investor.
