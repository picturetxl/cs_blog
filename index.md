## Welcome to txl Pages

### leetcode

#### 1. two sum
```cpp
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```
**说明**
> 一组数据里,把两个数据点相加等于目标值,则输出这两个数据点的位置,且不能使用相同的数据点两次

**思路**
> 先计算差值,然后再num中找是不是有这个差值,有的话则找这两个值的下标


```cpp
//采用map 使得算法复杂度减小为n,否则穷举,则需要n
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mymap;
	vector<int> result;
	int len = nums.size();
	for (int i = 0; i < len; ++i)
	{
		int remain = target - nums[i];//计算差值
                //是否含有这个差值
		if (mymap.count(remain)>0)//如果包含
		{//则将remain所在下标和当前的下标放入结果中去
			result.push_back(mymap[remain]);
			result.push_back(i);
			return result;
		}
		mymap[nums[i]] = i;//将值作为无序map的键(因为要寻找),下标作为值
	}
	return result;
    }
};
```
