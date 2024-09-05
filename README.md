# ELK-SOC-Tips
A comprehensive guide for SOC analysts on using the ELK Stack (Elasticsearch, Logstash, Kibana) for log analysis and incident response. This repository includes tips, tricks, and best practices for visualizing, searching, and alerting on security events in Kibana, tailored to enhance SOC efficiency.

# 1. Using the Kibana Discover Tab for Quick Log Searches 
### What It Is:
The **Discover** tab in Kibana is where you can view raw log data, search for specific events, and filter your data.
### Tip:
When investigating an alert (like a failed login attempt or an abnormal network connection), use **Discover** to quickly filter and locate relevant log entries.
### How to Use:
1. **Navigate to the Discover tab** in Kibana.
2. Use the search bar to filter by specific event codes or keywords.
- For example, to investigate Windows failed login attempts (EventCode 4625), type:
```
event.code: 4625
```
3. **Adjust the time range** to look at logs around the time of the alert (e.g., last 15 minutes or 1 hour).
4. Use **Add a Filter** to filter for specific hosts, usernames, or IP addresses.
### What to Look For:
- **Repeated failed login attempts** from the same source (potential brute-force attack).
- **Failed login attempts** followed by successful ones from the same IP (indicating potential credential compromise).
- **Unusual login hours** or geographic locations (anomalous behavior).
### Trick:
Use **field search filters** like `@timestamp` to narrow down the exact moment an event occurred. This is useful for correlating logs between different systems (e.g., firewall logs and system logs).

# 2. Visualizing Patterns with the Kibana Dashboard
### What It Is:
The **Dashboard** tab in Kibana lets you create interactive visualizations, such as bar charts, pie charts, and time series. These help SOC analysts track patterns like login attempts, network traffic anomalies, or suspicious IP activity.
### Tip:
Create **custom dashboards** for specific SOC use cases. For example, set up a dashboard to track **user login activity** or **network traffic** spikes.
### How to Use:
1. **Go to the Dashboard tab** and click **Create new dashboard**
2. Add a **Visualization** (e.g., bar chart, pie chart).
- Choose “**Create Visualization**” -> “**Vertical Bar**” to show failed login attempts by user or source IP.
3. Set the **Y-axis** to show the count of events, and use the **X-axis** to show the relevant field (e.g., source IP or username).
4. Save the dashboard and adjust it to track ongoing alerts.
### What to Look For:
- **Sudden spikes in login attempts** from certain IPs (indicating brute-force attacks).
- **Unusual countries** in the geographic location of incoming connections.
- **Network traffic spikes** outside normal operating hours.
### Trick:
Set up **auto-refresh** on your dashboard (using the timer in the upper right) to keep monitoring in real-time during incidents. You can adjust the refresh interval to as little as 5 seconds.

# 3. Advanced Search and Filtering in Elasticsearch
### What It Is:
You can use **advanced search queries** in the Kibana search bar to filter specific logs. These queries allow you to fine-tune searches for suspicious events, such as failed logins, network connections from unusual locations, or specific error codes.
### Tip:
Use **boolean operators** (`ND`, `OR`, `NOT`) and wildcards (`*`) in the search bar to refine your searches.
### How to Use:
1. **Navigate to Discover** and enter queries directly in the search bar.
2. Use **boolean operators** to combine conditions. For example:
- Search for **failed logins** and exclude known legitimate IPs:
```
event.code: 4625 AND NOT src_ip: "192.168.1.100"
```
3. Use **wildcards** to search for partial matches. For example, to search for any IP starting with `10.0.`:
```
src_ip: 10.0.*
```
### What to Look For:
- **Event combinations**: Failed logins (Event 4625) followed by account lockouts (Event 4740) may indicate an ongoing brute-force attack.
- **Anomalies**: Search for rare occurrences of certain log entries or user behaviors, such as an admin logging in from an unusual IP.
### Trick:
You can **save search queries** to quickly reuse them in future investigations. Click Save Search in the Discover tab after crafting a useful query.

# 4. Using Visual Alerts to Catch Unusual Activity
### What It Is:
Kibana allows you to **create alerts** based on predefined thresholds or unusual patterns. These alerts notify SOC teams via email or other channels when certain log patterns occur.
### Tip:
Set up **visual alerts** to trigger when there’s an unusual volume of events, such as a high number of failed login attempts, network traffic spikes, or repeated access from suspicious IP addresses.
### How to Use:
1. **Go to Stack Management -> Alerting -> Create new alert.**
2. Set a trigger condition. For example, an alert when there are more than 10 failed login attempts in a 5-minute window.
3. Set the action to **email** or integrate with a tool like **Slack** to notify your team immediately.
### What to Look For:
- **Burst of failed login attempts** (greater than X in Y minutes).
- **Network activity from blacklisted IPs.**
- **Repeated attempts to access restricted URLs or services.**
### Trick:
Use **anomaly detection** in **Elastic Machine Learning** to create alerts for traffic patterns or behaviors that deviate significantly from the norm. This can help detect zero-day attacks or insider threats.

