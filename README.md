**Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab**

**Overview**

This lab was designed to simulate the daily responsibilities of a Tier 1 Security Operations Center (SOC) Analyst using Splunk Enterprise. The project consists of twenty security event datasets representing common enterprise security telemetry sources, including authentication logs, firewall events, web server activity, DNS queries, VPN access logs, Windows security events, endpoint security alerts, and network traffic data.

To create a realistic SOC environment, all datasets were custom-generated using Python scripts. The Python-based log generator was used to produce structured security events that simulate real-world enterprise activity and common security monitoring scenarios. Generating the datasets programmatically provided complete control over the event content while ensuring that each dataset contained sufficient data for meaningful Splunk analysis and investigation.

After generating the datasets, each log file was ingested into Splunk Enterprise and assigned to its appropriate index based on the security domain it represented. The datasets were then analyzed using Splunk Search Processing Language (SPL) to perform statistical investigations and event analysis.

The primary objective of this lab is to develop practical experience with log ingestion, indexing, event monitoring, and statistical analysis using Splunk Enterprise. Unlike dashboard-focused projects, this lab intentionally emphasizes raw event analysis and SPL-based investigations. No dashboards, visualizations, panels, reports, alerts, lookups, or correlation searches were used. Instead, the focus remained on understanding security events, identifying patterns and trends, and developing the search and analysis skills commonly used by Tier 1 SOC analysts during daily monitoring and triage activities.

For example, the Failed Login Attempts dataset was ingested into the authentication index and analyzed using SPL to count failed password attempts by source IP address and user account. Similar statistical investigations were performed across all twenty security event datasets, allowing the analyst to practice working with multiple security data sources commonly found in enterprise environments.\
\
**\
Security Event Categories**

This lab covers the following twenty security monitoring scenarios:

1.  Failed Login Attempts

2.  Successful Logins

3.  Top Source IP Addresses

4.  Top Destination IP Addresses

5.  Firewall Blocked Traffic

6.  Firewall Allowed Traffic

7.  HTTP 404 Errors

8.  HTTP 500 Errors

9.  DNS Query Monitoring

10. VPN Login Activity

11. After-Hours Login Detection

12. New User Account Creation

13. User Account Lockouts

14. Disabled Account Usage

15. Antivirus Detections

16. Top Talkers by Bandwidth

17. Critical Security Events

18. Most Active Hosts

19. Login Failures by User

20. Event Counts by Sourcetype

**Skills Practiced**

Throughout this lab, the following skills were developed and reinforced:

- Python log generation and dataset creation

- Security data engineering fundamentals

- Splunk Enterprise administration

- Log ingestion and indexing

- Search Processing Language (SPL)

- Statistical analysis using SPL commands

- Security event monitoring

- Authentication analysis

- Firewall traffic analysis

- DNS activity investigation

- VPN monitoring and access analysis

- Windows security event analysis

- Endpoint security monitoring

- Network traffic analysis

- Security operations workflows

- Event triage and investigation

- Threat detection fundamentals

- SOC Tier 1 analyst methodologies

- Security telemetry analysis

- Log source management and organization

**Outcome**

Upon completion of this lab, the analyst gained hands-on experience creating realistic security datasets using Python, ingesting multiple log sources into Splunk Enterprise, and performing statistical investigations using SPL. The project provided practical exposure to authentication monitoring, firewall analysis, web log investigation, DNS monitoring, VPN activity analysis, Windows security events, endpoint security alerts, and network traffic analysis.

By working directly with raw security events rather than dashboards or visualizations, the lab strengthened the analyst's ability to interpret log data, identify security-relevant activity, and perform the types of investigations commonly conducted within a Security Operations Center. The completed project serves as a foundational SOC monitoring portfolio piece and prepares the analyst for more advanced Tier 2 activities such as threat hunting, incident response, malware analysis, persistence detection, and adversary behavior investigations.\
\
**1. Python script file** (**generate_tier1_soc_datasets.py)**

