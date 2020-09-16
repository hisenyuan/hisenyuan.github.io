---
title: ValidSudoku - leetCode - 算法
keywords: [Valid Sudoku,leetCode,算法,Sudoku,数独,回溯]
date: 2019/11/03 11:34
tags: [leetCode]
categories: java
---
## 一、题目
[Valid Sudoku链接](https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/769/)

题目要求：
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
- Each row must contain the digits 1-9 without repetition.
- Each column must contain the digits 1-9 without repetition.
- Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

每一行都不能重复1-9
每一列都不能重复1-9
每个3*3的小格子(9*9分为9个3*3)不能重复

## 二、解题分析
- 每一行每一列的数据，我们可以通过遍历二维数组解决；
- 每个3*3的怎么解决呢？还是遍历二维数组，循环大(9*9)的二维数组时，符合条件再循环小(3*3)数组;
- 怎么判断没有重复？方法很多，这里通过数组下标计数，数组初始值为0，根据下标+1，+1之后如果发现>1那么返回错误;

开始解题的时候可以一个大循环解决一个小问题，后续再把循环合并即可;
开始的时候我是写了三个双重for循环：
1. 解决行重复判断问题;
2. 解决列重复判断问题;
3. 解决子矩阵重复判断问题;

当发现每一步都可行之后，尝试着合并，以减少时间复杂度;

## 三、代码样例
```java
public static boolean isValidSudoku(char[][] board) {
    // 处理 9 * 9
    for (int i = 0; i < board.length; i++) {
        int[] rowFlag = new int[10];
        int[] columnFlag = new int[10];
        for (int j = 0; j < board.length; j++) {
            char row = board[i][j];
            char column = board[j][i];
            if (check(rowFlag, row)) {
                return false;
            }
            if (check(columnFlag, column)) {
                return false;
            }
            // 处理 3 * 3
            if (i % 3 == 0 && j % 3 == 0) {
                int[] smallFlag = new int[10];
                for (int k = i; k < i + 3; k++) {
                    for (int l = j; l < j + 3; l++) {
                        char now = board[k][l];
                        if (check(smallFlag, now)) {
                            return false;
                        }
                    }
                }
            }
        }
    }
    return true;
}
```

## 四、代码详情
可运行demo
<!--more-->
github地址：[ValidSudoku.java](https://github.com/hisenyuan/IDEAPractice/blob/master/src/main/java/com/hisen/algorithms/leetcode/ValidSudoku.java)
