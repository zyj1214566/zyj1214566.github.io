# nlp常用工具包

## python字符串处理

### 去除字符串两端的特殊字符
1. 
```python
input_str = ' 今天风和日丽 '
input_str.strip()
```
output:'今天风和日丽'

```python
input_str.rstrip()
```
output:' 今天风和日丽'

```python
input_str.lstrip()
```
output:'今天风和日丽 '

2. 
```python
input_str = 'AAA今天风和日丽AAA'
input_str.strip('A')
```
output:'今天风和日丽'
```python
input_str.rstrip('A')
```
output:'AAA今天风和日丽'
```python
input_str.lstrip('A')
```
output:'今天风和日丽AAA'

### 替换字符串中的特殊字符
```python
input_str = ' 今天风和日丽今天 '
input_str.replace('今天','昨天')
```
output:' 昨天风和日丽昨天 '

```python
input_str = '今天风和日丽今天'
input_str.replace('今天','')
```
output:'风和日丽'

### 查找操作
```python
input_str = '今天风和日丽今天'
input_str.find('今天')
```
output:0

### 判断操作
1. 判断是否为字母(全部是)
```python
   input_str = ' 今天风和日丽 '
   input_str.isalpha()
   ```
   output:False

2. 数字：
```python
input_str.isdigit()
```

### 分割合并操作
1. 
```python
input_str = '今天 风和日丽 的,天气 不错'
input_str = input_str.split(' ')
print(input_str)
```
output:['今天', '风和日丽', '的,天气', '不错']
```python
   ' '.join(input_str)
```
output1:'今天 风和日丽 的,天气 不错'
```python
   ''.join(input_str)
```
output2:'今天风和日丽的,天气不错'


### 帮助文档
```python
help(str)
```

## 正则表示基本语法
图片1

- 指定好匹配的模式-pattern
- 选择相应的方法-match,search等
- 得到匹配结果-group

- re.match #从开始位置开始匹配，如果开头没有则无
- re.search #搜索整个字符串
- re.findall #搜索整个字符串，返回一个list
### 字符集合
- []指定包含字符
- [a-zA-Z]指定所有英文字母的大小写
- [^a-zA-Z]指定不匹配所有英文字母
例子：
1.
```python
input = '自然语言处理很重要。 123abc'
import re
pattern = re.compile(r'.')
re.findall(pattern,input)
```
output:
['自',
 '然',
 '语',
 '言',
 '处',
 '理',
 '很',
 '重',
 '要',
 '。',
 ' ',
 '1',
 '2',
 '3',
 'a',
 'b',
 'c']

2.
```python
input = '自然语言处理很重要。 123abc'
import re
pattern = re.compile(r'[abc]')
re.findall(pattern,input)
```
output:
['a', 'b', 'c']

3.
```python
input = '自然语言处理很重要。 123abc'
import re
pattern = re.compile(r'[a-zA-Z]')
re.findall(pattern,input)
```
output:
['a', 'b', 'c']

4.
```python
input = '自然语言处理很重要。 123abc'
import re
pattern = re.compile(r'[^a-zA-Z]')
re.findall(pattern,input)
```
output:['自', '然', '语', '言', '处', '理', '很', '重', '要', '。', ' ', '1', '2', '3']

### 或方法
1.
```python
input = '自然语言处理很重要。 123abc'
import re
pattern = re.compile(r'[a-zA-Z]|[0-9]')
re.findall(pattern,input)
```
output:['1', '2', '3', 'a', 'b', 'c']

### 精确匹配和最小匹配
1. 精确匹配
'{m}'精确匹配m次
```python
input = '自然语言处理很重要。 12abc789'
import re
pattern = re.compile(r'\d{3}')
re.findall(pattern,input)
```
output: ['789']

2. 最小匹配
'{m,n}'最少匹配m次，最多匹配n次
```python
input = '自然语言处理很重要。 12abc789'
import re
pattern = re.compile(r'\d{1,3}')
re.findall(pattern,input)
```
output: ['12', '789']

### match与search
```python
input2 = '123自然语言处理很重要'
import re
pattern = re.compile(r'\d')
match = re.match(pattern,input2)
match.group()
```
output:'1'


