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

