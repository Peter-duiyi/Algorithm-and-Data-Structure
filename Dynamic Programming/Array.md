## DP quesion collection
### Backpack, coin change and climbing stairs
#### Backpack Problem
1. 0-1 backpack without value. Fill the backpack as full as possible(no value, items cannot be divided, can only be used once)  
[92. Backpack](https://www.lintcode.com/problem/backpack/description)

2. 0-1 backpack with value.



ref:  
[背包问题九讲](https://www.kancloud.cn/kancloud/pack/70125)  
[leetcode背包问题汇总](https://blog.csdn.net/u013166817/article/details/85449218)

#### Coin Change
1. Get least number of coin changes  
[322. Coin Change](https://leetcode.com/problems/coin-change/)  
for each Amount(Problem) `x`, we convert it into a smaller Amount(subproblem) `x - coin[i] + 1`, where `0 =< i < coin.size()`.
And we find the smallest result among these subproblems.
```
dp[i] = min(dp[i], 1 + dp[i - coins[j]]);
```
ref : https://blog.csdn.net/wdxin1322/article/details/9501163  

2. Get number of combination of coin change  
[518. Coin Change 2](https://leetcode.com/problems/coin-change-2/)  



#### climbing stairs



### Continuous Subarray
1. Get the maxSum of Continues Subarray(or get the array)  
[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
```
dp[i] = max[dp[i-1] + nums[i], nums[i]];
```
```
prev = max[prev + nums[i], nums[i]];
```
if we want to get the array:
my way is we go through dp and find the largest one(which is equal to sum), suppose the index is j, 
and then go backwards starting at that number until we find a negative number, suppose the index is i,
then the maxSubarray should be [i-1, j]

2. Get the length of longest Continuous increasing Subarray(or shortest)(or get the array)  
[674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)
```
if(nums[i] > nums[i-1]) dp[i] = dp[i-1] + 1;
```
```
if(nums[i] > nums[i-1]) prev = prev + 1;
```
notice dp[0] or the initial value of prev should be `1` not 0, and when when nums[i] <= nums[i-1], dp[i] or prev should be reset to `1` not 0  
if we want to get the array:  
It's actually the same way as written above, we go through dp and find the largest one(which is equal to sum), suppose the index is j, 
and then go backwards starting at that number until we find a `1`, suppose the index is i,
then the maxSubarray should be [i-1, j]


### Not Continuous Subarray
1. get the length of longest incresing subsequence(or array)  
[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

2. get the number of longest increasing Subarray  
[673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)