### 字符串的替换和修改
在目标字符串中规格规则查找匹配的字符串，再把它们替换成指定的字符串。你可以指定一个最多替换次数，否则将替换所有的匹配到的字符串。
sub( rule ,replace ,target [.count])
subn(rule ,replace , target [.count] )
第一个参数是正则规则，第二个参数是指定的用来替换的字符串，第三个参数是目标字符串，第四个参数是最多替换次数。
sub 返回一个被替换的字符串
subn 返回一个元组，第一个元素是被替换的字符串，第二个元素是一个数字，表明产生了多少次替换。
1.
```python
input2 = '123自然语言处理很重要'
import re
pattern = re.compile(r'\d')
re.sub(pattern,"数字",input2)
```
output:'数字数字数字自然语言处理很重要'
2.
```python
input2 = '123自然语言处理很重要'
import re
pattern = re.compile(r'\d')
re.subn(pattern,"数字",input2) 
```
output:('数字数字数字自然语言处理很重要', 3)

### split 切片函数
使用指定的正则规则在目标字符串中查找匹配的字符串，用它们作为分界，把字符串切片。
split( rule , target [,maxsplit] )
第一个参数是正则规则，第二个参数是目标字符串，第三个参数是最多切片次数,返回一个被切完的子字符串的列表
```python
input3 = '自然语言处理很重要123机器学习456深度学习'
import re
pattern = re.compile(r'\d+')
re.split(pattern,input3)
```
output:['自然语言处理很重要', '机器学习', '深度学习']

### '(?P...)'命名组
<...>'里面是你给这个组起的名字
1.
```python
input3 = '自然语言处理很重要123机器学习456深度学习'
import re
pattern = re.compile(r'(?P<dota>\d+)(?P<lol>\D+)')
m = re.search(pattern,input3)
m.group('dota')
```
output:'123'

2.
```python
input3 = '自然语言处理很重要123机器学习456深度学习'
import re
pattern = re.compile(r'(?P<dota>\d+)(?P<lol>\D+)')
m = re.search(pattern,input3)
m.group('lol')
```
output:'机器学习'

## NLTK工具包
下载需要翻墙
```python
import nltk
nltk.download()
```

### 分词
```python
import nltk
from nltk.tokenize import word_tokenize
from nltk.text import Text
input_str = "Today's weather is good, very windy and suny, we have no classes in the afternoon,we have to play baketball tomorrow." 
tokens = word_tokenize(input_str)
tokens =[word.lower() for word in tokens]
print(tokens[:5])
```
output:['today', "'s", 'weather', 'is', 'good']

### Text对象
```python
help(nltk.text)
```
```python
t = Text(tokens)
t.count('good')
t.index('good')
t.plot(8)
```
output:
1
4
图片2

### 停用词
```python
import nltk
from nltk.corpus import stopwords
print(stopwords.raw('english').replace('\n',' '))
```
output:i me my myself we our ours ourselves you you're you've you'll you'd your yours yourself yourselves he him his himself she she's her hers herself it it's its itself they them their theirs themselves what which who whom this that that'll these those am is are was were be been being have has had having do does did doing a an the and but if or because as until while of at by for with about against between into through during before after above below to from up down in out on off over under again further then once here there when where why how all any both each few more most other some such no nor not only own same so than too very s t can will just don don't should should've now d ll m o re ve y ain aren aren't couldn couldn't didn didn't doesn doesn't hadn hadn't hasn hasn't haven haven't isn isn't ma mightn mightn't mustn mustn't needn needn't shan shan't shouldn shouldn't wasn wasn't weren weren't won won't wouldn wouldn't

