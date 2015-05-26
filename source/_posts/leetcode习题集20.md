title: "leetcode习题集 20 Valid Parentheses"
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.


题意：各一个字符串包括各种括号，判断括号能不能匹配成功

思路：
很容易想到用栈处理

注意：
string里面可能包含不是括号的字符

```
class Solution {
public:
//使用栈来匹配括号
//如果是左括号，压栈，如果是右括号，比较
//注意：不是括号的元素，空s
       bool isValid(string s) {
        stack<char> stack;
        for(int i=0;i<s.size();i++)
        {
            if(isleft(s[i]))
                stack.push(s[i]);
            else if(isright(s[i]))
            {
                if(!stack.empty() && isMatch(stack.top(),s[i]))
                    stack.pop();
                else
                    return false;
            }
            else
                return false;
        }
        if(stack.empty())
            return true;
        else 
            return false;
    }
    
    bool isleft(char a)
    {
        if(a=='(' || a=='[' || a=='{')
            return true;
        else 
            return false;
    }
    
    bool isright(char a)
    {
        if(a==')' || a==']' || a=='}')
            return true;
        else 
            return false;
    }
    bool isMatch(char ch1,char ch2)
    {
        if(ch1=='(' && ch2==')') 
            return true;
        else if(ch1=='[' && ch2==']')
            return true;
        else if(ch1=='{' && ch2=='}')
            return true;
        return false;
    }
};
```