---
layout: page
title: Flight Schedule Parser
description: An automated system to manage flight schedules from OCR data, monitor flight status, and provide AI-powered weather notifications.
img: /assets/img/projects/flight_thumbnail.png
importance: 1
category: side projects
---

### Overview

This project was born from a personal challenge. My girlfriend is a flight attendant and would often send me her work schedule as a phone screenshot. The schedule listed various flights with departure and arrival times in different local time zones, making it surprisingly difficult for me to track her travels from my perspective in Korean Standard Time. Manually converting these times and adding them to my calendar was tedious and prone to error. 😥

{% include figure.html path="assets/img/projects/flight_schedule_example.jpg" title="Flight schedule example" class="img-fluid rounded z-depth-1" caption="Example image of a flight schedule" style="max-width: 550px;" %}

To solve this, I created the **Flight Schedule Parser**. It's an automated system that takes schedule data (originally from an OCR scan of an image), standardizes all flight times into a Korean Standard Time(KST), and automatically populates a Google Calendar. This eliminated the manual work and allowed me to see her schedule clearly at a glance. ✈️

Beyond just viewing her schedule, new needs quickly arose from our daily routines. I often pick her up from the airport, so I desperately needed real-time updates on flight delays to time my arrival perfectly. At the same time, my girlfriend always needed to check the local weather and temperature at her destination to prepare for her trip.

To solve these practical challenges, the project eventually grew beyond simple scheduling. It now serves as a comprehensive assistant, providing real-time flight delay notifications and AI-powered weather briefings for her destinations.

<br/>

### Key Features

The system is composed of three core features that work together to create a seamless user experience.
From left to right: Flight schedules are automatically added to Google Calendar. Users receive instant notifications about flight delays. AI summarizes destination weather and suggests what to wear.

**0. Text Extraction from Image**
The process begins with a screenshot of the flight schedule. Using the iPhone's native Live Text (OCR) feature, the text is automatically recognized and extracted from the image. This raw text is then sent to the system's API endpoint via a POST request, kicking off the entire automation workflow.

**1. OCR Data Processing and Calendar Storage**
The system accepts OCR data from a flight ticket via a POST request. It parses key information like flight number, times, and destination, then automatically creates an event in the user's Google Calendar.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/projects/google_calendar_example.png" title="Automatic Calendar Sync" class="img-fluid rounded z-depth-1" style="max-width: 500px;" %}
</div>

**2. Real-time Flight Status Monitoring**
Using `APScheduler`, the system periodically checks upcoming flights in the calendar. It registers a monitoring job for each flight, using the `AviationStack` API to track its real-time status. If a delay is detected, it immediately sends a notification to the user via KakaoTalk and Discord.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/projects/flight_monitor_example.png" title="Real-time Delay Alerts" class="img-fluid rounded z-depth-1" style="max-width: 500px;" %}
</div>

**3. AI-Powered Weather Notifications**
For international flights, the system fetches weather forecasts for the destination city via `Openweather` API. This data is then passed to an `GPT5-mini` model, which generates a concise, easy-to-understand summary and recommends appropriate clothing.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.html path="assets/img/projects/weather_monitor_example.png" title="AI Weather Briefing" class="img-fluid rounded z-depth-1" style="max-width: 500px;" %}
</div>

---

### Technology Stack
* **Deployment:** `AWS EC2`
* **API Server:** `FastAPI`
* **Job Scheduling:** `APScheduler`
* **Data Storage:** `Google Calendar API`
* **Notifications:** `KakaoTalk`, `Discord API`
* **Flight Data:** `AviationStack`
* **AI Features:** `OpenAI`
* **Image Hosting:** `Imgbb`
* **Weather Information:** `OpenWeather`

The full source code is available on [GitHub](https://github.com/taeyoungson/flight-schedule-parser). 