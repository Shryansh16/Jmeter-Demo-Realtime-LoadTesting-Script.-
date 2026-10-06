# JMeter Demo Realtime LoadTesting Script

This repository contains a JMeter performance testing script (`Demo_realTime_User-Jmeter-Script.jmx`) configured for high-load simulation (20,000 concurrent users). Real-time data has been removed from the API payloads to make it safe for sharing.

## Test Plan Structure

The script follows the exact structure below, mimicking real-world user activity distributions using Throughput Controllers.

```text
Test Plan
└── NBcc_20000_user_Login (bzm - Concurrency Thread Group)
    ├── CSV Data Set Config
    ├── Uniform Random Timer
    ├── NBCC_20000_User_Login (HTTP Request)
    │   ├── JSON Extractor
    │   ├── NBcc_Header Manager
    │   ├── View Results Tree
    │   └── Aggregate Report
    ├── Debug Sampler
    │   └── View Results Tree
    └── Loop Controller
        ├── HTTP Header Manager
        ├── News_Throughput Controller
        │   ├── Random Controller
        │   ├── Uniform Random Timer
        │   ├── news_tab
        │   ├── Like_news.
        │   └── news_Got_it
        ├── Polls_Throughput Controller
        │   ├── Random Controller
        │   ├── Uniform Random Timer
        │   ├── poll
        │   └── poll_submit 
        ├── Question_of_the_day_Throughput Controller
        │   ├── Question_of_the_day.
        │   └── Submit_questionOftheDay
        ├── Question_of_the_day_(no_activity perform).Throughput Controller
        │   └── Question_of_the_day.
        ├── Polls_Throughput Controller_(no-activity-perform)
        │   ├── Random Controller_(no-activity-perform)
        │   ├── Uniform Random Timer
        │   └── poll
        └── News_Throughput Controller (no activity perform)
            ├── Random Controller
            ├── Uniform Random Timer
            └── news_tab
```

## Details & Components Used

- **bzm - Concurrency Thread Group**: Configured to reach a target concurrency of 1,000 users with a 20-second ramp-up time (in 10 steps), holding the target rate for 300 seconds.
- **CSV Data Set Config**: Reads user credentials (emails) to authenticate.
- **JSON Extractor**: Grabs the `access_token` from the login response to pass in the HTTP Header Manager for subsequent requests.
- **Throughput Controllers**: 
  - Splits the user traffic logically into performing activities (reading/liking news, submitting polls, answering questions of the day) and just viewing (no activity perform paths).
- **Uniform Random Timers**: Added between requests to simulate human think time.

## How to Run

1. Open JMeter GUI (requires **JMeter Plugins Manager** with `bzm - Concurrency Thread Group` installed).
2. Open `Demo_realTime_User-Jmeter-Script.jmx`.
3. Provide your own valid `.csv` data file in the `CSV Data Set Config`.
4. Run in Non-GUI mode for best performance.
