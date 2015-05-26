title: "leetcode习题集 36 Valid Sudoku "
date: 2015-04-06 15:31:44
category: [算法]
tags: [leetcode习题集]
---

Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

Note:
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

题意：判断一个9*9的数独表是不是可解的，当且仅当每一行每一列，9个3*3小块都可解的时候，数独表是可解的

思路：

使用
```
bool row[9][9];
bool col[9][9];
bool block[9][9];
```
row[i][j]表示第i行数字为j的数字有没有出现
col[i][j]表示第i列数字为j的数字有没有出现
block[i][j]表示第i块数字为j的数字有没有出现
块序号为0-8，和`i`,`j`的换算关系为`i-i%3+j/3`

然后遍历数组即可

```
class Solution {
public:
//一次遍历，记录行列块中对应数字是否已经存在
//row[i][j]意思是记录i行，数字为j的数字有没有存在
//块序号0-8，计算结果为i-i%3+j/3
    bool isValidSudoku(vector<vector<char> > &board) {
        bool row[9][9];
        bool col[9][9];
        bool block[9][9];
        for(int i=0;i<9;i++)
            for(int j=0;j<9;j++)
            {
                row[i][j]=false;
                col[i][j]=false;
                block[i][j]=false;
            }
            
        for(int i=0;i<9;i++)
        {
            for(int j=0;j<9;j++)
            {
                if(board[i][j]!='.')
                {
                    if(row[i][board[i][j]-'1']||col[j][board[i][j]-'1']||block[i-i%3+j/3][board[i][j]-'1'])
                        return false;
                    else
                    {
                        row[i][board[i][j]-'1']=true;
                        col[j][board[i][j]-'1']=true;
                        block[i-i%3+j/3][board[i][j]-'1']=true;
                    } 
                }
            }
        }
        return true;
    }
};
```