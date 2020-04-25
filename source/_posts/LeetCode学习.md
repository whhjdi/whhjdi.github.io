---
title: LeetCode学习
date: 2019-05-04 20:50:04
tags: [LeetCode]
cover_img: https://i.loli.net/2019/05/14/5cda3d237cbf516076.jpg
feature_img:
description:
keywords:
---

## EASY

1. 有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
	let valid = true;
	const stack = [];
	const mapper = {
		"{": "}",
		"[": "]",
		"(": ")"
	};
	//利用栈的先入后出的特性
	for (let key in s) {
		let val = s[key];
		if (["{", "[", "("].indexOf(val) > -1) {
			stack.push(val);
		} else {
			let p = stack.pop();
			if (val !== mapper[p]) {
				valid = false;
			}
		}
	}
	if (stack.length > 0) return false;
	return valid;
};
```

2. [557] 反转字符串中的单词 III

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例 1:

输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc"
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

```js
/*
 * @lc app=leetcode.cn id=557 lang=javascript
 *
 * [557] 反转字符串中的单词 III
 */
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function(s) {
	return s
		.split(" ")
		.map(item => {
			return item
				.split("")
				.reverse()
				.join("");
		})
		.join(" ");
};
```

3. [17] 电话号码的字母组合
   给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

示例:

输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

```js
/*
 * @lc app=leetcode.cn id=17 lang=javascript
 *
 * [17] 电话号码的字母组合
 */
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function(digits) {
	let s = "abc,def,ghi,jkl,mno,pqrs,tuv,wxyz";
	let map = ["", 1].concat(s.split(","));
	let num = digits.split("");
	let code = [];
	//把数字对应的字母放到code数组中
	num.forEach(item => {
		if (map[item]) {
			code.push(map[item]);
		}
	});

	//组合的方法
	let compose = arr => {
		let temp = [];
		let firstStr = arr[0] || "";
		let secondStr = arr[1] || "";
		for (let i = 0; i < firstStr.length; i++) {
			if (!secondStr.length) {
				temp.push(`${firstStr[i]}`);
			} else {
				for (let j = 0; j < secondStr.length; j++) {
					temp.push(`${firstStr[i]}${secondStr[j]}`);
				}
			}
		}
		arr.splice(0, 2, temp);
		if (arr.length > 1) {
			compose(arr);
		}
		return arr[0];
	};

	return compose(code);
};
```

4. [605] 种花问题

假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给定一个花坛（表示为一个数组包含 0 和 1，其中 0 表示没种植花，1 表示种植了花），和一个数 n 。能否在不打破种植规则的情况下种入 n 朵花？能则返回 True，不能则返回 False。

示例 1:

输入: flowerbed = [1,0,0,0,1], n = 1
输出: True
示例 2:

输入: flowerbed = [1,0,0,0,1], n = 2
输出: False
注意:

数组内已种好的花不会违反种植规则。
输入的数组长度范围为 [1, 20000]。
n 是非负整数，且不会超过输入数组的大小。

```js
/*
 * @lc app=leetcode.cn id=605 lang=javascript
 *
 * [605] 种花问题
 */
/**
 * @param {number[]} flowerbed
 * @param {number} n
 * @return {boolean}
 */
var canPlaceFlowers = function(flowerbed, n) {
	let max = 0;
	for (let i = 0; i < flowerbed.length; i++) {
		if (flowerbed[i] === 0) {
			if (i === 0) {
				if (!flowerbed[1]) {
					max++;
					i++;
				} else {
					if (flowerbed[1] === 0) {
						max++;
						i++;
					}
				}
			} else if (flowerbed[i - 1] === 0) {
				if (i === flowerbed.length - 1) {
					max++;
				} else if (flowerbed[i + 1] === 0) {
					max++;
					i++;
				}
			}
		}
	}
	return max >= n;
};
```
