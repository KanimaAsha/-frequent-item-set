# -frequent-item-set
# Project using market basket analysis

import pandas as pd
import numpy as np
from apyori import apriori
df=pd.read_csv('Market_Basket_Optimisation.csv',header=None)
print(df)

#replacing empty value with 0.
df.fillna(0,inplace=True)
df.head()

#for using apriori need to convert data in list format..
#transaction=[['apple','almond'],['apple'],['banana','apple']]...

transactions=[]
for i in range(0,len(df)):
    transactions.append([str(df.values[i,j])for j in range(0,20) if str(df.values[i,j])!='0'])
transactions[0]

#call apriori function which requires minimum support,confidence and lift, min length is combination of item default is 2".

rules=apriori(transactions, min_support=0.003, min_confidence=0.2, min_lift=3, min_length=2)

#all rules to be converted in a list..

Results=list(rules)
Results

#convert result in a dataframe for further operation....

df_results=pd.DataFrame(Results)

#as we see order statistics itself a list so need to be converted in a proper format...

df_results.head()
#keep support in a seperate dataframe so we can use later...

support=df_results.support

'''
convert orderstatistic in a proper format..
order statistic has lhs=>rhs as well rhs=>lhs we can choose any one for convience i choose first one which is 'df_results['ordered_statistics'][i][0]'
'''

#all four empty list which will contain lhs,rhs,confidence and lift respectively.
first_values=[]
second_values=[]
third_values=[]
fourth_values[]

#loop number of rows time and append 1 by 1 value in a separate list... first and second elements was frozenset which needed to be converted in list...

for i in range(df_results.shape[0]):
     single_list=df_results['ordered_statistics'][i][0]
     first_values.append(list(single_list[0]))
     second_values.append(list(single_list[1]))
     third_values.append(single_list[2])
     fourth_values.append(single_list[3])

#convert all four list into dataframe for further operation...

lhs=pd.DataFrame(first_values)
rhs=pd.DataFrame(second_values)
confidence=pd.DataFrame(third_values, columns=['confidence'])
lift=pd.DataFrame(fourth_value,columns=['lift'])

#concat all list together in asingle dataframe
df_final=pd.concat([lhs,rhs,support,confidence,lift],axis=1)
df_final

'''
we have some of place only 1 item in lhs and some place 3 or more so we need a proper representation for user to understand.
removing none with ' ' extra so when we combine three column in 1 then only 1 item will be there with spaces which is proper rather than none ..
example: coffee, none, none which converted to coffee...
'''

df_final.fillna(value=' ', inplace=true)

#this is final output... you can sort based on the support lift and confidence...
df_final.head()
print(df_final)
