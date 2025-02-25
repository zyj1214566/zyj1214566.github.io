# pandas
https://www.kaggle.com/learn/pandas

## Creating, Reading and Writing


### 入门
要使用 pandas，通常你可以从以下代码行开始：

```python
import pandas as pd
```

### 创建数据
pandas 中有两个核心对象：**DataFrame** 和 **Series**。

#### DataFrame
DataFrame 是一个表格。它包含一个单独条目的数组，每个条目都有一个特定的值。每个条目对应一行（或记录）和一列。

例如，考虑以下简单的 DataFrame：

```python
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```

**输出：**

| Yes | No  |
|-----|-----|
| 50  | 131 |
| 21  | 2   |

在此示例中，“0, No” 条目的值为 131。“0, Yes” 条目的值为 50，依此类推。

DataFrame 的条目不限于整数。例如，这是一个值为字符串的 DataFrame：

```python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 'Sue': ['Pretty good.', 'Bland.']})
```

**输出：**

| Bob          | Sue          |
|-------------|-------------|
| I liked it. | Pretty good. |
| It was awful. | Bland. |

我们使用 `pd.DataFrame()` 构造函数来生成这些 DataFrame 对象。声明一个新的 DataFrame 的语法是一个字典，其键是列名（在此示例中为 Bob 和 Sue），其值是条目列表。这是构造新 DataFrame 的标准方法，也是你最可能遇到的方法。

字典 - 列表构造函数为列标签赋值，但仅将行标签按从 0 开始的计数 (0, 1, 2, 3, ...) 赋值。有时这没问题，但通常我们希望自己分配这些标签。

DataFrame 中使用的行标签列表称为 **索引**。我们可以在构造函数中使用 `index` 参数为其赋值：

```python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'],
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

**输出：**

|          | Bob          | Sue          |
|----------|-------------|-------------|
| Product A | I liked it. | Pretty good. |
| Product B | It was awful. | Bland. |

#### Series
相比之下，Series 是一个数据值序列。如果 DataFrame 是一个表格，Series 就是一个列表。实际上，你只需一个列表就可以创建一个 Series：

```python
pd.Series([1, 2, 3, 4, 5])
```

**输出：**

```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

从本质上讲，Series 是 DataFrame 的单个列。所以你可以像之前一样使用 `index` 参数为 Series 分配行标签。但是，Series 没有列名，它只有一个整体名称：

```python
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```

**输出：**

```
2015 Sales    30
2016 Sales    35
2017 Sales    40
Name: Product A, dtype: int64
```

Series 和 DataFrame 密切相关。将 DataFrame 看作是一堆 Series “粘合” 在一起是很有帮助的。我们将在本教程的下一部分中看到更多相关内容。

### 读取数据文件
能够手动创建 DataFrame 或 Series 很方便。但大多数时候，我们实际上不会创建自己的数据。相反，我们将处理已有的数据。

`pd.read_csv()` 函数功能强大，你可以指定 30 多个可选参数。例如，在这个数据集中，你可以看到 CSV 文件有一个内置索引，而 pandas 没有自动识别它。为了让 pandas 使用该列作为索引（而不是从头创建一个新索引），我们可以指定 `index_col`。

```python
wine_reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
wine_reviews.head()
```

**输出：**

| country | description | designation | points | price | province | region_1 | region_2 | taster_name | taster_twitter_handle |
|---------|------------|-------------|--------|-------|----------|----------|----------|-------------|-----------------------|
| Italy | Aromas include tropical fruit, broom, brimstone... | Val delle Rose - Vulkà Bianco | 87 | NaN | Sicily & Sardinia | Etna | NaN | Kevin O'Keefe | @kerinokeefe |
| Portugal | This is ripe and fruity, a wine that... | Avidagos | 87 | 15.0 | Douro | NaN | NaN | Roger Voss | @vossroger |
| US | Tart and snappy, the flavors flash... | Late Harvest | 87 | 14.0 | Oregon | Willamette Valley | Willamette Valley | Paul Gregutt | @paulgwine |
| US | Pineapple, lemon pith and orange blossom... | Reserve | 87 | 13.0 | Michigan | Michigan Shore | NaN | Alexander Peartree | NaN |

这样，我们就可以加载数据并进行分析了。









### loc和iloc
在选择或在 `loc` 和 `iloc` 之间转换时，有一个需要注意的“陷阱”，那就是这两种方法使用了略有不同的索引方案。

`iloc` 使用的是 Python 标准库的索引方案，其中范围的第一个元素会被包含，而最后一个元素会被排除。因此，`0:10` 会选择索引为 0 到 9 的条目。而 `loc` 则是包含式的索引方式。所以，`0:10` 会选择索引为 0 到 10 的条目。

为什么会有这样的变化呢？记住，`loc` 可以索引任何标准库类型，比如字符串。假设我们有一个数据框，其索引值为“苹果”……“土豆”……，如果我们想选择“苹果”和“土豆”之间的所有按字母顺序排列的水果选项，那么使用 `df.loc['Apples':'Potatoes']` 比使用类似 `df.loc['Apples', 'Potatoet']`（因为字母“t”在字母“s”之后）要方便得多。

当数据框的索引是一个简单的数字列表，比如 0……1000 时，这种情况尤其令人困惑。在这种情况下，`df.iloc[0:1000]` 会返回 1000 个条目，而 `df.loc[0:1000]` 则会返回 1001 个条目！如果你想使用 `loc` 获取 1000 个元素，你需要将索引范围降低一个单位，也就是请求 `df.loc[0:999]`。

除此之外，使用 `loc` 的语义与使用 `iloc` 是相同的。
```python
import pandas as pd

data = {
    'points': [90, 85, 92, 88, 95],
    'price': [120, 45, 90, 55, 110],
    'variety': ['Cabernet Sauvignon', 'Pinot Noir', 'Chardonnay', 'Pinot Noir', 'Merlot']
}

reviews = pd.DataFrame(data)

def remean_adjusted_points(row):
    # 初始化 adjusted_points 为原始 points
    row['adjusted_points'] = row['points']
    if row['variety'] == 'Pinot Noir':
        row['adjusted_points'] += 2
        return row
    # 根据 price 调整 points
    if row['price'] >= 100:
        row['adjusted_points'] += 5
    elif 50 <= row['price'] < 100:
        row['adjusted_points'] += 3
    elif row['price'] < 50:
        row['adjusted_points'] += 0  # 这里其实不需要加 0，但为了逻辑清晰保留
    return row

# 应用函数并创建新列
reviews = reviews.apply(remean_adjusted_points, axis='columns')

print(reviews)
```
output:
   points  price             variety  adjusted_points
0      90    120  Cabernet Sauvignon               95
1      85     45          Pinot Noir               87
2      92     90          Chardonnay               95
3      88     55          Pinot Noir               90
4      95    110              Merlot              100