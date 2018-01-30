---
title: leetcode
date: 2018-01-30 19:47:30
tags: leetcode
---
## Record leetcode practices

Since My major is not computer science & softward engineering, I am not good at data structure and algorithm. In order to overcome it, I choose to do more relative practices in leetcode. Practice makes perfect.

`I do practices by tags and from easy to difficult and I use javascript`

> Two sum

Description: 
  * Given an array of integers, return indices of the two numbers such that they add up to a specific target.
  * You may assume that each input would have exactly one solution, and you may not use the same element twice.

```javascript
  /**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
	var result = []
    nums.forEach(function(num,index) {
        if(index === nums.length - 1) {
           return
           }
        var index2 = index + 1
        for(var len= nums.length;index2  < len;index2 ++) {
            if(num + nums[index2] === target) {
			result = [index, index2]
               break;
               }
        }
    })
    return result
};
```

> Reverse Integer

Description:
  * Given a 32-bit signed integer, reverse digits of an integer.

```javascript
  /**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    var y = Math.abs(x) + ''
    y = y.split('').reverse()
    if(y[0] == 0) {
       y = y.slice(1)
       }
    if(+y.join('') > (Math.pow(2,31) - 1)) {
       return 0
       }
    else {
    if(x < 0) {
       y.unshift('-')
        y = y.join('')
       } else {
           y = y.join('')
       }
        return +y
    }
    
};
```