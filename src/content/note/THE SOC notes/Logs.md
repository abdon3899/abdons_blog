---
title: "Logs"
description: "My personal SOC notes and research on Logs."
publishDate: "2026-04-19T00:00:00Z"
tags: ["SOC"]
coverImage: "./Gifs/logs.gif"

---

# Logs

essential records that document events and activities within digital systems. They are a historical archive, providing critical insights that help in identifying patterns, mitigating threats, and enhancing overall security. 

A log is essentially a record of events within a system. It captures a wide range of activities, such as:

User logins, File accesses, System errors, Network connections, Changes to data or system configurations

**a typical log entry includes the following components:**

1. **Timestamp**: The date and time when the event occurred.
2. **System/Application Name**: The name of the system or application that generated the log entry.
3. **Event Type**: The nature of the event (e.g., login attempt, error, file access).
4. **Event Details**: Additional information about the event, such as: The user , The IP address and The outcome.

**Contextual Correlation:**

While a single log entry may seem insignificant on its own, the true power of logs lies in their **contextual correlation**. When log data is aggregated, analyzed, and cross-referenced with other sources of information, it becomes a powerful tool for investigation. Logs can answer critical questions about an event, such as:

- **What happened?**
- **When did it happen?**
- **Where did it happen?**
- **Who is responsible?**
- **What was the result of their action?**

## Log Types:

| Log Type | Purpose | Key Contents |
| --- | --- | --- |
| Application | Monitor specific applications | Status, errors, warnings, performance metrics |
| Audit | Track operational procedures for compliance | User actions, administrative activities, system changes |
| Security | Record security-related events | Logins, permission changes, firewall activity, intrusion alerts |
| Server | Capture server activities | System, event, error, and access logs |
| System | Record OS and hardware activities | Kernel messages, system errors, boot sequences, hardware status |
| Network | Track network traffic and connections | Connections, traffic data, firewall logs, IDS/IPS alerts |
| Database | Monitor database activities | Queries, transactions, user access, errors |
| Web Server | Track web server requests | URLs, response codes, client IPs, user agents |

## Log Formats:

### **Semi-structured Logs:**

These logs may contain structured and unstructured data, with predictable components accommodating free-form making them flexible yet somewhat organized. 

**Syslog Message Format:**

 A widely adopted logging protocol for system and network logs.

**`May 31 12:34:56 WEBSRV-02 CRON[2342593]: (root) CMD ([ -x /etc/init.d/anacron ] && if [ ! -d /run/systemd/system ]; then /usr/sbin/invoke-rc.d anacron start >/dev/null; fi)`**

**Windows Event Log (EVTX) Format:** 

Proprietary Microsoft log for Windows systems.

**`ProviderName: Microsoft-Windows-Security-SPP
TimeCreated                      Id LevelDisplayName Message
-----------                      -- ---------------- -------
31/05/2023 17:18:24           16384 Information      Successfully scheduled Protection service for re-start
31/05/2023 17:17:53           16394 Information      Offline downlevel migration succeeded.`**

### **Structured Logs:**

Following a strict and standardized format, these logs are conducive to parsing and analysis organized into well-defined fields, enabling efficient querying, filtering, and aggregation. 

**Field Delimited Formats:**

Comma-Separated Values (CSV) and Tab-Separated Values (TSV) are formats often used for tabular data

**`"time","user","action","status","ip","uri "
"2023-05-31T12:34:56Z","adversary","GET",200,"34.253.159.159","http://gitlab.swiftspend.finance:80/"`**

**JavaScript Object Notation (JSON):** 

Known for its readability and compatibility with modern programming languages.

**`{"time": "2023-05-31T12:34:56Z", "user": "adversary", "action": "GET", "status": 200, "ip": "34.253.159.159", "uri": "http://gitlab.swiftspend.finance:80/"}`**

**W3C Extended Log Format (ELF):**

Defined by the World Wide Web Consortium (W3C), customizable for web server logging. It is typically used by Microsoft Internet Information Services (IIS) Web Server.

**`#Fields: date time c-ip c-username s-ip s-port cs-method cs-uri**stem sc-status` 

`31-May-2023 13:55:36 34.253.159.159 adversary 34.253.127.157 80 GET /explore 200`

**eXtensible Markup Language (XML):**

Flexible and customizable for creating standardized logging formats.

