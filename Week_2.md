```python
import pandas as pd
import os
employee_payroll = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/Employee_Payroll2.csv')

#1. COUNT: to count all the rows in the table
len(employee_payroll)

len(employee_payroll.loc[employee_payroll['Employee_Gender'] == 'M'])


#2. SUM: to calculate the sum of all the instances in a column
employee_payroll.Salary.sum()

#3. MAX: maximum value of an attribute
employee_payroll.loc[employee_payroll['Employee_Gender'] == 'M', 'Salary'].max()
#可以把Salary也放进loc[]，会更简洁，仅需加上引号，不用再加[]

#4. MIN: minimum value of an attribute
employee_payroll.loc[employee_payroll['Employee_Gender'] == 'F', 'Salary'].min()

#5. AVERAGE: to calculate the average of all the instances in a column
employee_payroll.Salary.mean()

employee_payroll.Salary.sum() / len(employee_payroll)


# BIG BANG THEORY Exercise

# DOWNLOAD THE FILE "bigbangtheory.xlsx" FROM CANVAS
# CONVERT TO A CSV FILE
# LOAD INTO AN R DATAFRAME
bigbang = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/bigbangtheory.csv')
bigbang.info()
bigbang.head(5)

#1. How many characters are in the cast?  How many female characters?
len(bigbang)

len(bigbang.loc[bigbang.gender == 'Female'])


#2. What are the occupations of characters?
bigbang.job.unique()


#3. What is the average IQ of the cast?
bigbang.IQ.mean()


#4. What is the mean IQ of the males?
bigbang.loc[bigbang.gender == 'Male', 'IQ'].mean()


#5. What is the mean IQ of the biologists?
bigbang.loc[bigbang.job.isin(['Neurobiologist', 'Microbiologist']), 'IQ'].mean()

bigbang.loc[bigbang.job.str.contains('Neurobiologist|Microbiologist', case=False), 'IQ'].mean()
#注意str.contains()里只有一对引号，所有内容都要写在同一对引号内

bigbang.loc[bigbang.job.str.contains('biologist', case=False), 'IQ'].mean()
#法2和法3中的case=False都代表不管大小写


#GROUP BY example

## USING THE BIGBANGTHEORY DATA, DISPLAY THE MEAN IQ BY GENDER, AND JOB
bigbang.groupby('gender')['IQ'].mean().reset_index(name='AvgIQ')
#注意groupby()要放在前面（与sql相反）
#注意reset_index(name='AvgIQ')，给所得列加上列名


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



sqldf("SELECT name, job,
      CASE WHEN INSTR(LOWER(job), 'physicist') <>0 THEN 8
      WHEN INSTR(LOWER(job), 'biologist')<>0 THEN 10
      ELSE 5 END as Jokes FROM bigbang")

