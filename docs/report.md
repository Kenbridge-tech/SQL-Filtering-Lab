# Executive Summary – SQL Filtering Lab

## Objective:
The goal of this lab was to practice using SQL filters to investigate login activity and prepare clean datasets for system updates. As a security analyst in training, I focused on writing queries that could uncover suspicious behavior while also supporting operational needs like account patching.

---

## Scope:

My analysis centered on five key areas:
- Tracking failed logins after business hours.
- Reviewing login activity on specific incident-related dates.
- Filtering out logins from Mexico to highlight foreign access attempts.
- Retrieving employee details from targeted departments for updates.
- Isolating non-IT employees to prioritize patch campaigns.

---

## Key Findings:

- 19 failed login attempts were detected after business hours, possible brute-force or unauthorized activity.
- 75 login attempts occurred across May 8–9, 2022 (the days under investigation).
- 144 login attempts originated from outside Mexico, raising red flags for potential remote access attempts.
- elarson was identified as a Marketing employee in the East building.
- lrodriqu appeared as the first Sales employee retrieved.
- 161 employees were found outside of IT and therefore still required patching.

---

## Conclusion:

This lab showed how basic SQL filters can quickly turn raw data into actionable insights. By writing simple queries, I was able to:
- Detect login activity worth further investigation.
- Identify employees who needed targeted system updates.
- Exclude irrelevant data so reports were clean and accurate.

For me, this exercise reinforced the value of SQL in day-to-day cybersecurity operations. It also gave me confidence in using databases as part of an investigation workflow, a skill I look forward to applying in real SOC environments.
