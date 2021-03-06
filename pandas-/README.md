# ***Pandas***
## 零、导入
import pandas as pd
## 一、数据类型
### 1.一维Series
Series是一个类似数组的数据结构，同时带有标签(lable)或者说索引(index)
### 2.二维DataFrame
DataFrame类似于表格对象
### 3.三维Panel
## 二、数据的读入或创建
### 1.读入数据与导出数据：  
pd.read\_csv()  
pd.to\_scv  
读入(导出)什么类型的数据，就在read后加什么，其中第二个参数为分隔符    
数据库中读取要导入pymysql包，使用connect函数连接，然后read\_sql导入
### 2.创建数据：  
pd.series  
pd.DataFrame  
pd.Panel
## 三、数据表信息查看（假设读入的数据为df）
### 1.维度查看：
df.shape()
### 2.数据表基本信息（维度、列名称、数据格式、所占空间等）：
df.info()
### 3.查看数据的格式：
df.dtypes()
### 4.查看前几行(默认为10行)
df.head()
### 5.查看后几行(默认为10行)
df.tail()
### 6.查看Series对象的唯一值和计数
df['user_id'].value\_counts(dropna=False)
### 7.查看数值型列的汇总统计
df.describe()
## 四、数据选取
### 1.name
df['user_id']
### 2.list
df[['user_id','item_id']]
### 3.iloc[row,col]
df.iloc[0,0]
### 4.sample
df.sample(frac=0.5)  
括号中为采样率，也可以直接指定采样个数
## 五、数据整理
### 1.空值处理
df.isnull()%检查整体的空值，并返回一个boolean数组  
df.notnull()%检查整体的非空值，并返回一个boolean数组  
df.dropna(axis=0，threash=n)%删除所有小于n个非空值的行，不设置第二个参数则为删除所有包含空值的行  
df.fillna(x)%用x替换所有空值  
df.fillna(df.mode().iloc[0])%众数填充  
df.fillna(df.median())%中位数填充  
df["user_age"][df.age.isnull()]="0"%对某一列填充
### 2.类型转换
df.astype(float)%将series中的数据类型更改为float类型  
df["user_age"]=df["user_age"].astype('int')%更改某列类型
### 3.重命名
df.replace(1,'one')%用‘one’代替所有等于1的值  
df.colunms=['a','b','c']%重命名列名  
df.rename(columns=lambda x：x+1)%批量更改列名  
df.rename(index=lambda x：x+1)%批量重命名索引  
df.rename(columns={'old_name':'new_name'})%选择性更改列名  
df.set\_index('column_one')%更改索引列
### 4.重置索引
df.reset\_index(drop=True)%重置索引，主要用在各种操作之后，索引会被打乱
## 六、数据处理
### 1.过滤
df[df[col]>0.5]%选择col列的值大于0.5的行
### 2.排序
df.sort\_values(by='col1',ascending=True)%按照列col1排列数据，默认升序排列  
df.sort\_values（[col1,col2],ascending=[True,False]）%先按列col1升序排列，后按col2降序排列  
df.groupby('shop_id',as_index=False)['item_cvr'].rank(ascending=False，method='dense')
### 3.分组
df.groupby(col)%返回一个按列col进行分组的Groupby对象  
df.groupby([col1,col2])%返回一个按多列进行分组的Groupby对象  
df.groupby(col1)[col2].apply(np.mean)%返回按列col1进行分组后，列col2的均值  
df.groupby(col1).agg(np.mean)%返回按列col1分组的所有列的均值
### 4.迭代
for index，row in df.iterrows()：%index是行号，row是一行  
for key，df in df.groupby('user_id')：%key就是user_id，df就是分组后的dataframe
### 5.apply函数
df.apply(func, axis=0, broadcast=False, raw=False,reduce=None, args=(), **kwds)
### 6.数据统计
df.mean()%返回均值  
df.corr()%返回所有列与列之间的相关系数  
df.count()%返回每一列中的非空值的个数  
df.max()%返回每一列的最大值  
df.min()%返回每一列的最小值  
df.median()%返回每一列的中位数  
df.std()%返回每一列的标准差  
df.dtypes()%查看数据类型  
df.hist()%查看变量分布  
df.isnull().sum()%查看每一列缺失值情况  
df.value\_counts()%查看统计值  
df.unique()%查看数据取值
## 七、数据合并
### 1.行列合并
df.append(df2)%将df2中的行添加到df的尾部  
pd.concat([df1，df2]，axis=1)%按列合并
### 2.按列join
pd.merge(df1，df2，on='user_id'，how='inner')%对df1的列和df2的列执行sql形式的join