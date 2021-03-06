## Typical DP Questions Collection
### Backpack, coin change and climbing stairs
#### Backpack Problem
1. 0-1 backpack without value. Fill the backpack as full as possible(no value, items cannot be divided, can only be used once).  
[92. Backpack](https://www.lintcode.com/problem/backpack/description)
```
class Solution {
public:
    /**
     * @param m: An integer m denotes the size of a backpack
     * @param A: Given n items with size A[i]
     * @return: The maximum size
     */
    int backPack(int m, vector<int> &A) {
        int capacity = m;
        int numOfItems = A.size();
        if(numOfItems == 0 || capacity == 0) return 0;
        vector<int> dp(capacity + 1, 0); // [0, m]
        for(int i = 0; i < numOfItems; i++){
            for(int j = capacity; j >= 0; j--){
                if(j >= A[i]){
                    dp[j] = max(dp[j], dp[j - A[i]] + A[i]);
                }
            }
        }
        return dp[capacity];
    }
};
```
Note that we go through each item in the first loop and go through each capacity of backpack in the second loop. And in the second loop, we start at the end of the dp array instead of the head, otherwise items can be picked multiply times and it becomes a complete backpack.

2. 0-1 backpack with value and fill the backpack to get the max value.  
[0-1背包有价值](https://www.lintcode.com/problem/backpack-ii/)
```
        for(int i = 0; i < numOfItems; i++){
            for(int j = capacity; j >= 0; j--){
                if(j >= A[i]){
                    dp[j] = max(dp[j], dp[j - A[i]] + v[i])
                }
            }
        }
```
It's also an 0-1 backpack quesion. So for the second loop, we also need to do it from the end.  
ref: [花花酱 0-1 Knapsack Problem 01背包问题 - 刷题找工作 SP10](https://www.youtube.com/watch?v=CO0r6kcwHUU&t=896s)  

3. Compelete backpack  
[完全背包问题](https://www.lintcode.com/problem/backpack-iii/description)  
```
        for(int i = 0; i < numOfItems; i++){
            for(int j = 0; j <= capacity; j--){
                if(j >= A[i]){
                    dp[j] = max(dp[j], dp[j - A[i]] + v[i])
                }
            }
        }
```
Since we can get same item multiple times from the backpack this time, we should go from the start of the array at the second loop. 

4. Compute the number of ways that the backpack can be fullfilled  
[背包问题——“01背包”及“完全背包”装满背包的方案总数分析及实现](https://blog.csdn.net/wumuzi520/article/details/7021210)
[0-1背包方案数](https://www.lintcode.com/problem/backpack-v/description)
```
     vector<int> dp(capacity + 1, 0); // [0, m]
     dp[0] = 1;
     for(int i = 0; i <= capacity; i++){
            for(int j = numOfItem; j > 0; j--){
                if(j >= A[i]){
                    dp[i] += dp[i - A[j]];
                }
            }
     }
```

[完全背包方案数](https://www.lintcode.com/problem/backpack-iv/description)
```
     vector<int> dp(capacity + 1, 0); // [0, m]
     dp[0] = 1;
     for(int i = 0; i <= capacity; i++){
            for(int j = 0; j < numOfItem; j++){
                if(j >= A[i]){
                    dp[i] += dp[i - A[j]];
                }
            }
     }
```


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
```
for(int i = 0; i < coins.size(); i++){
            for(int j = 0; j <= amount; j++){
                if(j >= coins[i]) dp[j] += dp[j - coins[i]];
            }
        }
```


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


