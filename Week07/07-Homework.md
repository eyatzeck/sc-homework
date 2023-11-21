# SECTION 07 HW 
# Elena Yatzeck
<br>

# 7.1 HW Questions 

1. Using EVregistry, Write a query to select the `ModelYear`, `Make`, and `Model` off all of the vehicles in the registry.
## Question 1 (assumes you want unique combinations, not all the info without vins)
```SQL

select distinct ModelYear, Make, Model from evRegistry

```
2. Using the EVRegistry table, Write a query that lists all of the unique types of EV's. your reult set should have one column, `ElectricVehicleType`. 
```SQL

select distinct ElectricVehicleType from evRegistry

```
3. Using the EVRegistry, Write a query that shows all of the information on Battery Electric Vehicles (BEV) that are in the registry. 
```SQL

select * from  evRegistry where ElectricVehicleType = 'Battery Electric Vehicle (BEV)'

```
4. Using the EVRegistry, wirte a query that returns the `Make` and `Model` of all of the EV's that have a BaseMSRP between 20000 and 35000?  
```SQL

select Make, Model from  evRegistry where BaseMSRP between 20000 and 350007

```
# 7.2 HW Questions 

1. Using EVRegistry, write a query to find a record  where the `City` attribute is NULL. Return all of the available columns. (got no rows)
```SQL

select * from  evRegistry where City is NULL

```

2. Write a query to find the `make`, `model`, and `ElectricVehicleType` where the VIN number has  that ends in '3E1EA1J'.
```SQL

select Make, Model, ElectricVehicleType from evRegistry where VIN like '%3E1EA1J'

```
3. Select the `ModelYear`, `make`, `model`, `ElectricVehicleType`, and `range` of the Tesla vehicles or cheverolet vehicles in the registry. Order the result set by Make and Model year in from newest to oldest. 
```SQL

select ModelYear, Make, Model, ElectricVehicleType, ElectricRange from evRegistry where make in ('TESLA','CHEVROLET') order by Make, ModelYear DESC

```
4. Using EVCharging, Write a query to find out how many many times those stations were used. Order them by the most used to the least used and limit the output to 5 records. 
```SQL
select stationId, count(sessionId) as session_count from evCharge group by stationId order by session_count desc limit 5

```
5.  Using EVCharging, For the folks who charged longer than 0.5 hours, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. 
```SQL
select distinct userId, min(ChargeTimeHrs) as minTime, max(ChargeTimeHrs) as maxTime
from evCharge 
where userId in (select userId from EVCharge where chargeTimeHrs > .5)
group by userId order by minTime, maxTime

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

1. Using EVCharging, Which day of the week has the highest average charging time? Round the answer to 2 decimal points.  Answer is Wednesday, 2.94 hours.
```SQL
select weekday, round(avg(ChargeTimeHrs),2) as avgChg from evCharging group by weekday order by avgChg  desc limit 1

```
2. Using, EV charging, Find the total power consumed from charging EV's by each User. Return the `userId` and name the calculated column, `totalPower`. Round the answer to 2 deciaml points and list the out put in highest to lowest order. Limit the order to the top 15 users. 
```SQL
select userId, round(sum(kwhTotal),2) as totalPower from EVCharging group by userId order by totalPower desc limit 15

```
3. Using dimfacility and factCharge, write a query to find out which type of facility (GROUP BY) has the most amount of charging stations. Return `type Facility` and name the calculated column `numStation`. Order the result set from highest to lowest number of charging stations.  
```SQL
select dF.typeFacility as typeFacility,count(fC.stationID) as numStation
from dimFacility dF, factCharge fC
where df.FacilityKey = fC.facilityID
group by typeFacility
order by numStation DESC

```
4. In your own words, Briefly explain Primary Keys and Foreign Keys. 

A primary key is the column (or set of columns) which uniquely defines a row
in a table.  For example, table 'person' could have columns person_id, first_name, last_name, age, eye_color, etc.  In this table, the primary key would be person_id

A foreign key is a column in a table which refers to the primary key of another table.  For example, table 'address' could have columns address_id, person_id, street, city, zip.  In this table, person_id would be a foreign key.

5. Using EV Charging, For the folks who charged longer than one hour, show the min and max of the charging time for each user. Your output columns should be `userid`, `minTime`, and `maxTime`. Order this result set by the last two columns respectively. HINT: USE `HAVING`
```SQL
select userId, 
round(min(chargeTimeHrs) ,2) as minTime, 
round(max(chargeTimeHrs) ,2) as maxTime 
from EVCharging
where userId in (select userId from EVCharging where chargeTimeHrs > .5)
group by userId
order by minTime desc, maxTime desc
```

## When done with homework

When all the week #3 homework has been completed, do the following...

- Make sure your homework file is well named
- Add the homework to your Homework folder
- Use  `git add .`, `git commit -m "message"`, and `git push` to upload your homework changes to GitHub in the cloud
- Create a JIRA ticket with your homework repo url, to indicate that your Homework is ready for review
