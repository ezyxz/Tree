

# 数据结构之树

## 多叉树实现

**在计算机科学中，树（英语：tree）是一种抽象数据类型（ADT）或是实现这种抽象数据类型的数据结构，用来模拟具有树状结构性质的数据集合。它是由n（n>0）个有限节点组成一个具有层次关系的集合。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。它具有以下的特点：**

- 每个节点都只有有限个子节点或无子节点；

- 没有父节点的节点称为根节点；

- 每一个非根节点有且只有一个父节点；

- 除了根节点外，每个子节点可以分为多个不相交的子树；

- 树里面没有环路(cycle)

  

```java
/**
 * @author Xinyuan Zuo
 */
public class Tree {
	private Node root;
	private Node currentParent;
	private final Random random;
	
	class Node{
		Object oValue;
		ArrayList<Node> children;
		
		public Node(Object oValue) 
		{
			this.oValue = oValue;
			children = new ArrayList<Node>();
		}
		@Override
		public String toString() {
			return  oValue.toString();
		}
	}
	
	public Tree(Object obj) {
		this.random = new Random();
		this.root = new Node(obj);
		currentParent = root;
	}
	
	public void addCurretLevel(Object obj) {
		currentParent.children.add(new Node(obj));
	}
	
	
	
	public void addNextLevel(Object obj) {
		//default to insert random children node
		int childrenSize = currentParent.children.size();
		currentParent = currentParent.children.get(random.nextInt(childrenSize));
		currentParent.children.add(new Node(obj));
	}
	
	
	public void LevelOrderTraversalPrint() {
		System.out.print(root);
		TraversalPrint(root);
	}
	
	private void TraversalPrint(Node root) {
		if(root != null) {
			LevelPrint(root);
			for (Node child  : root.children) {
				TraversalPrint(child);
			}
		}
	}
	
	private void LevelPrint(Node nodeParent) {
		if(nodeParent.children.size() > 0) {
			for (Node child  : nodeParent.children) {
				System.out.print(child);
			}
		}
	}
	
	public static void main(String[] args) {
		String str = "H";
		Tree tree = new Tree(str);
		tree.addCurretLevel("e");
		tree.addCurretLevel("l");
		tree.addCurretLevel("l");
		tree.addCurretLevel("o");
		tree.addNextLevel(",");
		tree.addCurretLevel("W");
		tree.addCurretLevel("o");
		tree.addNextLevel("r");
		tree.addNextLevel("l");
		tree.addCurretLevel("d");
		tree.LevelOrderTraversalPrint();		
	}

}

```

## 二叉树搜索树

**二叉树（binary tree）是指树中节点的度不大于2的有序树。通常二叉树的两个子节点被称为左节点和右节点。**