```python
import nltk
from nltk.corpus import stopwords
print(stopwords.raw('chinese').replace('\n',' '))
```
output:一 一下 一些 一切 一则 一天 一定 一方面 一旦 一时 一来 一样 一次 一片 一直 一致 一般 一起 一边 一面 万一 上下 上升 上去 上来 上述 上面 下列 下去 下来 下面 不一 不久 不仅 不会 不但 不光 不单 不变 不
只 不可 不同 不够 不如 不得 不怕 不惟 不成 不拘 不敢 不断 不是 不比 不然 不特 不独 不管 不能 不要 不论 不足 不过 不问 与 与其 与否 与此同时 专门 且 两者 严格 严重 个 个人 个别 中小 中间 丰富 临 为 
为主 为了 为什么 为什麽 为何 为着 主张 主要 举行 乃 乃至 么 之 之一 之前 之后 之後 之所以 之类 乌乎 乎 乘 也 也好 也是 也罢 了 了解 争取 于 于是 于是乎 云云 互相 产生 人们 人家 什么 什么样 什麽 今后 今天 今年 今後 仍然 从 从事 从而 他 他人 他们 他的 代替 以 以上 以下 以为 以便 以免 以前 以及 以后 以外 以後 以来 以至 以至于 以致 们 任 任何 任凭 任务 企图 伟大 似乎 似的 但 但是 何 何况 何处 何时 作为 你 你们 你的 使得 使用 例如 依 依照 依靠 促进 保持 俺 俺们 倘 倘使 倘或 倘然 倘若 假使 假如 假若 做到 像 允许 充分 先后 先後 先生 全部 全面 兮 共同 关于 其 其一 其中 其二 其他 其余 其它 其实 
其次 具体 具体地说 具体说来 具有 再者 再说 冒 冲 决定 况且 准备 几 几乎 几时 凭 凭借 出去 出来 出现 分别 则 别 别的 别说 到 前后 前者 前进 前面 加之 加以 加入 加强 十分 即 即令 即使 即便 即或 即若 
却不 原来 又 及 及其 及时 及至 双方 反之 反应 反映 反过来 反过来说 取得 受到 变成 另 另一方面 另外 只是 只有 只要 只限 叫 叫做 召开 叮咚 可 可以 可是 可能 可见 各 各个 各人 各位 各地 各种 各级 各自 合理 同 同一 同时 同样 后来 后面 向 向着 吓 吗 否则 吧 吧哒 吱 呀 呃 呕 呗 呜 呜呼 呢 周围 呵 呸 呼哧 咋 和 咚 咦 咱 咱们 咳 哇 哈 哈哈 哉 哎 哎呀 哎哟 哗 哟 哦 哩 哪 哪个 哪些 哪儿 哪天 哪年 哪怕 
哪样 哪边 哪里 哼 哼唷 唉 啊 啐 啥 啦 啪达 喂 喏 喔唷 嗡嗡 嗬 嗯 嗳 嘎 嘎登 嘘 嘛 嘻 嘿 因 因为 因此 因而 固然 在 在下 地 坚决 坚持 基本 处理 复杂 多 多少 多数 多次 大力 大多数 大大 大家 大批 大约  大量 失去 她 她们 她的 好的 好象 如 如上所述 如下 如何 如其 如果 如此 如若 存在 宁 宁可 宁愿 宁肯 它 它们 它们的 它的 安全 完全 完成 实现 实际 宣布 容易 密切 对 对于 对应 将 少数 尔后 尚且 尤其 就  就是 就是说 尽 尽管 属于 岂但 左右 巨大 巩固 己 已经 帮助 常常 并 并不 并不是 并且 并没有 广大 广泛 应当 应用 应该 开外 开始 开展 引起 强烈 强调 归 当 当前 当时 当然 当着 形成 彻底 彼 彼此 往 往往  待 後来 後面 得 得出 得到 心里 必然 必要 必须 怎 怎么 怎么办 怎么样 怎样 怎麽 总之 总是 总的来看 总的来说 总的说来 总结 总而言之 恰恰相反 您 意思 愿意 慢说 成为 我 我们 我的 或 或是 或者 战斗 所 所 以 所有 所谓 打 扩大 把 抑或 拿 按 按照 换句话说 换言之 据 掌握 接着 接著 故 故此 整个 方便 方面 旁人 无宁 无法 无论 既 既是 既然 时候 明显 明确 是 是否 是的 显然 显著 普通 普遍 更加 曾经 替 最后 最大 最好 最後 最近 最高 有 有些 有关 有利 有力 有所 有效 有时 有点 有的 有着 有著 望 朝 朝着 本 本着 来 来着 极了 构成 果然 果真 某 某个 某些 根据 根本 欢迎 正在 正如 正常 此 此外 此时 此间 毋宁 每  每个 每天 每年 每当 比 比如 比方 比较 毫不 没有 沿 沿着 注意 深入 清楚 满足 漫说 焉 然则 然后 然後 然而 照 照着 特别是 特殊 特点 现代 现在 甚么 甚而 甚至 用 由 由于 由此可见 的 的话 目前 直到 直接  相似 相信 相反 相同 相对 相对而言 相应 相当 相等 省得 看出 看到 看来 看看 看见 真是 真正 着 着呢 矣 知道 确定 离 积极 移动 突出 突然 立即 第 等 等等 管 紧接着 纵 纵令 纵使 纵然 练习 组成 经 经常 经 过 结合 结果 给 绝对 继续 继而 维持 综上所述 罢了 考虑 者 而 而且 而况 而外 而已 而是 而言 联系 能 能否 能够 腾 自 自个儿 自从 自各儿 自家 自己 自身 至 至于 良好 若 若是 若非 范围 莫若 获得 虽 虽则 虽然 虽说 行为 行动 表明 表示 被 要 要不 要不是 要不然 要么 要是 要求 规定 觉得 认为 认真 认识 让 许多 论 设使 设若 该 说明 诸位 谁 谁知 赶 起 起来 起见 趁 趁着 越是 跟 转动 转变 转贴 较 较之 边 达 到 迅速 过 过去 过来 运用 还是 还有 这 这个 这么 这么些 这么样 这么点儿 这些 这会儿 这儿 这就是说 这时 这样 这点 这种 这边 这里 这麽 进入 进步 进而 进行 连 连同 适应 适当 适用 逐步 逐渐 通常 通过 造成 遇到 遭到 避免 那 那个 那么 那么些 那么样 那些 那会儿 那儿 那时 那样 那边 那里 那麽 部分 鄙人 采取 里面 重大 重新 重要 鉴于 问题 防止 阿 附近 限制 除 除了 除此之外 除非 随 随着 随著 集中 需要 非 但 非常 非徒 靠 顺 顺着 首先 高兴 是不是

