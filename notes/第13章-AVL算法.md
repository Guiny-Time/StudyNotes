---
attachments: [Clipboard_2021-03-24-23-34-31.png, Clipboard_2021-03-24-23-37-49.png, Clipboard_2021-03-25-10-18-11.png, Clipboard_2021-03-25-10-19-02.png, Clipboard_2021-03-25-10-20-22.png, Clipboard_2021-03-25-10-20-31.png, Clipboard_2021-03-25-10-20-33.png, Clipboard_2021-03-25-10-46-43.png, Clipboard_2021-03-25-10-49-06.png, Clipboard_2021-03-25-10-50-48.png, Clipboard_2021-03-25-10-51-05.png, Clipboard_2021-07-06-16-53-48.png, Clipboard_2021-07-06-17-00-10.png, Clipboard_2021-07-06-17-14-53.png]
tags: [CS211]
title: 第13章-AVL算法
created: '2021-03-24T14:24:19.268Z'
modified: '2021-07-06T09:16:57.992Z'
---

# 第13章-AVL算法

## <font color="red">定义</font>
动机：因为有的时候二叉树简直退化成了链表，所以为了得到相对平衡的二叉树，发明了诸如AVL/RBT/Splay这样的平衡二叉树算法
节点创建：在创建的时候带上高度(height)这一新的参数，并写一个与高度有关的方法
```JAVA
// AVL tree implementation in Java 
// Create node 
class Node { 
	int item, height; 
	Node left, right; 
	Node(int d) { 
		item = d;       // 节点的值
		height = 1;     // 节点高度，不是BF
	} 

  // 获取节点高度
  int height(Node N) { 
	  if (N == null) return 0; 
	  return N.height; 
  } 

  // 获取最大的数
  int max(int a, int b) { 
	  return (a > b) ? a : b; 
  } 

  // 获取平衡系数
  int getBalanceFactor(Node N) { 
	  if (N == null) return 0; 
	  return height(N.left) - height(N.right); 
  }
}
```
AVL算法的目的是通过旋转让树每个节点的平衡系数BF的绝对值≤1。这样直接的好处是，**AVL树的插入、搜索和删除的时间复杂度都是O(log~2~N)**
> 平衡系数(Balance Factor)
BF是衡量一个节点的平衡情况的系数。一个节点的BF值 = 该节点左子树的高度 - 该节点右子树的高度
当树中所有节点的BF的绝对值都≤1时，我们认为这棵树是平衡的

## <font color="red">AVL旋转</font>
### 左旋
将一个节点的父节点变为自己的左节点
如果该节点本身以及有两个节点：
- 如果y有一个左子树，将x赋值为y的左子树的父节点
- 如果x的父节点为空，则让y作为树的根
- 否则如果x是p的左子节点，让y是p的左子节点
- 否则将y赋值为p的右子节点
- 让y作为x的父节点
![](@attachment/Clipboard_2021-03-24-23-37-49.png)

#### 图示
![](@attachment/Clipboard_2021-03-24-23-34-31.png)

#### 伪代码
```JAVA
Node leftRotate(Node x) { 
	Node y = x.right; 
	Node T2 = y.left; 
	y.left = x; 
	x.right = T2; 
	x.height = max(height(x.left), height(x.right)) + 1; 
	y.height = max(height(y.left), height(y.right)) + 1; 
	return y; 
} 
```

### 右旋
和左旋类似，但是不同的是把操作放到了右边
- 如果x有一个右子树，将y赋值为x右子树的父节点。
- 如果y的父节点为空，则让x作为树的根。
- 否则如果y是其父p的右子节点，那么x就是p的右子节点。
- 否则，将x赋值为p的左子节点。
- 让x作为y的父节点。
![](@attachment/Clipboard_2021-03-25-10-18-11.png)

#### 图示
![](@attachment/Clipboard_2021-03-25-10-19-02.png)

#### 伪代码
```JAVA
Node rightRotate(Node y) { 
	Node x = y.left; 
	Node T2 = x.right; 
	x.right = y; 
	y.left = T2; 
	y.height = max(height(y.left), height(y.right)) + 1; 
	x.height = max(height(x.left), height(x.right)) + 1; 
	return x; 
}
```

