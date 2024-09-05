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


