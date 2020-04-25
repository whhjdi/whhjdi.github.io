---
title: JS练习
date: 2018-08-20 20:58:51
tags: [JavaScript]
categories: ["Javascript"]
cover_img: https://ws1.sinaimg.cn/large/006tNbRwly1fxzevadfi6j31650u0npe.jpg
---

> 练习 js,看看自己掌握的如何,思路可以有很多,有些真的很难想到,amazing

1. 题目描述
   找出元素 item 在给定数组 arr 中的位置
   输出描述:
   如果数组中存在 item，则返回元素在数组中的位置，否则返回 -1

   ```javascript
   function indexOf(arr, item) {
     if (Array.prototype.indexOf) {
       return arr.indexOf(item);
     } else {
       for (let i = 0; i < arr.length; i++) {
         if (arr[i] === item) {
           return i;
         }
       }
     }
     return -1;
   }
   indexOf([1, 2, 3, 4, 5], 3); //2
   ```

   <!--more-->

2. 题目描述
   计算给定数组 arr 中所有元素的总和
   输入描述:
   数组中的元素均为 Number 类型

   ```javascript
   function sum(arr) {
     return arr.reduce((pre, cur) => {
       return pre + cur;
     });
   }
   sum(1, 2, 3, 4); //10
   ```

   还可以用 for 循环,用 forEach,这里只写最简便的

3. 题目描述
   移除数组 arr 中的所有值与 item 相等的元素。不要直接修改数组 arr，结果返回新的数组

   ```javascript
   function remove(arr, item) {
     return arr.filter(function(val) {
       return val !== item;
     });
   }
   ```

4. 题目描述
   移除数组 arr 中的所有值与 item 相等的元素，直接在给定的 arr 数组上进行操作，并将结果返回

   ```javascript
   function removeWithoutCopy(arr, item) {
     var n = arr.length;
     for (var i = 0; i < n; i++) {
       if (arr[0] !== item) {
         arr.push(arr[0]);
       }
       arr.shift();
     }
     return arr;
   }
   ```

   这个思路赞,真的没想到

5. 题目描述
   在数组 arr 末尾添加元素 item。不要直接修改数组 arr，结果返回新的数组

   ```javascript
   function append(arr, item) {
     return arr.concat([item]);
   }
   var append2 = function(arr, item) {
     var newArr = arr.slice(0);
     newArr.push(item);
     return newArr;
   };
   ```

6. 题目描述
   删除数组 arr 最后一个元素。不要直接修改数组 arr，结果返回新的数组
   第一反应就是用 slice,还有就是拷贝一个数组,比如 concat,然后 pop

   ```javascript
   function truncate(arr) {
     return arr.slice(0, -1);
   }
   ```

7. 题目描述
   在数组 arr 开头添加元素 item。不要直接修改数组 arr，结果返回新的数组
   还想到一种 slice 复制数组,然后 unshift

   ```javascript
   function prepend(arr, item) {
     return [item].concat(arr);
   }
   ```

8. 题目描述
   删除数组 arr 第一个元素。不要直接修改数组 arr，结果返回新的数组

   ```javascript
   function curtail(arr) {
     return arr.slice(1);
   }
   ```

9. 题目描述
   合并数组 arr1 和数组 arr2。不要直接修改数组 arr，结果返回新的数组

   ```javascript
   function concat(arr1, arr2) {
     return arr1.concat(arr2);
   }
   ```

10. 题目描述
    在数组 arr 的 index 处添加元素 item。不要直接修改数组 arr，结果返回新的数组
    第一反应就是这个,slice(0)复制数组真的好用,同样的用 concat 复制数组也可以

    ```javascript
    function insert(arr, item, index) {
      var newArr = arr.slice(0);
      newArr.splice(index, 0, item);
      return newArr;
    }
    ```

11. 题目描述
    统计数组 arr 中值等于 item 的元素出现的次数
    emmm,把等于 item 的值放到新数组里,然后新数组的 length 就是次数
    还有一种思路就是声明一个计数的变量 a,遍历数组,值等于 item,a++

    ```javascript
    function count(arr, item) {
      return arr.filter(function(val) {
        return val === item;
      }).length;
    }
    ```

12. 题目描述
    找出数组 arr 中重复出现过的元素
    这个相对来说比较复杂

    ```javascript
    function duplicates(arr) {
    return arr.sort().filter((\_, i) =>
    arr[i] === arr[i + 1] && arr[i] !== arr[i - 1]
    );
    }
    ```

13. 题目描述
    为数组 arr 中的每个元素求二次方。不要直接修改数组 arr，结果返回新的数组
    ```javascript
    function square(arr) {
    return arr.map(function(val){
    return val\*val
    })
    }
    ```
14. 题目描述
    在数组 arr 中，查找值与 item 相等的元素出现的所有位置
    最好想的方法
    ```javascript
    function findAllOccurrences(arr, target) {
      let result = [];
      arr.forEach(function(val, index) {
        if (val === target) {
          result.push(index);
        }
      });
      return result;
    }
    ```
