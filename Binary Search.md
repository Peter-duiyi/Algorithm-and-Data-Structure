## Binary Search

### Principles
* binary search can only be used on a sorted array since it is the assumption.
* It's basically divide and conquer: Every time we look at the number that in the middle of the array and compare it with our target,
then move to the left or right part accordingly.
* Time Complexity: O(lgn). Suppose we have a size n problem and we have to divide k times to get 1 element left, 
then we will have this formular below:
```
n * (1/2)^k = 1
log2n = k
```
That means we need to divide log2n(which is k we mentioned before) times untill we get 1 element left.

### Code
notice: _v is a private vector<int> I used in the code below 
```
int binarySearch(int begin, int end, int target) {
		if (begin > end) return -1;  // if cannot find the number

		int mid = begin + (end - begin) / 2;
		if (_v[mid] < target) { // target is at the right side
			return binarySearch(mid + 1, end, target); // return here is necessary
		}
		if (_v[mid] > target) {                  //target is at the left side
			return binarySearch(begin, mid - 1, target);
		}
		if (_v[mid] == target) {
			return mid;
		}
}

```
This is a iterative version of BinarySearch, but I don't like the return type of this function because it always confuses me that where I should add a return.
Therefore, personally, I prefer to write a void function and store the result at a variable.
```
int res = 0;
void binarySearch(int begin, int end, int target) {
		if (begin > end) res = -1;  // if cannot find the number

		int mid = begin + (end - begin) / 2;
		if (_v[mid] < target) { // target is at the right side
			binarySearch(mid + 1, end, target); // return here is necessary
		}
		if (_v[mid] > target) {                  //target is at the left side
			binarySearch(begin, mid - 1, target);
		}
		if (_v[mid] == target) {
			res = mid;
		}
}
```
That looks much clear for me.
And we also have a while loop version: 
```
int bianrySearch(int begin, int end, int target) {
		while (begin <= end) {
			int mid = begin + (end - begin) / 2;
			if (_v[mid] < target) { // target is on the right
				begin = mid + 1;
			}
			if (_v[mid] > target) { // target is on the left
				end = mid - 1;
			}
			if (_v[mid] == target) { // find the number
				return mid;
			}
		}
		return -1; // cannot find 
	}
```
I think I like this one better than the previous one. And I feel free to use return type without worrying about where I should set exits for the program.

------
### Reference
wiki: https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95