**2. Terminal execution showing the datasets being created**

**3. The folder containing the 20 generated datasets**

**4. Splunk ingestion**\
**5. Splunk searches and results**\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\

| **\#** | **Dataset File**           | **Splunk Index** |
|--------|----------------------------|------------------|
| 1      | Failed_logins.log          | auth             |
| 2      | successful_logins.log      | auth             |
| 3      | top_source_ips.log         | network          |
| 4      | top_destination_ips.log    | network          |
| 5      | firewall_blocked.log       | firewall         |
| 6      | Firewall_allowed.log       | Firewall         |
| 7      | http_404.log               | web              |
| 8      | http_500.log               | web              |
| 9      | dns_queries.log            | dns              |
| 10     | vpn_logins.log             | vpn              |
| 11     | after_hours_logins.log     | auth             |
| 12     | new_users.log              | windows          |
| 13     | account_lockouts.log       | windows          |
| 14     | disabled_account_usage.log | windows          |
| 15     | antivirus_detections.log   | endpoint         |
| 16     | bandwidth_top_talkers.log  | network          |
| 17     | critical_events.log        | security         |
| 18     | active_hosts.log           | security         |
| 19     | login_failures_by_user.log | auth             |
| 20     | sourcetype_count.log       | security         |

**Note:** This repository contains one security event investigation from the Splunk Enterprise Tier 1 SOC 20 Security Event Monitoring Lab series. For the remaining security event investigations, please refer to the corresponding repositories in this project collection.

**Successful Logins Event**

Dataset: successful_logins.log**\**
Index: auth**\**
Sourcetype: successful_logins

This dataset represents **successful authentication activity**. Unlike failed logins, we're looking at who successfully gained access to the system\
\
\
\
1. Confirm Dataset Ingestion**

index=auth sourcetype=successful_logins\
\| stats count by source index sourcetype

Purpose: Verify that the dataset was successfully ingested into Splunk.


**2. View Raw Successful Login Events**

index=auth sourcetype=successful_logins\
\| table \_time host user src_ip action message

Purpose: Review all successful authentication events.

**.** Who logged in? Refer to the user field.

**.** From which IP? Refer to the src_ip field.\
\
**.** To which host? Refer to the host field.\
\
**.** When? Refer to the \_time field.\
\
Five user accounts (admin, john, mary, alice, and svc_backup) successfully authenticated to the host MacBookPro. The successful logins originated from six source IP addresses (10.0.0.10, 10.0.0.15, 10.0.0.20, 88.88.88.88, 123.123.123.123, and 192.168.1.50). The authentication events occurred between 08:00:00 and 08:21:00 on June 3, 2026. All login attempts were accepted and resulted in successful access to the target system.\
\


**\
3. Successful Logins by User**

index=auth sourcetype=successful_logins\
\| stats count by user\
\| sort -count

Purpose: Identify which users logged in most frequently.

**.** Which accounts are most active?

| **user** | **count** |
|----------|-----------|

| mary | 8   |
|------|-----|

| admin | 7   |
|-------|-----|

| svc_backup | 6   |
|------------|-----|

| john | 5   |
|------|-----|

| alice | 4   |
|-------|-----|

The **mary** and **admin** accounts were the most active users in the dataset, generating the highest number of successful login events. This indicates that these accounts authenticated more frequently than the other user accounts during the observed time period.

\
\
4. Successful Logins by Source IP**

index=auth sourcetype=successful_logins\
\| stats count by src_ip\
\| sort -count

Purpose: Identify the IP addresses generating successful authentications.

**.** What IP address are successfully authenticating most often?\
\
The source IP address **10.0.0.20** was the most active authentication source, generating **8 successful login events**.\
Also Refer to the image \# 9 in the repository for the complete list of source IP addresses and their login counts.\
\

**\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
5. Successful Logins by User and Source IP**

