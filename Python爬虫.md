# Python爬虫

---

## 一、Python

### 1、数据类型转换

- 转换int类型注意事项：

1、float ---> int 类型是直接舍去小数点后面的数字

2、str ---> int 中不能有非法字符或者字母

- 转换boolean类型注意事项：

1、如果是对非零的数（包含正数、负数）进行转换，那么转换的类型都是True

2、只要字符串（列表、元组、字典）中有内容，在强制类型转换后都返回True；否则返回False



### 2、运算符

逻辑运算符的性能优化：

- and：短路与，当and前面是True，后面的语句才执行；如果前面的是False，后面的语句不执行
- or：短路或，当or前面是False，后面的语句才执行；如果前面的是True，后面的语句则不执行



### 3、输出

格式化输出

```python
age = 18
name = 'Celery'
print("我的名字是%s,我的年龄是%d" %(name,age))
```

常用于scrapy框架



### 4、输入

```python
age = input("Enter your age: ")
print("我的年龄是%s" %age)
```

input输入的类型是字符串类型



### 5、流程控制语句

for循环

```python
a_list = ['周杰伦','林俊杰','陶喆']
for i in range(len(a_list)):
    print(a_list[i])
```

应用场景：爬取列表返回



### 6、数据类型高级

#### 6.1 字符串高级

常见操作：

- 获取长度：len()
- 查找内容：find()   获取查找目标的第一次出现位置的索引，若不存在则返回-1
- 判断字符串是否以其开头（结尾）：startswith()、endwith()
- 计算出现次数：count()
- 替换字符串中指定内容：replace()
- 切割字符串：split()
- 修改大小写：upper()、lower()
- 去空格：strip()
- 字符串拼接：join()    a.join(b) 是将b中的内容插入a的每一个空隙中

#### 6.2 列表高级

列表的增删改查

*添加*

**append**：在列表末端加入元素

**insert**：在指定的索引前加插入元素

**extend**：将另一个列表的元素加入到列表中

*修改*

直接通过索引修改

*查找*

**in**：元素在列表中

**not in**：元素不在列表中

*删除*

**del**：根据索引删除元素

**pop**：删除最后的元素

**remove**：根据元素值进行删除

#### 6.3  元组高级

元组中的元素不能修改

当元组中的数据只有一个元素时，要在后面加逗号

#### 6.4 字典高级

*查询*

```python
person = {'name':'John', 'age':14}
print(person['name'])
print(person.get('name'))
# 不存在的key
print(person.get('sex'))
```

输出： 

John
John
None

```python
print(person['sex'])
```

```python
Traceback (most recent call last):
  File "C:\Users\86156\pythonProject\Demo1.py", line 5, in <module>
    print(person['sex'])
          ~~~~~~^^^^^^^
KeyError: 'sex'
```

*删除*

**del**：

- 删除字典中某一个元素
- 删除整个字典

**clear**：清空字典，但是保留字典对象



### 7、文件

#### 7.1 序列化和反序列化

默认情况下，对象是无法写入文件中的

- 序列化：对象 ---> 字节序列
- 反序列化：字节序列 ---> 对象

**JSON模块**

序列化：

```python
import json
#dumps
fp = open("test.txt","w")
nameList = ["aa","bb"]
fp.write(json.dumps(nameList))

#dump
fp = open("test.txt","w")
nameList = ["aa","bb"]
json.dump(nameList,fp)

fp.close()
```

反序列化：

```python
import json
#loads
fp = open("test.txt","r")
content = json.loads(fp.read())
print(content)
print(type(content))

#load
fp = open("test.txt","r")
content = json.load(fp)
print(content)
print(type(content))

fp.close()
```

---



## 二、爬虫

*爬虫核心*：

1. 爬取网页：爬取整个网页，包含了网页中的所有内容
2. 解析数据：将网页中所得到的数据进行解析

难点：爬虫和反爬虫之间的博弈



### 1、urllib库使用

```python
import urllib.request
url = 'http://www.baidu.com'
# 模拟浏览器向服务端发送请求
response = urllib.request.urlopen(url)
content = response.read().decode('utf-8')
# 输出源码
print(content)

# 数据类型是HTTPResponse
print(type(response))

# read()方法 按字节读取内容
content = response.read()

# 返回5个字节的内容
content = response.read(5)

# 读取一行的行内容
content = response.readline()

# 按行读取内容直到结束
content = response.readlines()

# 返回状态码
print(response.getcode())

print(response.geturl())
```



### 2、下载

```python
import urllib.request
# 下载网页、图片、视频
urllib.request.urlretrieve(url, filename)
```



### 3、请求对象的定制

> UA：User Agent，中文名叫用户代理，使服务器能够识别用户使用的操作系统及版本、浏览器内核等等

```python
import urllib.request
url = 'https://www.bilibili.com/'
headers = {'User-Agent':
'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36 Edg/122.0.0.0'}

# 使用关键字传参，因为url和header之间还有data
request = urllib.request.Request(url = url,headers = headers)

response = urllib.request.urlopen(request)

content = response.read().decode('utf-8')

print(content)
```



