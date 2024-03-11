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

#5. Architectural design

#5.1. First draft

![First draft](https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=HLA_architectural_design_0001.drawio#R7Z1bl6K4Fsc%2FjY%2FtIoFweey69OmH7jm9Ts2smTkvtShNKadRHKBu%2FelPUIKQgESEBDVP3VKImvyys%2Fd%2FJ9kT83b1%2Fq%2FY3yy%2FR3McTqAxf5%2BYdxMIgWlZ5J%2FsysfuiuN4uwuLOJjnN%2B0vPAS%2FcH7RyK%2B%2BBHOcVG5MoyhMg0314ixar%2FEsrVzz4zh6q972HIXVT934C8xdeJj5IX%2F1z2CeLvOrwDD2f%2FiKg8Uy%2F2gX5X9Y%2BfTm%2FEKy9OfRW%2BmSeT8xb%2BMoSnf%2FW73f4jBrPNouu%2Fd9afhr8cVivE5F3vD0%2B7%2BdTwvoW%2F%2F8NG3jP9bqv5vHT3D3lFc%2FfMl%2F8LfIn5MrT37or2c4zr97%2BkEbJPmJ01n2y4yJeeMnm12jPwfvmHzOzSYK1imO71%2FJl8paHpBrxQ%2FP3jH3k2V25%2FbFMl2F9KY0jn7i2yiMYnJlHa3Jx92E%2FhMOf0RJkAbRmlye4ezh5A%2BvOE4D0knfmBueojSNVqUbPofBIvtDGm2yr5u%2FKp7zHIQh%2FcwJNA0DuO7d7itvst%2B7el9kWE%2F9Xy8xnoakaR5pwzwu8BrHwSxvQPJ5%2BL2xZ0DR32Sg4GiF0%2FiD3JK%2FAdnGFO3e9EHHjbN7%2FbZnjl5almijsPk55Yvi2XsQyH9yFo7hwuLA4EjAczJS8pdRnC6jRbT2w%2Fv91Zs4elnPi97e3%2FMtyrpj2%2B%2F%2Fw2n6kQ97%2FyWNqlTsPjP7oMNNS75X9BLP8IFfZObGw48XOG0bEXxXxTj00%2BC1%2Bj16b3azvdX3bZo10NsySPHDxt%2F%2B9DfCal37nQSnhbwKmhZPJrQMHk17KDQFyLxmG%2BVvsuf00O9Otd9tyPU7qul2c6hut3W313b7Kpn5eLp91yYOEjx9i%2BKfSepnH%2Fs4C4Osu%2FuYoqo0IDEaBpufXE3DASOwip4C0gynd7uNrLZuN5HEbvfauz1YbT36m8PdX%2BrQLQnbL4puJuiutvWjdZr7KIC01k3%2BGXfBakF%2BRhg8ZT8mG4fk37vfHqbJ66KfydcE4xp2NCo76Bau55%2BzwCtrwNBPEuIiZ6PGj1P%2Bcqkbqr4ifg%2FSv7K%2FTA03f%2Fl39ifiJ%2B9e3b2X7rz7KL34Qbxy8muzjttdW5Nf%2Flf5RelJ2cv9o7av6LN69D09Qd%2FTrIdBsLeFXdT8E35k4JfGulFlDSKn%2Bojdz8zfVQ402QcxHiM0mAft2oF70JbI4mefACngIL3dzoMJedwfCY4TjlkyQtMqjpwNYA31KpjPd0EOToJf%2FtP2UUa9PSFxTZKbj6YZpGzg80t9TNtm1X4DxNkPT6YTB%2FgA5%2FbuNz1176fuWRi9zHvoeZcZzcgVmjlMOFTPiwgKvcwc1Nx%2FMqaAsfie1WLzt6%2B4CYTORUdMRD1OHqLCBVA6eziwyhuNHo%2BdPBy3qoQ5DpXGJM0ecIQaDECMPljI46XOtRCY1vnigxlziLiW%2BhLE%2BM3P3qotek1o%2FlxpnhORcKtEmDwRdcOd5mP6p8GRbeAZf94Bpxj3vUH%2Fu2zPhaOMHg0%2BHVddpWo5Br%2FIa1HX0usYLkCHeZAlFi4QZPyP0m253934hYFrVj%2FHMRnod0%2FsdzYRUCzOK48CXVE6UT2ecjIpkFcIdWazOWswUGYT2BbjuZgGP09JzWyavIT1x8NnDoZLVQQsWDWCrmBcOFh3wHYDKdvdNtl0vOnVQAtkJj1NXtPS9qvWx%2FY3m5A8f5v%2BSnD8Sg39aaPGZHjgk%2BD0FjlGTCfBVeIAbJfhgddV5fLAh%2BKaB3k8QMcZmX0QWC0x6mDctLyK2lpkAZVE5HT2bY953HpO5ITkVpVCy2DwEo3IPYZmFtOBFVhTvpI03eFG%2Be0bXkspvNZZwOv1BC%2BwFNMrsDSpX3pBz6bXgCV6PxF8baX4CquhSvEtRIwm7ITlUJd5EDsQBubX4vUR7UrulbJ%2FXvAL7lMhs6orNIpltyVsnRpsBxNOLV5p%2BBM%2FbUfqjHR7NhKDGvH0UvUyYh8YMciqWQEP6sSgwTQzSyDaq51iWpfbGVOI7EnZd%2FGO9126LrirnZf6nElEvXjLqEdCzkzCTCQ2i5HoRMLMI8iVPI90DUFF3HVUIes0f4cjXm2oKeqtK4YUsopHR0pNVjqxJGPaNdg82i9Hfa5PmHqeUkpFnXJNaT%2BU8kHlQxpjfxWsF%2BTyU%2BYOXZFPRr2tAz4zgFI9MoFlHsqzmIhCqyyLiepiSzvMICURhr3I%2FnP%2FO71EPqK4ejVsk%2FiTTT7bNcklmXAjnXsWTC7N%2FdR%2F8hM8WOIZWTwLVHCUkliiG9A0C%2FJZALbBwGArhkFgqbyGYRgYoOMyMNScGCIVhq4bfAREqXLoQ3fhdNGjetANJv0FUXQSbw2iUAMKcoIoxCbUugZRiM0rSw6i0LkoUqfhroRQqAntg9DhxKirJ1Tp7sjLIbRudQMTwX%2B77gAemKzuUrNZ05QawPPq1B1xSLNvFMVb4fDacrqQ7aKaBbxyc7p2nTSmWkFkm8muCzKkKog2r0Ulof%2BKubbScWcl7uxvwQqbnq47Ekrmfi56Ll2JiJWfpHp%2FnzwkAB2Vo2Hi4nbd2qKngNgNkZicXbc2rwhq8yx1LLLLftSPxf51waaYdmqcvHC%2FvPRZaWBLk26dB7ysQ%2BOYVWY0YXX8oXHM%2Bn%2BPedDAga3d%2F5rKwTAdjfpCbYuGVBKk8hTsC4L0VNdJQ3ocpNK273U4l%2B%2Bc1qvb2gWQCq7DS3AxLnaL8wyTPt5GDsNJpJOmeCCvdpO%2FeVKcxVXG7MDgbIwePhlTaNAgrisX9Jbo%2BTnBw5gYaXssz9vEtFsOpVkx4DIlJhybPexTeNckQm2PGtp68Mr05VmPhi22%2BeMzpxGN3XY4kOunr9gPCe6kYZZ49nOSNUee0oxp%2BnIVrYM0irfL7bWURTfDLrcN98g2zmn5BZoap16A7U755Fzt0aawGbOTJCyHVzW%2Fl36y8XnTPM6vkg5O6MwJeYxf1mvSZo%2F%2BZpP0wApilkdDxxIixfSGIoUP0D%2FHs2VmCDmTQv71V1kzr5%2BS7J%2BVv%2FYXOm3VuoL2Yz1bxoSnX6UBd6LFYSRzWJPl9pq9oP4pUn2Ucie3VPEx%2BXTJQavPuxujqnxeyETLbtft3ZCJlh3J%2B7tdgQUr3dZ7VwFyWqKtvYBJWraqYLqKBRx6tmk7kqMScCx29U5nAQdIRlJgo1pnedxg9HGrE131jENpVpJuVNVISkKSjzb7Q9LyKkyelLCpADm%2BOVvT2AuNAhv2uu7R8mDFpKk9sJOKPO1gqS25Z7FaR1ewWNFENlh83PsjDqI4SD%2Fq5TScJFkJzbq%2FNRxmcZVh8DBnziHDZbCrk9iknjrn8iHv17svD5qCWjFk%2ByvXuJfCzqDKgmNAjoRaCW0wEmTn9oypDatnKxXH93baY7wPO8exjIBqkq1zoduQG5Y0F7rVKQzBjnOhYzJzoSN5LhRZfD68MNIjQcJBo6tUWrPZY6G6HsFrMwdtcGdRD0wQdeaGcNMBsEoUZac7wxNO%2BGSPuGsxdT0y6YpaNVNp6OiwHn5Xq2a6aj18T0Bbk70%2F0TbtKVsXlfdepNdF9fgoW9dFPejP9lgX1aUiMCXC5ImQWhfVE9n%2FclZ70TzRwz%2B9BnFFzl40j9%2FRoStANmoMQ1WAdF2HPc%2BwJsKUuiPNE9hFIXsqcxB7aoTp1TST1L32nsAyhKscMXKKijl0t08BBH%2FKrdSiYp6AMqN5GIwH1wMMD4qLzHkCQofmYbgilIZhTEZlIIDRVbjQZQcPTcDtjnZDqXVJGofDymWw%2BgjhiiWQARoxDxpY4wBG1wVEuvLgIYdh5Pyy2HXn11HNL5TN72XXHvREEx9qAfbsFu6Ea2caTArFZIfC4ATrI8VlrgRxbWZBGlVPVa0DAQYvl15z%2BUHPYXUhy%2BK9fLlHVQJD1x8c1J3PraAyf56ZTmyaQjnaH2JmE%2BQyDxp8NjmXE9%2FHVYNQ2G9XDCoxRKwC0tXxAayWwm4mHhxVXYdwSAddk9ofqboWYdlBM6tb0Os8aLm1CIExwmKEfH4TUW6V5TcLr1mXIzyQZIBcYtquyTtJBRwIyLRXKRJIqDvGZqWV1x0DdMmfpkE%2BDa7L1qd0VdOgBUR1BSoNmkArcOC1Ksk46KqER4ZUhQTfHlOBBhwkxVSQTbd1jqlMNvEsO6YC8k4f7y%2F4V3qq8zGYQo1pT5iei556lpiqVaguCdM6LVUXKax4atzWKVSzdUpqlUIAeGHxqssUeg7iyhTywbbk3C8YpbjINpRdF3jIFRchLy7qUlhSS2FxeWxaol7VxiMAeeFS1yqUC4XrWmODArZb1PPaIAqgsNPb1FtytogWX1QbaVXjkVtyOoIBqSsWdhr1NC3XfdRLCnVN5oQgZHWsOgK4Uzk8uUVHANRlC7uQCjWp0knVtQu7kHqyJ6VJPZpUXcCwrzhA9HxnTW9%2F9PIq6sXVIcuHaGNQcSZlDAHselTklVkaAfuhNHnmUYeSxrBOcaDysQbEc9y2Rw1tQUxeub5AC%2BIdtCBnUcoQ0EOYdS3DU2Su4WoZ2nbVLoygliGgm%2Fl0ibozKlFnO9V1tspL1AFzuIWVFR9FvPzXrgDJiMp%2FeaJHFOd2fDTRj2Uz1qd79AOYJw3uuwypMvVZbyk728Qtu%2BPkAoCHWR9YaxKP1jWvvfEqW2sidvJMg8D2I%2BGFz1dQeyY8Yos1sZU7hesUsIfLs8u%2BBqe3a60WEWvrgZONrThcIy9t4TDbx9H5IsNLXvcvcUQ8a%2FjlcxL4HD8Xu7oTVV164PJb57waooZb2mnxuk9%2Fo9mteOdgCpB1ivtEq%2F2O5KTOQokZecHBy5l6rK5HdQp4SE55TY2hkitHNP2rHZp%2BqOLFMTGqxnKAJhoLud55kMvGf2ytOGFy2ZCUrfPUQC5Bxv8o3ZZ7KI1f2DOqxy%2BA3ELsB8LuiT0Pi%2F7rBneNIRvCWJWoC5ci85TGEIg9prtrsWHW3JteX1aavIyjKC3fnknt36N5ll28%2Fz8%3D)