### 左右旋
顾名思义，先左旋再右旋的一种方法。有时仅通过左旋或者右旋并没有办法达成目标，比如有拐点的二叉树，此时需要通过左右旋或者右左旋
![](@attachment/Clipboard_2021-07-06-17-00-10.png)

### 右左旋
左右旋的反过程，先右旋再左旋的一种方法
![](@attachment/Clipboard_2021-03-25-10-46-43.png)
<br>

## <font color="red">AVL新增</font>
### 步骤
- 正常插入新节点
- 更新BF，判断是否为平衡二叉树
- 如果不是的话那么开始旋转吧！！！
### 伪代码
```JAVA
Node insertNode(Node node, int item) { 
  // 正常的插入新节点
	if (node == null) return (new Node(item)); 
	if (item < node.item) node.left = insertNode(node.left, item); 
	else if (item > node.item) node.right = insertNode(node.right, item); 
	else return node; 
  //更新节点的BF
	node.height = 1 + max(height(node.left), height(node.right)); 
	int balanceFactor = getBalanceFactor(node); 
  // 旋转开始
	if (balanceFactor > 1) {                    // 当节点的BF大于1，右旋
		if (item < node.left.item) { 
      return rightRotate(node); 
    } 
		else if (item > node.left.item) { 
			node.left = leftRotate(node.left); 
			return rightRotate(node); 
    }
  } 
	if (balanceFactor < -1) {                  // 当节点的BF小于-1，左旋
			if (item > node.right.item) { 
        return leftRotate(node); 
      } 
			else if (item < node.right.item) { 
				node.right = rightRotate(node.right); 
				return leftRotate(node); 
      } 
	} 
	return node; 
}
```
<br>

## <font color="red">AVL删除</font>
### 步骤
- 正常删除
- 更新BF，判断是否为平衡二叉树
- 如果不是那么开始旋转吧！！！
### 伪代码
```JAVA
// Delete a node 
Node deleteNode(Node root, int item) { 
// Find the node to be deleted and remove it 
	if (root == null) return root; 
	if (item < root.item) root.left = deleteNode(root.left, item); 
	else if (item > root.item) root.right = deleteNode(root.right, item); 
	else { 
		if ((root.left == null) || (root.right == null)) { 
			Node temp = null; 
			if (temp == root.left) temp = root.right; 
			else temp = root.left; 
			if (temp == null) { temp = root; root = null; } 
			else root = temp; 
		} 
		else { 
			Node temp = nodeWithMimumValue(root.right); 
			root.item = temp.item; 
			root.right = deleteNode(root.right, temp.item); 
		} 
	} 
	if (root == null) return root; 
  // Update the balance factor of each node and balance the tree 
  root.height = max(height(root.left), height(root.right)) + 1; 
  int balanceFactor = getBalanceFactor(root); 
  if (balanceFactor > 1) {
    if (getBalanceFactor(root.left) >= 0) { 
      return rightRotate(root); 
      } else {
        root.left = leftRotate(root.left); 
        return rightRotate(root); 
        } 
      } if (balanceFactor < -1) { 
        if (getBalanceFactor(root.right) <= 0) { 
          return leftRotate(root); 
        } else { 
          root.right = rightRotate(root.right); 
          return leftRotate(root); 
          } 
        } 
        return root; 
      }
      root.height = max(height(root.left), height(root.right)) + 1; 
	  int balanceFactor = getBalanceFactor(root); 
	  if (balanceFactor > 1) { 
		  if (getBalanceFactor(root.left) >= 0) { return rightRotate(root); } 
		  else { 
			  root.left = leftRotate(root.left); 
			  return rightRotate(root); 
		  } 
	  }
 	  if (balanceFactor < -1) { 
		  if (getBalanceFactor(root.right) <= 0) { return leftRotate(root); } 	
		  else { 
			  root.right = rightRotate(root.right); 
			  return leftRotate(root); 
		  } 
	  } 
	  return root; 
  }
```


## AVL查找
AVL过的树也是BST，所以不用多说了
```JAVA
Node nodeWithMimumValue(Node node) {
	Node current = node; 
	while (current.left != null) 
		current = current.left; 
	return current; 
}
```

