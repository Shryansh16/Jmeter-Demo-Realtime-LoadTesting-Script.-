# JMeter Real-Time User Script Demo

This repository contains a performance testing script built with [Apache JMeter](https://jmeter.apache.org/). The script is designed to simulate a high-load, real-time user environment by running a concurrent load test with 20,000 users.

**Note**: All sensitive real-time data, API endpoints, and hostnames have been anonymized or removed to keep the script safe for public sharing on GitHub.

## Test Plan Overview

- **Test Plan Name**: `Nbcc_BACKEND_20000_USER_NON_GUI_TEST`
- **Thread Group**: Concurrency Thread Group (`NBcc_20000_user_Login`)
  - Target Concurrency: 1000
  - Ramp-Up Time: 20 seconds
  - Ramp-Up Steps: 10
  - Hold Target Rate Time: 300 seconds

## Features & Workflows Included

The test simulates typical user behavior using **Throughput Controllers** to distribute load among different application features proportionally:

1. **User Login & Authentication**: 
   - Reads user credentials (emails) from a CSV file (`backend_users_20000.csv`).
   - Performs a user login (`POST`).
   - Extracts the `access_token` from the response using a JSON Extractor.
   - Passes the token in the `Authorization: Bearer ${access_token}` header for all subsequent requests.

2. **News Feed (35% Throughput)**: 
   - View `news_tab` (`GET`)
   - Like news (`POST` with a `LIKE` reaction)
   - Acknowledge news (`news_Got_it`)

3. **Polls (25% Throughput)**:
   - View polls (`GET`)
   - Submit poll option (`POST`)

4. **Question of the Day (25% Throughput)**:
   - View Question of the Day (`GET`)
   - Submit Answer (`POST`)

## Prerequisites

- **Apache JMeter 5.6.3** or higher.
- **JMeter Plugins Manager**: Requires plugins for `bzm - Concurrency Thread Group` (Custom Thread Groups).
- A valid CSV dataset with user data (e.g., `email`) to feed the `CSV Data Set Config`.

## How to Run

1. Open JMeter GUI.
2. Load the `Demo_realTime_User-Jmeter-Script.jmx` file.
3. Update the `CSV Data Set Config` file path to point to your local dataset file (currently points to `C:/Users/LENOVO/Downloads/Load_test_csv_file/backend_users_20000.csv`).
4. Update the HTTP Request samplers with your target application's Server Name / IP and Paths.
5. For high load, it is highly recommended to run the test in **Non-GUI mode**:
   ```bash
   jmeter -n -t Demo_realTime_User-Jmeter-Script.jmx -l results.csv -e -o /path/to/html/report
   ```