```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.text import Text
input_str = "Today's weather is good, very windy and suny, we have no classes in the afternoon,we have to play baketball tomorrow." 
tokens = word_tokenize(input_str)
test_words = [word.lower()for word in tokens]
test_words_set = set(test_words)
print(test_words_set.intersection(set(stopwords.words('english'))))
```
output:{'have', 'to', 'and', 'no', 'is', 'in', 'very', 'the', 'we'}

过滤掉停用词:
```python
filtered = [w for w in test_words_set if(w not in stopwords.words('english'))]
print(filtered)
```
output:[',', 'classes', 'afternoon', "'s", 'baketball', 'suny', 'windy', 'today', 'weather', 'good', '.', 'play', 'tomorrow']

### 词性标注

|缩写|全称|中文释义|示例|
| ---- | ---- | ---- | ---- |
|CC|Coordinating conjunction|连接词|and, or, but|
|CD|Cardinal number|基数词|one, two, three|
|DT|Determiner|限定词|this, that, these, those, no, some, any|
|EX|Existential there|存在句|There is a book on the table.|
|FW|Foreign word|外来词|ketchup, sushi|
|IN|Preposition or subordinating conjunction|介词或从属连词|in, on, at, because, if|
|JJ|Adjective|形容词或序数词|big, small, first|
|JJR|Adjective, comparative|形容词比较级|bigger, smaller|
|JJS|Adjective, superlative|形容词最高级|biggest, smallest|
|LS|List item marker|列表标示|1., 2., A., B.|
|MD|Modal|情态助动词|can, could, will, would|
|NN|Noun, singular or mass|常用名词 单数形式|book, water|
|NNS|Noun, plural|常用名词 复数形式|books, tables|
|NNP|Proper noun, singular|专有名词，单数形式|China, John|
|NNPS|Proper noun, plural|专有名词，复数形式|the United States, the Smiths|
|PDT|Predeterminer|前位限定词|all, both, half|
|POS|Possessive ending|所有格结束词|'s, s'|
|PRP|Personal pronoun|人称代词|I, you, he, she, it|
|PRP$|Possessive pronoun|所有格代名词|my, your, his, her, its|
|RB|Adverb|副词|quickly, slowly|
|RBR|Adverb, comparative|副词比较级|more quickly, more slowly|
|RBS|Adverb, superlative|副词最高级|most quickly, most slowly|
|RP|Particle|小品词|up, down, in, out|
|SYM|Symbol|符号|+, -, *, /|
|TO|to (as preposition or infinitive marker)|to 作为介词或不定式格式|to the park, to go|
|UH|Interjection|感叹词|oh, ah, ouch|
|VB|Verb, base form|动词基本形式|go, do, have|
|VBD|Verb, past tense|动词过去式|went, did, had|
|VBG|Verb, gerund or present participle|动名词和现在分词|going, doing, having|
|VBN|Verb, past participle|过去分词|gone, done, had|
|VBP|Verb, non - 3rd person singular present|动词非第三人称单数|go, do, have (I go, you do, we have)|
|VBZ|Verb, 3rd person singular present|动词第三人称单数|goes, does, has (he goes, she does, it has)|
|WDT|Wh - determiner|限定词|whose, which, what (in relative or interrogative sentences)|
|WP|Wh - pronoun|代词|who, whose, which|
|WP$|Possessive wh - pronoun|所有格代词|whose|
|WRB|Wh - adverb|疑问代词|how, where, when|

