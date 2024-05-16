# High-load Architecture Course Project

The modal verbs in the document are used in accordance with
[RFC-2119](https://datatracker.ietf.org/doc/html/rfc2119).

## 1. The high-level task description

A **social network** where the customers can publish their posts which can be reacted by other customers. The *
*reactions** are provided in the form of emoji.

A **feed** is a set of the posts ordered by date and collected by the criteria:

- authored by a specified user;
- from the feeds subscribed by a set of the specified users;
- a set of keywords.

The system **MUST** provide:

- live statistics of the most popular emoji
    1. per hour;
    2. per day;
    3. per week;
    4. per month;

- advanced search:
    1. by emoji;
    2. by keyword set;
    3. fuzzy search by posts in the specified set of the feeds.

Retention policy: messages must be searchable in one year.

## 2. The detailed requirements

### 2.1. Functional requirements

#### 2.1.1. User registration

2.1.1.1. A new user can self-register in the system through the following OAuth2 providers: Google, Facebook.
During registration the system requires to specify three mandatory fields:

- name;
- cell;
- email.
  Other fields can be added later after approval of the security team for strictly following the GDPR requirements.

2.1.1.2. A user **MUST** be informed about the personal data collected by the system and the ways of their usage in the
future.
The user must perform an action that the US judicial system recognizes as legal action. The legal department **MUST**
provide the detailed recommendation regarding this

Verification of email and cell are mandatory procedures:

- email should be verified by sending an email with a link for registration confirmation;
- cell should be verified by sending a randomly 4-digit generated pin-code via sms mechanism.

#### 2.1.2. Creating chatting room

A user is able to start/initiate a conversation based on a level of privacy:

- private chatting room - a user defines a forum of participants (one or more).
- public chatting room:
    - limited - participation in the room is limited by a condition (sex, location, language etc.)
    - unlimited - any user can join the conversation.

There are two kind of roles: chat administrator and chat member.
Administrators can drop the messages into the conversation, chat members can react on the posted messages only.

##### 2.1.2.1. Private chatting room

Private chatting room is created by users. A user creates a private chatting room is the chat administrator.
Invited users are chat members and can react on the post in the chat.
Any member of the chat can get the administrative privileges granted by the chat creator.

##### 2.1.2.2. Public chatting room

A public chatting room is creating by any user became the chat administrator.
During chat creation the admin selects one from two possible modes: limited chat or unlimited chat.
If the administrator selects the limited chat mode then a condition of visibility **MUST** be specified.

### 2.2. Non-functional requirements

#### 2.2.1. Reliability

The percentage of time that a computer system **MUST** work without failing - 99.999 percent.
Average annual downtime - till five minutes.

##### 2.2.1.1. Common requirements

##### 2.2.1.2. Fault-tolerance

For mitigating faults the system **MUST** work in accordance with Active-Active approach where Main and DR MUST be
placed in
different datacenters.

##### 2.2.1.2. Security

###### 2.2.1.2.1. Common requirements

The system should be protected in accordance with COBIT 6 and ISO 27000.

###### 2.2.1.2.2. GDPR

The system MUST be GDPR-compliant.

###### 2.2.1.2.3. Brand-safety

A brand safety mechanism **MUST** be proposed during the architectural design phase.
This mechanism **MUST** restrict the posts related to sex, politics, religions etc.

#### 2.2.2. Scalability

On-demand scalability for the business supporting microservices based on the traffic.
Progressive scalability for databases, inverted indexes, caches and file storages.

##### 2.2.2.1 Load profile

| Parameter                                                    | Value                                                                                                                           |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Traffic distribution in time (in every time zone percentage) | 3% 12PM - 6 AM  <br>7% 6AM - 9 AM <br>10% 9AM - 12AM <br>15% 12AM - 3PM <br>10% 3PM - 6PM <br>50% 6PM - 10PM <br>5% 10PM - 12PM |
| Rough estimate of market penetration (in the first year)     | 3% of population in every time zone                                                                                             | 
| Population per time zone                                     | [Source](https://distributionofthings.com/world-population-by-time-zone/)                                                       |
| Total users in first year (estimate)                         | 300M                                                                                                                            | 
| Average post publishing (per user per day)                   | 1 post every day per user                                                                                                       |
| Average post reading (per user per day)                      | 100 posts every day per user                                                                                                    |
| Post size limit                                              | 200 chars                                                                                                                       |  
| Max number of reaction (estimate)                            | 10 millions                                                                                                                     | 
| Post loading page size                                       | 10 posts per page (on demand)                                                                                                   |
| Posts page loading timing (round trip in millis)             | 300                                                                                                                             |

**Note**: size of the metadata **MUST** be calculated based on the data structures design.

#### 2.2.3. Maintainability

#### 2.2.3.1. Customer supporting

| Level | Nature of the support | SLA (in hours) |
|-------|-----------------------|----------------|
| 0     | Self-Support          | -              |
| 1     | Basic Support         | 1              |
| 2     | Troubleshooting       | 8              |
| 3     | Expert Support        | 72             |
| 4     | External Support      | 120            |

#### 2.2.3.2. Proactive maintainability

Artificial intelligence (AI) **MUST** be used for proactive maintainability: contextual recommendations MUST be provided
to
a customer based on the customer behaviour model.

#### 2.2.4. Documenting of the architectural solution

The architectural design **MUST** be composed in accordance with the recommendations of the TOGAF Standard (10th
edition).

# 3. Back-of-envelop calculation

In accordance with [Source](https://distributionofthings.com/world-population-by-time-zone/)  the most intensive traffic
is observable in 6PM - 10PM in the UTC+8 time-zone.

Approximate number of potential users:

| Time-zone | Population <br> (in millions) | Users <br> (in percent,<br> UTC+8, <br> 6PM - 10PM) | Users <br> (millions,<br> UTC+8, <br> 6PM - 10PM) |
|-----------|-------------------------------|-----------------------------------------------------|---------------------------------------------------|
| UTC-11    | 0                             | 3%                                                  | 0                                                 |
| UTC-10    | 0                             | 3%                                                  | 0                                                 |
| UTC-9     | 0                             | 3%                                                  | 0                                                 |
| UTC-8     | 100                           | 3%                                                  | 0.9                                               |
| UTC-7     | 80                            | 7%                                                  | 0.168                                             |
| UTC-6     | 300                           | 7%                                                  | 0.63                                              |
| UTC-5     | 350                           | 7%                                                  | 0.735                                             |
| UTC-4     | 100                           | 10%                                                 | 0.3                                               |
| UTC-3     | 320                           | 10%                                                 | 0.96                                              |
| UTC-2     | 0                             | 10%                                                 | 0                                                 |
| UTC-1     | 0                             | 15%                                                 | 0                                                 |
| UTC       | 300                           | 15%                                                 | 1.35                                              |
| UTC+1     | 800                           | 15%                                                 | 3.6                                               |
| UTC+2     | 550                           | 10%                                                 | 1.65                                              |
| UTC+3     | 600                           | 10%                                                 | 1.8                                               |
| UTC+4     | 100                           | 10%                                                 | 0.3                                               |
| UTC+5     | 80                            | 10%                                                 | 0.24                                              |
| UTC+6     | 50                            | 50%                                                 | 0.75                                              |
| UTC+7     | 250                           | 50%                                                 | 3.75                                              |
| UTC+8     | 1300                          | 50%                                                 | 19.5                                              |
| UTC+9     | 1700                          | 50%                                                 | 25.5                                              |
| UTC+10    | 200                           | 5%                                                  | 0.3                                               |
| UTC+11    | 80                            | 5%                                                  | 0.12                                              |
| UTC+12    | 80                            | 5%                                                  | 0.12                                              |

Roughly estimates:

| Parameter                           | Value                |
|-------------------------------------|----------------------|
| Total registered users              | 220.2 million users  |
| Max number of active users per hour | 61.863 million users |
| Max post reading (per second)       | 17184 posts          | 
| Max post submitting (per second)    | 1719 posts           | 
| Max hourly post reading             | 6.1863 billion posts | 
| Max hourly post submitting          | 61.863 million posts | 
| Max daily post reading              | 22.02 billion posts  | 
| Max daily post submitting           | 220.2 million posts  | 
| Max monthly post reading            | 660.6 billion posts  | 
| Max monthly post submitting         | 6.606 billion posts  | 
| Average post size                   | 800 bytes            |  
| Average metadata size per post      | 100 bytes            |  
| Average number of reactions         | 1000 emoji           |
| Bandwidth per second (posting)      | 1.4MB                |
| Bandwidth per second (reactions)    | 0.75MB               |
| Bandwidth per second (reading)      | 15.2MB               |
| Storage (in TB, per month)          | 5.6                  |

# 4. Formats and persistence layer

## 4.1. Emoji

| Parameter              | Value  | Comment                                                                                                                           |
|------------------------|--------|-----------------------------------------------------------------------------------------------------------------------------------|
| Nature                 | Static | Static resources, no versioning is needed                                                                                         |                                                                                        
| Format                 | Binary | Images are binary by nature. Compressed binary will be most effective distributed by location in CDN and delivered on the client. |
| Storage                | CDN    | Reduce delivery time to the clients in different geolocations, high level of resilience.                                          |
| Estimated size (im MB) | 100    | Average size of emoji is 100KB, available max capacity on CDN - 10000 different emoji                                             | 

## 4.2. Active posts + metadata

### 4.2.1. The requirements

##### 4.2.1.1. Replication

Replicate chat message data across multiple servers or data centers to ensure data availability and redundancy.
In the context of the project, the bidirectional replication seems like the most effective solution because
applications can write to the local db within each datacenter, and read across multiple dbs for a global view of all
information.

#### 4.2.1.2. Partitioning

The primary use of partitioning is scalability.
Distributed search, aggregations (including map-reduce) is highly desired features of the application.

#### 4.2.1.3. Transactions

No needed.

#### 4.2.1.4. Integrity

Data integrity exists to ensure the data remains accurate and uncompromised throughout this process.
There are no legal or financial consequences for the system owner.

#### 4.2.1.5. Consensus

Consensus algorithms are needed to guarantee a total order of messages, ensuring that all nodes process messages in the
same sequence.

#### 4.2.1.6. PACELC

Trade-off between consistency and latency is preferred.
For the system we determine the number of nodes that take in synchronous reads and writes using the formula:

R + W > N, R + W ≤ N

R: synchronous READs

W: synchronous WRITEs

N: number of Replica Nodes

If the equation is R+W>N then consistency is maintained but is still subject to latency overhead. If it is R+W ≤ N
latency is reduced, but it is possible to retrieve responses from asynchronous updates which could lead to
inconsistencies of the data. With this bound in place the system will either sacrifice a little more consistency or a
little more latency for the other and is really trying to achieve the best effort of both properties.

### 4.2.2. Architectural proposal

| Parameter              | Value         | Comment                                                                                                                                                                                                                                                                                                                   |
|------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nature                 | Dynamic       | Every post is unique, updatable and must be indexed for full-text search                                                                                                                                                                                                                                                  |
| Format                 | JSON          | A small message size (up to 200 characters) eliminates the need for compression, but JSON can be optimized by using acronyms instead of full field names.  For example, for 10 fields with single-character names, the efficiency of the message format with an effective text of 200 characters will be greater than 95% |
| Storage                | ElasticSearch | Sharded by time-zone (grouped time-zones) and post language. Native integration between Elasticsearch and Apache Spark, in the form of an RDD (Resilient Distributed Dataset) - an excellent opportunity for big data analytics                                                                                           |
| Estimated size (in TB) | 5.6 TB        | Max capacity per shard ~ 2 billions of records. Theoretically, every shard will store 1.8TB approximately. With 30% capacity reservation, expected amount of shards: ~ 6 shards                                                                                                                                           |

Note: 3 instances of ElasticSearch with location in west (USA), east (Asia) and central europe (France/Germany) regions.

### 4.3. Archived posts

| Parameter              | Value     | Comment                                                                            |
|------------------------|-----------|------------------------------------------------------------------------------------|
| Nature                 | Static    | Archive records cannot be changed                                                  |
| Format                 | HDFS File | Convertible into CSV or TSV. Analytics with Apache Mahout                          |
| Storage                | HDFS      | A distributed file system that provides high-throughput access to application data |
| Estimated size (in TB) | 200 TB    |                                                                                    |

### 4.3. Analytics (statistics) with history

| Parameter              | Value   | Comment                                                                                                                                                                       |
|------------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nature                 | Static  | Collected statistics per date                                                                                                                                                 |
| Format                 | JSON    | In fact, BSON                                                                                                                                                                 |
| Storage                | MongoDB | 3 node in replica set with secondary preferred reading mode (high availability, low consistence),  20K rps for reading, excellent integration with Apache Spark for analytics |
| Estimated size (in MB) | 200 MB  |                                                                                                                                                                               |

# 5. Architectural design

## 5.1. High Level Design

![Architectural design](/resoures/drawio/HLA_architectural_design_0001.drawio.png)

Multi-DC solutions play a critical role in ensuring **high availability**, **fault tolerance**, and **scalability**.

| Key aspects                          | Description                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Redundancy                           | Redundancy is achieved by replicating critical infrastructure components, such as servers, storage, networking equipment, and power sources, across multiple data centers. This redundancy ensures that if one data center fails or becomes unavailable, the workload can be seamlessly transferred to another data center without disrupting services. |
| Load Balancing                       | Load balancing distributes incoming network traffic across multiple servers or data centers to ensure optimal resource utilization, maximize throughput, minimize response time, and avoid overload on any single server or data center. It can be implemented using hardware or software-based solutions.                                              |
| Data Replication and Synchronization | Data replication involves copying data across multiple data centers in real-time or near-real-time to ensure data availability and consistency. Synchronization mechanisms are employed to keep data consistent across different data centers, often utilizing techniques like synchronous or asynchronous replication.                                 |
| Global Traffic Management            | Global Traffic Management solutions direct user requests to the nearest or most suitable data center based on factors such as geographical location, network latency, server load, or other customized criteria. This ensures optimal performance and user experience.                                                                                  |
| Disaster Recovery (DR)               | Multi-DC solutions often include disaster recovery plans and procedures to mitigate the impact of catastrophic events such as natural disasters, cyber-attacks, or hardware failures. These plans typically involve automated failover processes to switch traffic to backup data centers in case of a disaster.                                        |
| Consistency Models                   | Multi-DC architectures may implement different consistency models depending on the specific requirements of the application. These models include strong consistency, eventual consistency, and causal consistency, each offering trade-offs between data consistency, availability, and partition tolerance.                                           |

## 5.2. Middle-level Design

### 5.2.1. Global Traffic Management

The solution is based on the CQRS pattern. 
Priority queues usage allow to reach out the high level of **linearizability** at the level of the particular v-chat. 

#### 5.2.1.1. The problem statement:

- user connects to **the nearest available datacenter (DC)** via WebSocket;
- virtual chat (v-chat) can move between the datacenters depends on the locations of the participants to **minimize the
  average round trip time for all participants** of the particular v-chat.
- changes in the v-chat must be delivered to all active participants with **high level of guarantee**.

#### 5.2.1.2. Components

| Component                    | Responsibility                                                                                                                       | Comment                                                         |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Load Balancer                | Traffic balancing  between DCs                                                                                                       | Based on the user geolocation                                   |
| FrontEnd Coordinator Service | Managing WS connections                                                                                                              |                                                                 |
| Authentication Service       | User authentication                                                                                                                  | Auth Service                                                    |
| User Correlation Service     | Mapping of connected/linked users                                                                                                    | Traffic optimization for liveness messages                      |
| DC Message Broker            |                                                                                                                                      | Async delivering of the messages inside the DC (priority queue) |
| Enterprise Service Bus (ESB) |                                                                                                                                      | Inter-DC message broker (priority queue)                        |
| Database                     | 1. User Credentials (single leader with the shards in the DCS) <br> 2. Chats and messages (single leader with shards in the same DC) |                                                                 |          

#### 5.2.1.3. CQRS (the selected commands)

| Command                    | Priority | Weight |
|----------------------------|----------|--------|
| User liveness              | Medium   | 5/10   |
| Chat creation              | Medium   | 5/10   |
| Chat moving                | Extreme  | 10/10  |
| Chat message               | High     | 8/10   |
| Chat message (replication) | Medium   | 5/10   |

#### 5.2.1.4. UMLs

![1. DC discovering](/resoures/uml/traffic/dc_discovering.png)

![2. Connection establishing](/resoures/uml/traffic/connection_establishing.png)

![2. Chat joining and chat moving](/resoures/uml/traffic/v-chat-moving.png)

## 5.3. Monitoring, metrics and alerting

This part of the document defines the requirementS and the best practices in monitoring, metrics and alerting of User
Auth Module. This module is responsible for the typical business operation related to user registration,
authentication.

### 5.3.1. Performance monitoring

This involves tracking metrics like response times, request rates, resource utilization, and error rates to ensure
optimal performance.

The metrics:

| Metric               | Description                                                                                                                                | Unit   | Percentile         |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------|--------|--------------------|
| Response time        | This metric measures the time taken for the app to respond to a user request. It helps in evaluating the app's speed and efficiency.       | ms     | 50%, 75%, 95%, 99% |
| Request throughput   | The number of requests the app can handle per unit of time, indicating its capacity and scalability.                                       | rps    |                    |
| Error rate           | Monitoring the percentage of unsuccessful requests or errors occurring within the app provides crucial insights into its reliability.      | %      |                    |
| Apdex score          | A metric that combines response time and error rate to evaluate user satisfaction with the app's performance.                              | rps    |                    |
| Round Trip Time      | The time taken for a request to travel from the client to the server and back.                                                             | ms     | 50%, 75%, 95%, 99% |
| Latency              | The time used by server for for a request processing.                                                                                      | ms     | 50%, 75%, 95%, 99% |
| Resource utilization | Tracking CPU, memory, disk usage, queues lengths, and other resources helps ensure efficient resource allocation and prevents bottlenecks. | %, mpq |                    |

### 5.3.2. Health and liveness checks (probes)

Regularly checking the app's status and components to detect any potential issues or failures.
The requirements:

| Requirement class                 | Description                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Frequency                         | Regularly schedule health checks to monitor the app's status and components at predefined intervals.                          |
| Timeouts                          | Set appropriate timeouts for health checks to prevent them from causing delays or bottlenecks in the app.                     |
| Thresholds                        | Define thresholds for health check results to determine when an app component is considered unhealthy or  unresponsive.       |
| Customization                     | Allow for customization of health checks to include specific checks for different app components or services.                 |
| Logging and reporting             | Log the results of health checks and provide detailed reports to track app performance and status over time.                  |
| Alerting                          | Configure alerts to notify administrators or stakeholders when an app component fails a health check or becomes unresponsive. |
| Integration with monitoring tools | Integrate health and liveness checks with existing monitoring tools or systems to streamline monitoring processes.            |
| Automatic recovery                | Implement automatic recovery processes based on health check results to ensure prompt resolution of issues.                   |

### 5.3.3. Alerting

Setting up alerts for specific thresholds or conditions to notify when performance degrades or errors occur.

What to alert on:

- Alerts should be actionable and relevant to users.
- Some examples of things to alert on are availability, latency, and integrity/durability.
- Alerts are the “start” of an action or an investigation; they may only represent a small portion of what you monitor.

When to alert on things:

- Alerts can be more or less urgent depending on how long the issue has been going on and how severe the impact is.
  Consider that impact changes over time.
- You can use alerts to give people enough time to act before there are consequences for issues such as quota
  consumption.

Who to alert, and how:

- Only notify people you intend to act in response, and trust them to inform more people if needed.
- Page the person on-call if the situation requires an immediate response.
- Consider not paging the person on-call and creating a ticket if the issue is not urgent.

### 5.3.4. Logging

Collecting and analyzing logs to track app behavior, diagnose issues, and troubleshoot problems.

- Know What to Log
- Know When to Use Each Log Level
- Use English Language and Friendly Log Messages
- Have a Consistent Structure Across All Logs
- Understand Metrics
- Make Each Log Message Unique
- Always Provide Context
- Reporting Alerts and Exception Handling
- Write Log Parsers and Proactively Monitor Logs

### 5.3.5. Security monitoring

Monitoring for suspicious activities, potential intrusions, and vulnerabilities to protect the app from security
threats.

Please, follow [NIST SP 800-171](https://csrc.nist.gov/pubs/sp/800/171/r2/upd1/final) recommendations in security
monitoring.

### 5.3.6. User experience monitoring

Tracking user interactions, user journeys, and feedback to improve the overall user experience.

Please, use the following sorts of monitoring:

| Monitoring sort                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Synthetic Monitoring                   | The synthetic monitoring approach uses scripts to emulate the interaction between users and applications. Periodically running these scripts helps identify issues before they affect end user experience. Synthetic monitoring provides performance baselines and valuable info on availability, especially for applications with inconsistent usage patterns. This technique is also helpful to evaluating third-party services and APIs.                                                                                                                                                                                                                                                                                                                                                                                                     |
| Packet-based End-User Experience       | Packet-based end user experience monitoring is another way to view the response time for web pages from an end-user perspective. Using network packets, you can passively monitor the time it takes the browser to load the entire web page. You can also monitor the time it takes to load the individual objects that make up the web page or even measure the load time for a group of web pages that work as a single web transaction. Packet-based, end-user experience monitoring does not provide a complete picture of end-user experience as it does not measure device performance. However, packet capture is useful when monitoring consumer web applications that cannot otherwise be easily instrumented.                                                                                                                         |
| JavaScript Injection                   | Injecting JavaScript code directly into a web application and provides IT Operations with information about what is happening at a user’s web browser. This technique can also call third-party services and APIs, delivering better insights into a user’s actual experience. JavaScript injection gets closer to complete end user experience monitoring, at least for web apps. However, it requires either direct control of the application or proxy servers for externally hosted applications. While this works well for web and hybrid mobile applications, it becomes increasingly complicated and expensive, and does not cover other types of applications at all. Code injection also does not collect any details on the conditions of the user’s device nor the many network segments and cloud services between user and server. |
| Physical and Virtual Device Monitoring | Device monitoring is a popular approach for measuring operating system health and behavior. This includes metrics such as CPU and memory utilization, storage performance, network response at the device, and crashed or not-responding processes. These are important factors, but do not provide a complete picture of end user experience. One of the big challenges with device monitoring is relating operating systems metrics to user complaints. When a user complains about delays in retrieving a patient record, saving changes, or opening a document, or just the screen taking too long to load, it can take considerable time and effort to determine exactly where the problem is occurring.                                                                                                                                   |

### 5.3.7. Scalability monitoring

Monitoring app performance under varying loads to ensure it can scale effectively.

Scalability metrics:

| Metric class        | Description                                                                                                                                                                                                                                                                                                                                             |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Performance Metrics | These metrics measure the effect of increased workload on the responsiveness, throughput, and resource utilization of the application. Metrics such as response time, requests per second, and CPU or memory usage help developers evaluate the performance of their applications under varying load conditions.                                        |
| Elasticity Metrics  | These metrics address the system's ability to seamlessly and quickly deploy new instances, respond to changing demands, and release resources when they are no longer required. Metrics such as instance startup time, time to scale out/in, auto-scaling accuracy, and resource utilization ratios can be used to evaluate the elasticity of a system. |
| Resilience Metrics  | These metrics assess the robustness and fault tolerance of a system, ensuring that it can continue to operate effectively and recover gracefully from failures. Metrics such as mean time between failures (MTBF), mean time to recovery (MTTR), and availability can help developers evaluate their application's resilience under scaled conditions.  |

### 5.3.8. Third-party service monitoring

Monitoring external services or dependencies that the app relies on to prevent disruptions.

The metrics are the same as for:

- performance monitoring and
- health and liveness probes.

### 5.3.9. Business metrics

| Metric                     | Description                                                                                                                              | Unit            |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| Retention Rate             | Retention rate refers to the percentage of customers who continue paying for a product over a given timeframe                            | %               |
| Churn Rate                 | The number of customers or subscribers that leave a provider in a given time period (1-Retention Rate).                                  | %               |
| Daily Active Users (DAU)   | The number of unique users who engage with a product or a website within the last day                                                    | person          |
| Monthly Active Users (MAU) | The number of unique users who engage with a product or a website within the last month                                                  | person          |
| Daily Sessions per DAU     | Dividing the total number of sessions in a day by the number of daily active users (DAU)                                                 | sessions/person |
| Stickiness                 | How often users are returning to an app: is measured by dividing the number of daily active users by the number of monthly active users. | %               |
| Productivity               | Is measured by dividing the number of daily posts by the number of active users per day                                                  | posts/person    |
| Engagement rate            | is a metric used to assess the average number of interactions your social media content receives per follower                            | posts/person    |







