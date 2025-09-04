---
layout: page
title: Data-Driven Automated Stock Investment System(WIP)
description: A fully automated system that handles the entire pipeline from data collection and AI-driven analysis to order execution.
img: /assets/img/projects/trader_thumbnail.png
importance: 2
category: side projects
---

## Automated Stock Trading Bot for the Korean Market
*Please note that this project is work in progress.*

This project is a fully automated trading bot I developed to analyze and trade stocks on the Korea Exchange (KRX). It systematically gathers market data, uses an LLM to identify promising stocks, and executes trades autonomously through the Korea Investment & Securities (KIS) API.

---

### Key Features

* **Automated Data Pipeline**: The bot runs on a schedule to automatically fetch and synchronize comprehensive corporate information from DART and daily market quotes, ensuring all decisions are based on the latest data.
* **AI-Powered Analysis**: It leverages **LangChain** and **OpenAI** to analyze historical price data, identifying key support and resistance levels for its trading strategy.

* **Scheduled Trading Execution**: The entire process is orchestrated with **APScheduler**. The bot automatically places buy and sell orders at scheduled times based on the AI's analysis, operating without manual intervention.
* **Containerized & Reproducible**: The entire application, including the MySQL database, is containerized using **Docker**. Makefile commands are provided for easy setup, deployment, and management.

---

### System Workflow

The bot operates in a continuous, automated cycle designed to maximize efficiency and timeliness.

1.  **Weekly Data Sync (Monday, 5:00 AM)**: Fetches a complete list of all publicly listed corporations from Data Analysis, Retrieval and Transfer System(DART) to build a database.
2.  **Daily Market Data (Weekdays, 4:00 PM)**: After the market closes, it retrieves the day's closing prices for every company in the list.
3.  **AI Candidate & Strategy Selection (Weekdays, 7:00 AM)**: Before the market opens, the system uses an LLM to analyze candidates and determine key support and resistance price levels for the day's trading strategy. Following is a example analysis over "SK텔레콤"
```markdown
회사: SK텔레콤
종목 코드: 017670
재무건전성: 7
성장지수: 4
매수지표: Fairly valued
지지선(lower bound): 51000
저항선(upper bound): 58800
기술적 판단: Range-bound Movement
AI 요약: SK텔레콤 shows solid liquidity (current ratio ~1.04) and sizeable equity (≈11.97T KRW) with total liabilities ≈17.33T KRW; gross interest-bearing debt is material but net debt after cash is moderate (net-debt/equity ~0.57). Operating cash flow improved year-over-year, which supports financial stability, though leverage and interest-bearing liabilities are significant. Revenue is roughly flat-to-slightly down versus prior period while net income in the current half fell markedly versus the prior comparable period, indicating weaker near-term profitability and lower growth momentum. Market multiples (PER 9.43, PBR 1.0) are in line with a typical telecom profile and suggest the stock is fairly valued. Price action over the last six months is oscillating within a range with a recurring support cluster around 51,000 KRW and resistance near 58,800 KRW; overall the technical picture is range-bound.
```
4.  **Scheduled Order Placement (Weekdays, at set times)**: Bot periodically places buy and sell orders via the KIS API based on the pre-analyzed support and resistance levels.

---

### Technology Stack

* **Backend**:  APScheduler
* **AI & Data Science**: LangChainLangchain-OpenAI, Pandas
* **Database**: MySQL 8.0SQLAlchemy
* **Financial APIs**: python-kis (Korea Investment & Securities), DART
* **DevOps & Tooling**: Docker, uv (Package Management), Makefile, AWS EC2

---

### Project Architecture

The codebase is organized to separate core infrastructure from trading logic, promoting maintainability and scalability.

* **/core**: Manages foundational components like the database session, scheduling, Discord notifications, and API client configurations.
* **/trading**: Contains all business logic, including data models, backtesting pipelines, trading strategies, and the runners that execute them.

The full source code is available on [GitHub](https://github.com/taeyoungson/trader). 