index=auth sourcetype=successful_logins\
\| stats count by user src_ip\
\| sort -count

Purpose: Determine which users are authenticating from which IP addresses.

**.** Which users authenticated from multiple source IP addresses?\
\
Several users authenticated from multiple source IP addresses. For example, **svc_backup** successfully authenticated from 10.0.0.10, 10.0.0.15, 123.123.123.123, and 192.168.1.50. Similarly, **admin**, **john**, **mary**, and **alice** also authenticated from more than one source IP address.\
\

\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
6. Successful Logins by Host**

index=auth sourcetype=successful_logins\
\| stats count by host\
\| sort -count

Purpose: Identify which hosts receive the most successful logins.\
\
**.** Which host received the highest number of successful login events?\
\
The host **MacBookPro** received **35 successful login events**, making it the most frequently accessed host in the dataset.


**\
\
\
\
\
\
\
\
\
\
\
\
\
\
7. Unique Source IPs Per User**

index=auth sourcetype=successful_logins\
\| stats dc(src_ip) as unique_ips values(src_ip) as source_ips by user\
\| sort -unique_ips

Purpose: Identify users authenticating from multiple IP addresses.\
\
**.** Which user accounts were associated with the greatest number of unique source IP addresses?\
\
The accounts **mary** and **svc_backup** authenticated from the highest number of unique source IP addresses, each using **5 different source IPs**. The **admin** and **john** accounts authenticated from **4 unique source IPs**, **alice** from **3**, and **bob** from **2**.


\
\
\
\
\
\
\
\
\
\
\
\
\
\
8. Unique Users Per Source IP**

index=auth sourcetype=successful_logins\
\| stats dc(user) as unique_users values(user) as users by src_ip\
\| sort -unique_users

Purpose: Identify systems authenticating as multiple users.

**.** Which source IP addresses were associated with multiple user accounts?\
\
Five source IP addresses (10.0.0.10, 10.0.0.15, 10.0.0.20, 123.123.123.123, and 88.88.88.88) were each associated with **4 unique user accounts**. Source IP address 192.168.1.50 was associated with **3 unique user accounts**.

\
\**
\
9. Successful Logins Over Time**

index=auth sourcetype=successful_logins\
\| timechart span=10m count

Purpose: Monitor successful authentication trends.

**.** Did login activity spike during a particular period?\
\
No significant spike is visible.

| **Time Period** | **Login Count** |
|-----------------|-----------------|

| 08:00 | 10  |
|-------|-----|

| 08:10 | 10  |
|-------|-----|

| 08:20 | 10  |
|-------|-----|

| 08:30 | 5   |
|-------|-----|

Successful login activity remained stable throughout most of the observation period. Three consecutive 10-minute intervals each recorded 10 successful login events, followed by a decrease to 5 events during the final interval. No unusual spike in authentication activity was identified.

10. SOC Executive Summary Query**

index=auth sourcetype=successful_logins\
\| stats count as total_logins dc(user) as unique_users dc(src_ip) as unique_source_ips dc(host) as unique_hosts

Purpose: Provide a quick summary of authentication activity.


**SOC Analyst Interpretation**

Unlike the failed login dataset:

failed_logins.log

where source IPs may represent attack attempts,

the successful login dataset shows:

User\
↓\
Successfully authenticated\
↓\
Access granted

#### \
. How many successful logins occurred?

A total of 35 successful login events were recorded.

**.** How many unique user accounts successfully authenticated?

6 unique user accounts successfully authenticated.

**.** How many unique source IP addresses generated successful logins?

Successful logins originated from 6 unique source IP addresses.

**.** How many hosts received successful logins?

Successful logins were recorded on 1 host.

**.** What is the overall summary of successful authentication activity in the dataset?

The dataset contains **35 successful login events** involving **6 unique user accounts**, **6 unique source IP addresses**, and **1 host**. This provides a high-level summary of authentication activity during the observation period.

**\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\**