```python
import nltk
from nltk.tokenize import word_tokenize
from nltk import pos_tag
input_str = "Today's weather is good, very windy and suny, we have no classes in the afternoon,we have to play baketball tomorrow." 
tokens = word_tokenize(input_str)
test_words = [word.lower()for word in tokens]
tags = pos_tag(tokens)
print(tags)
```

output:
[('Today', 'NN'), ("'s", 'POS'), ('weather', 'NN'), ('is', 'VBZ'), ('good', 'JJ'), (',', ','), ('very', 'RB'), ('windy', 'JJ'), ('and', 'CC'), ('suny', 'JJ'), (',', ','), ('we', 'PRP'), ('have', 'VBP'), ('no', 'DT'), ('classes', 'NNS'), ('in', 'IN'), ('the', 'DT'), ('afternoon', 'NN'), (',', ','), ('we', 'PRP'), ('have', 'VBP'), ('to', 'TO'), ('play', 'VB'), ('baketball', 'DT'), ('tomorrow', 'NN'), ('.', '.')]

### 分块
```python
import nltk
from nltk.chunk import RegexpParser
sentence = [('the','DT'),('little','JJ'),('yellow','JJ'),('dog','NN'),('died','VBD')]                                                          
grammer = "MY_NP:{<DT>?<JJ>*<NN>}"
cp=nltk.RegexpParser(grammer)#生成规则
result=cp.parse(sentence)#进行分块
print(result)
result.draw()#调用matplotlib库画出来
```
output:(S (MY_NP the/DT little/JJ yellow/JJ dog/NN) died/VBD)
图片3

### 命名实体

```python
from nltk import ne_chunk
sentence = "Edison went to Tsinghua University today"
print(ne_chunk(pos_tag(word_tokenize(sentence))))
```
output:
(S
  (PERSON Edison/NNP)
  went/VBD
  to/TO
  (ORGANIZATION Tsinghua/NNP University/NNP)
  today/NN)