**`<log><time>2023-05-31T12:34:56Z</time><user>adversary</user><action>GET</action><status>200</status><ip>34.253.159.159</ip><url>https://gitlab.swiftspend.finance/</url></log>`**

### **Unstructured Logs:**

Comprising free-form text, these logs can be rich in context but may pose challenges in systematic parsing. 

**NCSA Common Log Format (CLF):**

 standardized web server log format for client requests. It is typically used by the Apache HTTP Server by default.

**`34.253.159.159 - adversary [31/May/2023:13:55:36 +0000] "GET /explore HTTP/1.1" 200 4886`**

**NCSA Combined Log Format (Combined):** 

An extension of CLF, adding fields like referrer and user agent. It is typically used by Nginx HTTP Server by default.

**`34.253.159.159 - adversary [31/May/2023:13:55:36 +0000] "GET /explore HTTP/1.1" 200 4886 "http://gitlab.swiftspend.finance/" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0"`**

## Log operations:

### Log collection:

Aggregate logs from diverse sources (servers, network devices, applications, databases).

**Key Steps**:

- **Identify Sources**: List all potential log sources.
- **Choose a Log Collector**: Select a tool compatible with your infrastructure (e.g., Fluentd, Logstash).
- **Configure Parameters**: Enable **NTP** for time synchronization, Set logging intervals and priorities critical events.
- **Test Collection**: Verify logs are collected accurately from all sources.

**Time Synchronization**: Use **NTP** (e.g., `ntpdate pool.ntp.org`) to ensure accurate timestamps. Critical for maintaining a chronological sequence of events.

### Log Management:

Store, organize, and secure logs for efficient retrieval and analysis.

**Key Steps**:

- **Storage**: Choose secure storage with defined retention periods.
- **Organization**: Classify logs by source, type, or priority.
- **Backup**: Regularly back up logs to prevent data loss.
- **Review**: Periodically check logs for proper storage and categorization.

**Hybrid Approach**: Store all logs but selectively trim less important data to balance storage and utility.

### **Log Centralization**

Consolidate logs for swift access, analysis, and incident response.

**Key Steps:**

- **Choose a Centralized System**: Use tools like **Elastic Stack (ELK)** or **Splunk**.
- **Integrate Sources**: Connect all log sources to the centralised system.
- **Set Up Monitoring**: Enable real-time monitoring and alerts for critical events.
- **Incident Management Integration**: Ensure seamless integration with incident management tools (e.g., PagerDuty, ServiceNow).

**Benefits**: Unified log access, Real-time detection and notifications, Streamlined incident response.

### Log Storage:

it can be stored in different locations like , **Local System:** Logs stored on the device where they are generated. **Centralized Repository:** Logs aggregated in a single location . **Cloud-Based Storage:** Logs stored in cloud platforms.

**Factors:**

- **Security Requirements**: compliance with organizational protocols. Protect logs from unauthorized access .
- **Accessibility Needs**: how quickly logs need to be accessed and by whom. Centralized /cloud provides faster access.
- **Storage Capacity**: High log volumes may require scalable solutions (e.g., cloud storage).
- **Cost Considerations**: Balance budget constraints with needs, Cloud storage may offer pay-as-you-go pricing.
- **Compliance Regulations**: based on industry regulations. Ensure logs are stored and retained for required periods.
- **Retention Policies**: how long logs should be stored, Ensure easy retrieval for audits or investigations.
- **Disaster Recovery Plans**: Ensure logs are available even during failures. Use redundant or distributed storage.

### Log Retention:

Log storage is not infinite; balance retention needs with storage costs. Retain logs for future analysis, compliance, and incident investigations.

**Storage Tiers:**

- **Hot Storage:**  past **3-6 months** that are most accessible. Query speed should be near real-time.
- **Warm Storage:** past **6 months to 2 years**, acting as a data lake, easily accessible but not like Hot storage.
- **Cold Storage:** Archived /compressed from **2-5 years**. not easily accessible usually used for retroactive analysis or scoping purposes.

### Log Deletion:

Manage log size, comply with regulations, and control storage costs. Avoid deleting logs that may still be valuable. Backup critical logs before deletion.

- Maintain a manageable size of logs for analysis.
- Comply with privacy regulations, such as GDPR, which require unnecessary data to be deleted.
- Keep storage costs in balance.

### Best Practices: Log Storage, Retention and Deletion:

