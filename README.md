# High-load Architecture Course Project

The modal verbs in the document are used in accordance with the [RFC-2119](https://datatracker.ietf.org/doc/html/rfc2119)

## The task description

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
  3. fuzzy search by posts in the specified feed.