### 数据清洗实例
```python
import re
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

# 输入数据
s = 'RT @Amila #Test\nTom\'s newly listed Co & Mary\'s unlisted Group to supply tech for nlTK.\nh$TSLA$AAPL https://t.co/x34afs'

# 指定停用词
cache_english_stopwords = stopwords.words('english')

def text_clean(text):
    print('原始数据:', text, '\n')

    # 去掉HTML标签(e.g. &amp;)
    text_no_special_entities = re.sub(r'\&\w*;|#\w*|@\w*', '', text)
    print('去掉特殊标签后的:', text_no_special_entities, '\n')

    # 去掉一些价值符号
    text_no_tickers = re.sub(r'\$\w*', '', text_no_special_entities)
    print('去掉价值符号后的:', text_no_tickers, '\n')

    # 去掉超链接
    text_no_hyperlinks = re.sub(r'https?://\S+|www\.\S+', '', text_no_tickers)
    print('去掉超链接后的:', text_no_hyperlinks, '\n')

    # 去掉一些专门名词缩写，简单来说就是字母比较少的词
    text_no_small_words = re.sub(r'\b\w{1,2}\b', '', text_no_hyperlinks)
    print('去掉专门名词缩写后:', text_no_small_words, '\n')

    # 去掉多余的空格
    text_no_whitespace = re.sub(r'\s+', ' ', text_no_small_words)
    text_no_whitespace = text_no_whitespace.strip()
    print('去掉空格后的:', text_no_whitespace, '\n')

    # 分词
    tokens = word_tokenize(text_no_whitespace)
    print('分词结果:', tokens, '\n')

    # 去停用词
    list_no_stopwords = [i for i in tokens if i not in cache_english_stopwords]
    print('去停用词后结果:', list_no_stopwords, '\n')

    # 过滤后结果
    text_filtered = ' '.join(list_no_stopwords)  # ' '.join() would join with spaces between words.
    print('过滤后:', text_filtered)

text_clean(s)
```
output:
原始数据: RT @Amila #Test
Tom's newly listed Co & Mary's unlisted Group to supply tech for nlTK.
h$TSLA$AAPL https://t.co/x34afs

去掉特殊标签后的: RT
Tom's newly listed Co & Mary's unlisted Group to supply tech for nlTK.
h$TSLA$AAPL https://t.co/x34afs

去掉价值符号后的: RT
Tom's newly listed Co & Mary's unlisted Group to supply tech for nlTK.
h https://t.co/x34afs

去掉超链接后的: RT
Tom's newly listed Co & Mary's unlisted Group to supply tech for nlTK.
h

去掉专门名词缩写后:
Tom' newly listed  & Mary' unlisted Group  supply tech for nlTK.


去掉空格后的: Tom' newly listed & Mary' unlisted Group supply tech for nlTK.

分词结果: ['Tom', "'", 'newly', 'listed', '&', 'Mary', "'", 'unlisted', 'Group', 'supply', 'tech', 'for', 'nlTK', '.']

去停用词后结果: ['Tom', "'", 'newly', 'listed', '&', 'Mary', "'", 'unlisted', 'Group', 'supply', 'tech', 'nlTK', '.']

过滤后: Tom ' newly listed & Mary ' unlisted Group supply tech nlTK .

## Spacy工具包
```python
import spacy
nlp = spacy.load('en_core_web_sm')
doc = nlp('Weather is good, very windy and sunny. We have no classes in the afternoon.')
#分词
for token in doc:
    print(token)
#分句
for sent in doc.sents:
    print(sent)
#词性
for token in doc:
    print('{}-{}'.format(token,token.pos_))
```
output:
Weather
is
good
,
very
windy
and
sunny
.
We
have
no
classes
in
the
afternoon
.
Weather is good, very windy and sunny.
We have no classes in the afternoon.
Weather-NOUN
is-AUX
good-ADJ
,-PUNCT
very-ADV
windy-ADJ
and-CCONJ
sunny-ADJ
.-PUNCT
We-PRON
have-VERB
no-DET
classes-NOUN
in-ADP
the-DET
afternoon-NOUN
.-PUNCT   

### 命名体识别
```python
import spacy
nlp = spacy.load('en_core_web_sm')
doc = nlp('I went to Paris where I met my old friend Jack from uni.')

for ent in doc.ents:
    print('{}-{}'.format(ent,ent.label_))
```
output:
Paris-GPE
Jack-PERSON

## 结巴分词器
```python
import jieba

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print("全模式: " + "/".join(seg_list))  # 全模式

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("精确模式: " + "/".join(seg_list))  # 精确模式

seg_list = jieba.cut("他来到了网易杭研大厦")  # 默认是精确模式
print(", ".join(seg_list))
```
output:
全模式: 我/来到/北京/清华/清华大学/华大/大学
精确模式: 我/来到/北京/清华大学
他, 来到, 了, 网易, 杭研, 大厦