- Determine the storage, retention, and deletion policy based on both business needs and legal requirements.
- Regularly review and update the guidelines per changing conditions and regulations.
- Automate the storage, retention, and deletion processes to ensure consistency and avoid human errors.
- Encrypt sensitive logs to protect data.
- Regular backups should be made, especially before deletion.

## Log Analysis Process:

- **Data Sources:**  systems or applications configured to log system events or user activities. These are the origin of logs.
- **Parsing:** Breaking down log data into manageable components. to Extract valuable information from logs in various formats.
- **Normalization:** Standardizing parsed data into a consistent format. simplifying comparison and analysis of logs from different sources.
- **Sorting:** Organizing logs based on parameters like time, source, or severity. to efficiently relative data and identify patterns.
- **Classification:** Assigning categories to logs based on characteristics (e.g., severity, event type). to quickly filter and focus on relevant logs.
- **Enrichment:** Adding context to logs (e.g., geographical data, user details, threat intelligence).  Make logs more meaningful and easier to analyze. resulting in better decisions and more accurately respond to incidents.
- **Correlation:** Linking related log entries to identify patterns or trends. to detect security threats or system performance issues.
- **Visualization:** Representing log data graphically (e.g., charts, graphs, heat maps). to simplify interpretation of large data.
- **Reporting:** Summarizing log data into structured formats. Provide insights, support decision-making, and meet compliance requirements.

## Log analysis techniques:

methods or practices used to interpret and derive insights from log data.  can range from simple to complex and are vital for identifying patterns, anomalies, and critical insights. 

- **Pattern Recognition:** Identifies recurring sequences or trends to detect normal or abnormal system behavior.
- **Anomaly Detection:** Spots deviations from expected patterns, useful for identifying security threats.
- **Correlation Analysis:** Links related log entries to understand event relationships and root causes.
- **Timeline Analysis:** Examines logs over time to reveal trends, seasonal patterns, and performance insights.
- **Machine Learning & AI:** Automates classification, anomaly detection, and predictive insights.
- **Visualization:** Uses graphs and charts for intuitive data interpretation and pattern recognition.
- **Statistical Analysis:** Applies quantitative methods like regression and hypothesis testing to validate insights.

## **Log Configuration**

Address security, operational stability, regulatory compliance, and debugging needs.

### **Purposes:**

- **Security** : Anomaly and threat detection. Logging user authentication data. Ensuring system integrity and data confidentiality. Ex.  Configuring logs to detect unauthorized access attempts.
- **Operational** : Proactively creating reports and notifications. Troubleshooting system errors. Capacity planning and service billing. Ex. Logging system performance metrics for capacity planning.
- **Legal** : Alignment with standards, regulations, and laws (e.g., GDPR, HIPAA, PCI DSS). Ensuring compliance with logging and retention policies. Ex. Configuring logs to meet PCI DSS requirements (12-month retention, 3-month searchable data).
- **Debug** : Increasing visibility for application debugging. Enhancing efficiency and speeding up development. Ex. Enabling verbose logging during development to identify bugs.

### Planning:

- **What will you log, and for what purpose?** (Define the asset scope and logging purpose.)
- **Is additional commitment or effort required to achieve the purpose?** (Assess any extra effort or resources needed.)
- **How much are you going to log?** (Determine the level of detail required.)
- **How much do you need to log?** (Decide the volume of logs necessary.)
- **How are you going to log?** (Plan the collection method.)
- **How are you going to store collected logs?** (Define how and where logs will be stored.)
- **Is there a standard, process, legislation, or law that you must comply with due to the data you log?** (Ensure compliance with regulations.)
- **How are you going to protect the logs?** (Plan for security and protection.)
- **How are you going to analyze collected logs?** (Define the log analysis approach.)
- **Do you have enough resources and workforce to do logging?** (Assess available resources and personnel.)
- **Do you have enough budget to plan, implement, and maintain logging?** (Evaluate financial feasibility.)

## **Logging Principles:**

| Category | Key Points |
| --- | --- |
| Collection | Define the logging purpose. Collect what you will need and use. Do not collect irrelevant data. Avoid log noise. |
| Format | Log at the correct level and detail. Implement a consistent log format. Ensure that timestamps in logs are accurate and synchronized. |
| Archiving and Accessibility | Define log retention policies and implement them. Store log data and make sure the important part is available for analysis. Create backups of stored log data and used systems. |
| Monitoring and Alerting | Create alerts and notifications for important and noteworthy cases. Focus on actionable alerts and avoid noise. |
| Security | Protect logs by implementing access controls. Implement encryption if required. Use a dedicated log management solution. |
| Continuous Change | Logging sources, types, and messages are constantly changing and being updated. Be open to continuous change. Train your personnel. |

