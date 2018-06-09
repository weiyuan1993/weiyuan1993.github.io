---
layout: post
title:  "[Leetcode]257.Binary tree paths"
date:   2017-09-23 19:28:26 +0800
categories: Leetcode
tags:
-  Data Structure
-  Leetcode
-  Binary Tree
---

退伍一個多月了，也開始進入職場上班，自己覺得進入了一個不錯的地方，待遇及工作環境都很滿意，尤其可以接受到很多挑戰，像是每週刷兩題leetcode出來大家討論，
對於我這種資料結構、演算法都極差的碼農來說，真的是一大挑戰，因此決定於此紀錄寫題心得。

## 257.Binary tree paths
 [題目連結](https://leetcode.com/problems/binary-tree-paths/description/)  
 難易度：Easy  

Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:
``` javascript
   1
 /   \
2     3
 \
  5
All root-to-leaf paths are:

["1->2->5", "1->3"]
```
敘述很清楚，就是要找到給定的二元樹中所有從根到葉的路徑

## 思路
先看一下二元樹結構， 使用console.log可以發現傳入的node結構，
``` javascript
 var input =  {
    val: 1,
    right:  { val: 3, right: null, left: null },
    left: 
      {
       val: 2,
       right:  { val: 5, right: null, left: null },
       left: null } }
 
```
開始構思一個函式，帶入三個參數，當前node、路徑字串、結果。  

首先先傳入最初的root node開始遍歷，如果左leaf有node，則使用遞回再次呼叫此函式，此時傳入當前node為node.left，路徑字串為當前node的值，並加上“->"符號，右left有node也一樣遞回此函式。  

當當前node的左右leaf為空時，代表已經抵達root node的葉，為一完整的路徑，此時就可以把此路徑字串push進結果，但要記得加上當前node的值。

## 解題

``` javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {string[]}
 */


var binaryTreePaths = function(root) {
    if(!root) return [];
    var result = [];

    searchBinaryTree(root, "" ,result);

    return result;
};
function searchBinaryTree(node,pathString, result){
    // if left and right of node is empty
    if(node.left == null && node.right == null){
        result.push(pathString+node.val);
    }else{
        if(node.left !== null){
            searchBinaryTree(node.left,pathString + node.val + "->", result);
        }
        if(node.right !== null){
            searchBinaryTree(node.right,pathString+ node.val + "->", result);
        }
    }

}
```
