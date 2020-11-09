# 创建，阅读和写作

## 简介

在本微型课程中，您将学习有关[pandas](https://pandas.pydata.org/)的全部知识，pandas是用于数据分析的最受欢迎的Python库。

在此过程中，您将完成一些有关实际数据的动手练习。我们建议您在阅读相应教程的同时进行练习。

**要开始第一个练习，请单击[此处](https://www.kaggle.com/kernels/fork/587970)。**

在本教程中，您将学习如何创建自己的数据，以及如何使用已经存在的数据。

## 入门

要使用pandas常需要从以下代码行开始。

In [1:

``` python
import pandas as pd
```

这个程序演示了Python许多重要的东西，以及它是怎样工作的。让我们从上到下来检查代码。

## 创建数据

pandas有两个核心对象：**DataFrame**和**Series**。

### DataFrame

DataFrame是一个表。它包含单个条目的数组，每个条目都有一个特定值。每个条目对应于一行（或记录）和一列。

例如，考虑以下简单的DataFrame：

In [2]

``` text
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})

```

Out [2]:

|      | Yes  | No   |
| :--- | :--- | :--- |
| 0    | 50   | 131  |
| 1    | 21   | 2    |

在此示例中，“ 0，No”条目的值为131。“ 0，Yes”条目的值为50，依此类推。

DataFrame条目的值不限于整数。例如，这是一个DataFrame，其值是字符串：

In [3]:

```
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 'Sue': ['Pretty good.', 'Bland.']})
```

Out[3]:

|      | Bob           | Sue          |
| :--- | :------------ | :----------- |
| 0    | I liked it.   | Pretty good. |
| 1    | It was awful. | Bland.       |



We are using the `pd.DataFrame()` constructor to generate these DataFrame objects. The syntax for declaring a new one is a dictionary whose keys are the column names (`Bob` and `Sue` in this example), and whose values are a list of entries. This is the standard way of constructing a new DataFrame, and the one you are most likely to encounter.



我们正在使用 `pd.DataFrame()`构造函数来生成这些DataFrame对象。声明一个新对象的语法是字典，其关键字是列名（在本示例中为`Bob`和`Sue` ），其值是条目列表。这是构造新DataFrame的标准方法，也是您最有可能遇到的一种方法。

The dictionary-list constructor assigns values to the *column labels*, but just uses an ascending count from 0 (0, 1, 2, 3, ...) for the *row labels*. Sometimes this is OK, but oftentimes we will want to assign these labels ourselves.

字典列表构造函数将值分配给*列标签*，但仅对*行标签*使用从0（0、1、2、3，...）开始的递增计数。有时这样也是OK的，但通常我们会希望自己分配这些标签。

The list of row labels used in a DataFrame is known as an **Index**. We can assign values to it by using an `index` parameter in our constructor:

DataFrame中使用的行标签列表称为**索引**。我们可以通过在构造函数中使用`index`参数来为其赋值：

In [4]:

```
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

Out[4]:

|           | Bob           | Sue          |
| :-------- | :------------ | :----------- |
| Product A | I liked it.   | Pretty good. |
| Product B | It was awful. | Bland.       |

### Series

<u>A Series, by contrast</u>, is a sequence of data values. If a DataFrame is a table, a Series is a list. And in fact you can create one with nothing more than a list:

一个系列，相比之下，是数据值的序列。如果DataFrame是表，则Series是列表。实际上，您可以创建一个只包含一个列表的列表：

In [5]:

```
pd.Series([1, 2, 3, 4, 5])
```

Out[5]:

```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

A Series is, in essence, a single column of a DataFrame. So you can assign column values to the Series the same way as before, using an `index` parameter. However, a Series does not have a column name, it only has one overall `name`:

本质上，系列是DataFrame的单个列。因此，您可以使用`index`参数，以与以前相同的方式将列值分配给系列。但是，系列没有列名，只有一个整体`name`：

In [6]:

```
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```

Out[6]:

```
2015 Sales    30
2016 Sales    35
2017 Sales    40
Name: Product A, dtype: int64
```

The Series and the DataFrame are intimately related. It's helpful to think of a DataFrame as actually being just a bunch of Series "glued together". We'll see more of this in the next section of this tutorial.

系列和数据框架密切相关。将DataFrame视为实际上只是“粘合在一起”的一堆系列很有帮助。我们将在本教程的下一部分中看到更多信息。

# 读取数据文件

Being able to create a DataFrame or Series by hand is handy. But, most of the time, we won't actually be creating our own data by hand. Instead, we'll be working with data that already exists.

能够手动创建DataFrame或系列很方便。但是，在大多数情况下，我们实际上不会手动创建自己的数据。相反，我们将使用已经存在的数据。

Data can be stored in any of a number of different forms and formats. By far the most basic of these is the humble CSV file. When you open a CSV file you get something that looks like this:

数据可以多种不同形式和格式中的任何一种存储。到目前为止，最基本的是不起眼的CSV文件。当您打开CSV文件时，您将获得如下所示的内容：

```
Product A,Product B,Product C,
30,21,9,
35,34,1,
41,11,11
```

So a CSV file is a table of values separated by commas. Hence the name: "Comma-Separated Values", or CSV.

因此，CSV文件是用逗号分隔的值表。因此，名称为：“逗号分隔值”或CSV。

Let's now set aside our toy datasets and see what a real dataset looks like when we read it into a DataFrame. We'll use the `pd.read_csv()` function to read the data into a DataFrame. This goes thusly:

现在让我们搁置玩具数据集，看看当我们将其读入DataFrame时真实数据集的外观。我们将使用`pd.read_csv()`函数将数据读取到DataFrame中。因此：

In [7]:

```
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
```

We can use the `shape` attribute to check how large the resulting DataFrame is:

我们可以使用`shape`属性检查生成的DataFrame的大小：

In [8]:

```
wine_reviews.shape
```

Out[8]:

```
(129971, 14)
```

So our new DataFrame has 130,000 records split across 14 different columns. That's almost 2 million entries!

因此，我们的新DataFrame拥有130,000条记录，分布在14个不同的列中。差不多有200万个条目！

We can examine the contents of the resultant DataFrame using the `head()` command, which grabs the first five rows:

我们可以使用`head()`命令检查生成的DataFrame的内容，该命令将获取前五行：

In [9]:

```
wine_reviews.head()
```

Out[9]:

|      | Unnamed: 0 | country  | description                                       | designation                        | points | price | province          | region_1            | region_2          | taster_name        | taster_twitter_handle | title                                             | variety        | winery              |
| :--- | :--------- | :------- | :------------------------------------------------ | :--------------------------------- | :----- | :---- | :---------------- | :------------------ | :---------------- | :----------------- | :-------------------- | :------------------------------------------------ | :------------- | :------------------ |
| 0    | 0          | Italy    | Aromas include tropical fruit, broom, brimston... | Vulkà Bianco                       | 87     | NaN   | Sicily & Sardinia | Etna                | NaN               | Kerin O’Keefe      | @kerinokeefe          | Nicosia 2013 Vulkà Bianco (Etna)                  | White Blend    | Nicosia             |
| 1    | 1          | Portugal | This is ripe and fruity, a wine that is smooth... | Avidagos                           | 87     | 15.0  | Douro             | NaN                 | NaN               | Roger Voss         | @vossroger            | Quinta dos Avidagos 2011 Avidagos Red (Douro)     | Portuguese Red | Quinta dos Avidagos |
| 2    | 2          | US       | Tart and snappy, the flavors of lime flesh and... | NaN                                | 87     | 14.0  | Oregon            | Willamette Valley   | Willamette Valley | Paul Gregutt       | @paulgwine            | Rainstorm 2013 Pinot Gris (Willamette Valley)     | Pinot Gris     | Rainstorm           |
| 3    | 3          | US       | Pineapple rind, lemon pith and orange blossom ... | Reserve Late Harvest               | 87     | 13.0  | Michigan          | Lake Michigan Shore | NaN               | Alexander Peartree | NaN                   | St. Julian 2013 Reserve Late Harvest Riesling ... | Riesling       | St. Julian          |
| 4    | 4          | US       | Much like the regular bottling from 2012, this... | Vintner's Reserve Wild Child Block | 87     | 65.0  | Oregon            | Willamette Valley   | Willamette Valley | Paul Gregutt       | @paulgwine            | Sweet Cheeks 2012 Vintner's Reserve Wild Child... | Pinot Noir     | Sweet Cheeks        |

The `pd.read_csv()` function is well-endowed, with over 30 optional parameters you can specify. For example, you can see in this dataset that the CSV file has a built-in index, which pandas did not pick up on automatically. To make pandas use that column for the index (instead of creating a new one from scratch), we can specify an `index_col`.

 `pd.read_csv()` 函数功能强大，可以指定30多个可选参数。例如，您可以在此数据集中看到CSV文件具有内置索引，而pandas并没有自动建立索引。为了使pandas使用该列作为索引（而不是从头开始创建新列），我们可以指定 `index_col`。

In [10]:

```
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()
```

Out[10]:

|      | country  | description                                       | designation                        | points | price | province          | region_1            | region_2          | taster_name        | taster_twitter_handle | title                                             | variety        | winery              |
| :--- | :------- | :------------------------------------------------ | :--------------------------------- | :----- | :---- | :---------------- | :------------------ | :---------------- | :----------------- | :-------------------- | :------------------------------------------------ | :------------- | :------------------ |
| 0    | Italy    | Aromas include tropical fruit, broom, brimston... | Vulkà Bianco                       | 87     | NaN   | Sicily & Sardinia | Etna                | NaN               | Kerin O’Keefe      | @kerinokeefe          | Nicosia 2013 Vulkà Bianco (Etna)                  | White Blend    | Nicosia             |
| 1    | Portugal | This is ripe and fruity, a wine that is smooth... | Avidagos                           | 87     | 15.0  | Douro             | NaN                 | NaN               | Roger Voss         | @vossroger            | Quinta dos Avidagos 2011 Avidagos Red (Douro)     | Portuguese Red | Quinta dos Avidagos |
| 2    | US       | Tart and snappy, the flavors of lime flesh and... | NaN                                | 87     | 14.0  | Oregon            | Willamette Valley   | Willamette Valley | Paul Gregutt       | @paulgwine            | Rainstorm 2013 Pinot Gris (Willamette Valley)     | Pinot Gris     | Rainstorm           |
| 3    | US       | Pineapple rind, lemon pith and orange blossom ... | Reserve Late Harvest               | 87     | 13.0  | Michigan          | Lake Michigan Shore | NaN               | Alexander Peartree | NaN                   | St. Julian 2013 Reserve Late Harvest Riesling ... | Riesling       | St. Julian          |
| 4    | US       | Much like the regular bottling from 2012, this... | Vintner's Reserve Wild Child Block | 87     | 65.0  | Oregon            | Willamette Valley   | Willamette Valley | Paul Gregutt       | @paulgwine            | Sweet Cheeks 2012 Vintner's Reserve Wild Child... | Pinot Noir     | Sweet Cheeks        |

# 轮到你了

If you haven't started the exercise, you can **[get started here](https://www.kaggle.com/kernels/fork/587970)**.

如果您尚未开始练习，则可以[从这里开始](https://www.kaggle.com/kernels/fork/587970)。

------

*Have questions or comments? Visit the [Learn Discussion forum](https://www.kaggle.com/learn-forum/161299) to chat with other Learners.*

有问题或意见吗？访问[学习讨论论坛](https://www.kaggle.com/learn-forum/161299)，与其他学习者聊天。