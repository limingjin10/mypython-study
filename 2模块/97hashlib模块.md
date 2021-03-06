目录

[TOC]



# 一、hashlib模块

密码加密：无论你丢什么字符串，他都会返回一串 固定长度的字符串

1. 变成固定的字符串
2.  相同的字符串哈希后结果一样
3.  叠加性

### 1.0.1 hashlib是什么

hash是一种算法（Python3.版本里使用hashlib模块代替了md5模块和sha模块，主要提供 SHA1、SHA224、SHA256、SHA384、SHA512、MD5 算法），该算法接受传入的内容，经过运算得到一串hash值。

hashlib值的特点：

1. 只要传入的内容一样，得到的hash值一样，可用于非明文密码传输时密码校验
2. 不能由hash值返解成内容，即可以保证非明文密码的安全性
3. 只要使用的hash算法不变，无论校验的内容有多大，得到的hash值长度是固定的，可以用于对文本的哈希处理

hash算法其实可以看成如下图所示的一座工厂，工厂接收你送来的原材料，经过加工返回的产品就是hash值

![hashlib模块](http://www.chenyoude.com/Python%E4%BB%8E%E5%85%A5%E9%97%A8%E5%88%B0%E6%94%BE%E5%BC%83/hashlib%E6%A8%A1%E5%9D%97.jpg?x-oss-process=style/watermark)

```python
import hashlib
import hashlib
m = hashlib.md5()

m.update('hello'.encode('utf8'))
print(m.hexdigest())
#
5d41402abc4b2a76b9719d911017c592
```

```python
import hashlib

m = hashlib.md5()  # 固定的写法
m.update(b'123456')
# m.update(b'456')
print(m.hexdigest())
#
e10adc3949ba59abbe56e057f20f883e
```



```python
import hashlib

m = hashlib.md5()  # 固定的写法
m.update(b'123')
m.update(b'456')
print(m.hexdigest())
#
e10adc3949ba59abbe56e057f20f883e
```

### 1.0.2 撞库破解hash算法加密

hash加密算法虽然看起来很厉害，但是他是存在一定缺陷的，即可以通过撞库可以反解，如下代码所示。

```python
import hashlib

# 假定我们知道hash的微信会设置如下几个密码
pwd_list = [
    'hash3714',
    'hash1313',
    'hash94139413',
    'hash123456',
    '123456hash',
    'h123ash',
]


def make_pwd_dic(pwd_list):
    dic = {}
    for pwd in pwd_list:
        m = hashlib.md5()
        m.update(pwd.encode('utf-8'))
        dic[pwd] = m.hexdigest()
    return dic


def break_code(hash_pwd, pwd_dic):
    for k, v in pwd_dic.items():
        if v == hash_pwd:
            print('hash的微信的密码是===>%s' % k)


hash_pwd = '0562b36c3c5a3925dbe3c4d32a4f2ba2'
break_code(hash_pwd, make_pwd_dic(pwd_list))
```

hash的微信的密码是===>hash123456

为了防止密码被撞库，我们可以使用python中的另一个hmac 模块，它内部对我们创建key和内容做过某种处理后再加密。

如果要保证hmac模块最终结果一致，必须保证：

1. hmac.new括号内指定的初始key一样
2. 无论update多少次，校验的内容累加到一起是一样的内容



# 二、hmac模块

 hmac模块：对密码加密，可以加盐

注意hmac模块只接受二进制数据的加密

用法相同

```python
import hmac

# 注意hmac模块只接受二进制数据的加密
h1 = hmac.new(b'hash')
h1.update(b'hello')
h1.update(b'world')
print(h1.hexdigest())
905f549c5722b5850d602862c34a763e
```



```python
h2 = hmac.new(b'hash')
h2.update(b'helloworld')
print(h2.hexdigest())
905f549c5722b5850d602862c34a763e
```



```python
h3 = hmac.new(b'hashhelloworld')
print(h3.hexdigest())
a7e524ade8ac5f7f33f3a39a8f63fd25
```

