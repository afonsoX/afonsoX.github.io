---

layout:     post
title:      "python爬虫入门(三)"
SEOTitle:   阿翔的博客 | AShane's Blog
subtitle:   "Re库与正则表达式入门"
date:       2017-07-28
author:     "AShane"
header-img: "img/python-WebSpider2.jpg"
catalog: true
tags:
    - python
    - 正则表达式
    - 入门
    - Re   
---

> “ To be continued …” 

> **关键字**：[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html), python ,re


## 正则表达式(regular expression , regex)
```
PY+					->		'PYYY..'  无穷字符串的简洁表达
PY[^PY]{0,10}		->		'PYABC' 	后续存在不多于10字符，后续不能是'P'或者'Y'
P(Y|YT|YTH|YTHO)?N	-> 		'PN'、'PYN'、'PYTN'、'PYTHN'、'PYTHON'PYTHON+				-> 		'PYTHON'、'PYTHONN'、'PYTHONNN' ...
PY[TH]ON			->    	'PYTON'、'PYHON'PY[^TH]?ON 			->		'PYON'、'PYaON'、'PYbON'、'PYcON'... 
PY{0,3}N				->		'PN'、'PYN'、'PYYN'、'PYYYN'...

```
操作符|说明|实例
---|---|---
.|任何单个字符|
[ ]|字符集，对单个字符给出取值范围|[abc]表示a、b、c ，[a - z]表示a到z的单个字符
[^ ]|字符集，对单个字符给出取值范围|[^abc]表示非a非b非c的单个字符
\* |前一个字符0次获无限次拓展| abc* 表示 ab、abc、abcc、abccc等
+ |前一个字符的1次或者无限次拓展| abc+ 表示 abc、abcc、abccc等
？ | 前一个字符的0次或者1次拓展|abc? 表示ab、abc
\| | 左右表达式任意一个 |abc \| def 表示abc、def
{m}| 拓展前一个字符m次 |ab{2}c表示abbc
{m,n} | 拓展前一个字符m至n次（含n次）| ab{1,2}c表示abc，abbc
^ |匹配字符串开头 |^abc表示abc且在一个字符串的开头
$ |匹配字符串结尾 |abc$表示abc且在一个字符串的结尾
( ) |分组标记，内部职能使用 \| 操作符 |（abc）表示abc，(abc|def)表示abc、def
\d | 数字，等价于[0-9]|
\w | 字符单词，等价于[A-Z a-z 0-9]|

#### 经典正则表达式实例
` ^[A-Za-z]+$ : 由26个字母责成的字符串 `
` ^[A-Za-z0-9]+\$ : 由26个字母和数字组成的字符串 `
` ^-?[1-9]\d+$ : 整数形式的字符串 `
` ^(0|-?[1-9]\d*)$ : 整数形式的字符串`
` ^(0|[1-9]\d*)$ : 正整数 `
` ^[1-9]\d{5}$ : 邮政编码 `
` [\u4e00 - \u9fa5] : 匹配中文字符 `
` \d{3} - \d{8}|\d{4} - \d{7} : 国内电话号码 `
` (\d{1,3}\.){3}(\d{1,3}) : IP地址简易模式 `
` (([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5])\.){3}([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5]) : IP地址`


### Re库
Re库主要用于Python的字符串匹配
re库采用raw string类型表示正则表达式，表示为： **r'text'**  
e.g. r'[1-9]\d{5}'   
raw srting 式不包含对转义符在此转义的字符串
上式用string类型表示为 '[1-9]**\\**\d{5}'  

主要功能函数：

函数|说明
---|---
re.search()|在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象
re.match()|从一个字符串中的开始位置起匹配正则表达式，返回match对象
re.findall()|搜索字符串，以列表类型返回全部能匹配的子串
re.split()|将一个字符串按照正则表达式匹配结果进行分割，返回列表类型
re.finditer()|搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象
re.sub()|在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串

> re.search(pattern,string.flags = 0)  
> 在一个字符串中搜索匹配正则表达式的第一个位置返回match对象
> > pattern : 正则表达式的字符串  
> > string : 待匹配的表达式  
> > flag : 正则表达式使用时的控制标志  
> > `re.I : 忽略大小写 ` 
> > `re.M : 正则表达式中的^操作符能够将给定字符串的每行当作匹配开始`  
> > `re.S : 正则表达式中的.操作符能够匹配所有字符，默认匹配除换行外的所有字符`

> re.match(pattern, string, flags=0)   
> 从一个字符串的开始位置起匹配正则表达式返回match对象

```
import re
 
line = "Cats are smarter than dogs"
 
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if matchObj:
   print "matchObj.group() : ", matchObj.group()
   print "matchObj.group(1) : ", matchObj.group(1)
   print "matchObj.group(2) : ", matchObj.group(2)
else:
   print "No match!!"

searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if searchObj:
   print "searchObj.group() : ", searchObj.group()
   print "searchObj.group(1) : ", searchObj.group(1)
   print "searchObj.group(2) : ", searchObj.group(2)
else:
   print "Nothing found!!"

输出：

matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter

searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter

```
> re.match 与 re.serch 的区别
> > re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

```
import re
 
line = "Cats are smarter than dogs";
 
matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
   print "match --> matchObj.group() : ", matchObj.group()
else:
   print "No match!!"
 
matchObj = re.search( r'dogs', line, re.M|re.I)
if matchObj:
   print "search --> matchObj.group() : ", matchObj.group()
else:
   print "No match!!"
   
输出：

No match!!
search --> matchObj.group() :  dogs
```
> re.findall(pattern, string, flags=0)  
> 搜索字符串，以列表类型返回全部能匹配的子串   

> re.split(pattern, string, maxsplit=0, flags=0) 
> 将一个字符串按照正则表达式匹配结果进行分割返回列表类型
> > maxsplit : 最大分割数 

> re.finditer(pattern, string, flags=0)
> 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象 

> re.sub(pattern, repl, string, count=0, flags=0)
> 在一个字符串中替换所有匹配正则表达式的子串返回替换后的字符串
>> repl : 替换匹配字符串的字符串

#### 等价用法：

```
rst = re.search(r'[1-9]\d{5}',BIT 100081')
等价于
pat = re.compile(r'[1-9]\d{5})
rst = pat.search('BIT 100081')
```

#### Match对象

属性|说明
---|---
.string | 待匹配的文本
.re | 匹配时使用的patter对象(正则表达式)
.pos | 正则表达式搜索文本的开始位置
.endpos | 正则表达式搜索文本的结束位置

方法 | 说明
---|---
.group(0) | 获得匹配后的字符串
.start() | 匹配字符串在原始字符串的开始位置
.end() | 匹配字符串在原始字符串中的结束位置
.span | 返回( .start() , .end() ) 

#### 贪婪匹配

Re库默认采用贪婪匹配，即输出匹配最长的子串

```
match = re.search(r'PY.*N', 'PYANBNCNDN')
match.group(0)
'PYANBNCNDN'
```

#### 最小匹配

最小匹配操作符
只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配

操作符|说明
----|----
*？ | 前一个字符0次或无限次拓展，最小匹配
+？ | 前一个字符1次或者无限次拓展，最小匹配
？？ | 前一个字符0次或者1次脱渣，最小匹配
{m,n}? | 拓展前一个字符m至n次(含n)，最下匹配

```
match = re.search(r'PY.*?N', 'PYANBNCNDN')
match.group(0)
'PYAN'
```