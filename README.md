# IntermediateSQL
Demonstrate usage of Intermediate knowledge of SQL 

--Inner Joins, Full/Left/Right Outer Joins
SELECT * FROM EmployeeDemographic;

-- Joining the EmployeeSalary table with the EmployeeDemographic table based by employeeID
SELECT * FROM EmployeeDemographics
INNER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID;

SELECT * FROM EmployeeDemographics
FULL OUTER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID;

--Adding into EmployeeDemographics Table is understand the use of JOIN with Null values
INSERT INTO EmployeeDemographics VALUES
(1010,'Phyllis','Lapin-Vance',37,'Female'),
(1011,'Ryan','Howard',26,'Male'),
(1012,'Holly','Flax',30,'Female'),
(1013,'Darryl','Philban',29,'Male')

SELECT * FROM EmployeeDemographics
FULL OUTER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID;
--Full Outer Join is showing us null value because our statements output is to show us values from the EmployeeDemographic table and EmployeeSalary table regardless if there is a value to match.

--Organizing data visualally by EmployeeID.EmployeeDemographic in Ascending orderbecause I rearranged the table by INSERTing and DELETE(ing) new values 
SELECT * FROM EmployeeDemographics
FULL OUTER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
Order BY EmployeeID ASC;

SELECT * FROM EmployeeDemographics
LEFT OUTER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
Order BY EmployeeID ASC;

SELECT * FROM EmployeeDemographics
RIGHT OUTER JOIN EmployeeSalary
on EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
Order BY EmployeeID ASC;
-- Shows everything in the right table (EmployeeSalary) without values from the left table (EmployeeDemogrphic) because thtere is no matching value 

--Demonstrating various results using Union, Union All
Select*
FROM EmployeeDemographics
UNION 
SELECT*
FROM WarehouseEmployeeDemographics;

Select*
FROM EmployeeDemographics
UNION ALL
SELECT*
FROM WarehouseEmployeeDemographics;

Select*
FROM EmployeeDemographics
UNION ALL
SELECT*
FROM WarehouseEmployeeDemographics
ORDER BY EmployeeID;
--Combines the two tables into one row

--Demonstrating various resultsCase Statement
SELECT FirstName, LastName, Age,
CASE
	WHEN Age > 30 THEN 'Old'
	WHEN Age BETWEEN 27 And 30 THEN 'Young'
	ELSE 'Baby'
END
FROM EmployeeDemographics
Where AGE is NOT NULL
Order BY Age;

SELECT*,
CASE
	WHEN JobTitle = 'Salesman' THEN Salary + (Salary * .10)
	WHEN JobTitle = 'Accountant' THEN Salary + (Salary * .05)
	WHEN JobTitle = 'HR' THEN Salary +(Salary*.00001)
	ELSE Salary + (Salary *.03)
END AS SalaryAfterRaise
FROM EmployeeDemographics
JOIN EmployeeSalary
ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID;
-- Demonstrating a table with Case Statement, with a calculated column of SalaryAfterRaises

--Demonstrating various results Having Clause
SELECT JobTitle, COUNT(JobTitle)
FROM EmployeeDemographics
JOIN EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING COUNT(JobTitle) > 1;

SELECT JobTitle, AVG(Salary)
FROM EmployeeDemographics
JOIN EmployeeSalary
	ON EmployeeDemographics.EmployeeID = EmployeeSalary.EmployeeID
GROUP BY JobTitle
HAVING AVG(Salary) > 45000
ORDER BY AVG(Salary);
-- Displays a table with JobTitles that have an average of over $45000

-- Demonstrating various results Updating/Deleting Data
SELECT *
FROM EmployeeDemographics;

UPDATE EmployeeDemographics
SET Age = 31
WHERE FirstName = 'Holly' AND LastName = 'Flax';
-- Updated Holly's age from 30 to 31.

SELECT *
FROM WarehouseEmployeeDemographics;

DELETE FROM WarehouseEmployeeDemographics
WHERE EmployeeID = 1013; 

--Demonstrating various results Aliasing
SELECT FirstName AS Fname
FROM [EmployeeDemographics];

SELECT FirstName + ' ' + LastName AS FullName
FROM EmployeeDemographics;

SELECT Avg(Age) AS AvgAge
FROM EmployeeDemographics;

SELECT Demo.EmployeeID, Sal.Salary
FROM [EmployeeDemographics] AS Demo
JOIN [EmployeeSalary] AS Sal
	ON Demo.EmployeeID = Sal.EmployeeID;
	
-- Joining 3 seperate tables, and after each table we have an alias
SELECT Demo.EmployeeID, Demo.FirstName, Demo.FirstName, Sal.JobTitle, Ware.Age
FROM EmployeeDemographics AS Demo
LEFT JOIN EmployeeSalary AS Sal
	ON Demo.EmployeeID = Sal.EmployeeID
LEFT JOIN WarehouseEmployeeDemographics AS Ware
	ON Demo.EmployeeID = Ware.EmployeeID;
--offers more organized, and easily readable 

--Demonstrating various results using Partition By
SELECT FirstName, LastName, Gender, Salary
, COUNT(Gender) OVER (PARTITION BY Gender) as TotalGender
FROM EmployeeDemographics AS demo
JOIN EmployeeSalary AS sal
	ON demo.EmployeeID = sal.EmployeeID
-- partition by isolates just one column to perform aggragate function 
