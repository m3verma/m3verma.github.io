---
layout: default
---

# Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

### Constraints

1 <= s.length <= 10<sup>4</sup><br>
s consists of parentheses only '()[]{}'.

### Example 1

**Input:** s = "()"<br>
**Output:** true<br>

### Example 2

**Input:** s = "()[]{}"<br>
**Output:** true<br>

### Example 3

**Input:** s = "(]"<br>
**Output:** False<br>


## Solution

Here is the step-by-step approach of the algorithm:

1. Initialize an empty stack.
2. Traverse the input string character by character.
3. If the current character is an opening bracket (i.e., '(', '{', '['), push it onto the stack.
4. If the current character is a closing bracket (i.e., ')', '}', ']'), check if the stack is empty. If it is empty, return false, because the closing bracket does not have a corresponding opening bracket. Otherwise, pop the top element from the stack and check if it matches the current closing bracket. If it does not match, return false, because the brackets are not valid.
5. After traversing the entire input string, if the stack is empty, return true, because all opening brackets have been matched with their corresponding closing brackets. Otherwise, return false, because some opening brackets have not been matched with their corresponding closing brackets.

Since we have set a baseline lets try to code it :

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st; // create an empty stack to store opening brackets
        for (char c : s) { // loop through each character in the string
            if (c == '(' || c == '{' || c == '[') { // if the character is an opening bracket
                st.push(c); // push it onto the stack
            } else { // if the character is a closing bracket
                if (st.empty() || // if the stack is empty or 
                    (c == ')' && st.top() != '(') || // the closing bracket doesn't match the corresponding opening bracket at the top of the stack
                    (c == '}' && st.top() != '{') ||
                    (c == ']' && st.top() != '[')) {
                    return false; // the string is not valid, so return false
                }
                st.pop(); // otherwise, pop the opening bracket from the stack
            }
        }
        return st.empty(); // if the stack is empty, all opening brackets have been matched with their corresponding closing brackets,
                           // so the string is valid, otherwise, there are unmatched opening brackets, so return false
    }
};
```

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack=[]
        if len(s)%2==1:
            return False
        stack.append(s[0])
        for i in range(1, len(s)):
            if s[i] == '(' or s[i] == '{' or s[i] == '[':
                stack.append(s[i])
            elif s[i]==')' and len(stack)!=0 and stack[-1]=='(':
                stack.pop()
            elif s[i]=='}' and len(stack)!=0 and stack[-1]=='{':
                stack.pop()
            elif s[i]==']' and len(stack)!=0 and stack[-1]=='[':
                stack.pop()
            else:
                return False
        if len(stack)==0:
            return True
        else:
            return False
```
