# SSH Log Analysis and Attack Detection using Splunk

## Project Overview
This project demonstrates how security analysts can use Splunk to analyze SSH authentication logs and detect suspicious login activity. The goal of the project is to identify potential brute-force attacks, suspicious IP addresses, and abnormal login patterns using Splunk's Search Processing Language (SPL) and dashboards.

The project simulates a basic Security Operations Center (SOC) investigation workflow where logs are ingested into Splunk, analyzed using SPL queries, and visualized through dashboards.

---

## Project Architecture

The architecture of this project follows a simple log analysis pipeline:

SSH Log Dataset → Splunk Data Ingestion → SPL Query Analysis → Dashboard Visualization → Attack Detection

1. SSH authentication logs are collected from a server.
2. Logs are uploaded and indexed in Splunk.
3. SPL queries are used to analyze login activity.
4. Results are visualized using dashboards.
5. Suspicious login patterns and attacks are detected.

---

## Dataset

The dataset used in this project consists of SSH authentication logs that simulate login activity on a Linux server.

Each log entry contains information such as:

- Timestamp
- Username
- Source IP address
- Login status (success or failure)
- SSH port and protocol

Example log entry:

Mar 10 14:22:31 server sshd[1023]: Failed password for root from 192.168.1.25 port 54321 ssh2

These logs help identify failed login attempts and potential brute-force attacks.

The dataset is included in this repository as:

ssh_logs_new.zip

---

## Splunk Queries (SPL)

### 1. Top Source IP Addresses

This query identifies the IP addresses generating the most login attempts:

sourcetype=ssh_logs
| stats count by src_ip
| sort -count
---

### 2. Failed Login Attempts

This query helps identify IPs attempting multiple failed logins:

sourcetype=ssh_logs action=failure
| stats count by src_ip
| sort -count
---

### 3. Login Activity Over Time

This query visualizes login activity trends:

sourcetype=ssh_logs
| timechart count by action
---

### 4. User Login Attempts

This query shows which usernames are targeted most often:

sourcetype=ssh_logs
| stats count by user
| sort -count

---

## Dashboard

A Splunk dashboard was created to visualize security insights from the SSH logs.

The dashboard includes:

- Login success vs failure chart
- Top attacking IP addresses
- Login activity over time
- Most targeted usernames

The dashboard configuration is stored in this repository as:

dashboard.json

---

## Attack Detection

Using the queries and dashboard, the following attack patterns can be detected:

### Brute Force Attack
Multiple failed login attempts from the same IP address targeting a user account.

Detection Query:

sourcetype=ssh_logs action=failure
| stats count by src_ip
| where count > 5

---

### Suspicious Login Activity

Unusual login spikes within short time periods.

Detection Query:
sourcetype=ssh_logs
| timechart count

---

### Targeted Username Attacks

Attackers repeatedly attempting to access privileged accounts such as root.

Detection Query:

sourcetype=ssh_logs
| stats count by user
| sort -count

---

## Screenshots

### Splunk Dashboard

![Dashboard](screenshots/dashboard.png)

### Login Analysis

![Login Analysis](screenshots/login_activity.png)

---

## Tools Used

- Splunk Enterprise
- Search Processing Language (SPL)
- Linux SSH Authentication Logs

---

## Skills Demonstrated

- Log analysis
- Threat detection
- Security monitoring
- SPL query development
- Security dashboard creation
- SOC investigation workflow

---

## Conclusion

This project demonstrates how Splunk can be used by SOC analysts to analyze authentication logs and detect potential security threats such as brute-force attacks. By combining log ingestion, SPL queries, and dashboard visualization, security teams can quickly identify suspicious behavior and respond to incidents.


