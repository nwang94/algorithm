Leetcode 234
-------------------------------------
Basic idea: 
- cut the linked list into first half and second half
- reverse the second half
- check val one by one

**recursive**</br>
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
    bool isPalindrome(ListNode* head) {
        if(!head || head->next == nullptr) return true;
        int size = dfs(head, 0);
        int todo = 0;
        ListNode* first = head;
        ListNode* second = head;
        ListNode* temp = first;
        ListNode* r_second = nullptr;
        while (todo < size) {
            if(todo < size / 2) {
                temp = temp->next;
                second = second->next;
            } else {
                temp = nullptr;
                break;
            }
            todo++;
        }
        // reverse a linkedlist
        r_second = reverseLinkedList(second);
        for(int i = 0; i < size / 2; i++) {
            if(first->val != r_second->val) {
                return false;
            }
            first = first->next;
            r_second = r_second->next;
        }
        return true;
    }
    
private:    
    int dfs(ListNode* head, int dep) {
        if(head == nullptr) return dep;
        
        return dfs(head->next, dep + 1);
    }
    
//recursive
    ListNode* reverseLinkedList(ListNode* head) {
        if(!head || head->next == nullptr) {
          return head;
        }
        
        ListNode* reverseHead = reverseLinkedList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return reverseHead;
    }
```
*time complexity = O(n)</br>
space complexity = O(n) (because of recusion stack)*

**iterative**
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
    bool isPalindrome(ListNode* head) {
        if(!head || head->next == nullptr) return true;
        int size = dfs(head, 0);
        int todo = 0;
        ListNode* first = head;
        ListNode* second = head;
        ListNode* temp = first;
        ListNode* r_second = nullptr;
        while (todo < size) {
            if(todo < size / 2) {
                temp = temp->next;
                second = second->next;
            } else {
                temp = nullptr;
                break;
            }
            todo++;
        }
        // reverse a linkedlist
        r_second = reverseLinkedList(second);
        for(int i = 0; i < size / 2; i++) {
            if(first->val != r_second->val) {
                return false;
            }
            first = first->next;
            r_second = r_second->next;
        }
        return true;
    }
    
private:    
    int dfs(ListNode* head, int dep) {
        if(head == nullptr) return dep;
        
        return dfs(head->next, dep + 1);
    }
    
    //iterative
    ListNode* reverseLinkedList(ListNode* head) {
        if(!head || head->next == nullptr) {
            return head;
        }
        
        ListNode* prev = nullptr;
        ListNode* curr = head;
        ListNode* next = curr->next;
        while(curr) {
            next = curr->next;
            curr->next = prev;
            
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```
*time complexity = O(n)</br>
space complexity = O(1) (because of recusion stack)*
