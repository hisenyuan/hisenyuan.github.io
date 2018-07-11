---
title: 利用Java构造二叉树 - 前序、中序、后续、层次遍历
keywords: [二叉树的遍历,java二叉树,java遍历二叉树]
date: 2017/11/13 13:04
tags: [二叉树,java,数据结构]
categories: java
---
定义
---
最多有两棵子树的有序树，称为二叉树。二叉树是一种特殊的树。

性质
---
这里规定二叉树的根结点的层次为1。

1. 性质1：则二叉树的第i 层最多有2i-1个结点（在此二叉树的层次从1开始，i≥1）
2. 性质2：深度为k的二叉树最多有2k-1个结点。(k≥1)
3. 性质3：对任何一棵二叉树T, 如果其叶结点个数为n0, 度为2的非叶结点个数为n2, 则有
             n0 = n2 + 1
4. 性质4：具有 n(n>0)个结点的完全二叉树的深度为⎣log2n⎦+1；⎦x⎦表示不超过x的最大整数。
5. 性质5：如果对一棵有n个结点的完全二叉树的结点按层序编号（从第1层到第⎣l og2n⎦ +1层，每层从左到右）,则对任一结点i（1≤i≤n),有：
5.1 (1)如果i=1，则结点i无双亲，是二叉树的根；如果i>1，则其双亲是结点⎣i/2⎦。
5.2 (2) 如果2i<=n, 则结点i的左孩子结点是2i；否则，结点i为叶子结点，无左孩子结点。
5.3 (3)如果2i＋1<=n，则结点i的右孩子是结点2i＋1; 否则，结点i为叶子结点，无右孩子结点。

完整代码
---
<a href="https://github.com/hisenyuan/IDEAPractice/tree/master/src/main/java/com/hisen/interview/tiger20171110/btree" target="_blank">https://github.com/hisenyuan/btree</a>

二叉链表的实现
---
```
package com.hisen.interview.tiger20171110.btree;

/**
 * @author : yhx
 * @date : 2017/11/10 18:42
 * @descriptor : 二叉树的 - 二叉链表实现
 */
public class LinkBTree implements BTree {

  private Object data;
  private BTree lChild;
  private BTree rChild;

  public LinkBTree() {
    this.clearTree();
  }

  public LinkBTree(Object data) {
    this.data = data;
    this.rChild = null;
    this.lChild = null;
  }

  @Override
  public void addLfetTree(BTree lChild) {
    this.lChild = lChild;
  }

  @Override
  public BTree getLfetTree() {
    return lChild;
  }

  @Override
  public void addRightTree(BTree rChild) {
    this.rChild = rChild;
  }

  @Override
  public BTree getRightTree() {
    return rChild;
  }

  @Override
  public void clearTree() {
    this.data = null;
    this.rChild = null;
    this.lChild = null;
  }

  @Override
  public int getDeep() {
    return deep(this);
  }


  @Override
  public Object getRootData() {
    return data;
  }

  @Override
  public boolean hasLeftTree() {
    if (lChild != null) {
      return true;
    }
    return false;
  }

  @Override
  public boolean hasRightTree() {
    if (rChild != null) {
      return true;
    }
    return false;
  }

  @Override
  public boolean isEmptyTree() {
    if ((lChild == null && rChild == null && data == null) || this == null) {
      return true;
    }
    return false;
  }

  @Override
  public boolean isLeaf() {
    if (lChild == null && rChild == null) {
      return true;
    }
    return false;
  }

  @Override
  public void removeLeftTree() {
    lChild = null;
  }

  @Override
  public void removeRightTree() {
    rChild = null;
  }

  @Override
  public BTree getRoot() {
    return this;
  }

  @Override
  public void setRootData() {
    this.data = data;
  }

  @Override
  public int size() {
    return size(this);
  }

  private int size(BTree bTree) {
    if (bTree == null) {
      return 0;
    } else if (bTree.isLeaf()) {
      return 1;
    } else {
      if (bTree.getLfetTree() == null) {
        return size(bTree.getRightTree()) + 1;
      } else if (bTree.getRightTree() == null) {
        return size(bTree.getLfetTree()) + 1;
      } else {
        return size(bTree.getLfetTree()) + size(bTree.getRightTree()) + 1;
      }
    }
  }

  /**
   * 计算二叉树的高度
   */
  private int deep(BTree bTree) {
    if (bTree.isEmptyTree()) {
      return 0;
    } else if (bTree.isLeaf()) {
      return 1;
    } else {
      if (bTree.getLfetTree() == null) {
        return deep(bTree.getRightTree()) + 1;
      } else if (bTree.getRightTree() == null) {
        return deep(bTree.getLfetTree()) + 1;
      } else {
        return Math.max(deep(bTree.getLfetTree()), deep(bTree.getRightTree())) + 1;
      }
    }
  }
}
```

二叉树的各种遍历
---
遍历方式：前序、中序、后序、层次
<!--more-->
```
package com.hisen.interview.tiger20171110.btree;

import java.util.LinkedList;

/**
 * @author : yhx
 * @date : 2017/11/13 12:15
 * @descriptor : 二叉树的遍历
 */
public class OrderBTree implements Visit {

  /**
   * 前序遍历
   *
   * @param root 根节点
   */
  public void preOrder(BTree root) {
    visit(root);
    if (root.getLfetTree() != null) {
      preOrder(root.getLfetTree());
    }
    if (root.getRightTree() != null) {
      preOrder(root.getRightTree());
    }
  }

  /**
   * 中序遍历
   *
   * @param root 根节点
   */
  public void inOrder(BTree root) {
    if (root.getLfetTree() != null) {
      inOrder(root.getLfetTree());
    }
    visit(root);
    if (root.getRightTree() != null) {
      inOrder(root.getRightTree());
    }
  }

  /**
   * 后序遍历
   * @param root 根节点
   */
  public void postOrder(BTree root) {

    if (root.getLfetTree() != null) {
      postOrder(root.getLfetTree());
    }
    if (root.getRightTree() != null) {
      postOrder(root.getRightTree());
    }
    visit(root);
  }

  /**
   * 层次遍历 - 利用队列
   * @param bTree 根节点
   */
  public void levelOrder(BTree bTree){
    if (bTree == null){
      return;
    }
    LinkedList<BTree> queue = new LinkedList<>();
    BTree current = null;
    // 将根节点入队列
    queue.offer(bTree);
    while (!queue.isEmpty()){
      // 队头元素出队列
      current = queue.poll();
      visit(current);
      // 如果左节点不为空，入队列
      if (current.getLfetTree() != null){
        queue.offer(current.getLfetTree());
      }
      // 如果右节点不为空，入队列
      if (current.getRightTree() != null){
        queue.offer(current.getRightTree());
      }
    }
  }
  /**
   * 访问二叉树的节点
   * @param bTree 树的节点
   */
  @Override
  public void visit(BTree bTree) {
    System.out.print(bTree.getRootData() + "\t");
  }
}
```

参考
---
http://blog.csdn.net/luoweifu/article/details/9077521

http://blog.csdn.net/snow_7/article/details/51815787