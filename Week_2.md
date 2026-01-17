```python
import pandas as pd
import os
employee_payroll = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/Employee_Payroll2.csv')

#1. COUNT: to count all the rows in the table

#Example: 
sqldf("SELECT COUNT(*)
FROM employee_payroll")

sqldf("SELECT COUNT(*)
FROM employee_payroll
      WHERE [Employee_Gender] = 'M'")

#2. SUM: to calculate the sum of all the instances in a column
#Syntax:
#sqldf("select sum(<column name>) from <table name>")

#Example:
sqldf("SELECT SUM(Salary)
FROM employee_payroll")

## alt average calc 



#3. MAX: maximum value of an attribute
#Syntax:
#sqldf("select max(<column name>) from <table name>")

#Example:
sqldf("SELECT MAX(Salary)
FROM employee_payroll
      WHERE [Employee_Gender] = 'M'")

#4. MIN: minimum value of an attribute
#Syntax:
#sqldf("select min(<column name>) from <table name>")

#Example:
sqldf("SELECT MIN(Salary)
FROM employee_payroll
      WHERE [Employee_Gender] = 'F'")

#5. AVERAGE: to calculate the average of all the instances in a column
#Syntax:
#sqldf("select avg(<column name>) from <table name>")

#Example:
sqldf("SELECT AVG(Salary)
FROM employee_payroll")

sqldf("SELECT SUM(Salary) / COUNT(*)
FROM employee_payroll")

# BIG BANG THEORY Exercise

# DOWNLOAD THE FILE "bigbangtheory.xlsx" FROM CANVAS
# CONVERT TO A CSV FILE
# LOAD INTO AN R DATAFRAME
bigbang <- read.csv("bigbangtheory.csv", header=TRUE)
str(bigbang)
head(bigbang)

#1. How many characters are in the cast?  How many female characters?


## example of "pure" SQL code
sqldf("SELECT COUNT(*) AS CastSize
FROM bigbang")


#2. What are the occupations of characters?
sqldf("SELECT DISTINCT job AS Occup
FROM bigbang")


#3. What is the average IQ of the cast?
sqldf("SELECT AVG(IQ) AS AvgIQ
FROM bigbang")


#4. What is the mean IQ of the males?
sqldf("SELECT AVG(IQ) AS AvgIQ
FROM bigbang
      WHERE gender = 'Male'")



#5. What is the mean IQ of the biologists?

## THESE THREE APPROACHES WILL PRODUCE THE SAME RESULT
sqldf("SELECT AVG(IQ) AS AvgIQ
FROM bigbang
      WHERE job = 'Neurobiologist' or job = 'Microbiologist'")


sqldf("SELECT AVG(IQ) AS AvgIQ
FROM bigbang
      WHERE job like 'Neurobiologist' or 'Microbiologist'")



#GROUP BY example

## USING THE BIGBANGTHEORY DATA, DISPLAY THE MEAN IQ BY GENDER, AND JOB
sqldf("SELECT gender, AVG(IQ) AS AvgIQ
FROM bigbang
      GROUP BY gender")


#HAVING example



## USING THE BIGBANGTHEORY DATA, DISPLAY THE MEAN IQ FOR THE OCCUPATIONS WITH AN IQ GREAT THAN 170 
sqldf("SELECT job, AVG(IQ) AS AvgIQ
FROM bigbang
      GROUP BY job
      HAVING IQ > 170")




#CASE and INSTR -- POWERPOINT


## WORKSHOP QUESTION
#CONDITIONAL BONUS 
#You need to modify the previous bonus report to conditionally calculate bonuses based on the employee’s job title. 
#Level I employees receive a 5% bonus.
#Level II employees receive a 7% bonus.
#Level III employees receive a 10% bonus.
#Level IV employees receive a 12% bonus.
#All others receive an 8% bonus.
#The Staff table contains all of the information that you need to create this report.
staff <- read.csv("Staff.csv", header=TRUE)
head(staff)

sqldf("SELECT DISTINCT Job_Title
      FROM staff")


sqldf("SELECT Job_Title, Salary, 
CASE WHEN INSTR(Job_Title, 'IV') > 5 THEN Salary * 0.12
WHEN INSTR(Job_Title, 'III') > 5 THEN Salary * 0.1
WHEN INSTR(Job_Title, 'II') > 5 THEN Salary * 0.07
WHEN INSTR(Job_Title, 'I') > 5 THEN Salary * 0.05
ELSE Salary * 0.08 END AS Bonus
      FROM staff")


#CASE statement works as: as long as the data pass the test, it stops without looking at the rest rows



## EXTRA QUESTION
## THE WRITING TEAM HAS DECIDED TO GIVE JOKES TO THE BIG BANG THEORY CHARACTERS BASED ON THEIR JOB.  WRITE A ROUTINE USING "CASE" AND "INSTR" TO ASSIGN THE NUMBER OF JOKES ACCORDING TO THIS RULE:  PHYSICISTS GET 8 JOKES, BIOLOGISTS GET 10 JOKES, EVERYONE ELSE GETS 5 JOKES.
## DISPLAY THE NAME, JOB AND NUMBER OF JOKES
sqldf("SELECT name, job, 
CASE WHEN INSTR(LOWER(job), 'physicist') THEN 8 
WHEN INSTR(LOWER(job), 'biologist') THEN 10
ELSE 5 END AS Jokes
FROM bigbang")



### SPOILER ALERT!!!  ANSWER CODE BELOW! 
###  TRY TO WRITE YOUR CODE BEFORE PEEKING!























sqldf("SELECT DISTINCT job FROM bigbang")

# > sqldf("SELECT DISTINCT job FROM bigbang")
# job
# 1      Physicist
# 2 Neurobiologist
# 3 Astrophysicist
# 4 Microbiologist
# 5       Engineer
# 6      Sales Rep


sqldf("SELECT name, job,
      CASE WHEN INSTR(LOWER(job), 'physicist') <>0 THEN 8
      WHEN INSTR(LOWER(job), 'biologist')<>0 THEN 10
      ELSE 5 END as Jokes FROM bigbang")

