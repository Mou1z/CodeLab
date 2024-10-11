# Merge Two Sorted Lists

- **Source:** [https://projecteuler.net/problem=1](https://leetcode.com/problems/merge-two-sorted-lists)
- **Language Used:** C++

### Description
It's nothing extra-oridinary. Simply my solution to the "Merge Two Sorted Linked-Lists" problem on LeetCode. In this solution I minimized the usage of if-conditions and focused on code readability.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode * output = new ListNode();
        ListNode * current = output;

        while(list1 && list2) {
            if(list1->val < list2->val) {
                current->next = list1;
                list1 = list1->next;
            } else {
                current->next = list2;
                list2 = list2->next;
            }

            current = current->next;
        }

        if(list1) 
            current->next = list1;
        if(list2) 
            current->next = list2;

        output = output->next;
        return output;
    }
};
```
