# Namma Yatri

## Overview
Namma Yatri is a SQL Server-based project aimed at analyzing ride-sharing trips to extract meaningful insights. By leveraging SQL queries, we can derive key statistics related to trips, cancellations, earnings, and driver-customer interactions.

## Features
- Analyze trip data including fare, distance, and duration.
- Identify the most frequently used routes.
- Track cancellations by customers and drivers.
- Compute total earnings and top-performing drivers.
- Extract insights into user behavior and payment methods.

## Technologies Used
- **SQL Server** (for database management and querying)

## Installation
### 1. Clone the repository:
  ```bash```
  git clone https://github.com/yourusername/namma-yatri.git
###2. Navigate to the project directory:
  ```bash```
  cd namma-yatri
### 3. Set up the SQL Server database:
  ```sql```
  CREATE DATABASE namma_yatri;
### 4. Load the provided SQL file into the database:
  ```sql```
 USE namma_yatri;
 GO
 :r namma_1.sql
### 5. Run SQL queries using SQL Server Management Studio (SSMS) or another SQL client.

## How It Works
1. The SQL Server database stores trip-related data
2. Queries extract useful statistics such as trip frequency, earnings, and cancellations.
3. Insights help optimize operations for better ride-sharing efficiency.

## Sample Queries

### 1.Which area got the highest fares, cancellations,trips

select * from
(select * , rank() over(order by total_fare desc) rnk
from
(select loc_from,SUM(fare) total_fare from trips
group by loc_from)b)c
where rnk=1

select * from
(select * , rank() over(order by cnt desc) rnk
from
(
select loc_from,COUNT(*)-sum(driver_not_cancelled) as cnt from trips_details
group by loc_from)b)c
where rnk=1


### 2.Which driver , customer pair had more orders
select * from
(select *,rank() over(order by cnt desc) rnk from
(select driverid,custid,COUNT(distinct tripid) cnt from trips
group by driverid,custid)b)c
where rnk=1


### 3.which is the most used payment method 
select a.method from payment a
INNER JOIN
  (select top 1 faremethod,count(faremethod) as n from trips
   group by(faremethod)
   order by COUNT(faremethod) desc) b
   ON 
   a.id=b.faremethod;

   
### 4. Top 5 earning drivers   
select top 5 driverid,sum(fare),COUNT(driverid) from trips
group by driverid
order by SUM(fare) desc;

## Future Enhancements

- Integration with real-time analytics.
- Visualization dashboards.
- Machine learning models for ride demand prediction.


   
   


