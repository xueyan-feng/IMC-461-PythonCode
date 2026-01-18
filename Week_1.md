```python #必须在开头规定语言才能有对应的高亮
import pandas as pd
import os   #import是每次开文档都要重新写的
#os查看文件、路径等等，标准库

print('Hello')

os.getcwd()
# os.chdir('directory path')

# sample read.csv file code
pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/iris.csv')

#assign a data frame called irisdata
irisdata = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/iris.csv')

#display the fields and structure within a defined data frame
irisdata.info()  #查看数据表格的行列属性

irisdata.head(5)

len(irisdata)    #表格行数，等同于count(*)


# display all the data related to iris flowers that are of virginica species and also have Sepal.Length greater than 7.0
irisdata.loc[(irisdata['Species'] == 'virginica') & (irisdata['Sepal.Length'] > 7.0)]
#.loc[row,column]
#.loc[:,:]等同于select *
#例如irisdata.loc[irisdata[‘species’] == ‘virginica’] 等同于select species from irisdata where species = ‘virginica’
#在筛选选列或者对筛选结果赋值的时候必须要加，语义更清楚
#外层的irisdata是告诉pandas要从哪个表里取行，内层的irisdata是用这个表来生成筛选条件


# display rows with Species "setosa"
irisdata.loc[irisdata['Species'].str.startswith('set')]


# display rows with Species "versicolor"
irisdata.loc[irisdata['Species'].str.endswith('color')]


# I just want Petal Length and Petal Width
irisdata.loc[:,['Petal.Length', 'Petal.Width']].head(5)
#此处是按照loc[行索引,列索引]来写的，所以Python知道此处的名称为列名，故不需要加内层的irisdata


# I want to give my own names to the columns
irisdata.loc[:, ['Petal.Length', 'Petal.Width']].rename(columns={'Petal.Length': 'PLeng', 'Petal.Width': 'PWid'}）
#()调用函数，传参数
#[]索引，取子集
#{}字典，映射

#最好写成irisdata_renamed = irisdata.rename(columns = {'petal_length': 'PLeng', 'petal_width': 'PWidth'})
#此种写法会将改名的内容保存在一个新的副本里
#如果是在原数据里改，则可以在()内的最后面加上inplace = True，但这种写法后面将无法加链式（例如加上.head()），因为rename(..., inplace=True)返回的是None


# I want to know unique species of iris flower
irisdata['Species'].unique()


# Let's sort Petal Length in descending order
iris_renamed = irisdata.rename(columns={'Petal.Length': 'PLeng'})
iris_renamed.sort_values(by='PLeng', ascending=False).head(25)
#不写ascending=False时默认按ascending排序，ascending=False表示按descending排序
#如果需要按两列排序，则写irisdata.sort_values(by = ['Petal.Length', 'Petal.Width'])


#create new columns and perform calculations
iris_renamed = irisdata.rename(columns={'Petal.Length': 'length', 'Petal.Width': 'width'})
iris_renamed['ratio'] = iris_renamed['length'] / iris_renamed['width']
iris_renamed.sort_values(by=['length', 'width'], ascending=False).head(25)



#Your Turn
# Orion Employee Payroll Caselet
# Overall objective is to produce a report that contains the employee identifier, gender, and salary for all Orion Star employees. The data is contained in the
# Employee_Payroll table, a table with which you are not familiar.

# 1. Download the employee_payroll.csv file from Canvas and place in your working directory. Assign the file to a data frame called "employee_payroll"
employee_payroll = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/Employee_Payroll2.csv')

# 2. Determine the column names in the Employee Payroll file
employee_payroll.info()

# 3. Run a report to show the female employees, with the ID, gender, and salary, sorted by descending salary
employee_payroll.loc[employee_payroll['Employee_Gender'] == 'F', ['Employee_ID', 'Employee_Gender', 'Salary']].sort_values(by = 'Salary', ascending = False)

# 4. At the end of the year, Orion would like to give the employees a 10% bonus. Run a report that shows the employee ID,
#salary and what their bonus amount would be
employee_payroll['Bonus'] = employee_payroll['Salary'] * 0.1
employee_payroll.loc[:, ['Employee_ID', 'Salary', 'Bonus']]

# 5. Run a report to show the male employees with a salary over $35,000, with the ID, gender, and salary, sorted by descending salary
employee_payroll.loc[(employee_payroll['Employee_Gender'] == 'M') & (employee_payroll['Salary']>35000), ['Employee_ID', 'Employee_Gender', 'Salary']].sort_values(by= 'Salary', ascending=False)

# 6. After more discussion, Orion is changing the bonus scheme to be a straight $5,000 amount. Run a report that shows the employee ID, salary and
#what their bonus amount would be, and the salary + bonus amount
employee_payroll['Bonus'] = 5000
employee_payroll['Earnings'] = employee_payroll['Bonus'] + employee_payroll['Salary']
employee_payroll.loc[:, ['Employee_ID', 'Salary', 'Bonus', 'Earnings']]