# 5. Geolocation Insights with Kibana Maps
### What It Is:
Kibana’s **Maps** tool allows you to visualize network traffic, events, or log data based on geographic locations. This is especially useful for tracking network connections to and from specific countries.
### Tip:
Use **GeoIP fields** to map traffic and identify unusual countries or regions where traffic originates.
### How to Use:
1. **Go to the Maps tab** and create a new map.
2. Use GeoIP fields to plot the location of source IPs.
- For example, map **incoming SSH connections** to see where login attempts are originating.
3. Set up filters to highlight traffic from certain regions.
### What to Look For:
- **Unusual geolocation activity**: Connections from countries your company normally doesn’t interact with.
- **Clusters of malicious IPs**: Identify potential botnets or command-and-control servers by mapping suspicious connections.
### Trick:
Add **heatmaps** to your geolocation dashboard to visualize the density of incoming connections, making it easy to spot where most traffic is coming from.

# 6. Creating Incident Response Workflows in Kibana
### What It Is:
Kibana allows you to create **incident response dashboards** that guide SOC analysts through investigating alerts. These dashboards combine different visualizations to track multiple aspects of an ongoing incident.
### Tip:
Set up a dedicated **Incident Response Dashboard** for your SOC. This dashboard can include sections for log search, IP address investigation, and alert monitoring.
### How to Use:
1. **Create a new dashboard** and add the following visualizations:
- A **line chart** for event frequency over time (useful for detecting attack spikes).
- A **table** that lists all failed login attempts or other critical event codes.
- A **geolocation map** for tracking IP origin.
2. Use this dashboard during incident response to track alerts in real-time.
### What to Look For:
- **Patterns** of repeated events across different systems (e.g., failed logins on multiple hosts).
- **IP addresses** that repeatedly trigger different types of alerts.
### Trick:
Create **shortcuts to saved searches** on your incident response dashboard, allowing you to quickly investigate certain behaviors without needing to manually retype search queries.

# 7. Using the Kibana Lens for Simplified Visualizations
### What It Is:
**Kibana Lens** is a powerful, yet user-friendly, visualization tool that allows SOC analysts to quickly create visualizations using drag-and-drop functionality. It simplifies the process of creating complex charts and graphs compared to traditional methods in Kibana.
### Tip:
Use **Kibana Lens** to create interactive visualizations with minimal effort. It’s especially useful for exploring data dynamically without needing to configure individual metrics or buckets manually.
### How to Use:
1. **Go to the Lens tab** from the Visualize menu
2. **Drag and drop fields** into the workspace to generate visualizations automatically.
- Example: Drag `@timestamp` to the X-axis and `event.code` to the Y-axis to see a time-based breakdown of specific events.
3. **Switch between chart types** (bar chart, line chart, pie chart) easily by selecting the options at the top.
### What to Look For:
- Quickly identify **spikes** or **anomalies** by visualizing large log datasets.
- **Explore relationships** between different fields, such as user behavior and network traffic, without complex configuration.
### Trick:
Use **filters directly in Lens** to quickly apply conditions without going back to Discover. This is particularly useful when you want to adjust what data you’re viewing on the fly (e.g., focusing on failed login attempts during a specific time range).

# 8. Custom Time Ranges for Deep Dive Investigations
### What It Is:
Custom time ranges in Kibana allow you to **drill down into specific periods** of interest when investigating an incident.
### Tip:
When responding to an alert or investigating an event, use **custom time ranges** to zoom in on the exact time of suspicious activity. This helps to avoid sifting through unnecessary logs and focus on the time-sensitive data.
### How to Use:
1. Click the time range selector in the top-right corner of Kibana.
2. Choose predefined ranges like "Last 15 minutes" or select **Custom** to specify exact times.
3. Use **Absolute time** to zero in on a specific minute or second.
### What to Look For:
- **Precursor events** leading up to the alert (e.g., suspicious user activity right before a login failure).
- **Subsequent behavior** after an alert (e.g., abnormal network traffic immediately after a failed login).
### Trick:
If you’re unsure when exactly the suspicious activity started, use the **“Quick” time range** option and expand gradually. Start with the last 15 minutes and increase to 1 hour or 24 hours to spot the initial compromise or unusual behavior.

