# High-load Architecture Course Project

The modal verbs in the document are used in accordance with
[RFC-2119](https://datatracker.ietf.org/doc/html/rfc2119)

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
Administrators can drop the messages into the conversation, chart members can react on the posted messages only.

##### 2.1.2.1. Private chatting room

Private chatting room is created by users. A user creates a private chatting room is the chat administrator.
Invited users are chat members and can react on the post in the chat.
Any member of the chat can get the administrative privileges granted by the chart creator.

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

| Parameter              | Value         | Comment                                                                                                                                                                                                                                                                                                                   |
|------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nature                 | Dynamic       | Every post is unique, updatable and must be indexed for full-text search                                                                                                                                                                                                                                                  |
| Format                 | JSON          | A small message size (up to 200 characters) eliminates the need for compression, but JSON can be optimized by using acronyms instead of full field names.  For example, for 10 fields with single-character names, the efficiency of the message format with an effective text of 200 characters will be greater than 95% |
| Storage                | ElasticSearch | Sharded by time-zone (grouped time-zones) and post language. Native integration between Elasticsearch and Apache Spark, in the form of an RDD (Resilient Distributed Dataset) - an excellent opportunity for big data analytics                                                                                           |
| Estimated size (in TB) | 5.6 TB        | Max capacity per shard ~ 2 billions of records. Theoretically, every shard will store 1.8TB approximately. With 30% capacity reservation, expected amount of shards: ~ 6 shards                                                                                                                                           |

Note: 3 instances of ElasticSearch with location in west (USA), east (Asia) and central europe (France/Germany) regions.

## 4.3. Archived posts

| Parameter              | Value     | Comment                                                                            |
|------------------------|-----------|------------------------------------------------------------------------------------|
| Nature                 | Static    | Archive records cannot be changed                                                  |
| Format                 | HDFS File | Convertible into CSV or TSV. Analytics with Apache Mahout                          |
| Storage                | HDFS      | A distributed file system that provides high-throughput access to application data |
| Estimated size (in TB) | 200 TB    |                                                                                    |

## 4.3. Analytics (statistics) with history

| Parameter              | Value   | Comment                                                                                                                                                                       |
|------------------------|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nature                 | Static  | Collected statistics per date                                                                                                                                                 |
| Format                 | JSON    | In fact, BSON                                                                                                                                                                 |
| Storage                | MongoDB | 3 node in replica set with secondary preferred reading mode (high availability, low consistence),  20K rps for reading, excellent integration with Apache Spark for analytics |
| Estimated size (in MB) | 200 MB  |                                                                                                                                                                               |

