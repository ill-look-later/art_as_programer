### 1. Two Sum


Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **_exactly_** one solution.

**Example:**  

<pre>
Given nums = [2, 7, 11, 15], target = 9,

Because nums[**0**] + nums[**1**] = 2 + 7 = 9,
return [**0**, **1**].
</pre>

就是给定一个整型数组，给定一个target整型数；要求找出数组中两个数相加等于给定的target；返回这两个数在数组中的索引；即`nums[i] + nums[j] == target`; 返回 `[i， j]`;