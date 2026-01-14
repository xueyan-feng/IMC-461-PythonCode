```python #必须在开头规定语言才能有对应的高亮
import pandas as pd
import os   #import是每次开文档都要重新写的
#os查看文件、路径等等，标准库

print('Hello')

os.getcwd()
# os.chdir('directory path')

pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/iris.csv')

irisdata = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/iris.csv')

irisdata.info()  #查看数据表格的行列属性

irisdata.head(5)

len(irisdata)    #表格行数，等同于count(*)

irisdata.loc[(irisdata['Species'] == 'virginica') & (irisdata['Sepal.Length'] > 7.0)]
#.loc[row,column]
#.loc[:,:]等同于select *
#例如irisdata.loc[irisdata[‘species’] == ‘virginica’] 等同于select species from irisdata where species = ‘virginica’
#在筛选选列或者对筛选结果赋值的时候必须要加，语义更清楚
#外层的irisdata是告诉pandas要从哪个表里取行，内层的irisdata是用这个表来生成筛选条件

irisdata.loc[irisdata['Species'].str.startswith('set')]

irisdata.loc[irisdata['Species'].str.endswith('color')]

irisdata.loc[:,['Petal.Length', 'Petal.Width']].head(5)
#此处是按照loc[行索引,列索引]来写的，所以Python知道此处的名称为列名，故不需要加内层的irisdata

irisdata.loc[:, ['Petal.Length', 'Petal.Width']].rename(columns={'Petal.Length': 'PLeng', 'Petal.Width': 'PWid'}）
#()调用函数，传参数
#[]索引，取子集
#{}字典，映射

#最好写成irisdata_renamed = irisdata.rename(columns = {'petal_length': 'PLeng', 'petal_width': 'PWidth'})
#此种写法会将改名的内容保存在一个新的副本里
#如果是在原数据里改，则可以在()内的最后面加上inplace = True，但这种写法后面将无法加链式（例如加上.head()），因为rename(..., inplace=True)返回的是None

irisdata['Species'].unique()

iris_renamed = irisdata.rename(columns={'Petal.Length': 'PLeng'})
iris_renamed.sort_values(by='PLeng', ascending=False).head(25)
#不写ascending=False时默认按ascending排序，ascending=False表示按descending排序
#如果需要按两列排序，则写irisdata.sort_values(by = ['Petal.Length', 'Petal.Width'])


#Your Turn

employee_payroll = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/Employee_Payroll2.csv')

employee_payroll.info()

employee_payroll.loc[employee_payroll['Employee_Gender'] == 'F', ['Employee_ID', 'Employee_Gender', 'Salary']].sort_values(by = 'Salary', ascending = False)

employee_payroll['Bonus'] = employee_payroll['Salary'] * 0.1
employee_payroll.loc[:, ['Employee_ID', 'Salary', 'Bonus']]

employee_payroll.loc[(employee_payroll['Employee_Gender'] == 'M') & (employee_payroll['Salary']>35000), ['Employee_ID', 'Employee_Gender', 'Salary']].sort_values(by= 'Salary', ascending=False)

employee_payroll['Bonus'] = 5000
employee_payroll['Earnings'] = employee_payroll['Bonus'] + employee_payroll['Salary']
employee_payroll.loc[:, ['Employee_ID', 'Salary', 'Bonus', 'Earnings']]
