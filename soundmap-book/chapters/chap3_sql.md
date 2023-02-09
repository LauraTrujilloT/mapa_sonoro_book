# SQL - Date, Datetime, and Time Manipulations

```{important}
Often timestamps in the database are not encoded with the time zone, and you will need to consult with the source or developer to figure out how your data was stored. UTC has become most common in the data sets, but that is certainly not universal.
```

## Time Zone Manipulations

All local time zones have a UTC offset. Timestamps in databases are stored in the format `YYYY-MM-DD hh:mi:ss`. Timestamps with the time zone have an additional piece of information for the UTC offset, expressed as a positive or negative number. Converting from one time zone to another can be accomplished with at `time zone` followed by the destination time zone's abbreviation.

```sql 
SELECT 
    '2020-09-01 00:00:00 -0' at time zone 'pst';
FROM table_name
----
timezone
-----
2020-08-31 16:00:00
```

## Date and Timestamp Format Conversions

**Current Date or Time**

Returning the current date or time is a common analysis task. They are referred to as _system time_, and while returning them is easy to do with SQL, there are some syntax differences between databases.

```sql
current_time
localtimestamp
get_date()
now()
localtime
timeofday()
```

To reduce the granularity of a timestamp, use the `date_trunc` function.

```sql
date_trunc(text, timestamp)

SELECT date_trunc('month', '2020-10-04 12:33:35'::timestamp);
---
date_trunc
---
2020-10-01 00:00:00
```

Databases that don't support `date_trunc` such as MySQL, have an alternate function `date_format`

```{important}
If you ever encounter timestamps stored as Unix epochs, you can convert them to timestamps using the `to_timestamp` function
```

**Date Math**

Date math involves two types of data: the dates themselves and intervals. We need the concept of intervals because date and time components don't behave like integers. Intervals allow us to move smoothly between units of time. Intervals come in two types: year-month intervals and day-time ones. 

| action          |   syntax        |   DB          |
|-----------------|-----------------|---------------|
| Subtraction     | `- ; datediff`  | MySQL; Alchemy|
| Subtraction     | `date_part ('granularity',age(date1, date2))`  | Posgres|
| Addition        | `+ interval 'x granularity'`  | almost every DB|
| Addition        | `date_add`  | Limited DB|

**Time Math**

Time math is less common in many areas of analysis, but it can be useful in some situations. Whenever the elapsed time between two events is less than a day, or when rounding the result to a number of days doesn't provide enough information, time manipulation comes into play. Time math works similarly to date math, by leveraging intervals.

```{admonition} Remember
Dates and timestamps that are in different formats can be standarized with SQL. JOINing on dates or including date fields in UNIONs generally requires that the dates or timestamps be in the same format. Earlier in the chapter, I showed techniques for formatting dates and timestamps that will serve well with these problems. Take care with time zones when combining data from multiple sources. For example, an internal database may use UTC time, but data from a third party could be in a local time zone. I have seen data sourced from SaaS that was recorded in a variety of lcoal times. Note that the timestamp values themselves won't necessarily have the time zone embedded. You may need to consult the vendor's documentation and convert data to UTC if the rest of your data is stored that way. Another optio is to store the time zone in a field so that the timestamp value can be converted as needed.
```