# 数据预处理

## 删除数据中的空缺值

数据中的空缺值导入到DataFrame中一般会以`NaN`或者`null`的形式进行显示。通常情况下数据清洗的第一步我们要先除掉这些空缺值。

ana是没有办法和任何数据进行比较的，它不和任何值相等，包括他自己（因此也可以用 a ！= a 来判断a是否是nan）。

所以在后面的数据处理中如果进行了比较操作则会报错：

```python
TypeError: invalid type comparison
```

所以有两种思路

**第一种：** 比如说要清除`Date_received`列的所有空缺值：

```python
dfoff = pd.read_csv('datalab/59/ccf_offline_stage1_train.csv')
dfoff[~(dfoff['Date_received'] != dfoff['Date_received'] )]
```
这种方法比较复杂，代码不易阅读，不推荐。

**第二种：** 在读取csv的时候加上参数`keep_default_na=False`，这样这样没有数据的条目就会被识别为空字符’ ‘而不是nan了。我们实现删除数据中的空缺值办法可以这样写：

```python
dfoff = pd.read_csv('datalab/59/ccf_offline_stage1_train.csv', keep_default_na=False)
dfoff[(dfoff['Date_received'] != 'null') & (dfoff['Date'] != 'null')] #将Date_received和Data列的空缺值都剔除
```

## 类型转换

有的时候可能导入的数据类型不能满足处理要求，这时候需要进行类型转换，比如`str`转换成`int `类型。

```python
df['distance'] = df['Distance'].replace('null', -1).astype(int)
```

## 对已有的列进行处理生成新的特征列

我们可以使用Pandas的`apply`方法对已有的列进行处理，该方法的第一个参数为func，也就是我们可以自己定义一个对列数据进行处理的函数，演示如下：

```python
def getDiscountType(row):
    if row == 'null':
        return 'null'
    elif ':' in row:
        return 1
    else:
        return 0
        
df['discount_rate'] = df['Discount_rate'].apply(getDiscountType)

# 其中Discount_rate是已有的列，discount_rate将会是对Discount_rate列处理后的新生成的列
# 处理函数是getDiscountType
```
