import pandas as pd
import os

print('Hello')

os.getcwd()
# os.chdir('directory path')

irisdata = pd.read_csv('/Users/eloisefeng/Desktop/NU/Data Management/iris.csv')
irisdata.loc[(irisdata['Species'] == 'virginica') & (irisdata['Sepal.Length'] > 7.0)]

print(irisdata[irisdata['Species'].str.startswith('set')])
