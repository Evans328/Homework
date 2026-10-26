# 7.1 HW Questions
1. Using EVregistry, Write a query to select the `ModelYear`, `Make`, and `Model` off all of the vehicles in the registry.

```sql
SELECT "ModelYear", Make, Model
FROM EVRegistry
```
2. Using the EVRegistry table, Write a query that lists all of the unique types of EV's. your reult set should have one column, `ElectricVehicleType`.
```sql
SELECT "Electric Vehicle Type"
FROM EVRegistry
ORDER BY "Electric Vehicle Type";
```

3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry.

```sql
SELECT *
FROM EVRegistry
WHERE ElectricVehicleType LIKE '%BEV%';

```

4. Using the EVRegistry, wirte a query that returns the `Make` and `Model` of all of the EV's that have a BaseMSRP between 20000 and 35000?

```sql
SELECT Make, Model, "Base MSRP"
FROM EVRegistry
WHERE "Base MSRP" BETWEEN 20000 AND 35000;
---
Or take Base MSRP from the first line to show just the make and model.

```
# 7.2 HW Questions 

1. Using EVRegistry, write a query to find a record  where the `City` attribute is NULL. Return all of the available columns. 
```sql
SELECT *
FROM EVRegistry
WHERE City IS NULL
```

2. Write a query to find the `make`, `model`, and `ElectricVehicleType` where the VIN number has  that ends in '3E1EA1J'.
```sql
SELECT Make, Model, ElectricVehicleType
FROM EVRegistry
WHERE VIN LIKE '%3E1EA1J';
```

3. Select the `ModelYear`, `make`, `model`, `ElectricVehicleType`, and `range` of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest. 
```sql

SELECT ModelYear, Make, Model, ElectricVehicleType, ElectricRange
FROM EVRegistry
WHERE Make IN ('TESLA', 'CHEVROLET')
ORDER BY ModelYear, Make DESC;
```

4. Using EVCharging, Write a query to find out how many many times those stations were used. Order them by the most used to the least used and limit the output to 5 records. 
```sql
SELECT stationId, COUNT(*) AS TimesUsed
FROM EVCharging
GROUP BY stationId
ORDER BY TimesUsed DESC
LIMIT 5;
```
5.  Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. 
```sql

SELECT userId,
       MIN(chargeTimeHrs) AS minTime,
       MAX(chargeTimeHrs) AS maxTime
FROM EVCharging
WHERE chargeTimeHrs > 0.5
GROUP BY userId
ORDER BY minTime, maxTime;
```

# Before moving on with the rest of the questions please set up the new database
1. in SQLlite close any open DB
2. file----> Open Database
3. Choose SavvyCoders_SQL_chargeDB.db from the resource repository from this section in the curriculum
4. Make sure that you have 5 tables: 
    - dimDay 
    - dimFacility
    - dimUser
    - factCharge
    - EVCharging


# 7.3 HW Questions

1. Using EVCharging, Which day of the week has the highest average charging time? Round the answer to 2 decimal points.
```sql
SELECT weekday,
       ROUND(AVG(chargeTimeHrs), 2) AS avgChargeTime
FROM EVCharging
GROUP BY weekday
ORDER BY avgChargeTime DESC
```


2. Using, EV charging, Find the total power consumed from charging EV's by each User. Return the `userId` and name the calculated column, `totalPower`. Round the answer to 2 deciaml points and list the out put in highest to lowest order. Limit the order to the top 15 users. 
```sql
SELECT userId,
       ROUND(SUM(kwhTotal), 2) AS totalPower
FROM EVCharging
GROUP BY userId
ORDER BY totalPower DESC
LIMIT 15;
```
3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return `type Facility` and name the calculated column `numStation`. Order the result set from highest to lowest number of charging stations.  
```sql
SELECT f.typeFacility,
       COUNT(DISTINCT c.stationID) AS numStation
FROM factCharge AS c
JOIN dimFacility AS f ON c.facilityID = f.FacilityKey
GROUP BY f.typeFacility
ORDER BY numStation DESC;
```
4. In your own words, Briefly explain Primary Keys and Foreign Keys. 

- A primary key is the column that identifies each row in a table and a foreign key is a column in a different table that stores the same ID and join the tables together. A persons work ID could be a primary key and that same ID on paperwork could be a foreign ID. It just points back to that person.

5. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. HINT: USE `HAVING`

```SQL
SELECT userId, MIN(chargeTimeHrs) AS minTime, MAX(chargeTimeHrs) AS maxTime
FROM EVCharging
GROUP BY userId
HAVING MIN(chargeTimeHrs) > 1
ORDER BY MIN(chargeTimeHrs) ASC, MAX(chargeTimeHrs) ASC;
```

## When done with homework

When all the week #3 homework has been completed, do the following...

- Make sure your homework file is well named
- Add the homework to your Homework folder
- Use  `git add .`, `git commit -m "message"`, and `git push` to upload your homework changes to GitHub in the cloud
- Create a JIRA ticket with your homework repo url, to indicate that your Homework is ready for review
