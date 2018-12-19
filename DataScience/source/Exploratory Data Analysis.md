# 探索性数据分析

## 对单独一列信息的探索
### 去一列信息去重后显示（去掉该列信息中重复的值）

```python
print('Discount_rate 类型:',dfoff['Discount_rate'].unique()) #dfoff是打开的某pandas文件
```

### 统计该列中各个值出现的次数
使用`value_counts()`方法就可以。这个方法可以用于计数。这是Pandas内置的方法，用Pandas读取完文件之后就可以直接调用这个方法。

```python
dfoff['label'].value_counts()
```


## 比较两列的数据信息（对重复数值聚合）以及画图显示

### 比较两列的数据信息

我们在探索性数据分析的时候常常需要比较某两个特征之间的关系。这时候往往需要用到`Pandas`的`groupby`方法。

#### 时间序列作为排序准则对特征数据进行比较

比如说对于这样的数据，我们想按照Data_received的时间日期对其进行统计，同一时间日期下有多少个Date：

![image](https://github.com/Einstellung/DataScience_learning/blob/master/DataScience/images/Exploratory%20Data%20Analysis/1.png?raw=true)

```python
couponbydate = dfoff[dfoff['Date_received'] != 'null'][['Date_received', 'Date']].groupby(['Date_received'], as_index=False).count()
```
统计结果会像这样：
```
Date_received	Date
0	20160101	554
1	20160102	542
2	20160103	536
3	20160104	577
4	20160105	691
5	20160106	808
```
这里说一下`as_index`这个参数，如果改成`True`的话，被groupby的那一列会单独成为一列。如果是`False`的话，则不会单独成为一列。

## 对多列数据进行分析

### 显示总计有多少个特征

一般情况下，数据的第一行表示的是特征。如果想用列表的形式显示特征可以使用Pandas的`tolist()`进行转换：

```python
dfoff.columns.tolist() #dfoff是用Pandas读取的csv数据
```



