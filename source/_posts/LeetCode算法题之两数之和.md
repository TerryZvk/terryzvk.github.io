---
title: LeetCode算法题之两数之和
date: 2019-01-09 23:08:29
tags: ['LeetCode', '算法', '两数之和']
---
### 题目
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
<!-- more -->
示例:
```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9

所以返回 [0, 1]
```
### 解答
我的解答，简单暴力。。
#### 解答一
* 时间复杂度：O(n^2)， 对于每个元素，我们试图通过遍历数组的其余部分来寻找它所对应的目标元素，这将耗费 O(n) 的时间。因此时间复杂度为 O(n^2)。
* 空间复杂度：O(1)
* 执行时间 148ms
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let length = nums.length;
    for(let i = 0; i <length; i++){
        for(let j = i+1; j < length; j++){
            if(nums[i] + nums[j] === target){
                return [i,j];
            }
        }
    }
};
```
#### 解答二

用es6的Map , 遍历一次即可

```
var twoSum = function(nums, target) {
    let targetMap = new Map()
    for (let i = 0; i < nums.length; i++) {
      const key = target - nums[i]
      if (targetMap.has(key)) {
        return [targetMap.get(key), i]
      }
      targetMap.set(nums[i], i)
    }
};
```