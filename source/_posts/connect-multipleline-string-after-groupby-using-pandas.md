---
title: 使用Pandas，在按列groupby后，合并多行字符串
date: 2022-01-20 16:44:34
tags: 
  - pandas
  - technote
cover: /images/pandas.png
---



## 问题描述
在解决数据分析或者机器学习问题时，我们可能会需要先将数据按照某列groupby，之后，再把另一列的值汇总到一起，比如：

| Name         | Location       |
| :---:        |     :---:      |
| Tom          | Beijing        |
| Jim          | Shanghai       |
| Leon         | Beijing        |
| Shawn        | Shanghai       |

按照 Location groupby，之后，再把Name的值用 “，”连起来，得到：

| Location       | Names      |
|     :---:      | :---:      |
| Beijing        | Tom, Leon  |
| Shanghai       | Jim, Shawn |


## 解决方案
针对这种场景，可以使用pandas的 groupby + transform 解决，示例代码如下：

``` python
import pandas as pd

df=pd.DataFrame({'Name': ['Tom', 'Jim',
                              'Leon', 'Shawn'],
                   'Location': ['Beijing', 'Shanghai', 'Beijing', 'Shanghai']})
df

>>>
Name	Location
Tom	Beijing
Jim	Shanghai
Leon	Beijing
Shawn	Shanghai


df['Names']=df.groupby(['Location'])['Name'].transform( lambda x: ','.join(x))
df

>>>
Name	Location	Names
Tom	Beijing	        Tom,Leon
Jim	Shanghai	Jim,Shawn
Leon	Beijing	        Tom,Leon
Shawn	Shanghai	Jim,Shawn

df[['Location', 'Names']].drop_duplicates()

>>>
Location	Names
Beijing	        Tom,Leon
Shanghai	Jim,Shawn
```

全文完

---
## 参考文档：
- [Pandas Doc](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)
- [Stack Overflow: Concatenate strings from several rows using Pandas groupby](https://stackoverflow.com/questions/27298178/concatenate-strings-from-several-rows-using-pandas-groupby)