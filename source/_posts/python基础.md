---
title: python基础
date: 2019-09-10 15:26:55
tags: python
cover_img:
feature_img:
description:
keywords: python

---

## python 基础知识总结

```python
    # 变量
    # 数据类型
    int，float，str，bool，tuple，list，dict
    # 类型转换
    int()
    float()
    str()
    # 打印
    print(11)
    print(123, 'hello', sep="&", end='\n\n')
    print(11213)
    # 输入
    result = int(input('请输入'))
    print(type(result),result)

    # 条件语句
    if result<10 :
    print('小于10')
    elif result >=10 and result<20:
    print('正确')
    else:
    print('其他')
    #在python中非0即真
    # 逻辑运算符and ,or,not
    # 字符串的常见用法
    # 查找对应的下标
    str = 'hello'
    result = str.index('h')
    #找不到时会报错
    print(result)

    result = str.find('h')
    #找不到返回-1
    print(result)
    # 字符串的长度
    result = len(str)
    print(result)
    # 统计某个字符出现的次数
    result = str.count('l')
    print(result)
    # 替换字符串的指定数据
    result = str.replace('l','x')
    print(result)
    # 字符串的分割
    str = '你,我,他'
    result = str.split(',')
    print(result)
    # 判断一个字符串是否是指定数据开头/结尾
    url = 'http://123.com'
    result = url.startswith('http')
    print(result)
    result = url.endswith('http')
    print(result)
    # 把字符串用指定数据分割
    str = 'aaabcccc'
    result = str.partition('b')
    print(result)
    sub_str = '_'
    #也可以分割列表
    result = sub_str.join(str)
    print(result)
    # 去除左右空格
    str = ' hello '
    print(str)
    result = str.strip()
    print(result)
    #去除左边的空格
    str.lstrip()
    #去除右边的空格
    str.rstrip()
    #去除指定的数据
    str= 'asdhello'
    result = str.strip('asd')
    print(result)
    list.append(1)
    # 列表
    list = [123, '12', 12, 'hello', 123, 123, '你好']
    print(list, type(list))
    #增删改查
    list.append(1)
    list.insert(3, '插入到hello前面')
    #删除了第一个123
    list.remove(123)
    #删除第一个元素
    del list[0]
    #判断指定数据是否在列表中
    result = 12 in list
    print(result)
    #获取数据在列表中的索引
    result = list.index(123)
    print(list, result)
    #获取数据在列表中的个数
    result = list.count(123)
    print(list, result)
    #元组
    #数据不能修改
    myTuple = (12, 21，'hello')
    print(myTuple, type(myTuple))
    #如果元组只有一个元素，那么元组的类型就变成了元素的类型，除非加了逗号
    # 判断数据是否在元组中
    myTuple = (12, 21, 'hello')
    result = 12 not in myTuple
    print(result)
    # 元素在元组中的个数
    result = myTuple.count(12)
    print(result)

    # 字典
    #字典是无序的
    my_dict = {'name':'沐雪','123':123}
    value = my_dict['name']
    value = my_dict.get('name','默认值')
    print(value)
    #增加
    my_dict["age"] = 18
    print(my_dict)
    #删除
    del my_dict['age']
    print(my_dict)
    value = my_dict.pop('123')
    print(value)
    #随机删除
    my_dict.popitem()
    print(my_dict)
    # 获取所有value/key
    result = my_dict.values()
    print(result)
    result = my_dict.keys()
    print(result)
    # 判断key是否在字典里面
    result = 'age' in my_dict
    print(result)
    # 格式化输出符
    # %s %d %f %x
    name = '沐雪'
    print('我叫%s' % name)

    # 循环
    #for和while都可以搭配else语句使用
    # while
    num = 0
    while num < 5:
        print(num)
        num += 1
    else:
    print('over')
    #for
    for i in range(0, 6, 2):
        print(i)
    else:
    print('over')
    #break
    #不会执行else的代码
    num = 0
    while num < 5:
        num += 1
        if num == 3:
            break
        print(num)
    else:
    print('over')
    #continue
    会执行else的代码
    num = 0
    while num < 5:
        num += 1
        if num == 3:
            continue
        print(num)
    else:
    print('over')
    # 集合
    #集合里面的数据不能够重复
    #集合列表元组之间可以相互转换

    my_set = set()
    my_set = {'123','hello','world'}
    #删除
    my_set.remove('123')
    #可以删除没有的数据
    my_set.discard('hello')
    print(my_set)
    #增加
    my_set.add('2333')
    #集合是无序的

    #遍历取出数据
    for val in my_set:
    print(val)

    #遍历：获取所有容器类型（字符串，列表，元组，集合，字典）里面的元素
    #遍历列表元素和下标
    my_list= enumerate([12,3112])
    for val in my_list:
    print(val)
    # 获取元素和下标
    #列表
    for index,value in my_list:
    print(index,value)
    
    print(my_dict.values())
    #字典
    for key in my_dict:
        print(key)

    for value in my_dict.values():
        print(value)

    for key, value in my_dict.items():
        print(key, value)

    #集合
    my_set = {12, 3123, 123}
    for value in my_set:
        print(value)

    #拆包(和js的解构赋值差不多的)
    #把容器类型每一个数据都用变量保存
    my_dict = {'name': '123', 'age': '1'}
    a1, a2 = my_dict
    print(a1, a2)
    my_set = {1, 2}
    b1, b2 = my_set
    print(b1, b2)
    my_list = [1, 2]
    num1, num2 = my_list
    print(num1, num2)

    #函数
    a = 10


    def fn(name='muxue', age=18):
        #使用global 可以在函数里修改全局变量
        global a
        a = 20
        print(name, age, a)

    fn()
    fn('joke', 122)
    fn(age=12, name='qwe')
    # 关键字传参必须放在后边
    fn('qweq', age=123)
    print(a)
    # 不定长位置参数
    def sum(*args):
        #把参数封装到元组里
        print(args, type(args))
        result = 0
        for val in args:
        result += val
        return result
    result = sum(1, 2, 3, 4)
    print(result)
    #不定长关键字参数
    def sum(**kwargs):
        # 把参数封装到字典里
        print(kwargs, type(kwargs))
        for key, value in kwargs.items():
            print(key, value)
    sum(a=1, b=2)

    #装饰器
    def decorator(func):
        print('装饰器')

        def inner(sum1, sum2):
            func(sum1, sum2)
        return inner
    @decorator
    def my_func(sum1, sum2):
        result = sum1 + sum2
        print(result)
    my_func(1, 2)

    #文件操作
    #r只读，w写，a追加，rb以二进制读取，wb以二进制写，ab以二进制追加，
    #r+ w+ a+支持读写 rb+ wb+ rb+支持二进制读写

    file = open('test.py', 'r+',encoding='utf-8')
    d='hello'
    data= d.encode('utf-8')
    file.write(data)
    content = file.read()
    print(content)
    file.close()

    #类
    class Person():
        def __init__(self, name, age):
            self.name = name
            self.age = age

        def __str__(self):
            return self.name

        def __del__(self):
            print('销毁了')


    p = Person('沐雪', 12)
    print(p)

    #继承封装和多态
    class Person():
        def __init__(self, name, age):
            self.name = name
            self.age = age

        def __str__(self):
            return self.name

        def __del__(self):
            print('销毁了')

    ```





