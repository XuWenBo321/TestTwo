
<!-- GTM-TOC -->
* [1. 非空链表两数相加](#1-非空链表两数相加)
* [2. 三个数之和](#2-三个数之和)

<!-- GTM-TOC -->


# 1.非空链表两数相加
[题目描述](https://leetcode-cn.com/problems/add-two-numbers/)

```cpp
  /**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1 == NULL && l2 == NULL) {
            return NULL;
        }
        if(l1 == NULL || l2 == NULL) {
            return l1?l1:l2;
        }
        ListNode* result = new ListNode(0);
        ListNode* head = result;
        int tmp = 0;
        int Sum = 0;
        while(l1 && l2) {
            Sum = (tmp + l1->val + l2->val) % 10;
            tmp = (tmp + l1->val + l2->val) / 10;
            result->next = new ListNode(Sum);
            result = result->next;
            l1 = l1->next;
            l2 = l2->next;
        }
        while(l1) {
            Sum = (tmp + l1->val) % 10;
            tmp = (tmp + l1->val) / 10;
            result->next = new ListNode(Sum);
            result = result->next;
            l1 = l1->next;
        }
        while(l2) {
            Sum = (tmp + l2->val) % 10;
            tmp = (tmp + l2->val) / 10;
            result->next = new ListNode(Sum);
            result = result->next;
            l2 = l2->next;
        }
        if(tmp != 0) {
            result->next = new ListNode(tmp);
            result = result->next;
        }
        return head->next;
    }
};
```

# 三个数之和
[题目描述](https://leetcode-cn.com/problems/3sum/)

```cpp
  class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int length = nums.size();
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        if(length < 3 || nums[0] > 0 || nums[length - 1] < 0) {
            return result;
        }
        for(int i = 0;i < length; i++) {
            if(nums[i] > 0) {
                break;
            }
            int tmp = nums[i];
            if(i>0 && tmp == nums[i - 1]) 
                continue;
            int L = i + 1, R = length - 1;
            while(L < R) {
                if(nums[L] + nums[R] < -tmp) {
                    L++;
                }
                else if(nums[L] + nums[R] > -tmp) {
                    R--;
                }
                else {
                    if(L == i + 1 || R == length - 1) {
                        result.push_back(vector<int> {tmp, nums[L], nums[R]});
                        L++;
                        R--;
                    }
                    else if(nums[L] == nums[L-1]) {
                        L++;
                    }
                    else if(nums[R] == nums[R+1]) {
                        R--;
                    }
                    else {
                        result.push_back(vector<int> {tmp, nums[L], nums[R]});
                        L++;
                        R--;
                    }
                }
            }
        }
        return result;
    }
};
```