### 添加自定义词典
```python
import jieba

jieba.load_userdict("D:\zyj's notebook\zyj-s-notebook\mydict.txt")  # 需UTF-8，可以在另存为里面设置

# 也可以用jieba.add_word("乾清宫")
text = "故宫的著名景点包括乾清宫、太和殿和黄琉璃瓦等"

# 全模式
seg_list = jieba.cut(text, cut_all=True)
print(u"[全模式]: ", "/".join(seg_list))

# 精确模式
seg_list = jieba.cut(text, cut_all=False)
print(u"[精确模式]: ", "/".join(seg_list))
```
output:
[全模式]:  故宫/的/著名/著名景点/景点/包括/乾清宫/清宫/、/太和/太和殿/和/黄琉璃瓦/琉璃/琉璃瓦/等
[精确模式]:  故宫/的/著名景点/包括/乾清宫/、/太和殿/和/黄琉璃瓦/等

### 关键词抽取
```python
import jieba

jieba.load_userdict("D:\zyj's notebook\zyj-s-notebook\mydict.txt")  # 需UTF-8，可以在另存为里面设置

# 也可以用jieba.add_word("乾清宫")
text = "故宫的著名景点包括乾清宫、太和殿和黄琉璃瓦等"

import jieba.analyse

seg_list = jieba.cut(text, cut_all=False)
print(u"分词结果:")
print("/".join(seg_list))

# 获取关键词
tags = jieba.analyse.extract_tags(text, topK=5)
print(u"关键词:")
print(" ".join(tags))
tags = jieba.analyse.extract_tags(text, topK=5, withWeight=True)
for word, weight in tags:
    print(word, weight)

```
output:
分词结果:
故宫/的/著名景点/包括/乾清宫/、/太和殿/和/黄琉璃瓦/等
关键词:
著名景点 乾清宫 黄琉璃瓦 太和殿 故宫
著名景点 2.3167796086666668
乾清宫 1.9924612504833332
黄琉璃瓦 1.9924612504833332
太和殿 1.6938346722833335
故宫 1.5411195503033335

### 词性标注
```python
import jieba.posseg as pseg

words = pseg.cut("我爱北京天安门")
for word, flag in words:
    print("%s %s" % (word, flag))
```
output:
我 r
爱 v
北京 ns
天安门 ns

### 词云展示
先将停用词写成一个txt:
```python
import nltk
from nltk.corpus import stopwords

# 获取中文停用词表
chinese_stopwords = stopwords.words('chinese')

# 指定要写入的文件路径
file_path = 'stopwords.txt'  

# 将停用词表写入文件
with open(file_path, 'w', encoding='utf-8') as file:
    for word in chinese_stopwords:
        file.write(word + '\n')
```
```python
import jieba
from wordcloud import WordCloud
import imageio
from collections import Counter
import matplotlib.pyplot as plt

# 读取文本文件和停用词表
with open('19大.txt', 'r', encoding='utf-8') as text_file:
    text = text_file.read()
with open('stopwords.txt', encoding='utf-8') as file:
    stopwords = {line.strip() for line in file}

# 分词并统计词频，除去停用词
seg_list = jieba.cut(text, cut_all=False)
data = {}
for word in seg_list:
    if len(word) >= 2 and word not in stopwords:
        if word not in data:
            data[word] = 0
        data[word] += 1

# 读取遮罩图片
mask = imageio.imread('图片\\th.jpg')  # 确保路径正确

# 创建词云对象
my_wordcloud = WordCloud(
    background_color='white',  # 设置背景颜色
    max_words=400,  # 设置最大实现的字数
    font_path=r'SimHei.ttf',  # 设置字体格式，如不设置显示不了中文
    mask=mask,  # 指定在什么图片上画
    width=1000,
    height=1000,
    stopwords=stopwords
).generate_from_frequencies(data)

# 展示词云
plt.figure(figsize=(18, 16))
plt.imshow(my_wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()  # 展示词云

# 保存词云图像
my_wordcloud.to_file('result.jpg')
```
result:图片四