## **Logging Challenges:**

| Challenge | Challenge |
| --- | --- |
| Data Volume and Noise | Having multiple data sources to deal with. Differences in the log volumes created by applications. Some applications generate an insufficient amount of logs. Large-scale applicants could generate massive log volumes. Some logs can provide non-essential data and make the identifying process difficult. |
| System Performance and Collection | Log collection can slow down the system's performance. Systems are not always "state of the art".Some "sensitive" or "ancient" systems are impossible to touch. Deployment and optimization challenges. Managing system and agent version updates and synchronization in large-scale networks is overwhelming. |
| Process and Archive | Having multiple data formats to deal with it. Parsing different data sources and formats is time-consuming and error-prone. Balancing the log retention can be challenging. Especially when dealing with many compliance regulations and standards. |
| Security | Ensuring data security is a task/challenge in itself. |
| Analysis | Combining, correlating, and analyzing data from multiple sources to understand the context of an incident is a time-consuming process that requires significant computing resources and expertise. Achieving this in real-time is also another challenge in the same scope. Avoiding false positives/negatives is overwhelming. |
| Misc | Lack of planning and roadmap. Lack of financial resources/budget. Lack of implementation scenarios, playbooks, and exercises. Lack of technical skills to implement, maintain, and analyze. Focusing on log collection instead of the analysis phase. Ignoring human factors and potential system errors. |

## Command-Line Tools:

### **Cat:**

it Display the entire content of a file. View small log files. `cat apache.log`Not suitable for large files due to overwhelming output.

### **Less:**

View large files page by page. Navigate through lengthy logs efficiently. `less apache.log` Use arrow keys, Page Up/Down, and `q` to exit.

### **Tail:**

View the end of a file. Monitor recent log entries in real-time. `tail -f -n 5 apache.log`  , `f`: Follow log updates in real-time. `n`: Specify the number of lines to display.

### **WC (Word Count):**

Count lines, words, and characters in a file. Quickly assess log file size. `wc apache.log`  Lines, words, and characters count.

### **Cut:**

Extract specific columns from a file. Isolate specific fields (e.g., IP addresses, URLs). `cut -d ' ' -f 1 apache.log` , `d`: Specify delimiter (e.g., space). `f`: Select field number.

### **Sort:**

Sort data in ascending or descending order. Organize log entries for easier analysis. `cut -d ' ' -f 1 apache.log | sort -n` ,  `n`: Sort numerically. `r`: Reverse the order.

### **Uniq:**

Remove duplicate lines from sorted input. Simplify lists (e.g., unique IP addresses). `cut -d ' ' -f 1 apache.log | sort -n | uniq` , `c`: Count occurrences of each unique line.

### **Sed (Stream Editor):**

 Search, replace, or manipulate text. Modify log entries (e.g., replace dates). `sed 's/31\/Jul\/2023/July 31, 2023/g' apache.log` ,  Use `i` to edit files in place (backup first!).

### **Awk:**

Process and analyze text-based data.  Filter logs based on conditions (e.g., HTTP errors). `awk '$9 >= 400' apache.log` , `$9` refers to the 9th field (HTTP status code).

### **Grep**

Search for specific patterns or keywords. Identify relevant log entries (e.g., admin access). `grep "admin" apache.log` , `c`: Count matching lines. `n`: Display line numbers. `v`: Invert match (exclude lines).

## **Regex:**

A sequence of characters defining a search pattern.  Search, match, and manipulate text data.

### **Regex with grep:**

Extract blog post IDs between 10-19 from a log file. `grep -E 'post=1[0-9]' apache-ex2.log` ,  `E`: Enables regex. `post=`: Matches literal text. `1[0-9]`: Matches numbers 10-19.

### **Logstash (Grok):**

A Logstash plugin for parsing unstructured logs.  `%{SYNTAX:SEMANTIC}`. Use Oniguruma regex syntax.

- **RegExr**: Online tool for building and testing regex patterns.
- **Grok Debugger**: Test Grok patterns for Logstash.