# 9. Creating Alerts Based on Multiple Conditions (Chained Alerts)
### What It Is:
Chained alerts trigger when multiple conditions are met. This allows for more advanced alerting based on combinations of events.
### Tip:
Create **chained alerts** to reduce false positives by requiring multiple triggers before sending a notification. For example, you can alert only when there are both failed login attempts and unusual network traffic from the same IP within a short period.
### How to Use:
1. **Set up multiple conditions** in the alerting tool (via the Kibana Alerting tab).
2. Use **logical operators** to link events (e.g., `AND` for both conditions to be true).
3. Specify actions to be triggered (e.g., email the SOC team or send to Slack).
### What to Look For:
- **Combining suspicious behaviors** to minimize false positives. For example, only alert when failed login attempts coincide with abnormal data exfiltration
- **Multi-stage attack patterns**, where attackers first try multiple login attempts and then perform data access or transfer.
### Trick:
Use **Elasticsearch Watcher** to combine multiple conditions and trigger alerts based on sophisticated criteria. You can chain together alerts that involve different fields and log types, such as failed logins and unusual outbound connections.

# 10. Contextual Data Enrichment with Logstash
### What It Is:
Data enrichment in Logstash enhances log data by adding contextual information. This can include **GeoIP data, threat intelligence**, or user role information, making log analysis more insightful.
### Tip:
Use **Logstash enrichment filters** to automatically add data like **GeoIP** (for identifying the location of IP addresses) or **threat intelligence** (to tag suspicious IPs based on external sources like MISP). This enriched data makes it easier to spot high-risk activity.
### How to Use:
1. Configure **Logstash filters** to enrich incoming logs with additional information.
2. Common enrichments include:
- **GeoIP** for geographic data.
- **Threat intelligence** feeds for known malicious IPs or domains.
- **User role lookup** to cross-reference with internal user databases.
### What to Look For:
- **Suspicious IP addresses** from high-risk regions or blacklisted domains.
- **User accounts** that show unusual behavior, especially if they’re marked as having high privileges.
### Trick:
Set up **auto-tagging** in Kibana to flag logs with enriched data, making it easier to filter for high-risk activity (e.g., automatically tag traffic from specific geographic regions for closer investigation).

# 11. Using Kibana Query Language (KQL) for Complex Filters
### What It Is:
**Kibana Query Language (KQL)** is a powerful tool that allows you to search and filter logs using advanced syntax. It’s a more intuitive alternative to traditional Elasticsearch queries.
### Tip:
Use **KQL** for complex searches that involve multiple fields or conditions. KQL is especially useful for investigating large datasets and creating precise filters without needing to know Elasticsearch's full query syntax.
### How to Use:
1. In the **Discover tab**, use the search bar to enter KQL expressions.
2. KQL allows for more natural language searches. For example:
- To search for failed logins with a specific username:
```
event.code: 4625 AND user.name: "john_doe"
```
3. Use **wildcards** and **boolean logic** to expand or narrow down your results as needed.
### What to Look For:
- **Multiple event codes** across different time periods (e.g., failed logins followed by successful ones).
- **Behavioral patterns** that span multiple fields, like user access across different systems.
### Trick:
You can save KQL queries in **Discover** and reuse them later. This is handy when you’re frequently searching for specific patterns or behaviors (e.g., a certain user account’s activity over time).

# 12. Tracking User Behavior Over Time
### What It Is:
Monitoring user behavior in Kibana helps detect **insider threats, compromised credentials**, and **suspicious account activity**. This is especially critical for high-privilege accounts.
### Tip:
Set up visualizations that track specific users’ actions across different systems and over time. Look for changes in behavior, such as access during unusual hours, attempts to access restricted files, or sudden spikes in activity.
### How to Use:
1. **Create a time-based visualization** that shows the number of actions by a specific user (e.g., login attempts, data access) over time.
2. Use **filters** to focus on key users, such as administrators or users with access to sensitive data.
3. Set up **alerts** to notify the SOC when abnormal activity is detected (e.g., a spike in file access by an administrator during off-hours).
### What to Look For:
- **Unusual login hours** or IP addresses not associated with the user’s typical behavior.
- **Spike in activity**, such as large file transfers or access to systems that the user doesn’t normally interact with.
### Trick:
Set up **trend analysis** to detect gradual increases in activity over time, which might indicate compromised credentials being tested for higher privileges.

