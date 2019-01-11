[toc]
# Java数据结构

---

## 一、java排序
### 1、直接插入排序
#### 1）排序思想：
再要进行排序的数组中，假设前 n-1 个数据是排好序了的，现在需要将第 n 个元素

#### 2）实现原理：
在排序过程中，先将第一个与第二个数进行排序，变成有序序列以后，再将第三个数进行有序插入，一次向下进行插入排序。这样的排序叫做---直接插入排序。

#### 3）流程图：
![image](https://note.youdao.com/yws/api/personal/file/DFC6045FD30F4BFDBA4D32FDAF4507C4?method=download&shareKey=dfbd25a00f11d714a8337cd4cd84a789)

#### 4）代码实现:
先创建一个工聚类，用于保存插入排序的算法的方法：
```
public class MyInsertSort {

    public long[] InsertSort(long[] arr,int len){
        long temp = 0;
        for(int i = 1;i < len;i++) {
            temp = arr[i];
            int j;
            for (j=i;j>0 && temp < arr[j-1];j--){
                arr[j] = arr[j-1];
            }
            arr[j] = temp;
        }
        return arr;
    }
}
```

再创建一个测试这个方法的测试用例：
```
public class TestInsertSort {

    public static void main(String[] args) {
        long arr[] = {-32,45,12,15,41};
        MyInsertSort insertSort = new MyInsertSort();
        long[] sort = insertSort.InsertSort(arr, arr.length);
        for (long i:sort) {
            System.out.println(i);
        }
    }
}
```


### 2、冒泡排序
#### 1）排序思想：
让前面的数与后面的数相比较，进行多轮比较，如果根据需求来进行位置的调换操作。

#### 2）实现流程：
![image](https://note.youdao.com/yws/api/personal/file/683DC2BF6F1E41D180D780983BD5B8FD?method=download&shareKey=aa0b40df61c5d4d326b72dd452a41782)

#### 3）基本概念：
一共会经过数组长度n的（n-1）次；每一轮比较就会确定一个数的位置；每一轮比较的次数再逐渐的减少（第一次是比较n-1次，第二次就是比较到n-2次，一直到最后一次的比较即可）。

#### 4）代码实现一：
```
/**
 * 冒泡排序
 * @param args
 */
public static void main(String[] args) {
	int a[] = {12,95,56,82,73,91,106,22,19,64};
	int i = 0; 
	int j = 0;
	int temp = 0;
	
	//第一个for循环，是专门针对于for循环是需要进行多少次循环
	for ( i=0;i<a.length-1;i++ ) {
		//第二个for循环，是为了让这个冒泡排序进行多少个数的比较
		for( j=0;j<a.length-i-1;j++ ) {
			if( a[j]>a[j+1] ) {
				temp =a[j];
				a[j] = a[j+1];
				a[j+1] = temp;
			}
		}
		
	}
	
	for (int k=0;k<a.length;k++) {
		System.out.println(a[k]);
	}
}
```

#### 5）代码实现二：
先创建一个专门用于处理冒泡排序的工具类，
```
public class MyBubble {

    public long[] bubbleSort(long arr[], int len) {
        int i,j;
        for (i=1;i<=len-1;i++){
            for (j=0;j<=len-i-1;j++){
                if (arr[j] > arr[j+1]){
                    long temp;
                    temp = arr[j+1];
                    arr[j+1] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        return arr;
    }
}
```

调用这个冒泡排序的工具类，来实现冒泡排序的操作。
```
public class TestBuuble {

    public static void main(String[] args) {

        long arr[] = {32,45,12,15,41};
        MyBubble bubble = new MyBubble();
        long[] sort = bubble.bubbleSort(arr, arr.length);
        for (long i:sort) {
            System.out.println(i);
        }
    }
}
```

### 3、直接选择排序
#### 1）排序思想：
将数据进行多轮比较。  
第一轮：默认第一个数为最小数，然后分别于后面的数作比较，比较一轮以后，找出一个最小的数，与这个第一个数进行位置的交换；  
第二轮：默认第二个数为最小数，再次与后面的数据进行比较，找出最小的数据，进行位置的交换。  
这样的执行 n-1 次大的循环就能够实现直接选择排序。

#### 2）代码实现：
先创建一个直接插入排序的工具类：
```
public class MySelectSort {

    public long[] SelectSort(long[] arr,int len){
        int i,j;
        int index;
        long temp;
        for (i=0;i<len-1;i++){
            index = i;
            for (j=i;j<len;j++){
                if (arr[index] > arr[j]){
                    index = j;
                }
            }
            if (index != i){
                temp = arr[i];
                arr[i] = arr[index];
                arr[index] = temp;
            }
        }
        return arr;
    }
}
```

调用这个工具类的方法来对数据进行排序；
```
public class TestSelectSort {

    public static void main(String[] args) {
        long arr[] = {32,45,12,15,41};
        MySelectSort selectSort = new MySelectSort();
        long[] sort = selectSort.SelectSort(arr, arr.length);
        for (long i:sort) {
            System.out.println(i);
        }
    }
}
```

### 4、希尔排序
#### 1）基本介绍：
希尔排序(Shell's Sort)是插入排序的一种，又称“缩小增量排序”，是直接插入排序算法的一种更加高效的改进版本。希尔排序是非稳定的排序算法。

#### 2）算法思想：
该方法实质上是一种分组插入方法。

先取一个小于n的整数d1作为第一个增量，把文件的全部记录分组。所有距离为d1的倍数的记录放在同一个组中。先在各组内进行直接插入排序；然后，取第二个增量 d2 < d1 重复上述的分组和排序，直至最后，所有记录放在同一组中进行直接插入排序为止。

#### 3）增量的设置公式：
```
n = n*3 + 1;
```

#### 4）代码实现：
先创建一个希尔排序的工具类：
```
public class MyShellSort {

    public long[] ShellSort(long[] arr, int len) {
        //先计算希尔排序的最大间隔
        int h = 1;
        while(h < len/3){
            h = h*3 +1;
        }
        //进行希尔排序
        while( h >0 ) {
            long temp = 0;
            for(int i = h;i < len;i++) {
                temp = arr[i];
                int j = i;
                while (j > h-1 && arr[j-h]>=temp){
                    arr[j] = arr[j-h];
                    j -= h;
                }
                arr[j] = temp;
            }
            h = (h-1)/3;
        }
        return arr;
    }
}
```

在创建一个测试希尔排序的测试类：
```
public class TestMyShellSort {
    public static void main(String[] args) {
        long[] arr = {54,33,-1,66,78,2,34,1,0,55,11};
        MyShellSort shellSort = new MyShellSort();
        long[] sort = shellSort.ShellSort(arr, arr.length);
        for (long i:sort) {
            System.out.println(i);
        }
    }
}
```

### 5、快速排序
#### 1）基本介绍：
快速排序是通过将一个数组分为两个子数组，然后通过递归调用自身的每一个子数组进行快速排序实现。

#### 2）划分对象：
设置关键字，将比关键字小的数据放在一组，比关键字大的放在另外一组中。通常是设置第一个数据为关键字。

#### 3）代码实现：




## 二、java查找
### 1、二分法查找
#### 1）实现原理：
二分法查找是在一个**有序的数组**中进行查询。

#### 2）实现流程：
![image](https://note.youdao.com/yws/api/personal/file/062F1733F6194BA9B662FBC2E51F25B6?method=download&shareKey=0655afbc697aaf8e53ada4c0c97716f6)

#### 4）代码实现：
```
public static void main(String[] args) {
		int[] a = new int[] { 12, 23, 34, 45, 56, 67, 77, 89, 90 };

		int aa = rank(3,a);
		if(aa == -1) {
			System.out.println("没查找到结果");
		} else {
			System.out.println("结果查找到了");
		}
	}
	
	/**
	 * 二分法查找
	 * @param key
	 * @param nums
	 * @return
	 */
	public static int rank(int key,int nums[])
	{
		int low=0;
		int high=nums.length-1;
		int notFind=-1;
		while(low<=high)
		{
			int mid=low+(high-low)/2;

			if(nums[mid]>key)
			{
				high=mid-1;
			}else if(nums[mid]<key)
			{
				low=mid+1;
			}
			else
			{
				return mid;
			}
		}
		return notFind;
	}
	
```

## 三、java数组
### 1、一维数组
#### 1）基本介绍：
java数组主要是用于存储相同数据类型的一种实现方式。

#### 2）代码实现：
先创建一个专门用于编写数据增删改查的类MyArray；
```
public class MyArray {

    private long[] arr;
    private int elements;

    public MyArray(){
        arr = new long[50];
    }

    public MyArray(int maxsioze){
        arr = new long[maxsioze];
    }

    /**
     * 正常添加数据
      * @param val
     */
    public void insert(int val){
        arr[elements] = val;
        elements++;
    }

    public void show(){
        for (int i=0;i<elements;i++){
            System.out.println("arr["+i+"]="+arr[i]);
        }
    }

    /**
     * 顺序插入数据
     * @param val
     * @return
     */
    public long insertBySort(long val) {
        int i;
        for (i=0;i<elements;i++){
            if (arr[i]>=val){
                break;
            }
        }

        int j;
        for (j=elements ; j>i ; j--){
            arr[j] = arr[j-1];
        }
        arr[i] = val;
        elements++;
        return arr[i];
    }

    //根据值查找数据
    public long findByVal(long val){
        int i;
        for (i=0;i<elements;i++){
            if (val == arr[i]){
                break;
            }
        }

        if (i>=elements){
            //没查找到
            return -1;
        }
        //查找到了
        return arr[i];
    }

    //根据下标查找数据
    public long findByIndex(int index){
        if (index >= elements || index < 0){
            return -1;
        }
        return arr[index];
    }

    //根据下标查找数据
    //返回-1，则表示下标错误，返回为1，则表示删除成功
    public long deleteByIndex(int index){
        int i;
        if (index >= elements || index < 0){
            return -1;
        }
        for (i = index;i<elements;i++){
            arr[i] = arr[i+1];
        }
        elements--;
        return 1;
    }

    //更新数据
    public long updateByIndex(int newVal,int index){
        //下标错误
        if (index >= elements || index < 0){
            return -1;
        }
        arr[index] = newVal;
        System.out.println("更新成功");
        return arr[index];
    }

    //根据下标插入数据
    public long insertByIndex(int val,int index){
        int i;
        //下标错误
        if (index >= elements || index < 0){
            return -1;
        }
        for (i=elements ; i>index ; i--){
            arr[i] = arr[i-1];
        }
        arr[index] = val;
        System.out.println("插入数据成功");
        elements++;
        return arr[index];
    }


}
```

在创建一个专门用于测试这些增删改查方法的测试类；
```
public class TestArray {

    public static void main(String[] args) {
        //创建数组
        MyArray array = new MyArray();
        //顺序的插入数据
        array.insertBySort(12);
        array.insertBySort(34);
        array.insertBySort(25);
        //显示数组
        array.show();

        //查找数据
        long l = array.findByVal(12);
        if (l == -1){
            System.out.println("未查找到数据");
        } else {
            System.out.println("查找到了数据，数据为："+l);
        }

        long l1 = array.deleteByIndex(1);
        if (l == -1){
            System.out.println("未查找到数据");
        } else {
            System.out.println("删除数据成功");
        }
        array.show();

        array.updateByIndex(55,0);
        array.show();

        //根据下标插入数据
        array.insertByIndex(67,1);
        array.show();

    }
}
```

## 四、java栈与队列
### 1、java栈
#### 1）基本介绍：
栈是一种只允许在一端进行插入或者删除的线性表。

- 栈的操作端通常是称为栈顶，另一端称为栈底。
- 栈的插入操作称为进栈（压栈|push），栈的删除操作称为出栈（弹栈|pop）。

#### 2）栈的特点：
- 栈只能够先进后出的操作。
- 按照顺序存储的栈称为顺序栈。
- 链式存储的栈称为链式栈。

#### 3）代码实现：
创建一个类，里面包含栈的各种操作方法：
```
public class MyStack {

    /**
     * 栈的底层是一个数组
     */
    private long[] arr;

    //最大数组容量
    private int maxSize;

    //栈顶
    private int top;

    /**
     *  默认构造方法，创建一个大小为10的数组
     */
    public MyStack(){
        arr = new long[10];
        top = -1;
    }


    /**
     * 带参数的构造方法，创建一个大小自己设定的数组
     * @param maxSize
     */
    public MyStack(int maxSize){
        this.maxSize = maxSize;
        arr = new long[maxSize];
        top = -1;
    }

    /**
     * 添加数据到栈中
     */
    public void push(int val){
        arr[++top] = val;
    }

    /**
     * 从栈中删除数据
     */
    public long pop(){
        return arr[top--];
    }

    /**
     * 查看当前数据
     */
    public long peek(){
        return arr[top];
    }

    /**
     * 查看栈中所有数据
     */
    public void show(){
        for (long i:arr) {
            System.out.println(i);
        }
    }

    /**
     * 判断是否为空
     */
    public boolean isEmpty(){
        return top == -1;
    }

    /**
     * 判断栈是否已经满了
     */
    public boolean isFull(){
        return top == arr.length-1;
    }

    /**
     * 查案还剩多少空间
     * @return
     */
    public int more(){
        return maxSize-1-top;
    }
}
```

创建一个使用栈的测试用例：
```
public static void main(String[] args) {
        MyStack stack = new MyStack(5);
        stack.push(23);
        stack.push(-90);
        stack.push(77);
        stack.push(1);

        //判断栈是否为空
        boolean full = stack.isFull();
        if (full == true) {
            System.out.println("栈已满");
        } else {
            System.out.println("栈未满");
        }

        //出栈
        long pop = stack.pop();
        System.out.println("出栈的数据为："+pop);

        int more = stack.more();
        System.out.println("栈还剩空间数为："+more);

        //查看所有数据
        System.out.println("遍历栈，数据为：");
        stack.show();

        //查看栈顶数据
        long peek = stack.peek();
        System.out.println("栈顶数据为："+peek);
    }
```

### 2、java非环形数组队列
#### 1）基本介绍：
一个队列就是一个先入先出（FIFO）的数据结构。是一种非环形的数组队列。

##### 注意事项：
非环形数组队列，当执行了多次的添加删除以后，会发现在队列未满的情况下无法继续加入数据。  
能够使用环形数组队列来解决这个问题。

#### 2）代码实现：
先创建一个非环形数组队列的类：
```
public class MyQueue {

    //队列底层存放数组
    private long[] arr;
    //有效数据大小
    private int elements;
    //队头
    private int front;
    //队尾
    private int end;

    /**
     * 默认构造方法
     */
    public MyQueue(){
        arr = new long[10];
        elements = 0;
        front = 0;
        end = -1;
    }

    /**
     * 自定义大小的构造方法
     */
    public MyQueue(int maxSize){
        arr = new long[maxSize];
        elements = 0;
        front = 0;
        end = -1;
    }

    /**
     * 添加数据,加入到队尾中
     */
    public void insert(long val){
        arr[++end] = val;
        elements++;
    }

    /**
     * 从队头删除数据
     */
    public long remove(){
        elements--;
        return arr[front++];
    }

    /**
     * 查看队头数据
     */
    public long peek(){
        return arr[front];
    }

    /**
     * 遍历队列所有数据
     */
    public void show(){
        for (int i = 0;i<elements;i++){
            System.out.println(arr[i]);
        }
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return elements == 0;
    }

    //判断队列是否已满
    public boolean isFull(){
        return elements == arr.length;
    }
}
```

再创建一个非环形数组队列的测试用例：
```
public class TestQueue {

    public static void main(String[] args) {
        MyQueue queue = new MyQueue(5);
        queue.insert(23);
        queue.insert(45);
        queue.insert(12);
        queue.insert(34);
        queue.show();

        //判断队列是否已满
        boolean full = queue.isFull();
        if (full == true ){
            System.out.println("队列已满");
        } else {
            System.out.println("队列未满");
        }

        long remove = queue.remove();
        System.out.println("remove的数据为："+remove);

        queue.show();

        System.out.println("对头数据为："+queue.peek());
    }
}
```

### 3、java环形数组队列
#### 1）基本介绍：
将数组形成一个环形的队列，实现数据的循环连接。

#### 2）代码实现：
先创建一个环形数组队列来实现队列的操作：
```
package com.data.struceure.queue;

/**
 * @author ${张世林}
 * @date 2018/10/10
 * 作用：
 */
public class MyCircularQueue {

    //队列底层存放数组
    private long[] arr;
    //有效数据大小
    private int elements;
    //队头
    private int front;
    //队尾
    private int end;

    /**
     * 默认构造方法
     */
    public MyCircularQueue(){
        arr = new long[10];
        elements = 0;
        front = 0;
        end = -1;
    }

    /**
     * 自定义大小的构造方法
     */
    public MyCircularQueue(int maxSize){
        arr = new long[maxSize];
        elements = 0;
        front = 0;
        end = -1;
    }

    /**
     * 添加数据,加入到队尾中
     */
    public void insert(long val){
        if (!isFull()) {
            if (arr.length - 1 == end) {
                end = -1;
            }
            arr[++end] = val;
            elements++;
        } else
        {
            System.out.println("队列已满");
        }
    }

    /**
     * 从队头删除数据
     */
    public long remove(){
        if(!isEmpty()) {
            long value = arr[front++];
            if (front == arr.length) {
                front = 0;
            }
            elements--;
            return value;
        } else
        {
            System.out.println("队列为空");
            return -1;
        }
    }

    /**
     * 查看队头数据
     */
    public long peek(){
        return arr[front];
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return elements == 0;
    }

    //判断队列是否已满
    public boolean isFull(){
        return elements == arr.length;
    }
}

```

编写一个测试用例：
```
public class TestCricularQueue {

    public static void main(String[] args) {
        MyCircularQueue queue = new MyCircularQueue(5);
        queue.insert(23);
        queue.insert(45);
        queue.insert(12);
        queue.insert(34);
        queue.insert(456);
        queue.insert(234);

        while (!queue.isEmpty()) {
            System.out.println(queue.remove());
        }

    }
}

```

## 五、java链表
### 1、基本介绍：
链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针(Pointer)。

使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大。

### 2、java单链表
#### 1）基本介绍：
单链表就是一个节点分为两部分，一部分为数据，一部分指向的是下一个节点。

#### 2）代码实现：
先创建一个节点的类：
```
public class Node {

    public long data;

    public Node next;

    public Node(){

    }

    public  Node(long data){
        this.data = data;
    }

}

```

在创建一个链表的类：
```
public class LinkList {

    /**
     * 创建一个头结点
     */
    private Node head;

    private int len;

    /**
     * 链表构造器，里面封装了head节点
     * @param head
     */
    public LinkList(Node head){
        this.head =head;
    }

    /**
     * 添加数据
     */
    public void addNode(Node node){
        Node temp = head;
        while (temp.next != null){
            temp = temp.next;
        }
        temp.next = node;
        len++;
    }

    /**
     * 插入节点到指定位置
     */
    public void addNodeByIndex(int index,Node node){
        if (index<=0 || index >len) {
            System.out.println("下标位置错误");
            return;
        }

        Node temp = head;
        for (int i=0;i==index;i++){
            temp = temp.next;
        }
        temp = temp.next;
        node.next = temp.next;
        temp.next = node;
        len++;
    }

    /**
     * 根据数据的大小进行有序的插入
     */
    public void addNodeBySort(Node node){
        Node temp = head;
        boolean flag = true;

        //让插入的节点的no，与temp的下一个节点的no进行比较
        for(;;) {
            //表明该节点进入到了链表的最后
            if (temp.next == null) {
                break;
            } else if (temp.next.data > node.data) {
                //说明插入的节点应该插入到temp的后面去
                break;
            } else if (temp.next.data == node.data) {
                //说明链表中已经存在这个no节点，不能够继续插入
                flag = false;
                break;
            }
            temp = temp.next;
        }

        if (flag == false) {
            System.out.println("该节点已经存在，不能够重新插入");
            return;
        } else {
            //进行节点的插入操作
            node.next = temp.next;
            temp.next = node;
            len++;
        }
    }

    /**
     * 将链表进行遍历
     */

    public void show(){
        if (len == 0){
            System.out.println("链表为空");
            return;
        }
        Node temp = head.next;
        while (temp != null){
            System.out.println(temp.data);
            temp = temp.next;
        }
    }

    /**
     * 根据下标进行删除节点
     */
    public void deleteByIndex(int index) {
        if (index <0 || index >= len){
            System.out.println("给定的下标位置不合适");
        }

        int i=0;
        Node temp = head;
        while(temp.next != null){
            if (index == i) {
                temp.next = temp.next.next;
                len--;
                return;
            }
            i++;
            temp = temp.next;
        }
    }
}
```

最后创建一个专门用于单链表测试的测试用例：
```
public class TestLinkList {
    public static void main(String[] args) {
        Node head = new Node();
        LinkList linkList = new LinkList(head);

        Node node1 = new Node(11);
        Node node2 = new Node(45);
        Node node3 = new Node(3);
        Node node4 = new Node(43);

        linkList.addNodeBySort(node1);
        linkList.addNodeBySort(node2);
        linkList.addNodeBySort(node3);
        linkList.addNodeBySort(node4);
        //根据下标进行删除
        linkList.deleteByIndex(2);
        //遍历链表
        linkList.show();

    }
}
```

### 3、java双向链表
#### 1）基本介绍：
java双向链表是一个巨头前节点、后节点、中间数据的一个链表。

#### 2）代码实现：
先创建一个node节点：
```
public class Node {

    public long data;

    public Node next;

    public Node pre;

    public Node(){

    }

    public  Node(long data){
        this.data = data;
    }

}
```

再创建一个双向链表：
```
public class LinkList {

    private Node head;

    private int len;

    public LinkList(Node head){
        this.head = head;
    }

    /**
     * 按照顺序进行增加节点
     * @param node
     */
    public void addNode(Node node){
        Node temp = head;
        while (temp.next != null){
            temp = temp.next;
        }
        temp.next = node;
        node.pre = temp;
        len++;
    }

    /**
     * 根据下标进行增加节点
     * @param index
     * @param node
     */
    public void addNodeByIndex(int index,Node node){
        Node temp = head;
        int i=0;
        while (i != index){
            temp = temp.next;
            i++;
        }
        node.next = temp.next;
        temp.next.pre = node;
        temp.next = node;
        node.pre = temp;
        len++;
    }

    /**
     * 根据下标进行删除节点
     */
    public boolean deleteNodeByIndex(int index){
        Node temp = head;
        int i=0;
        while( i != index ){
            temp = temp.next;
            i++;
        }
        temp.next = temp.next.next;
        if (temp.next != null){
            temp.next.pre = temp;
            return true;
        } else{
            return false;
        }
    }

    /**
     * 根据data来进行删除
     */
    public boolean deleteByData(int data){
        Node temp = head;
        //标识是否查找到了这个数据
        boolean flag = false;
        //循环遍历查找这个数据
        while(temp.next != null){
            if (temp.next.data == data){
                flag = true;
                break;
            }
            temp= temp.next;
        }

        //找到了数据，删除这个节点，让这个节点指向下下个数据节点即可
        if (flag) {
            temp.next = temp.next.next;
            if (temp.next != null){
                temp.next.pre = temp;
            }
            return true;
        } else {
            System.out.println("没有这个节点");
            return false;
        }
    }

    /**
     * 判断是否为空
     * @return
     */
    public boolean isEmpty(){
        return head == null;
    }

    /**
     * 前序遍历链表
     */
    public void show(){
        if (len == 0){
            System.out.println("链表为空");
            return;
        }
        Node temp = head.next;
        while (temp != null){
            System.out.println(temp.data);
            temp = temp.next;
        }
    }

    /**
     * 后续遍历链表
     */
    public void showAfter(){
        Node temp = head;

        if (temp.next == null) {
            System.out.println("链表为空");
            return;
        }

        for(;;) {
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
        for(;;) {
            System.out.println(temp.data);

            //判断是否链表头
            temp = temp.pre;
            if (temp.pre == null) {
                System.out.println("链表遍历完成");
                break;
            }
        }
    }
}
```

编写一个测试用例：
```
public class TestLinkList {
    public static void main(String[] args) {
        Node head = new Node();
        LinkList linkList = new LinkList(head);

        Node node1 = new Node(11);
        Node node2 = new Node(45);
        Node node3 = new Node(3);
        Node node4 = new Node(43);
        Node node5 = new Node(66);
        Node node6 = new Node(99);

        linkList.addNode(node1);
        linkList.addNode(node2);
        linkList.addNode(node3);
        linkList.addNode(node4);
        linkList.addNode(node5);
        //linkList.addNodeByIndex(2,node6);

        //linkList.deleteNodeByIndex(3);
        linkList.deleteByData(66);
        //遍历链表
        linkList.show();
        //后序遍历
        System.out.println("后序遍历：");
        linkList.showAfter();

    }
}
```

### 4、java环形链表





## 六、java递归
### 1、基本介绍：
递归的思想就是在方法里面调用方法本身。

在使用递归策略时，必须有一个明确的递归结束条件，称为递归出口。

递归调用的过程中系统每一层的返回点，局部变量等开辟了栈来存储。递归的次数过多的话，容易造成StackOverFlowerError（栈空间溢出）。

**注意：**   
在做递归算法的时候，一定要把我出口，也就是做递归算法必须要有一个明确的递归结束条件，如果符合了这个条件，就结束递归的运行。

### 2、代码简单演示一：
简单的递实例：
```
public class MyRecursive {

    private void show(){
        System.out.println("Hello World!");
    }

    private void show1(int n){
        if (n-- == 0){
            return;
        }
        show();
        show1(n);
    }

    public static void main(String[] args) {
        MyRecursive myRecursive = new MyRecursive();
        myRecursive.show1(5);
    }
}
```

### 3、代码简单演示二：
简单的递归实例：
```
public class MyRecursive1 {

    public int test(int n){
        if (n >50){
            return n;
        }
        n= n+1;
        return test(n);
    }

    public int test1(int n){
        if (n==1){
            return 1;
        }
        return n+test1(n-1);
    }

    public static void main(String[] args) {
        MyRecursive1 recursive1 = new MyRecursive1();
        int test = recursive1.test(1);
        System.out.println(test);

        int i = recursive1.test1(5);
        System.out.println(i);
    }
}

```

### 4、代码简单演示三：
解决 Fibonacci 数据：
```
public class MyRecursive2 {

    public int test(int n) {
        if (n == 1){
            return 0;
        } else if (n==2){
            return 1;
        } else {
            return test(n-1) + test(n-2);
        }
    }

    public static void main(String[] args) {
        MyRecursive2 myRecursive2 = new MyRecursive2();
        int test = myRecursive2.test(6);
        System.out.println(test);
    }

}
```

