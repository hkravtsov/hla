# High-load Architecture Course Project

The modal verbs in the document are used in accordance with
 [RFC-2119](https://datatracker.ietf.org/doc/html/rfc2119)

## 1. The high-level task description

A **social network** where the customers can publish their posts which can be reacted by other customers. The **reactions** are provided in the form of emoji.

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

For mitigating faults the system **MUST** work in accordance with Active-Active approach where Main and DR MUST be placed in
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
| Average post reading (per user per day)                      | 50 posts every day per user                                                                                                     |
| Post size limit                                              | 200 chars                                                                                                                       |  
| Max number of reaction (estimate)                            | 10 millions                                                                                                                     | 
| Post loading page                                            | 10 posts on demand                                                                                                              |

**Note**: size of the metadata **MUST** be calculated based on the data structures design.

#### 2.2.3. Maintainability

#### 2.2.3.1. Customer supporting

Level 0: Self-Support.
Level 1: Basic Support.
Level 2: Troubleshooting.
Level 3: Expert Support.
Level 4: External Support.

#### 2.2.3.2. Proactive maintainability

Artificial intelligence (AI) **MUST** be used for proactive maintainability: contextual recommendations MUST be provided to
a customer based on the customer behaviour model.

#### 2.2.4. Documenting of the architectural solution

The architectural design **MUST** be composed in accordance with the recommendations of the TOGAF Standard (10th edition).



