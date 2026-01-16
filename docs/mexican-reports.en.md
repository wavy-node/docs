---
title: Mexican Compliance Reports
description: Understand the automated compliance reports for Mexican financial authorities.
---

# Mexican Compliance Reports

WavyNode helps you automatically comply with Mexico's anti-money laundering regulations (LFPIORPI). Our platform generates the specific reports required by the Financial Intelligence Unit (UIF) for activities involving virtual assets.

## What to Expect

*   **Automatic Monthly Reports**: At the beginning of each month, WavyNode automatically generates official XML reports covering the previous month's activity. The process is fully automated, requiring no manual action from you.

*   **Threshold-Based Reporting**: Reports are created only for your end-users whose transaction volume during the month meets or exceeds the official threshold set by the authorities (based on the UMA value).

*   **Email Notifications**: As soon as the reports for your project are ready, your designated administrators will receive an email notification.

*   **Secure Downloads**: You can securely download the generated XML reports directly from your WavyNode dashboard. These files are ready for you to present to the corresponding authorities.

## Your Responsibilities

For the reports to be generated correctly, WavyNode relies on the user data you provide through your integration.

It is crucial that your `GET /users/:userId` endpoint is always active and provides accurate, up-to-date information for your users. This data is used to populate the required fields in the official reports, ensuring their validity and compliance.