15. 题目描述
    实现一个打点计时器，要求
    1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1
    2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作
    3、第一个数需要立即输出
    ```javascript
    function count(start, end) {
      //立即输出第一个值
      console.log(start++);
      var timer = setInterval(function() {
        if (start <= end) {
          console.log(start++);
        } else {
          clearInterval(timer);
        }
      }, 100);
      //返回一个对象
      return {
        cancel: function() {
          clearInterval(timer);
        }
      };
    }
    ```
16. 题目描述
    实现 fizzBuzz 函数，参数 num 与返回值的关系如下：
    1、如果 num 能同时被 3 和 5 整除，返回字符串 fizzbuzz
    2、如果 num 能被 3 整除，返回字符串 fizz
    3、如果 num 能被 5 整除，返回字符串 buzz
    4、如果参数为空或者不是 Number 类型，返回 false
    5、其余情况，返回参数 num

    ```javascript
    function fizzBuzz(num) {
      if (num % 3 === 0 && num % 5 === 0) return "fizzbuzz";
      else if (num % 3 === 0) return "fizz";
      else if (num % 5 === 0) return "buzz";
      else if (num === null || typeof num !== "number") return false;
      else return num;
    }
    ```

17. 题目描述
    将数组 arr 中的元素作为调用函数 fn 的参数
    ```javascript
    function argsAsArray(fn, arr) {
      return fn.apply(this, arr);
    }
    ```
18. 题目描述
    将函数 fn 的执行上下文改为 obj 对象
    ```javascript
    function speak(fn, obj) {
      return fn.call(obj);
    }
    ```
19. 题目描述
    实现函数 functionFunction，调用之后满足如下条件：
    1、返回值为一个函数 f
    2、调用返回的函数 f，返回值为按照调用顺序的参数拼接，拼接字符为英文逗号加一个空格，即 ‘, ‘
    3、所有函数的参数数量为 1，且均为 String 类型
    ```javascript
    function functionFunction(str) {
      return function f(str2) {
        return str + ", " + str2;
      };
    }
    ```
20. 题目描述
    实现函数 makeClosures，调用之后满足如下条件：
    1、返回一个函数数组 result，长度与 arr 相同
    2、运行 result 中第 i 个函数，即 resulti，结果与 fn(arr[i]) 相同
    ```javascript
    function makeClosures(arr, fn) {
      var result = [];
      arr.forEach(function(val, index) {
        result[index] = function() {
          return fn(arr[index]);
        };
      });
      return result;
    }
    ```
21. 题目描述
    已知函数 fn 执行需要 3 个参数。请实现函数 partial，调用之后满足如下条件：
    1、返回一个函数 result，该函数接受一个参数
    2、执行 result(str3) ，返回的结果与 fn(str1, str2, str3) 一致
    ```javascript
    function partial(fn, str1, str2) {
      var result = function(str3) {
        return fn(str1, str2, str3);
      };
      return result;
    }
    ```
22. 题目描述
    函数 useArguments 可以接收 1 个及以上的参数。请实现函数 useArguments，返回所有调用参数相加后的结果。本题的测试参数全部为 Number 类型，不需考虑参数转换。
    ```javascript
    function useArguments() {
      var sum = 0;
      for (var i = 0; i < arguments.length; i++) {
        sum += arguments[i];
      }
      return sum;
    }
    ```
23. 题目描述
    实现函数 callIt，调用之后满足如下条件
    1、返回的结果为调用 fn 之后的结果
    2、fn 的调用参数为 callIt 的第一个参数之后的全部参数
    ```javascript
    function callIt(fn) {
      var args = Array.prototype.slice.call(arguments, 1);
      return fn.apply(this, args);
    }
    ```
24. 题目描述
    实现函数 partialUsingArguments，调用之后满足如下条件：
    1、返回一个函数 result
    2、调用 result 之后，返回的结果与调用函数 fn 的结果一致
    3、fn 的调用参数为 partialUsingArguments 的第一个参数之后的全部参数以及 result 的调用参数
    ```javascript
    function partialUsingArguments(fn) {
      //先获取 p 函数第一个参数之后的全部参数
      var args = Array.prototype.slice.call(arguments, 1);
      //声明 result 函数
      var result = function() {
        //使用 concat 合并两个或多个数组中的元素
        return fn.apply(null, args.concat([].slice.call(arguments)));
      };
      return result;
    }
    ```
25. :star: 题目描述
    已知 fn 为一个预定义函数，实现函数 curryIt，调用之后满足如下条件：
    1、返回一个函数 a，a 的 length 属性值为 1（即显式声明 a 接收一个参数）
    2、调用 a 之后，返回一个函数 b, b 的 length 属性值为 1
    3、调用 b 之后，返回一个函数 c, c 的 length 属性值为 1
    4、调用 c 之后，返回的结果与调用 fn 的返回值一致
    5、fn 的参数依次为函数 a, b, c 的调用参数
    ```javascript
    function curryIt(fn) {
      var length = fn.length,
        args = [];
      var result = function(arg) {
        args.push(arg);
        length--;
        if (length <= 0) {
          return fn.apply(this, args);
        } else {
          return result;
        }
      };
      return result;
    }
    ```
