# pandas_learning

## chapter1

学习要点：

1. `sort_values([], ascending=False)` 对数据进行排序

## chapter2

学习要点：

1. 布尔运算 `chipo.loc[(chipo.quantity > 1) & (chipo.item_name == 'Canned Soda'),:].shape[0]` 这里记得与运算要加括号

2. `euro12[euro12.Goals > 6]`可以直接使用布尔运算

3. 字符串操作 `euro12[euro12.Team.str.startswith('G')]`可以选择以某个字母开始的行

4. 判断某个元素是否在要求中，比如只要求行是俄罗斯的，美国的。`euro12.loc[euro12.Team.isin(['England', 'Italy', 'Russia']), ['Team','Shooting Accuracy']]`。还有另外一种做法是直接使用set_index然后索引

## chapter3

1. `drinks.groupby('continent').spirit_servings.agg(['mean', 'min', 'max'])`使用agg进行某些操作

2. stack and unstack `regiment.groupby(['regiment', 'company']).preTestScore.mean().unstack()`

==unstack 就是将树状结构变成不是树状结构，groupby通常是树状结构==

```
regiment    company
Dragoons    1st         3.5
            2nd        27.5
Nighthawks  1st        14.0
            2nd        16.5
Scouts      1st         2.5
            2nd         2.5
Name: preTestScore, dtype: float64
```

```
company	1st	2nd
regiment		
Dragoons	3.5	27.5
Nighthawks	14.0	16.5
Scouts	2.5	2.5
```
3. for 循环打印groupby，groupby之后虽然返回的是一个对象。但是我们可以使用for循环打印出来

```
for name, group in regiment.groupby('regiment'):
    # print the name of the regiment
    print(name)
    # print the data of that regiment
    print(group)
```

## chapter4

1. columns也可以切片索引

```
stud_alcoh = df.loc[: , "school":"guardian"]
stud_alcoh.head()
```

2. 时间序列

将某列转换成时间序列

```
crime.Year = pd.to_datetime(crime.Year, format='%Y')

crime = pd.read_csv(url, parse_dates=[0])

```

3. 删除某列 del

4. dataframe.idxmax 参考 https://www.cjavapy.com/article/513/

```
      A    B     C
0    4    11    1
1    5    2     8
2    2    5     66 
3    6    8     4


df.idxmax(axis = 0) 

A      3
B      0
C      2
dtype: int64
```

## chapter5

1. df1.append(df2, axis=0) 默认是上下合并。1就是左右合并

2. 生成随机数 `nr_owners = np.random.randint(15000, high=73001, size=398, dtype='l')`

3. 和append类似 `pd.concat([data1, data2], axis = 1)`

4. on表示相同列合并 `pd.merge(all_data, data3, on='subject_id')`。会缺失数据 `pd.merge(data1, data2, on='subject_id', how='outer')`也会缺失数据。但是会用None填充

5. 给列重新命名 `housemkt.rename(columns = {0: 'bedrs', 1: 'bathrs', 2: 'price_sqr_meter'}, inplace=True)`

6. 重新设置index `bigcolumn.reset_index(drop=True, inplace=True)`

## chapter6

1. Series计数Count

- pd.Series.Count.idxmax()
- pd.Series.Count.median()
- pd.Series.Count一般和groupby连用。本教程中index是名字。特征是名字出现次数count。idxmax可以返回count出现最多的name

2. 文件读取，如果数据有多个空格 `data = pd.read_csv(data_url, sep = "\s+", parse_dates = [[0,1,2]]) `

3. 时间序列 。处理时间序列，一般将时间设置为index。如果数据中的时间出现错误怎么办 .==好好看看这一章==

```
def fix_century(x):
  year = x.year - 100 if x.year > 1989 else x.year
  return datetime.date(year, x.month, x.day)

# apply the function fix_century on the column and replace the values to the right ones
data['Yr_Mo_Dy'] = data['Yr_Mo_Dy'].apply(fix_century)
```

4. 选取月份为1的数据 `data.loc[data.index.month == 1].mean()` 

5. 时间频率 

```
（freq='M’月，'D’天，‘W’，周，'Y’年）

data.groupby(data.index.to_period('A')).mean() # 按年，只精确到年。月这些不会算

data.groupby(data.index.to_period('M')).mean() # 按年月。天不会算

data.groupby(data.index.to_period('W')).mean() # 按年月周。
```

