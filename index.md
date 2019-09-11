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
> 先计算差值,然后再num中找是不是有这个差值,有的话则找这两个值的下标 **难点在于**速度

**原题链接**
> [two_sum](https://leetcode.com/problems/two-sum/)

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


#### 2. Add Two Numbers
```cpp
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
**说明**
> 两个链表逆序保存着两个整数,输出这两个整数的和 **难点在于**处理进位

**思路**
> 遍历两个链表,然后实现带进位加法


**原题链接**
> [add_two_numbers](https://leetcode.com/problems/add-two-numbers/)

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *p = l1;//保存第一个整数的链表
        ListNode *q = l2;//保存第二个整数的链表
        int carry = 0;//进位
        int sum = 0;
        ListNode *head = new ListNode(0);
        ListNode *curr = head;
        while (p!=NULL || q!=NULL)//两个都不为空
        {
            int x = (p != NULL) ? p->val : 0;
            int y = (q != NULL) ? q->val : 0;
            int sum = carry + x + y;//带进位加法
            carry = sum / 10;//计算进位
            curr->next = new ListNode(sum % 10);//保存余数
            curr = curr->next;//更新结果指针
                        //移动两个整数指针
            if (p != NULL) p = p->next;
            if (q != NULL) q = q->next;
        }
        if (carry > 0) {//计算最后两个数进位 如果大于0 则新增节点
            curr->next = new ListNode(carry);
        }
        return head->next;
    }
};
```


#### 3. Longest Substring Without Repeating Characters
```cpp
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```
**说明**
> 最长子串 不能重复

**思路**
> 滑动窗口 --计算机网络中有应用
> 有点类似与快速排序 用两个游标指向左侧和右侧

**原题链接**
> [Longest_Substring_Without_Repeating_Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
	int ans = 0, i = 0, j = 0;//ans记录最大的窗口含有的值的数量 i表示left j表示right 相当于指针遍历s
	unordered_set<char> myset;//窗口
	while (i < n && j < n) {
            //如果不在窗口中,将char插入到窗口中 j++ 计算此时ans的最大值
            if (!myset.count(s[j]))
            {
                myset.insert(s[j]);
                j++;
                ans = max(ans, j - i);
            }//否则 即在窗口中,将该字符删除,使得i++
            else {
                myset.erase(s[i]);
                i++;
            }
	}
	return ans;
    }
};
```

#### 4. Median of Two Sorted Arrays
```cpp
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```
**说明**
> 将两个已经排序的数组计算出中值 要求 复杂度log(m+n)

**思路**
> 将两个数组合并成一个数组,因为两个数组是已经排好序的,联想到归并排序
> 归并排序的归并过程类似于两堆已经排好序的牌堆,需要将这两个牌堆放到一个牌堆,先比较牌堆最上面的两张牌,小于的那一张进入最终的牌堆,另一堆不动,一直到它小于另一牌堆的最上面的值

**原题链接**
> [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int i = 0, j = 0;//i是左侧牌堆 j控制右侧牌堆
	int mid = 0;
	int temp = 0;
	vector<int> v;//最终的牌堆
	//类似于归并排序  将两组数归并到一组数中
	while (i<nums1.size() && j<nums2.size())
	{//哪个小,就放入最终牌堆
            if (nums1[i] <= nums2[j])
                v.push_back(nums1[i++]);
            else
                v.push_back(nums2[j++]);
	}
        //因为可能一堆已经没牌了,所以需要将另一堆全部放入最终牌堆
	while (i<nums1.size())
	{
            v.push_back(nums1[i++]);
	}
	while (j < nums2.size())
	{
            v.push_back(nums2[j++]);
	}
	//计算中间值
	mid = v.size() / 2;
	if (v.size() % 2 == 0)
	{
            temp = mid - 1;
            return (v[temp] + v[mid]) / 2.0;
	}
	else
	{
            return v[mid];
	}
    }
};
```