26. 题目描述
    完成函数 createModule，调用之后满足如下要求：
    1、返回一个对象
    2、对象的 greeting 属性值等于 str1， name 属性值等于 str2
    3、对象存在一个 sayIt 方法，该方法返回的字符串为 greeting 属性值 + ‘, ‘ + name 属性值
    ```javascript
    function createModule(str1, str2) {
      var obj = {
        greeting: str1,
        name: str2,
        sayIt: function() {
          //两个属性前面都需要加上 this
          return this.greeting + ", " + this.name;
        }
      };
      return obj;
    }
    ```
27. 题目描述
    获取数字 num 二进制形式第 bit 位的值。注意：
    1、bit 从 1 开始
    2、返回 0 或 1
    3、举例：2 的二进制为 10，第 1 位为 0，第 2 位为 1

28. 题目描述
    给定二进制字符串，将其换算成对应的十进制数字
    ```javascript
    function base10(str) {
      return parseInt(str, 2);
    }
    ```
29. 题目描述
    将给定数字转换成二进制字符串。如果字符串长度不足 8 位，则在前面补 0 到满 8 位。
    这个高,实在是高
    ```javascript
    function convertToBinary(num) {
      var s = num.toString(2);
      return "00000000".slice(s.length) + s;
    }
    ```
30. 题目描述
    给定一个构造函数 constructor，请完成 alterObjects 方法，将 constructor 的所有实例的 greeting 属性指向给定的 greeting 变量。

    ```javascript
    function alterObjects(constructor, greeting) {
      constructor.prototype.greeting = greeting;
    }
    ```

31. 题目描述
    找出对象 obj 不在原型链上的属性(注意这题测试例子的冒号后面也有一个空格~)
    1、返回数组，格式为 key: value
    2、结果数组不要求顺序

    ```javascript
    //Object.keys 方法（156 ms）
    //返回可枚举的实例属性的数组。
    function iterate(obj) {
      return Object.keys(obj).map(function(key) {
        return key + ": " + obj[key];
      });
    }
    //for-in 和 hasOwnProperty 方法（171 ms）
    //前者用于遍历所有属性，后者用于判断是否为实例属性。
    function iterate(obj) {
      const res = [];
      for (var prop in obj) {
        if (obj.hasOwnProperty(prop)) {
          res.push(prop + ": " + obj[prop]);
        }
      }
      return res;
    }
    //Object.getOwnPropertyNames 方法（209 ms）
    //用法跟 1.一样，区别在于返回的是所有实例属性（包括不可枚举的）。
    function iterate(obj) {
      return Object.getOwnPropertyNames(obj).map(function(key) {
        return key + ": " + obj[key];
      });
    }
    ```

32. 题目描述
    给定字符串 str，检查其是否包含数字，包含返回 true，否则返回 false

    ```javascript
    function containsNumber(str) {
      var re = /\d/;
      return !!re.test(str);
    }
    ```

33. 题目描述
    给定字符串 str，检查其是否包含连续重复的字母（a-zA-Z），包含返回 true，否则返回 false
    ```javascript
    function containsRepeatingLetter(str) {
      return /([a-zA-Z])\1/.test(str);
    }
    ```
34. 题目描述
    给定字符串 str，检查其是否以元音字母结尾
    1、元音字母包括 a，e，i，o，u，以及对应的大写
    2、包含返回 true，否则返回 false
    ```javascript
    function endsWithVowel(str) {
      return /[a,e,i,o,u]$/i.test(str);
    }
    ```
35. 题目描述
    给定字符串 str，检查其是否包含 连续 3 个数字
    1、如果包含，返回最新出现的 3 个数字的字符串
    2、如果不包含，返回 false
    ```javascript
    function captureThreeNumbers(str) {
      //声明一个数组保存匹配的字符串结果
      var arr = str.match(/\d{3}/);
      //如果 arr 存在目标结果，则返回第一个元素，即最早出现的目标结果
      if (arr) return arr[0];
      else return false;
    }
    ```
36. 题目描述
    给定字符串 str，检查其是否符合如下格式
    1、XXX-XXX-XXXX
    2、其中 X 为 Number 类型
    ```javascript
    function matchesPattern(str) {
      return /^(\d{3}-){2}\d{4}$/.test(str);
    }
    ```
37. 题目描述
    给定字符串 str，检查其是否符合美元书写格式
    1、以 $ 开始
    2、整数部分，从个位起，满 3 个数字用 , 分隔
    3、如果为小数，则小数部分长度为 2
    4、正确的格式如：$1,023,032.03 或者 $2.03，错误的格式如：$3,432,12.12 或者 $34,344.3

    ```javascript
    function isUSD(str) {
      return /^\$\d{1,3}(,\d{3})\*(\.\d{2})?$/.test(str);
    }
    ```
