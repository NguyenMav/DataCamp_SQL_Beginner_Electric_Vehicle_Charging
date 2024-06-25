## Project: Analyzing Electric Vehicle Charging Habits (SQL Beginner)

Linked to workbook: https://www.datacamp.com/datalab/w/fd3e95d3-5dd4-4715-b831-26be4cb3423d/edit

As electronic vehicles (EVs) become more popular, there is an increasing need for access to charging stations, also known as ports. To that end, many modern apartment buildings have begun retrofitting their parking garages to include shared charging stations. A charging station is shared if it is accessible by anyone in the building.

But with increasing demand comes competition for these ports — nothing is more frustrating than coming home to find no charging stations available! In this project, you will use a dataset to help apartment building managers better understand their tenants’ EV charging habits.

The data has been loaded into a PostgreSQL database with a table named `charging_sessions` with the following columns:

## charging_sessions

| Column | Definition | Data type |
|-|-|-|
|`garage_id`| Identifier for the garage/building|`VARCHAR`|
|`user_id` | Identifier for the individual user|`VARCHAR`|
|`user_type`|Indicating whether the station is `Shared` or `Private`| `VARCHAR` |
|`start_plugin`|The date and time the session started |`DATETIME`|
|`start_plugin_hour`|The hour (in military time) that the session started | `NUMERIC`|
|`end_plugout`|The date and time the session ended | `DATETIME` |
|`end_plugout_hour`|The hour (in military time) that the session ended | `NUMERIC`|
|`duration_hours`| The length of the session, in hours|`NUMERIC`|
|`el_kwh`| Amount of electricity used (in Kilowatt hours)|`NUMERIC`|
|`month_plugin`| The month that the session started |`VARCHAR`|
|`weekdays_plugin`| The day of the week that the session started|`VARCHAR`|

Let’s get started!

#### Sources
- **Data**: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0), via [Kaggle](https://www.kaggle.com/datasets/anshtanwar/residential-ev-chargingfrom-apartment-buildings),
- **Image**: Julian Herzog, [CC BY 4.0](https://creativecommons.org/licenses/by/4.0), via Wikimedia Commons

1. Find the number of unique individuals that use each garage’s shared charging stations. The output should contain two columns: 'garage_id' and 'num_unique_users'. Sort your results by the number of unique users from highest to lowest. Save the result as 'unique_users_per_garage'.

```sql
-- unique_users_per_garage
SELECT garage_id, COUNT(DISTINCT user_id) AS num_unique_users
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY garage_id
ORDER BY num_unique_users DESC;
```

| garage_id | num_unique_users |
|-----------|------------------|
| Bl2       | 18               |
| AsO2      | 17               |
| UT9       | 16               |
| AdO3      | 3                |
| MS1       | 2                |
| SR2       | 2                |
| AdA1      | 1                |
| Ris       | 1                |

2. Find the top 10 most popular charging start times (by weekday and start hour) for sessions that use shared charging stations. Your result should contain three columns: 'weekdays_plugin', 'start_plugin_hour', and a column named 'num_charging_sessions' containing the number of plugins on that weekday at that hour. Sort your results from the most to the least number of sessions. Save the result as 'most_popular_shared_start_times'.

```sql
-- most_popular_shared_start_times
SELECT weekdays_plugin, start_plugin_hour, COUNT(*) AS num_charging_sessions
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY weekdays_plugin, start_plugin_hour
ORDER BY num_charging_sessions DESC
LIMIT 10;
```

| weekdays_plugin | start_plugin_hour | num_charging_sessions |
|-----------------|-------------------|-----------------------|
| Sunday          | 17                | 30                    |
| Friday          | 15                | 28                    |
| Thursday        | 19                | 26                    |
| Thursday        | 16                | 26                    |
| Wednesday       | 19                | 25                    |
| Sunday          | 18                | 25                    |
| Sunday          | 15                | 25                    |
| Monday          | 15                | 24                    |
| Friday          | 16                | 24                    |
| Tuesday         | 16                | 23                    |


3. Find the users whose average charging duration last longer than 10 hours when using shared charging stations. Your result should contain two columns: 'user_id' and 'avg_charging_duration'. Sort your result from highest to lowest average charging duration. Save the result as 'long_duration_shared_users'.
```sql
-- long_duration_shared_users
SELECT user_id, AVG(duration_hours) AS avg_charging_duration
FROM charging_sessions
WHERE user_type = 'Shared'
GROUP BY user_id
HAVING AVG(duration_hours) > 10
ORDER BY avg_charging_duration DESC;
```

| user_id  | avg_charging_duration |
|----------|-----------------------|
| Share-9  | 16.84583334           |
| Share-17 | 12.89455555           |
| Share-25 | 12.21447475           |
| Share-18 | 12.08880719           |
| Share-8  | 11.55043084           |
| AdO3-1   | 10.36938697           |