**二叉搜索树（binary search tree）是以一颗二叉树为原型加上两条约束：左子树上所有结点的值均小于它的[根结点](https://baike.baidu.com/item/根结点/9795570)的值，则右子树上所有结点的值均大于它的根结点的值。**

**二叉搜索树作为一种经典的数据结构，它既有链表的快速插入与删除操作的特点，又有数组快速查找的优势。**

#### 基于链表实现的二叉搜索树

```java
/**
 * @author Xinyuan Zuo
 */
public class BinarySearchTree {
    
	private Node root;
    // inner class Node 
	class Node{
		Node left;
		Node right;
		Integer iValue;
		
		public Node(int Value) {
			this.iValue = Value;
		}
	}
    
	public BinarySearchTree createBinaryTree(int value) {
		BinarySearchTree binaryTree = new BinarySearchTree();
		binaryTree.root = new Node(value);
		return binaryTree;
	}

    
	// Insert a node
	public void insertNode(int value) {
		insertNode(value, root);
	}
	private void insertNode(int value, Node node) {
		if(node == null) {
			node = new Node(value);
			return;
		}	
		
		while(node != null) {
			if(value < node.iValue) {	
				if(node.left == null) {
					node.left = new Node(value);
					return;
				}else{
					node = node.left;
				}
			}else {
				if(node.right == null) {
					node.right = new Node(value);
					return;
				}else{
					node = node.right;
				}
			}		
		}	
		
	}
	// Insert a node
	
	
	// LevelOrderTraversal 层序遍历
	public void LevelOrderTraversalPrint() {
		System.out.print(root);
		TraversalPrint(root);
	}	
	private void TraversalPrint(Node root) {
		if(root != null) {
			LevelPrint(root);
	    TraversalPrint(root.left);
	    TraversalPrint(root.right);
			
		}
	}
	private void LevelPrint(Node nodeParent) {
		if(nodeParent.left != null) {
			System.out.print(nodeParent.left);	
		}
		if(nodeParent.right != null) {
			System.out.print(nodeParent.right);
		}
	}
	// LevelOrderTraversal

	

	
	public static void main(String[] args) {
        
		BinarySearchTree binaryTree = new BinarySearchTree().createBinaryTree(1);
        
		for (int i = 2; i < 10; i++) {
			binaryTree.insertNode(i);
		}
        
		binaryTree.LevelOrderTraversalPrint();
	}
```



## 堆(Heap)

![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/heap.jpg?raw=true)

堆分为两种：最大堆 和 最小堆

在最大堆中，父节点的值比每一个子节点的值都要大。在最小堆中，父节点的值比每一个子节点的值都要小。这就是所谓的“堆属性”，并且这个属性对堆中的每一个节点都成立。相比二叉搜索树， Heap 并没有要求左子树需要大于或小于右子树，但root节点必须是最小的值。

堆的常用方法：

- 构建优先队列 - 优先输出队列中的最值
- 支持堆排序
- 快速找出一个集合中的最小值（或者最大值）



### 堆的插入

1. 首先找到LastNode, 并将新Node插入下一个节点

   - LastNode 是父节点的右节点，则直接插在LastNode的左节点
   - LastNode 是父节点的左节点，若父节点的右节点为空则插入父节点的右节点，否则插入LastNode的左节点

2. Upheap

   - 循环比较插入节点的父节点，以最大堆为例，如果比父节点大则交换，直到该节点为root节点或小于其父节点

   ![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/insert1.jpg?raw=true)

​                                ![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/insert2.jpg?raw=true)

![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/insert3.jpg?raw=true)

### 堆的删除最小值

1. 将root节点的值和LastNode的值交换，删除LastNode，返回root节点的值
2. DownHeap
   - 循环比较root节点的左右节点，若root节点的值比左右节点的较小的值大，则交换他们的值，直到该节点到LastNode或该节点比它的左右节点都大。





![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/remove1.jpg?raw=true)

![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/remove2.jpg?raw=true)

​                             ![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/remove3.jpg?raw=true)  

![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/remove4.jpg?raw=true)



### 基于数组实现的二叉树（Array Heap Implementation）

![RUNOOB 图标](https://github.com/ezyxz/Tree/blob/master/Image/arrayheap.jpg?raw=true)

- 舍弃数组index 0 位置

- 将root节点放在 index 1 位置

- 对于每个节点其 index 为 i，左节点的index = 2i，右节点为2i+1，父节点index = i/2.

  

#### 堆排序时间复杂度分析（Time complexity）

- **将待排序序列构造成一个大顶堆，此时，整个序列的最大值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行，便能得到一个有序序列了**

- **简单的来说 就是将数组一次插入堆中，再一次移除堆中的root节点。**



#### void Insert(int value) 

- **堆中共有n个节点，最差情况在upheap中与夫节点交换log2 n 次，因为总共heap深度为log2 n；**

#### int removeMin()

- **最差情况在downheap中与夫节点交换log2 n 次；**

**总共有n个数组需要进行insert和removeMin 方法，所以时间复杂度为O(n log n) time**

##### 基于数组实现的Heap

```java
public class Heap2
{
	private static final int ROOT_INDEX    =  1;  // the index in the array at which the storage starts
	private int[] heap; // the array that actually stores the heap
	private int size = 0;  // number of elements currently stored
	
	
	public Heap2(int size) {
		
	this.heap = new int[size + 1];
	
	}
	
	
	public int insert(int value) {

	if ( size >= heap.length - 1) { return -1; }

	
	heap[size + 1] = value;
	int currentIndex = size + 1;

	while(heap[getParentIndex(currentIndex)] > heap[currentIndex]) {
		
		int temp = heap[getParentIndex(currentIndex)];
		heap[currentIndex] = temp;
		heap[getParentIndex(currentIndex)] = heap[currentIndex];
		
		currentIndex = getParentIndex(currentIndex);

	}
	size++;

	return 1;
}
	public int viewMin() {
		return (size > 0) ? heap[ROOT_INDEX] : -1;
	}
	
	public int removeMin() {
		if ( size() <= 0 )  { return -1; }
	//record min value, to be returned at end
	int min = viewMin();
	heap[ROOT_INDEX] = heap[getLastIndex()];
	heap[getLastIndex()] = 0;
	int currentIndex = ROOT_INDEX;
	count++;
	
	while(( getLeftChildIndex(currentIndex) <= size && heap[getLeftChildIndex(currentIndex)] !=0  && heap[getLeftChildIndex(currentIndex)] <  heap[currentIndex]) || 
			(getRightChildIndex(currentIndex) <= size && heap[getRightChildIndex(currentIndex)] <  heap[currentIndex] && heap[getRightChildIndex(currentIndex)] !=0)) 
	{
		int temp;
		int temp_index;
		// When it has only left node
		if(heap[getRightChildIndex(currentIndex)] ==0) {
			temp = heap[getLeftChildIndex(currentIndex)];
			temp_index = getLeftChildIndex(currentIndex);
			heap[getLeftChildIndex(currentIndex)] = heap[currentIndex];
			heap[currentIndex] = temp;
			count+=3;
			continue;
		}
		// When it has only right node
		if(heap[getLeftChildIndex(currentIndex)] ==0) {
			temp = heap[getRightChildIndex(currentIndex)];
			temp_index = getRightChildIndex(currentIndex);
			heap[getRightChildIndex(currentIndex)] = heap[currentIndex];
			heap[currentIndex] = temp;	
			count+=3;
			continue;
		}
		
		// Swap the smallest one
		if( heap[getLeftChildIndex(currentIndex)] < heap[getRightChildIndex(currentIndex)]) {
			temp = heap[getLeftChildIndex(currentIndex)];
			temp_index = getLeftChildIndex(currentIndex);
			heap[getLeftChildIndex(currentIndex)] = heap[currentIndex];
			heap[currentIndex] = temp;	
			count+=3;
			
		}else {
			temp = heap[getRightChildIndex(currentIndex)];
			temp_index = getRightChildIndex(currentIndex);
			heap[getRightChildIndex(currentIndex)] = heap[currentIndex];
			heap[currentIndex] = temp;		
			count+=3;
			
		}
		currentIndex = temp_index;
		count++;
	}
	
	size--;

	return min;
	}
	private int getParentIndex(int currentIndex) {return currentIndex >> 1;}
	
	private int getLeftChildIndex(int currentIndex) {return currentIndex << 1;}

	private int getRightChildIndex(int currentIndex) {return (currentIndex << 1) + 1;}


}

```

### 堆排序

```java
public static void heap2Sort(int[] array)
	{
		Heap2 pq = new Heap2(array.length);
		for(int key : array)
		{
			pq.insert(key);
		}
	
		for(int i = 0; i < array.length; i++)
		{
			array[i] = pq.removeMin();
		}
    
	}
```

