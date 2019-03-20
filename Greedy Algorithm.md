### Greedy Algorithm

#### [134. Gas Station](https://leetcode.com/problems/gas-station/)
Problem can be solved in two steps.
1. Notice that if sum(cost) > sum(gas), then we cannot make it no matter where we start to travel.
But the question is not only asking whether we can start traveling, but also where we should start.
2. if we start at `0` and run out of gas at `p`, for example. Let's take a further look at this problem.
from `0` to `p-1`
```
Sum_0_to_p-1 = gas[0] + gas[1] + gas[2] + ... + gas[p-2] - (cost[0] + cost[1] + ... cost[p-2]) > 0
Sum_0_to_p-1 = gas_left[0] + gas_left[1] + ... + gas_left[p-2] > 0
```

from `0` to `p`
```
Sum_0_to_p = (gas[0] + gas[1] + gas[2] + ... + gas[p-2] + gas[p-1]) - (cost[0] + cost[1] + ... cost[p-2] + cost[p-1]) > 0
Sum_0_to_p = gas_left[0] + gas_left[1] + ... + gas_left[p-2] + gas_left[p-1] > 0
Sum_0_to_p = gas_left[0] + gas_left[1] + ... + gas_left[p-2] < - gas_left[p-1] 
```


#### reference
https://blog.csdn.net/DERRANTCM/article/details/47678215
