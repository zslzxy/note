[TOC]
# 尚硅谷——Java数据结构

## 1 稀疏数组与队列

### 1.1 稀疏数组

#### 1.1.1 基本介绍

> 当一个数组中大部分元素为0，或者为相同值的一个数组时，可以使用稀疏数组来保存该数组。
>
> 稀疏数组处理方式：
>
> 1）记录数组一共有几行几列，有多少个不同的值。
>
> 2）把具有不通知的元素的行列及值记录在一个小规模数组中，从而缩小数组的规模。
>
> ![1559650025360](assets/1559650025360.png)
>
> 稀疏数组：稀疏数组的第一行是记录该数组的行、列和拥有的数据个数；后面的每一行都是记录的对应的数据。

#### 1.1.2 实现思路

> 二维数组转稀疏数组：
>
> 1）遍历 原始二维数组，得到有效数据的个数sum；
>
> 2）根据 sum 个数创建稀疏数组 sparseArr  int\[sum+1][3]；
>
> 3）将二维数组的有效数据存入到稀疏数组中。
>
> 稀疏数组转二维数组：
>
> 1）先读取稀疏数组的第一行，根据第一行的数据，创建原始数组， chessArr  int\[6]\[7]
>
> 2）在读取稀疏数组后几行的数据，并赋值给原始的二维数组即可。

#### 1.1.3 实现代码

```java
package com.data.structure;

public class SpareArr {

    /**
     * 创建一个二维数组
     * @return
     */
    public static int[][] createArrs() {
        int[][] arrs = new int[11][11];
        arrs[1][2] = 1;
        arrs[2][3] = 2;
        return arrs;
    }

    /**
     * 得到二维数组的长度
     * @param arrs
     * @return
     */
    public static int getNum(int[][] arrs) {
        int num = 0;
        for (int[] arr : arrs) {
            for (int i : arr) {
                if (i != 0) {
                    num++;
                }
            }
        }
        return num;
    }

    /**
     * 二维数组转换为稀疏数组
     */
    public static int[][] toParseArr() {
        int row = 1;
        int[][] arrs = createArrs();
        int num = getNum(arrs);
        int[][] sparseArrs = new int[num + 1][3];
        //为稀疏数组赋值第一行的数据
        sparseArrs[0][0] = arrs.length;
        sparseArrs[0][1] = arrs[0].length;
        sparseArrs[0][2] = num;
        //将非零数据存入到稀疏数组中进行保存
        for (int i = 0; i < arrs.length; i++) {
            for (int j = 0; j < arrs[i].length; j++) {
                if (arrs[i][j] != 0) {
                    sparseArrs[row][0] = i;
                    sparseArrs[row][1] = j;
                    sparseArrs[row][2] = arrs[i][j];
                    row++;
                }
            }
        }
        return sparseArrs;
    }

    public static int[][] tochessArr(int[][] pareseArrs) {
        int[][] arrs = new int[pareseArrs[0][0]][pareseArrs[0][1]];
        for (int i = 1; i < pareseArrs.length; i++) {
            arrs[pareseArrs[i][0]][pareseArrs[i][1]] = pareseArrs[i][2];
        }
        return arrs;
    }

    public static void main(String[] args) {
        int[][] parseArr = toParseArr();
        tochessArr(parseArr);
    }
}
```

### 1.2 队列

#### 1.2.1 基本介绍

> - 队列是一个有序列表，可以使用 `数组`或者 `链表`来实现。
> - 队列遵循 `先入先出`的原则。即：县存入队列的数据，要先取出；后存入队列的数据要后取出。

#### 1.2.2 数组模拟队列

> 创建一个Queue类，其中包含的属性有：
>
> - **`队列数组`**：用于存放队列中的数据。
> - **`maxSize`**：队列最大存放容量。
> - **`front`**：初始值为 -1，当从队列中取出数据，则对应的front数据值发生变化，对应到有队列中数据最前的前一个位置。
> - **`rear`**：初始值为 -1，当往队列中添加数据，则对应的rear数据值发生变化，对应到队列中数据最后的位置。
>
> ![1559704436953](assets/1559704436953.png)

```java
package com.data.structure.queue;

import java.util.Scanner;

/**
 * @author ${张世林}
 * @date 2019/06/05
 * 作用：使用数组实现队列
 */
public class ArrQueue {

    /**
     * 设置队列存放数据的默认最大值
     */
    private int maxSize;

    /**
     * 设置队列的前指针，指向的是队列中第一个元素的前一个元素
     */
    private int front;

    /**
     * 设置队列的后指针，指向的是队列中最后一个元素
     */
    private int rear;

    /**
     * 队列中的存放数据的数组
     */
    private int[] arr;

    /**
     * 创建队列构造器
     */
    public ArrQueue(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = -1;
        rear = -1;
    }

    /**
     * 判断队列是否已满
     * @return
     */
    public boolean isFull() {
        return rear == maxSize - 1;
    }

    /**
     * 当rear与front指向同一个位置，则表示队列为空
     * @return
     */
    public boolean isEmpty() {
        return rear == front;
    }

    /**
     * 添加数据
     * @param num
     */
    public void addQueue(int num) {
        if (isFull()) {
            System.out.println("队列已满，不能添加数据");
            return;
        }
        arr[++rear] = num;
    }

    /**
     * 取出数据
     */
    public int getQueue() {
        if (isEmpty()) {
            System.out.println("队列为空，无法获取数据");
            throw new RuntimeException("队列为空，无法获取数据");
        }
        //如果队列不为空，则front后移
        front++;
        return arr[front];
    }

    /**
     * 遍历Queue
     */
    public void show() {
        if (isEmpty()) {
            System.out.println("队列为空，无法遍历");
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.printf("arr[%d]=%d\n",i,arr[i]);
        }
    }

    /**
     * 显示队列的头数据
     */
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，没有头信息");
        }
        return arr[front + 1];
    }

    public static void main(String[] args) {
        ArrQueue queue = new ArrQueue(3);
        char key = ' ';
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s(show)");
            System.out.println("e(exit)");
            System.out.println("a(add)");
            System.out.println("g(get)");
            System.out.println("h(head)");
            //得到输入的第一个字符
            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queue.show();
                    break;
                case 'a':
                    System.out.println("请输入需要加入的一个数：");
                    int val = scanner.nextInt();
                    queue.addQueue(val);
                    break;
                case 'g':
                    try {
                        System.out.println("取出的数据为："+queue.getQueue());
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int headQueue = queue.headQueue();
                        System.out.println("队列头的数据是："+headQueue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出");
    }
}
```

#### 1.2.3 数组环形队列

> 创建一个环形Queue类，其中包含的属性有：
>
> - **`队列数组`**：用于存放队列中的数据。
> - **`maxSize`**：队列最大存放容量。
> - **`front`**：初始值为0；默认指向的是数组中第一个元素的位置。
> - **`rear`**：初始值为0；默认指向的是数组汇总最后一个元素的位置的后一个位置，因为可以做出一个空间作为约定。
>
> 条件判断：
>
> - 当队列满时，条件是 **(rear + 1) % maxSize == front**。
> - 当队列空时，条件是 **rear == front**。
> - 队列中有效的数据个数为 **(rear + maxSize - front) % maxSize**。

```java
package com.data.structure.queue;

import java.util.Scanner;

/**
 * @author ${张世林}
 * @date 2019/06/05
 * 作用：使用数组实现队列
 */
public class ArrAroundQueue {

    private int maxSize;

    //指向队列中的第一个元素，默认初始值为0
    private int front;

    //指向队列中最后一个元素的后一个位置，默认初始值为0
    private int rear;

    private int[] arr;

    /**
     * 创建队列构造器
     */
    public ArrAroundQueue(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        front = 0;
        rear = 0;
    }

    /**
     * 验证是否已满
     *
     * @return
     */
    public boolean isFull() {
        return (rear + 1) % maxSize == front;
    }

    /**
     * 判断是否为空
     *
     * @return
     */
    public boolean isEmpty() {
        return rear == front;
    }

    /**
     * 添加数据
     *
     * @param num
     */
    public void addQueue(int num) {
        if (isFull()) {
            System.out.println("队列已满，不能添加数据");
            return;
        }
        arr[rear] = num;
        //将rear指针进行后移工作
        rear = (rear + 1) % maxSize;
    }

    /**
     * 取出数据
     */
    public int getQueue() {
        if (isEmpty()) {
            System.out.println("队列为空，无法获取数据");
            throw new RuntimeException("队列为空，无法获取数据");
        }
        //因为front指向的是队列的第一个元素，所以直接可以取出
        int num = arr[front];
        //front进行后移操作
        front = (front + 1) % maxSize;
        return num;
    }

    /**
     * 遍历Queue
     */
    public void show() {
        if (isEmpty()) {
            System.out.println("队列为空，无法遍历");
        }
        for (int i = front; i < front + size(); i++) {
            System.out.printf("arr[%d]=%d\n", (i % maxSize), arr[i % maxSize]);
        }
    }

    /**
     * 获取当前队列中有效数据的个数
     */
    public int size() {
        return (rear + maxSize - front) % maxSize;
    }

    /**
     * 显示队列的头数据
     */
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，没有头信息");
        }
        return arr[front];
    }

    public static void main(String[] args) {
        ArrAroundQueue queue = new ArrAroundQueue(3);
        char key = ' ';
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop) {
            System.out.println("s(show)");
            System.out.println("e(exit)");
            System.out.println("a(add)");
            System.out.println("g(get)");
            System.out.println("h(head)");
            //得到输入的第一个字符
            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queue.show();
                    break;
                case 'a':
                    System.out.println("请输入需要加入的一个数：");
                    int val = scanner.nextInt();
                    queue.addQueue(val);
                    break;
                case 'g':
                    try {
                        System.out.println("取出的数据为：" + queue.getQueue());
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int headQueue = queue.headQueue();
                        System.out.println("队列头的数据是：" + headQueue);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出");
    }

}
```

### 1.3 链表

> ​	链表（Linked list）是一种常见的基础数据结构，是一种线性表，但是并不会按线性的顺序存储数据，而是在每一个节点里存到下一个节点的指针(Pointer)。
>
> ​	使用链表结构可以克服数组链表需要预先知道数据大小的缺点，链表结构可以充分利用计算机内存空间，实现灵活的内存动态管理。但是链表失去了数组随机读取的优点，同时链表由于增加了结点的指针域，空间开销比较大。其存储结构如下：
>
> ![1560157074810](assets/1560157074810.png)
>
> ​	链表的特征如下：
>
> - 链表是以节点的方式进行存储,，是链式存储；
> - 每一个节点包含data域：存放数据，next域：存放下一个节点地址；
> - 链表在各个节点不一定是连续存储的操作。
> - 链表分带头结点的链表和没有带头结点的链表。

#### 1.3.1 单向、头结点单链表	

​	添加方式一：这是一个单向链表，具有头结点，添加节点是按照添加节点的时间先后顺序进行添加操作。

​	添加方式二：这是一个单向链表，具有头结点，添加节点是data域中的数据进行排序以后插入到链表中。

```java
package linked;

/**
 * @author ${张世林}
 * @date 2019/06/10
 * 作用：单向的、带有头结点的、按照添加顺序加入链表的 单链表
 */
public class LinkedListDemo {

    public static void main(String[] args) {
        HeroNode node1 = new HeroNode(1, "张世林", "林林");
        HeroNode node2 = new HeroNode(2, "田水龙", "水水");
        HeroNode node3 = new HeroNode(3, "王旭", "嘘嘘");
        HeroNode node4 = new HeroNode(4, "杨书平", "杨总");
        LinkedList linkedList = new LinkedList();
        linkedList.addByOrder(node1);
        linkedList.addByOrder(node2);
        linkedList.addByOrder(node4);
        linkedList.addByOrder(node3);
        linkedList.show();

//        linkedList.changeNode(new HeroNode(2, "tom", "house"));
        System.out.println("======================");
        linkedList.deleteNode(5);
        linkedList.show();

    }
}

/**
 * 定义一个单链表
 */
class LinkedList {
    /**
     * 先初始化一个头结点，通常情况下，头结点是不会改变的。
     */
    private HeroNode head = new HeroNode(0, "", "");

    /**
     * 添加节点方式一：直接将节点添加到链表最后位置处
     * 步骤：
     * 1. 找到当前链表的最后的一个节点
     * 2. 将最后的一个节点的next指向 新的节点
     */
    public void addByLast(HeroNode heroNode) {
        //创建一个辅助节点，用于遍历该链表
        HeroNode temp = this.head;
        //遍历链表，一直到链表的最后
        while (true) {
            //如果temp这个辅助节点一直指向了最后一个节点，则将会跳出循环。
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
        //将temp.next指向新的节点
        temp.next = heroNode;
    }

    /**
     * 添加节点方式二：遍历链表数据，在数据合适的位置插入该节点。
     * 步骤：
     * 1. 找到该新的节点需要在链表的对应位置处。
     * 2. 将该位置的前一个节点的next指向新节点，新节点的next指向该位置的后一个节点
     */
    public void addByOrder(HeroNode heroNode) {
        HeroNode temp = this.head;
        boolean addSuccess = false;
        while (temp.next != null) {
            if (temp.no < heroNode.no && temp.next.no >= heroNode.no) {
                heroNode.next = temp.next;
                temp.next = heroNode;
                addSuccess = true;
            }
            temp = temp.next;
        }
        if (!addSuccess) {
            temp.next = heroNode;
        }
    }

    /**
     * 判断当前链表是否为空
     * @return
     */
    public boolean isEmpty() {
        return head.next == null;
    }

    /**
     * 遍历链表数据
     */
    public void show() {
        if (isEmpty()) {
            System.out.println("链表为空，无法进行遍历");
            return;
        }
        HeroNode temp = head.next;
        while (true) {
            if (temp == null) {
                System.out.println("链表遍历完成");
                break;
            }
            //将temp输出
            System.out.println(temp);
            temp = temp.next;
        }
    }

    /**
     * 根据id删除链表中的一个节点
     */
    public void deleteNode(int no) {
        HeroNode temp = this.head;
        while (true) {
            if (temp.next == null) {
                System.out.printf("未查到no=%d的数据，删除失败。\n",no);
                break;
            }
            if (temp.next.no == no) {
                temp.next = temp.next.next;
                System.out.println("删除对应的节点成功");
                return;
            }
            temp = temp.next;
        }
    }

    /**
     * 修改一个节点的数据
     */
    public void changeNode(HeroNode newHeroNode) {
        HeroNode temp = this.head;
        while (true) {
            if (temp == null) {
                System.out.printf("未查到no=%d的数据，修改失败。\n",newHeroNode.no);
                break;
            }
            if (temp.no == newHeroNode.no) {
                temp.name = newHeroNode.name;
                temp.nickName = newHeroNode.nickName;
                System.out.printf("修改no=%d的数据成功。\n",newHeroNode.no);
                return;
            }
            temp = temp.next;
        }
    }
}

/**
 * 定义一个节点
 */
class HeroNode {
    //id
    public int no;
    //姓名
    public String name;
    //昵称
    public String nickName;
    //下一个节点
    public HeroNode next;

    //创建一个节点的构造器
    public HeroNode(int no, String name, String nickName) {
        this.name = name;
        this.no = no;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
```

#### 1.3.2 单链表面试题

##### 1.3.2.1 单链表的反转

> 实现思路：
>
> 1. 首先先定义一个节点 reverseHead = new HeroNode();
> 2. 从头到尾遍历原来的链表，每遍历一个节点，就将其取出，放置到 reverseHead 链表的最前端。
> 3. 使用  head.next = reverseHead.next;

```java
    /**
     * 单链表的反转
     *  思路：
     *      1. 首先先定义一个节点 reverseHead = new HeroNode();
     *      2. 从头到尾遍历原来的链表，每遍历一个节点，就将其取出，放置到 reverseHead 链表的最前端。
     *      3. 使用  head.next = reverseHead.next;
     */
    public void reverseLinkedList() {
        //创建一个新链表的头结点
        HeroNode reverseNode = new HeroNode(1, "", "");
        //遍历原始链表
        HeroNode temp = this.head.next;
        //创建一个节点对象，其作用是用于保存 原始链表指向的当前节点的下一个节点
        HeroNode defaultNextNode = null;
//方式一：        
        //循环遍历原始链表
        while (temp != null) {
            //首先先将原数据的下一个节点地址保存在这个变量中
            defaultNextNode = temp.next;
            //将temp的下一个节点指向新的链表的最前端
            temp.next = reverseNode.next;
            //将temp连接到新的链表中
            reverseNode.next = temp;
            //让temp继续回到原始链表
            temp = defaultNextNode;
        }
        head.next = reverseNode.next;
        
//方式二：        
//        int size = size();
//        for (int i = 0; i < size; i++) {
//            //首先先将原数据的下一个节点地址保存在这个变量中
//            defaultNextNode = temp.next;
//            //将temp的下一个节点指向新的链表的最前端
//            temp.next = reverseNode.next;
//            //将temp连接到新的链表中
//            reverseNode.next = temp;
//            //让temp继续回到原始链表
//            temp = defaultNextNode;
//        }
//        head.next = reverseNode.next;
    }

```

#### 1.3.3 双向、头结点链表

```java
package doublelinked;

/**
 * @author ${张世林}
 * @date 2019/06/11
 * 作用：
 */
public class DoubleLinkedListDemo {

    public static void main(String[] args) {
        DobuleLinkedList linkedList = new DobuleLinkedList();
        HeroNode node1 = new HeroNode(1, "张世林", "林林");
        HeroNode node2 = new HeroNode(2, "田水龙", "水水");
        HeroNode node3 = new HeroNode(3, "王旭", "嘘嘘");
        HeroNode node4 = new HeroNode(4, "杨书平", "杨总");
//        linkedList.addByLast(node1);
//        linkedList.addByLast(node2);
//        linkedList.addByLast(node3);
//        linkedList.addByLast(node4);
        linkedList.addByOrder(node1);
        linkedList.addByOrder(node2);
        linkedList.addByOrder(node4);
        linkedList.addByOrder(node3);
//        linkedList.changeNode(new HeroNode(35, "ttt","yyy"));
        linkedList.show();
//        linkedList.delNode(7);
//        linkedList.show();
    }
}

class DobuleLinkedList {

    private HeroNode head = new HeroNode(0, "", "");

    public HeroNode getHead() {
        return head;
    }

    /**
     * 在双向链表的末尾添加节点
     * @param heroNode
     */
    public void addByLast(HeroNode heroNode) {
        HeroNode temp = this.head;
        while (temp.next != null) {
            temp = temp.next;
        }
        heroNode.pre = temp;
        temp.next = heroNode;
    }

    public void addByOrder(HeroNode heroNode) {
        HeroNode temp = this.head;
        boolean addSuccess = false;
        while (temp.next != null) {
            if (heroNode.no > temp.no && heroNode.no <= temp.next.no) {
                heroNode.next = temp.next;
                heroNode.pre = temp.next.pre;
                temp.next.pre = heroNode;
                temp.next = heroNode;
                addSuccess = true;
                break;
            }
            temp = temp.next;
        }
        if (!addSuccess) {
            temp.next = heroNode;
            heroNode.pre = temp;
        }
    }

    /**
     * 修改双向链表节点信息
     */
    public void changeNode(HeroNode node) {
        if (isEmpty()) {
            System.out.println("双向链表为空，无法进行修改操作");
            return;
        }
        boolean changeSuccess = false;
        HeroNode temp = this.head;
        while (temp.next != null) {
            temp = temp.next;
            if (temp.no == node.no) {
                temp.name = node.name;
                temp.nickName = node.nickName;
                changeSuccess = true;
                break;
            }
        }
        if (!changeSuccess) {
            System.out.println("未查找到对应的节点数据");
        } else {
            System.out.println("修改节点成功");
        }
    }

    /**
     * 删除双向链表中的节点
     */
    public void delNode(int no) {
        HeroNode temp = this.head.next;
        boolean findSuccess = false;
        while (true) {
            if (temp == null) {
                break;
            }
            if (temp.no == no) {
                findSuccess = true;
                break;
            }
            temp = temp.next;
        }
        if (findSuccess) {
            temp.pre.next = temp.next;
            if (temp.next != null) {
                temp.next.pre = temp.pre;
            }
            System.out.println("删除成功");
        } else {
            System.out.println("未查找到对应的节点，删除失败");
        }
    }


    /**
     * 判断链表是否为空
     */
    public boolean isEmpty() {
        return head.next == null;
    }

    /**
     * 遍历双向链表的方法
     */
    public void show() {
        if (isEmpty()) {
            System.out.println("双向链表中不存在节点，无法进行遍历");
            return;
        }
        HeroNode temp = this.head;
        while (temp.next != null) {
            temp = temp.next;
            System.out.println(temp);
        }
    }

}

/**
 * 定义一个双向节点
 */
class HeroNode {
    //id
    public int no;
    //姓名
    public String name;
    //昵称
    public String nickName;
    //下一个节点
    public HeroNode next;
    //前一个节点
    public HeroNode pre;

    //创建一个节点的构造器
    public HeroNode(int no, String name, String nickName) {
        this.name = name;
        this.no = no;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
```

#### 1.3.4 单向环形链表——约瑟夫问题

单向环形链表：

> 构建一个环形链表思路：
>
> 1. 先创建第一个节点，让first指针指向该节点，并将其自身相连组成一个环状。
> 2. 创建一个辅助指针，用于指向该环形链表的最后添加的位置。
> 3. 后面每创建一个新的节点，就把该节点，加入到已有的环形链表中，然后变种辅助指针指向该节点。
>
> 便利环形链表：
>
> 1. 首先创建一个辅助指针，指向first位置；
> 2. 然后遍历该环形链表，直到再次到达first位置就结束遍历。

约瑟夫问题：

> 约瑟夫问题描述：
>
> ​	设编号为1，2，...n的n个人围坐一圈，约定编号为k（1 <= k <= n）的人从1开始报数，数到m的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，直到所有的人出列为止，由此产生一个出队编号序列。
>
> 约瑟夫解决思路：
>
> ​	用一个不带头结点的循环链表来处理约瑟夫问题：先构成一个有n个节点的单循环链表，然后由k节点起开始计数，计数到m时，对应节点从链表中删除，让偶再从被删除节点的下一个节点又从1开始计数，直到最后一个节点从链表中删除。

总体代码实现：

```java
package singlecirclelinked;

/**
 * @author ${张世林}
 * @date 2019/06/11
 * 作用：单向环形链表来 解决 约瑟夫问题
 */
public class SingleCircleLinkedListDemo {

    public static void main(String[] args) {
        SingleCircleLinkedList linkedList = new SingleCircleLinkedList();
        linkedList.add(new Node(1));
        linkedList.add(new Node(2));
        linkedList.add(new Node(3));
        linkedList.add(new Node(4));
        linkedList.add(new Node(5));
        linkedList.show();
        System.out.println(linkedList.size());
        System.out.println("=============");
        linkedList.pop(2,3);
    }

}

/**
 * 创建单向环形链表
 */
class SingleCircleLinkedList {

    //创建first节点，用于指向第一个节点
    private Node first = null;
    //创建一个辅助节点，该辅助节点是用于指向最后添加的那个位置的节点
    private Node curNode = null;

    public void add(Node node) {
        if (first == null) {
            first = node;
            first.setNext(first);
            curNode = first;
        } else {
            curNode.setNext(node);
            node.setNext(first);
            curNode = node;
        }
    }

    /**
     * 解决约瑟夫问题
     *      设编号为1，2，...n的n个人围坐一圈，约定编号为k（1 <= k <= n）的人从1开始报数，
     *      数到m的那个人出列，它的下一位又从1开始报数，数到m的那个人又出列，依次类推，
     *      直到所有的人出列为止，由此产生一个出队编号序列。
     * 代码实现步骤：
     *  1. 第一步，先移动对应的节点，让first与curNode节点一起向前移动 k - 1 次
     *  1. 第二步，让first指向该被删除的下一个节点；
     *  2. 第三步，让curNode节点一直跟在first的上一个节点。
     * @param startNo:表示从第几个小孩开始数数
     * @param countNo:表示每一次数多少个数
     */
    public void pop(int startNo, int countNo) {
        //验证数据是否正常
        if (first == null || startNo < 1 || startNo > size() || countNo < 1) {
            System.out.println("链表为空，无法执行出圈操作");
            return;
        }
        //让first与curNode移动到startNO的位置
        for (int i = 0; i < startNo - 1; i++) {
            first = first.getNext();
            curNode = curNode.getNext();
        }
        //让其进行循环出圈的工作
        while (true) {
            //验证first与curNode相同的时候，说明圈中只有一个人
            if (first == curNode) {
                break;
            }
            //进行遍历，遍历完成以后，就是需要出圈的节点
            for (int i = 0; i < countNo - 1; i++) {
                first = first.getNext();
                curNode =curNode.getNext();
            }
            System.out.println("出圈的节点为："+first);
            first = first.getNext();
            curNode.setNext(first);
        }
        System.out.println("最后出圈的节点为："+first);

    }

    /**
     * 获取环形链表的大小
     * @return
     */
    public int size() {
        if (first == null) {
            System.out.println("链表为空，大小为0");
            return 0;
        }
        int num = 1;
        Node temp = this.first.getNext();
        while (true) {
            if (temp != null) {
                if (temp == first) {
                    break;
                }
                num++;
            }
            temp = temp.getNext();
        }
        return num;
    }


    /**
     * 遍历输出
     */
    public void show() {
        if (first == null) {
            System.out.println("链表为空，无法进行遍历");
        }
        Node temp = this.first;
        System.out.println(temp);
        while (temp.getNext() != first) {
            temp = temp.getNext();
            System.out.println(temp);
        }
    }

}

/**
 * 创建节点
 */
class Node {
    //编号
    private int no;
    //指向下一个节点
    private Node next;

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public Node getNext() {
        return next;
    }

    public void setNext(Node next) {
        this.next = next;
    }

    public Node(int no) {
        this.no = no;
    }

    @Override
    public String toString() {
        return "Node{" +
                "no=" + no +
                '}';
    }
}
```

## 2 栈

### 2.1 基本介绍

> - 栈的英文名称为 stack；
> - 栈是一种 **先入后出（First In Last Out）**的有序列表。
> - 栈是限制线性表中元素插入和删除只能在线性表的同一端进行的一种特殊线性表。允许插入和删除的一端，称为 **栈顶**；固定不变的一段，称为 **栈底**。
> - **最先放入栈中的元素在栈底，最后放入的元素在栈顶**；最先删除在栈顶，最后删除在栈底。

### 2.2 应用场景

> - 子程序的调用：在跳往子程序前，会先将下个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。   
>
> - 处理递归调用：和子程序的调用类似，只是除了储存下一个指令的地址外，也将参数、区域变量等数据存入堆栈中。
>
> - 表达式的转换[中缀表达式转后缀表达式]与求值(实际解决)。
>
> - 二叉树的遍历。
>
> - 图形的深度优先(depth一first)搜索法。

### 2.3 数组栈实现

#### 2.3.1 实现思路

> - 第一步，创建数组，用于存放数据，设置栈的最大空间值 maxTop；
> - 第二步，定义一个top来表示栈顶，初始化为 -1；
> - 第三步，入栈的操作，当有数据加入到栈时，top++，stack[top]=data;
> - 第四步，出栈的操作，当有数据出栈，则 int value = stack[top];    top--;     return  value;    

#### 2.3.2 代码实现

```java
package stack;

/**
 * @author ${张世林}
 * @date 2019/06/12
 * 作用：
 */
public class ArrayStackDemo {

    public static void main(String[] args) {
        ArrayStack stack = new ArrayStack(4);
        stack.push(1);
        stack.push(2);
        stack.push(3);
        stack.push(4);
        stack.push(5);
        stack.show();
        System.out.println("==================");
        System.out.println(stack.pop());
        System.out.println(stack.pop());
        stack.push(6);
        stack.push(7);
        stack.push(8);
        System.out.println(stack.pop());
    }
}

class ArrayStack {
    /**
     * 栈的最大容量
     */
    private int maxSize;

    /**
     * 数组，用于存放数据
     */
    private int[] stack;

    /**
     * 栈顶的位置，初始值为-1；
     */
    private int top = -1;

    /**
     * 构造器
     * @param maxSize
     */
    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }

    /**
     * 判断栈是否已满
     * @return
     */
    public boolean isFull() {
        return top == maxSize - 1;
    }

    /**
     * 判断栈是否为空
     * @return
     */
    public boolean isEmpty() {
        return top == -1;
    }

    /**
     * 向栈顶加入数据
     * @param value
     */
    public void push(int value) {
        if (isFull()) {
            System.out.println("栈空间已满，无法添加数据");
            return;
        }
        top++;
        stack[top] = value;
    }

    /**
     * 出栈，将栈顶的数据取出
     * @return
     */
    public int pop() {
        if (top == -1) {
            throw new RuntimeException("栈为空，无法取出数据");
        }
        return stack[top--];
    }

    /**
     * 遍历栈
     */
    public void show() {
        for (int i = top; i >= 0; i--) {
            System.out.println(stack[i]);
        }
    }
}
```

### 2.4 栈的三种表达式

> - **前缀表达式**   
>
>   - 实现方式：从右至左扫描表达式，遇到数字时，将数字压入堆栈中，当遇到运算符时，弹出栈顶的两个数，用运算符对他们做相应的计算操作，并将结果存入到栈中；重复上诉过程即可实现前缀表达式。
>
>   - 例如: (3+4)×5-6 对应的前缀表达式就是 **- × + 3 4 5** **6 ,** 针对前缀表达式求值步骤如下：
>
>     1)从**右至左扫描**，将6、5、4、3压入堆栈
>
>     2)遇到+运算符，因此弹出3和4（3为栈顶元素，4为次顶元素），计算出3+4的值，得7，再将7入栈
>
>     3)接下来是×运算符，因此弹出7和5，计算出7×5=35，将35入栈
>
>     4)最后是-运算符，计算出35-6的值，即29，由此得出最终结果
>
> - **中缀表达式**  
>
>   - 中缀表达式是常见的运算表达式，如 (3+4)*6-5;
>   - 中缀表达式的求值是我们人最熟悉的，但是对计算机来说却不好操作(前面我们讲的案例就能看的这个问题)，因此，在计算结果时，往往会将中缀表达式转成其它表达式来操作(一般转成后缀表达式.)
>
> - **后缀表达式（逆波兰表达式） ** 
>
>   - 后缀表达式又称**逆波兰表达式**,与前缀表达式相似，只是运算符位于操作数之后;
>
>   - 举例说明：(3+4)×5-6 对应的后缀表达式就是 34+5*6-
>
>     1)从左至右扫描，将3和4压入堆栈；
>
>     2)遇到+运算符，因此弹出4和3（4为栈顶元素，3为次顶元素），计算出3+4的值，得7，再将7入栈；
>
>     3)将5入栈；
>
>     4)接下来是×运算符，因此弹出5和7，计算出7×5=35，将35入栈；
>
>     5)将6入栈；
>
>     6)最后是\-运算符，计算出35-6的值，即29，由此得出最终结果

### 2.5 栈的考题

#### 2.5.1 栈实现综合计算器（中缀表达式）

> ​	使用栈来实现对应的算术表达式的计算：
>
> ![1560485119943](assets/1560485119943.png)

代码实现：

```java
package calculator;

/**
 * @author ${张世林}
 * @date 2019/06/14
 * 作用：计算器(计算一个字符串的表达式的值，该计算不包含括号操作)
 * 步骤：
 * 1. 创建索引index，用于遍历表达式，在创建两个栈，数栈和符号栈
 * 2. 如果发现是一个数字，则直接加入到 数栈 中。
 * 3. 如果发现扫描到时一个符号，则：
 *  3.1 如果当前 符号栈 为空，则直接入栈；
 *  3.2 如果当前 符号栈 不为空，则与栈顶符号进行比较：
 *          - 如果当前的操作符的优先级小于或者等于栈中的操作符，就从数栈中取出两个数，符号栈中取出一个数，
 *              进行运算以后将结果存入数栈中，再将当前操作如入符号栈
 *          - 如果当前的操作符的优先级大于栈顶的符号，就直接入符号栈；
 * 4. 当表达式扫描完毕，就顺序的从数栈和符号栈中取出相应的数和符号，并运行。
 * 5. 最后在数栈只有一个数字，就是表达式的结果。
 */
public class Calculator {

    public static void main(String[] args) {
        String expression = "33+20*6-2";
        int length = expression.length();
        ArrayStack numStack = new ArrayStack(10);
        ArrayStack operStack = new ArrayStack(10);
        //用于扫描表达式的位置
        int index = 0;
        //第一个数
        int num1 = 0;
        //第二个数
        int num2 = 0;
        //运算符
        int oper = 0;
        //计算的结果
        int res = 0;
        //将每次扫描得到的char保存到ch中
        char ch = ' ';
        //用于数据的多位数据
        String keepNum = "";
        while (true) {
            ch = expression.substring(index, index + 1).charAt(0);
            //如果是符号
            if (operStack.isOper(ch)) {
                //判断符号栈是否为空
                if (!operStack.isEmpty()) {
                    //栈不为空，比较优先级
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, oper);
                        numStack.push(res);
                        operStack.push(ch);
                    } else {
                        operStack.push(ch);
                    }
                } else {
                    //栈为空，将ch存入到栈中
                    operStack.push(ch);
                }
            } else {
                //如果是数字，则直接进入到栈中
                keepNum += ch;
                //如果ch是expression已经是最后一位，则直接入栈
                if (index == length - 1) {
                    numStack.push(Integer.parseInt(keepNum));
                } else {
                    //验证expression的下一位还是符号，则简单入栈
                    if (operStack.isOper(expression.substring(index + 1, index + 2).charAt(0))) {
                        numStack.push(Integer.parseInt(keepNum));
                        keepNum = "";
                    }
                }
            }
            //让index = index + 1
            index = index +1;
            if (index >= length) {
                break;
            }
        }
        //表达式扫描完毕
        while (!operStack.isEmpty()) {
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1, num2, oper);
            numStack.push(res);
        }
        System.out.println("表达式的结果 = "+numStack.pop());
    }

}

class ArrayStack {
    /**
     * 栈的最大容量
     */
    private int maxSize;

    /**
     * 数组，用于存放数据
     */
    private int[] stack;

    /**
     * 栈顶的位置，初始值为-1；
     */
    private int top = -1;

    /**
     * 构造器
     * @param maxSize
     */
    public ArrayStack(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[maxSize];
    }


    /**
     * 判断栈是否已满
     * @return
     */
    public boolean isFull() {
        return top == maxSize - 1;
    }

    /**
     * 判断栈是否为空
     * @return
     */
    public boolean isEmpty() {
        return top == -1;
    }

    /**
     * 向栈顶加入数据
     * @param value
     */
    public void push(int value) {
        if (isFull()) {
            System.out.println("栈空间已满，无法添加数据");
            return;
        }
        top++;
        stack[top] = value;
    }

    /**
     * 出栈，将栈顶的数据取出
     * @return
     */
    public int pop() {
        if (top == -1) {
            throw new RuntimeException("栈为空，无法取出数据");
        }
        return stack[top--];
    }

    /**
     * 遍历栈
     */
    public void show() {
        for (int i = top; i >= 0; i--) {
            System.out.println(stack[i]);
        }
    }

    /**
     * 返回栈顶的元素
     * @return
     */
    public int peek() {
        return stack[top];
    }

    /**
     * 返回运算符的优先级
     * 该优先级是用于确定运算符的执行顺序：可以使用数字来表示
     * 数字越大，优先级越高
     */
    public int priority(int oper) {
        if (oper == '*' || oper == '/') {
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        } else {
            return -1;
        }
    }

    /**
     * 判断是不是运算符
     */
    public boolean isOper(char val) {
        return val == '+' || val == '-' || val == '*' || val == '/';
    }

    /**
     * 计算方法
     */
    public int cal(int num1, int num2, int oper) {
        int result = 0;
        switch (oper) {
            case '+':
                result = num1 + num2;
                break;
            case '-':
                result = num2 - num1;
                break;
            case '*':
                result = num1 * num2;
                break;
            case '/':
                result = num2 / num1;
                break;
            default:
                break;
        }
        return result;
    }

}
```

#### 2.5.2 逆波兰计数器--后缀表达式

> 问题描述：
>
> 1)输入一个逆波兰表达式(后缀表达式)，使用栈(Stack),计算其结果
>
> 2)支持小括号和多位数整数，因为这里我们主要讲的是数据结构，因此计算器进行简化，只支持对整数的计算。

代码实现：

```java
package poland;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

/**
 * @author ${张世林}
 * @date 2019/06/14
 * 作用：逆波兰表达式——后缀表达式
 */
public class PolandNotation {

    public static void main(String[] args) {
        String suffixEcpression = "33 4 + 5 * 6 -";
        List<String> list = getListByString(suffixEcpression);
        int result = calculate(list);
        System.out.println("计算的结果为："+result);
    }

    /**
     * 判断是不是运算符
     */
    public static boolean isOper(String val) {
        return val.equals("+") || val.equals("-") || val.equals("*") || val.equals("/");
    }

    /**
     * 计算方法
     */
    public static int calculate(List<String> list) {
        Stack<String> stack = new Stack<String>();
        int num1 = 0;
        int num2 = 0;
        for (int i = 0; i < list.size(); i++) {
            if (isOper(list.get(i))) {
                num1 = Integer.parseInt(stack.pop());
                num2 = Integer.parseInt(stack.pop());
                int result = cal(num1, num2, list.get(i));
                stack.push(String.valueOf(result));
            } else {
                stack.push(list.get(i));
            }
        }
        return Integer.parseInt(stack.pop());
    }

    /**
     * 计算方法
     */
    public static int cal(int num1, int num2, String oper) {
        int result = 0;
        switch (oper) {
            case "+":
                result = num1 + num2;
                break;
            case "-":
                result = num2 - num1;
                break;
            case "*":
                result = num1 * num2;
                break;
            case "/":
                result = num2 / num1;
                break;
            default:
                break;
        }
        return result;
    }

    /**
     * 将逆波兰表达式，依次将其放置在ArrayList中
     */
    public static List<String> getListByString(String suffixExpression) {
        String[] split = suffixExpression.split(" ");
        List<String> list = new ArrayList<String>();
        for (String str : split) {
            list.add(str);
        }
        return list;
    }

}
```

#### 2.5.3  中缀表达式转后缀表达式

> ​	大家看到，后缀表达式适合计算式进行运算，但是人却不太容易写出来，尤其是表达式很长的情况下，因此在开发中，我们需要将 中缀表达式转成后缀表达式。
>
> 具体步骤如下:
>
> - 初始化两个栈：运算符栈s1和储存中间结果的栈s2；
>
> - 从左至右扫描中缀表达式；
>
> - 遇到操作数时，将其压s2；
>
> - 遇到运算符时，比较其与s1栈顶运算符的优先级：
>   - 如果s1为空，或栈顶运算符为左括号“(”，则直接将此运算符入栈；
>   - 否则，若优先级比栈顶运算符的高，也将运算符压入s1；
>   - 否则，将s1栈顶的运算符弹出并压入到s2中，再次转到(4-1)与s1中新的栈顶运算符相比较；
> - 遇到括号时：
>   - 如果是左括号“(”，则直接压入s1
>   - 如果是右括号“)”，则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
>
> - 重复步骤2至5，直到表达式的最右边
>
> - 将s1中剩余的运算符依次弹出并压入s2
>
> - 依次弹出s2中的元素并输出，结果的逆序即为中缀表达式对应的后缀表达式。

```java
package infixtosuffix;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

/**
 * @author ${张世林}
 * @date 2019/06/15
 * 作用：将中缀表达式转成后缀表达式
 */
public class InfixToSuffix {

    public static void main(String[] args) {
        //将中缀表达式添加到集合中
        String infixExpression = "(3+14)*5-6";
        List<String> list = toList(infixExpression);
        List<String> calculate = calculate(list);
        System.out.println(calculate);
    }

    /**
     * 将中缀表达式数据填入到集合中
     *
     * @param s
     * @return
     */
    public static List<String> toList(String s) {
        List<String> list = new ArrayList<String>();
        String str;
        char c;
        int i = 0;
        do {
            if ((c = s.charAt(i)) < 48 || (c = s.charAt(i)) > 57) {
                list.add("" + c);
                i++;
            } else {
                str = "";
                while (i < s.length() && (c = s.charAt(i)) >= 48 && (c = s.charAt(i)) <= 57) {
                    str += c;
                    i++;
                }
                list.add(str);
            }
        } while (i < s.length());
        return list;
    }

    /**
     * 将 中缀表达式 切换为 后缀表达式
     * @param list
     * @return
     */
    public static List<String> calculate(List<String> list) {
        Stack<String> operStack = new Stack<String>();
        List<String> list2 = new ArrayList<String>();
        //扫描遍历集合
        for (String item : list) {
            //如果这个是一个数
            if (item.matches("\\d+")) {
                list2.add(item);
            } else if (item.equals("(")) {
                operStack.push(item);
            } else if (item.equals(")")) {
                while (!operStack.peek().equals("(")) {
                    list2.add(operStack.pop());
                }
                //将 ( 进行删除
                operStack.pop();
            } else {
                //当item符号的优先级小于等于operStack栈顶符号的优先级
                while (operStack.size() != 0 && Operation.getValue(operStack.peek()) >= Operation.getValue(item)) {
                    list2.add(operStack.pop());
                }
                //需要将item入栈
                operStack.push(item);
            }
        }
        //将剩下的数据放到集合中
        while (operStack.size() != 0) {
            list2.add(operStack.pop());
        }
        return list2;
    }

}

/**
 * 返回运算符对饮的优先级
 */
class Operation {
    private static int ADD = 1;
    private static int SUB = 1;
    private static int MUL = 2;
    private static int DIV = 2;

    public static int getValue(String opertaion) {
        int result = 0;
        switch (opertaion) {
            case "+":
                result = ADD;
                break;
            case "-":
                result = SUB;
                break;
            case "*":
                result = MUL;
                break;
            case "/":
                result = DIV;
                break;
            default:
                System.out.println("不存在该运算符");
                break;
        }
        return result;
    }

}

```

#### 2.5.4 完整版逆波兰表达式

```java
package reversepolishcalcase;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Stack;
import java.util.regex.Pattern;

public class ReversePolishCalCase {

    /**
     * 匹配 + - * / ( ) 运算符
     */
    static final String SYMBOL = "\\+|-|\\*|/|\\(|\\)";

    static final String LEFT = "(";
    static final String RIGHT = ")";
    static final String ADD = "+";
    static final String MINUS = "-";
    static final String TIMES = "*";
    static final String DIVISION = "/";

    /**
     * 加減 + -
     */
    static final int LEVEL_01 = 1;
    /**
     * 乘除 * /
     */
    static final int LEVEL_02 = 2;

    /**
     * 括号
     */
    static final int LEVEL_HIGH = Integer.MAX_VALUE;


    static Stack<String> stack = new Stack<>();
    static List<String> data = Collections.synchronizedList(new ArrayList<String>());

    /**
     * 去除所有空白符
     *
     * @param s
     * @return
     */
    public static String replaceAllBlank(String s) {
        // \\s+ 匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]
        return s.replaceAll("\\s+", "");
    }

    /**
     * 判断是不是数字 int double long float
     *
     * @param s
     * @return
     */
    public static boolean isNumber(String s) {
        Pattern pattern = Pattern.compile("^[-\\+]?[.\\d]*$");
        return pattern.matcher(s).matches();
    }

    /**
     * 判断是不是运算符
     *
     * @param s
     * @return
     */
    public static boolean isSymbol(String s) {
        return s.matches(SYMBOL);
    }

    /**
     * 匹配运算等级
     *
     * @param s
     * @return
     */
    public static int calcLevel(String s) {
        if ("+".equals(s) || "-".equals(s)) {
            return LEVEL_01;
        } else if ("*".equals(s) || "/".equals(s)) {
            return LEVEL_02;
        }
        return LEVEL_HIGH;
    }

    /**
     * 匹配
     *
     * @param s
     * @throws Exception
     */
    public static List<String> doMatch(String s) throws Exception {
        if (s == null || "".equals(s.trim())) {
            throw new RuntimeException("data is empty");
        }
        if (!isNumber(s.charAt(0) + "")) {
            throw new RuntimeException("data illeagle,start not with a number");
        }

        s = replaceAllBlank(s);

        String each;
        int start = 0;

        for (int i = 0; i < s.length(); i++) {
            if (isSymbol(s.charAt(i) + "")) {
                each = s.charAt(i) + "";
                //栈为空，(操作符，或者 操作符优先级大于栈顶优先级 && 操作符优先级不是( )的优先级 及是 ) 不能直接入栈
                if (stack.isEmpty() || LEFT.equals(each)
                        || ((calcLevel(each) > calcLevel(stack.peek())) && calcLevel(each) < LEVEL_HIGH)) {
                    stack.push(each);
                } else if (!stack.isEmpty() && calcLevel(each) <= calcLevel(stack.peek())) {
                    //栈非空，操作符优先级小于等于栈顶优先级时出栈入列，直到栈为空，或者遇到了(，最后操作符入栈
                    while (!stack.isEmpty() && calcLevel(each) <= calcLevel(stack.peek())) {
                        if (calcLevel(stack.peek()) == LEVEL_HIGH) {
                            break;
                        }
                        data.add(stack.pop());
                    }
                    stack.push(each);
                } else if (RIGHT.equals(each)) {
                    // ) 操作符，依次出栈入列直到空栈或者遇到了第一个)操作符，此时)出栈
                    while (!stack.isEmpty() && LEVEL_HIGH >= calcLevel(stack.peek())) {
                        if (LEVEL_HIGH == calcLevel(stack.peek())) {
                            stack.pop();
                            break;
                        }
                        data.add(stack.pop());
                    }
                }
                start = i;    //前一个运算符的位置
            } else if (i == s.length() - 1 || isSymbol(s.charAt(i + 1) + "")) {
                each = start == 0 ? s.substring(start, i + 1) : s.substring(start + 1, i + 1);
                if (isNumber(each)) {
                    data.add(each);
                    continue;
                }
                throw new RuntimeException("data not match number");
            }
        }
        //如果栈里还有元素，此时元素需要依次出栈入列，可以想象栈里剩下栈顶为/，栈底为+，应该依次出栈入列，可以直接翻转整个stack 添加到队列
        Collections.reverse(stack);
        data.addAll(new ArrayList<>(stack));

        System.out.println(data);
        return data;
    }

    /**
     * 算出结果
     *
     * @param list
     * @return
     */
    public static Double doCalc(List<String> list) {
        Double d = 0d;
        if (list == null || list.isEmpty()) {
            return null;
        }
        if (list.size() == 1) {
            System.out.println(list);
            d = Double.valueOf(list.get(0));
            return d;
        }
        ArrayList<String> list1 = new ArrayList<>();
        for (int i = 0; i < list.size(); i++) {
            list1.add(list.get(i));
            if (isSymbol(list.get(i))) {
                Double d1 = doTheMath(list.get(i - 2), list.get(i - 1), list.get(i));
                list1.remove(i);
                list1.remove(i - 1);
                list1.set(i - 2, d1 + "");
                list1.addAll(list.subList(i + 1, list.size()));
                break;
            }
        }
        doCalc(list1);
        return d;
    }

    /**
     * 运算
     *
     * @param s1
     * @param s2
     * @param symbol
     * @return
     */
    public static Double doTheMath(String s1, String s2, String symbol) {
        Double result;
        switch (symbol) {
            case ADD:
                result = Double.valueOf(s1) + Double.valueOf(s2);
                break;
            case MINUS:
                result = Double.valueOf(s1) - Double.valueOf(s2);
                break;
            case TIMES:
                result = Double.valueOf(s1) * Double.valueOf(s2);
                break;
            case DIVISION:
                result = Double.valueOf(s1) / Double.valueOf(s2);
                break;
            default:
                result = null;
        }
        return result;

    }

    public static void main(String[] args) {
        //String math = "9+(3-1)*3+10/2";
        String math = "12.8 + (2 - 3.55)*4+10/5.0";
        try {
            doCalc(doMatch(math));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

```



## 3 递归

### 3.1 基本介绍

> - 递归的思想就是在方法里面调用方 法本身。
>
> - 在使用递归策略时，必须有一个明确的递归结束条件，称为递归出口。
>
> - 递归调用的过程中系统每一层的返回点，局部变量等开辟了栈来存储。递归的次数过多的话，容易造成StackOverFlowerError（栈空间溢出）。
>
> - 执行一个方法时，就创建一个新的受保护的独立空间(栈空间)
>
> - 方法的局部变量是独立的，不会相互影响, 比如n变量
>
> - 如果方法中使用的是引用类型变量(比如数组)，就会共享该引用类型的数据.
>
> - 递归必须向退出递归的条件逼近，否则就是无限递归,出现StackOverflowError，死龟了:)
>
> - 当一个方法执行完毕，或者遇到return，就会返回，遵守谁调用，就将结果返回给谁，同时当方法执行完毕或者返回时，该方法也就执行完毕。
>
> **注意：**   
> 在做递归算法的时候，一定要把我出口，也就是做递归算法必须要有一个明确的递归结束条件，如果符合了这个条件，就结束递归的运行。

### 3.2 递归-迷宫问题

> ​	迷宫问题主要是设计迷宫策略，确定迷宫的执行路径顺序：例如可以设置为 下右上左 的顺序进行判断，如果能走通，就走通，如果无法走通，则进行其他操作。
>
> ![1560844201122](assets/1560844201122.png)

```java
package recursion;

/**
 * @author ${张世林}
 * @date 2019/06/17
 * 作用：迷宫
 */
public class MiGong {

    /**
     * 创建墙壁：使用数字1表示
     *
     * @param args
     */
    public static void main(String[] args) {
        //创建一个二维数组，用于表示迷宫地图
        int[][] map = new int[8][7];
        //创建地图的墙
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        for (int i = 0; i < 8; i++) {
            map[i][0] = 1;
            map[i][6] = 1;
        }
        map[3][1] = 1;
        map[3][2] = 1;

        setWay(map, 1, 1);
        System.out.println("---------------");
        for (int i = 0; i < map.length; i++) {
            for (int j = 0; j < map[i].length; j++) {
                System.out.printf("%d  ", map[i][j]);
            }
            System.out.println();
        }

    }

    /**
     * map表示地图，（i，j）表示从地图的该位置出发
     * 如果小球能够到map的（6,5）位置，则表示找到了通路
     * 约定：当地图的（i，j）为0，则改点未被走过，
     * 当为1的时候，表示是墙，无法通过
     * 当为2的时候，表示可以通过
     * 当为3的时候，表示该位置已经走过，但是无法走通
     * 策略：下右上左
     *
     * @param map 表示地图
     * @param i   从什么位置开始找
     * @param j   从什么位置开始找
     * @return 如果找到了就返回true，否则返回false
     */
    public static boolean setWay(int[][] map, int i, int j) {
        if (map[6][5] == 2) { 
            return true;
        } else {
            if (map[i][j] == 0) { 
                map[i][j] = 2;
                if (setWay(map, i + 1, j)) {
                    //向下走
                    return true;
                } else if (setWay(map, i, j + 1)) {
                    //向右走
                    return true;
                } else if (setWay(map, i - 1, j)) {
                    //向上走
                    return true;
                } else if (setWay(map, i, j - 1)) {
                    //向左走
                    return true;
                } else {
                    map[i][j] = 3;
                    return false;
                }
            } else {
                //如果map(i)(j)!=0
                //则值 1,2,3
                return false;
            }
        }
    }


    /**
     * 策略：执行的是 上右下左  策略
     *
     * @param map
     * @param i
     * @param j
     * @return
     */
    public static boolean setWay3(int[][] map, int i, int j) {
        if (map[6][5] == 2) {
            return true;
        } else {
            if (map[i][j] == 0) {
                map[i][j] = 2;
                if (setWay3(map, i - 1, j)) {
                    return true;
                } else if (setWay3(map, i, j + 1)) {
                    return true;
                } else if (setWay3(map, i + 1, j)) {
                    return true;
                } else if (setWay3(map, i, j - 1)) {
                    return true;
                } else {// 走不通
                    map[i][j] = 3;
                    return false;
                }
            } else {
                return false;
            }
        }
    }

}

```

### 3.3 八皇后问题

> **问题描述：**
>
> ​	国际西洋棋棋手马克斯·贝瑟尔于1848年提出：在8×8格的国际象棋上摆放八个皇后，使其不能互相攻击，即：任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法。
>
> **实现过程：**
>
> 1. 第一个皇后先放第一行第一列
>
> 2. 第二个皇后放在第二行第一列、然后判断是否OK， 如果不OK，继续放在第二列、第三列、依次把所有列都放完，找到一个合适
>
> 3. 继续第三个皇后，还是第一列、第二列……直到第8个皇后也能放在一个不冲突的位置，算是找到了一个正确解
>
> 4. 当得到一个正确解时，在栈回退到上一个栈时，就会开始回溯，即将第一个皇后，放到第一列的所有正确解，全部得到.
>
> 5. 然后回头继续第一个皇后放第二列，后面继续循环执行 1,2,3,4的步骤即可。

```java
package queue8;

/**
 * @author ${张世林}
 * @date 2019/06/18
 * 作用：八皇后问题
 * 在8×8格的国际象棋上摆放八个皇后，使其不能互相攻击，
 * 即：任意两个皇后都不能处于同一行、同一列或同一斜线上，问有多少种摆法。
 */
public class Queue8 {

    /**
     * 定义有多少个皇后
     */
    int max = 8;
    /**
     * 定义一个一维数组，用于保存一次8皇后位置的各个点的位置
     */
    int[] array = new int[max];

    private static int count = 0;
    private static int judgeCount = 0;

    public static void main(String[] args) {
        Queue8 queue8 = new Queue8();
        queue8.check(0);
        System.out.println("count = "+ count);
        System.out.println("judgeCount = "+ judgeCount);
    }

    /**
     * 编写一个方法，放置第n个皇后
     *
     * @param n 放置第n个皇后
     */
    private void check(int n) {
        //因为是从0开始计数的，所以当n为max，则表示之前的数据已经放置完成
        if (n == max) {
            print();
            return;
        }
        //依次放入皇后，并判断是否冲突
        for (int i = 0; i < max; i++) {
            judgeCount++;
            //先把当前皇后放置到该行的第一列
            array[n] = i;
            //放置在该位置是否符合要求
            if (judge(n)) {
                //接着放n+1个皇后
                check(n + 1);
            }
            //如果冲突了以后，将在本行中进行后移

        }
    }

    /**
     * 当摆放了第n个皇后的时候，检测该皇后是否与前面已经摆放的皇后冲突
     * @param n 第n个皇后
     */
    private boolean judge(int n) {
        for (int i = 0; i < n; i++) {
            //验证是否在同一列 || 验证是否在同一条斜线上面
            if (array[i] == array[n] || Math.abs(n - i) == Math.abs(array[n] - array[i])) {
                return false;
            }
        }
        return true;
    }

    /**
     * 将皇后摆放的位置进行输出
     */
    private void print() {
        count++;
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
        System.out.println();
    }


}

```

## 4 排序算法

### 4.1 基本介绍

> ​	排序是将一组数据，依指定的顺序进行排列的过程。排序主要分为：
>
> - **内部排序**：指将需要处理的所有数据都加载到内部存储器中进行排序（也就是将数据加载到内存中）。
> - **外部排序**：当数据量过大，无法将数据全部加载到内存中，所以必须借助外部存储来实现排序。
>
> ![1560920352766](assets/1560920352766.png)

### 4.2 算法复杂度

> - 事后统计方法：对算法的运行性能进行评判，需要在同一台机器的相同状态下运行，比较算法速度。
> - 事前估算方法：衡量一个算法的优劣，是根据算法的时间复杂度来进行判断。

#### 4.2.1 算法时间复杂度

##### 4.2.1.1 时间频度

> 基本介绍 ：一个算法话费的时间与算法种语句的执行次数成正比，哪个算法种语句执行次数多，它话费的时间就越多。**一个算法中的语句执行次数称为语句频度或者时间频度**。

##### 4.2.1.2 时间复杂度

> - 一般情况下，算法中的基本操作语句的重复执行次数是问题规模n的某个函数，用T(n)表示，若有某个辅助函数f(n)，使得当n趋近于无穷大时，T(n) / f(n) 的极限值为不等于零的常数，则称f(n)是T(n)的同数量级函数。记作 T(n)=Ｏ( f(n) )，称Ｏ( f(n) )  为算法的渐进时间复杂度，简称时间复杂度。
>
> - T(n) 不同，但时间复杂度可能相同。 如：T(n)=n²+7n+6 与 T(n)=3n²+2n+2 它们的T(n) 不同，但时间复杂度相同，都为O(n²)。
>
> - 计算时间复杂度的方法：
>
>   - 用常数1代替运行时间中的所有加法常数  T(n)=n²+7n+6  => T(n)=n²+7n+1
>
>   - 修改后的运行次数函数中，只保留最高阶项  T(n)=n²+7n+1 => T(n) = n²
>
>   - 去除最高阶项的系数 T(n) = n² => T(n) = n² => O(n²)

> 常见的时间复杂度：
>
> - 常数阶`O(1)`：无论代码执行了多少行，只要是没有循环等复杂结构，那这个代码的时间复杂度就都是O(1)
>
> - 对数阶`O(log2n)`
>
> - 线性阶`O(n)`：for循环里面的代码会执行n遍，因此它消耗的时间是随着n的变化而变化的，因此这类代码都可以用O(n)来表示它的时间复杂度
>
> - 线性对数阶`O(nlogn)`：线性对数阶O(nlogN) 其实非常容易理解，将时间复杂度为O(logn)的代码循环N遍的话，那么它的时间复杂度就是n *O(logN)，也就是了O(nlogN)
>
> - 平方阶`O(n^2)`
>
> - 立方阶`O(n^3)`
>
> - k次方阶`O(n^k)`
>
> - 指数阶`O(2^n)`
>
> 说明：
>
> ​	常见的算法时间复杂度由小到大依次为：**Ο(1)＜O(log2n) < O(n) < O(nlog2n) < O(n^2) < O(n^3) < O(n^k) < O(2^n)** ，随着问题规模n的不断增大，上述时间复杂度不断增大，算法的执行效率越低。

> 算法的时间复杂度通常采用的是最坏时间复杂度。这样子能够在任何输入实例上运行时间的界限，保证算法的运行时间不会超出该时间。
>
> | 排序法    | 平均时间 | 最差时间         | 稳定度 | 额外空间 | 备注                 |
> | --------- | -------- | ---------------- | ------ | -------- | -------------------- |
> | 冒泡排序  | O(n^2)   | O(n^2)           | 稳定   | O(1)     | n小时比较好          |
> | 交换排序  | O(n^2)   | O(n^2)           | 不稳定 | O(1)     | n小时比较好          |
> | 选择排序  | O(n^2)   | O(n^2)           | 不稳定 | O(1)     | n小时比较好          |
> | 插入排序  | O(n^2)   | O(n^2)           | 稳定   | O(1)     | 大部分已经排序时较好 |
> | 基数排序  | O(logRB) | O(logRB)         | 稳定   | O(n)     | B是真数(0-9)         |
> | Shell排序 | O(nlogn) | O(n^s) 1 < s < 2 | 不稳定 | O(1)     | s是所选分组          |
> | 快速排序  | O(nlogn) | O(n^2)           | 不稳定 | O(nlogn) | n较大时较好          |
> | 归并排序  | O(nlogn) | O(nlogn)         | 稳定   | O(1)     | n较大时较好          |
> | 堆排序    | O(nlogn) | O(nlogn)         | 不稳定 | O(1)     | n较大时较好          |
>
> ![1561371966613](assets/1561371966613.png)
>
> - **n**: 数据规模
> - **k**: “桶”的个数
> - **In-place**:    不占用额外内存
>
> - **Out-place**: 占用额外内存

#### 4.2.2 算法空间复杂度

> - 类似于时间复杂度的讨论，一个算法的空间复杂度(Space Complexity)定义为该算法所耗费的存储空间，它也是问题规模n的函数。
>
> - 空间复杂度(Space Complexity)是对一个算法在运行过程中临时占用存储空间大小的量度。有的算法需要占用的临时工作单元数与解决问题的规模n有关，它随着n的增大而增大，当n较大时，将占用较多的存储单元，例如快速排序和归并排序算法就属于这种情况
>
> - 在做算法分析时，**主要讨论的是时间复杂度**。从用户使用体验上看，更看重的程序执行的速度。一些缓存产品(redis, memcache)和算法(基数排序)本质就是用空间换时间.

### 4.3 冒泡排序

> **1）排序思想：**
>
> 让前面的数与后面的数相比较，进行多轮比较，如果根据需求来进行位置的调换操作。
>
> **2）实现流程：**
>
> ![image](https://note.youdao.com/yws/api/personal/file/683DC2BF6F1E41D180D780983BD5B8FD?method=download&shareKey=aa0b40df61c5d4d326b72dd452a41782)
>
> **3）基本概念：**
>
> ​	一共会经过数组长度n的（n-1）次；每一轮比较就会确定一个数的位置；每一轮比较的次数再逐渐的减少（第一次是比较n-1次，第二次就是比较到n-2次，一直到最后一次的比较即可）。
>
> **4）优化策略：**
>
> ​	在进行冒泡排序的过程中，如果发现有一轮未进行数据的交换操作，则表明该集合中的元素数据已经有序。所以可以设置flag用于标识该轮是否交换了数据，如果未进行数据的交换，则break循环。

```java
public class BubblingSort {

    public static void main(String[] args) {
        int[] arr = {4,2,7,5,3,1,9,6};
        int[] ints = bubblingSort(arr);
        for (int anInt : ints) {
            System.out.println(anInt);
        }
    }

    /**
     * 冒泡排序：
     * @param arr
     * @return
     */
    private static int[] bubblingSort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            int temp = 0;
            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
        return arr;
    }
}
```

### 4.4 直接选择排序

> **1）排序思想：**
>
> ​	选择排序（selectsorting）也是一种简单的排序方法。它的**基本思想**是：
>
> - 第一次从arr[0]~arr[n-1]中选取最小值，与arr[0]交换，
> - 第二次从arr[1]~arr[n-1]中选取最小值，与arr[1]交换，
> - 第三次从arr[2]~arr[n-1]中选取最小值，与arr[2]交换，
> - …，
> - 第i次从arr[i-1]~arr[n-1]中选取最小值，与arr[i-1]交换，
> - …,
> - 第n-1次从arr[n-2]~arr[n-1]中选取最小值，与arr[n-2]交换，
> - 总共通过n-1次，得到一个按排序码从小到大排列的有序序列。
>
> **`选择排序效率比冒泡排序效率更高。`**

```java
public class SelectSort {

    public static void main(String[] args) {
        int[] arr = {3,1,5,4,7,9,0};
        int[] ints = selectSort(arr);
        for (int anInt : ints) {
            System.out.println(anInt);
        }
    }

    /**
     * 选择排序
     *
     * @param arr
     * @return
     */
    public static int[] selectSort(int[] arr) {
        int min = 0;
        int index = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            min = arr[i];
            index = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < min) {
                    min = arr[j];
                    index = j;
                }
            }
            if (index != i) {
                arr[index] = arr[i];
                arr[i] = min;
            }
        }
        return arr;
    }
}
```

### 4.5 直接插入排序

> **1）排序思想：**
>
> 再要进行排序的数组中，假设前 n-1 个数据是排好序了的，现在需要将第 n 个元素
>
> **2）实现原理：**
>
> 在排序过程中，先将第一个与第二个数进行排序，变成有序序列以后，再将第三个数进行有序插入，一次向下进行插入排序。这样的排序叫做---直接插入排序。
>
> **3）流程图：**
>
> ![1561018047183](assets/1561018047183.png)
>
> ![image](https://note.youdao.com/yws/api/personal/file/DFC6045FD30F4BFDBA4D32FDAF4507C4?method=download&shareKey=dfbd25a00f11d714a8337cd4cd84a789)

```java
package insert;

import java.util.Arrays;

/**
 * @author ${张世林}
 * @date 2019/06/20
 * 作用：直接插入排序
 */
public class InsertSort {

    public static void main(String[] args) {
        int[] arr = {5,3,7,2,8,1,0,6};
        int[] ints = insertSort(arr);
        for (int anInt : ints) {
            System.out.println(anInt);
        }
        System.out.printf(Arrays.toString(ints));
    }

    /**
     * 直接插入排序
     * @param arr
     * @return
     */
    public static int[] insertSort(int[] arr) {
        int temp = 0;
        for (int i = 1; i < arr.length; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[i] < arr[j]) {
                    temp =arr[i];
                    while (i > j) {
                        arr[i] = arr[--i];
                    }
                    arr[j] = temp;
                    break;
                }
            }
        }
        return arr;
    }
    
    /**
     * 方法二：直接使用双层for循环
     * @param arr
     * @return
     */
    public static int[] insertSortEasy(int[] arr) {
        int temp = 0;
        for (int i = 1; i < arr.length; i++) {
            temp = arr[i];
            int j = i;
            for (; j >0 && temp < arr[j - 1];) {
                arr[j] = arr[--j];
            }
            if (j != i) {
                arr[j] = temp;
            }
        }
        return arr;
    }

}

```

### 4.6 希尔排序

> **1）基本思想：**
>
> ​	希尔排序也是一种**插入排序**，它是简单插入排序经过改进之后的一个**更高效的版本**，也称为缩小增量排序。
>
> ​	希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止。也就是说：根据n的数量来进行除2分组，分组以后进行插入排序的操作；重复此过程，直到被分为1组即可。
>
> **2）流程图：**
>
> ![1561027294467](assets/1561027294467.png)

```java
public class ShellSort {

    public static void main(String[] args) {
        int[] arr = {8,9,1,7,2,3,5,4,6,0};
        exchangeShellSort(arr);
        moveShellSort(arr);
    }

    /**
     * 交换式希尔排序
     *	交换式是验证两个数据的大小，如果大小不同，就将两个数据进行交换的操作
     * @param arr
     * @return
     */
    public static int[] exchangeShellSort(int[] arr) {
        int groupNum = arr.length / 2;
        while (groupNum > 0) {
            int temp = 0;
            for (int i = groupNum; i < arr.length; i++) {
                for (int j = i - groupNum; j >= 0; j -= groupNum) {
                    if (arr[j] > arr[j + groupNum]) {
                        temp = arr[j];
                        arr[j] = arr[j + groupNum];
                        arr[j + groupNum] = temp;
                    }
                }
            }
            groupNum = groupNum / 2;
        }
        return arr;
    }


    /**
     * 移位式希尔排序，效率更高
     *	移位式是判断采用插入排序的方式，判断是否该放置在这个位置，如果可以放置在这个位置，才放下去
     * @param arr
     * @return
     */
    public static int[] moveShellSort(int[] arr) {
        int groupNum = arr.length / 2;
        while (groupNum > 0) {
            for (int i = groupNum; i < arr.length; i++) {
                int j = i;
                int temp = arr[j];
                while (j - groupNum >= 0 && temp < arr[j - groupNum]) {
                    arr[j] = arr[j - groupNum];
                    j -= groupNum;
                }
            }
            groupNum = groupNum / 2;
        }
        return arr;
    }
}
```

### 4.7 快速排序

> **1）基本思想：**
>
> ​	**快速排序（Quicksort）是对冒泡排序的一种改进。**设置关键字，将比关键字小的数据放在一组，比关键字大的放在另外一组中。通常是设置第一个数据为关键字。使用第一个元素作为第一次的分割对象，然后先进行右边的判断，如果右边的数据大于分割值，则 right对象减1，否则的话就行数据的交换操作。依次递归执行，知道left与right相等以后，停止递归操作，得到需要的结果。
>
> **2）示例图：**
>
> ![1561036476941](assets/1561036476941.png)

代码实现一：

```java
public class QuickSort {

	/**
	 * 进行排序的操作，一直到左边与右边重叠以后，停止递归，执行返回操作
	 * @param arr : 需要排序的数组
	 * @param left ：左边的位置
	 * @param right ： 右边的位置
	 * @return ： 排序完成以后的数组对象
	 */
	public static int[] quickSort(int[] arr,Integer left,int right) {
		if (left < right) {
			System.out.println(left.hashCode());
			int position = position(arr, left, right);
			quickSort(arr,left,position-1);
			quickSort(arr,position+1,right);
		}
		return arr;
	}

	/**
	 * 返回分割点，这里的作用主要是得到这个对象分割点以后，来进行排序的操作
	 * @param arr ：数组
	 * @param left ：左边的起点
	 * @param right ：右边的位置
	 * @return
	 */
	public static int position(int[] arr, Integer left, int right) {
		int midea = arr[left];

		while (left < right) {
			while (left < right && arr[right] >= midea) {
				right--;
			}
			swap(arr,left,right);

			while (left < right && arr[left] < midea) {
				left++;
			}
			swap(arr,left,right);
		}
		System.out.println("position中的left："+left.hashCode());
		return left;
	}

	/**
	 * 交换数据，将对应的两个数据进行交换
	 * @param arr
	 * @param left
	 * @param right
	 */
	public static void swap(int[] arr, int left, int right) {
		int temp = arr[right];
		arr[right] = arr[left];
		arr[left] = temp;
	}

	public static void main(String[] args) {
		int[] arr = new int[] { 12, 13, 1, 3, 9, 5, 7, 6, 11 };
		int[] ints = quickSort(arr, 0,arr.length-1);
		for (int anInt : ints) {
			System.out.println(anInt);
		}
	}

}
```

代码实现二：

```java
package quick;

import java.util.Arrays;

/**
 * @author ${张世林}
 * @date 2019/06/20
 * 作用：快速排序
 */
public class QuickSort {

    public static void main(String[] args) {
        int[] arr = {-9, 78, 0, 23, -567, 70};
        quickSort( 0, arr.length - 1,arr);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 返回分割点，用于得到分割点以后，来进行排序的操作
     *
     * @param arr   数组
     * @param left  左边的位置
     * @param right 右边的位置
     * @return
     */
    public static void quickSort(int left, int right, int[] arr) {
        int l = left;
        int r = right;
        int pivot = arr[(left + right) / 2];
        int temp = 0;
        //while循环是为了让比 pivot 小的值放到左边
        //              让比 pivot 大的值放到右边
        while (l < r) {
            //在pivot的左边一直找，直到找到一个值大于等于pivot
            while (arr[l] < pivot) {
                l += 1;
            }
            //在pivot的右边一直找，直到找到一个值小于等于pivot
            while (arr[r] > pivot) {
                r -= 1;
            }
            //如果 left >= right表明pivot两边的值，已经按照pivot来进行分界成功
            if (l >= r) {
                break;
            }
            //如果条件未满足，则进行交换的操作
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;
            //如果交换完成以后，发现pivot左边的值与pivot相等，则right--
            if (arr[l] == pivot) {
                r -= 1;
            }
            //如果交换完成以后，发现pivot右边的值与pivot相等，则left++
            if (arr[r] == pivot) {
                l += 1;
            }
        }
        if (l == r) {
            l += 1;
            r -= 1;
        }
        if (left < r) {
            quickSort(left, r, arr);
        }
        if (right > l) {
            quickSort(l, right, arr);
        }
    }


    /**
     * 交换数据，将对应的两个数据进行交换
     *
     * @param arr
     * @param left
     * @param right
     */
    public static void swap(int[] arr, int left, int right) {
        int temp = arr[right];
        arr[right] = arr[left];
        arr[left] = temp;
    }

}

```

### 4.8 归并排序

> **1）基本思想**
>
> - 归并排序（Merge Sort）与快速排序思想类似：将待排序数据分成两部分，继续将两个子部分进行递归的归并排序；然后将已经有序的两个子部分进行合并，最终完成排序。
> - 其时间复杂度与快速排序均为O(nlogn)，但是归并排序除了递归调用间接使用了辅助空间栈，还需要额外的O(n)空间进行临时存储。从此角度归并排序略逊于快速排序.
> - 但是**归并排序是一种稳定的排序算法**，快速排序则不然。
> - 所谓稳定排序，表示对于具有相同值的多个元素，其间的先后顺序保持不变。对于基本数据类型而言，一个排序算法是否稳定，影响很小，但是对于结构体数组，
> - 稳定排序就十分重要。例如对于student结构体按照关键字score进行非降序排序：
>
> **2）图解：**
>
> 首先需要将对应的数据拆分，拆分完成以后再进行合并的操作。
>
> ![1561354010423](assets/1561354010423.png)
>
> 最后一次合并的的详细过程：
>
> ![1561354046617](assets/1561354046617.png)

代码实现：

```java
package merge;

import java.util.Arrays;

/**
 * @author ${张世林}
 * @date 2019/06/24
 * 作用：归并排序
 */
public class MergeSort {

    public static void main(String[] args) {
        int[] arr = {8, 4, 5, 7, 1, 3, 6, 2};
        int[] temp = new int[arr.length];
        mergeSort(arr, 0, arr.length - 1, temp);
        System.out.println(Arrays.toString(arr));
    }

    /**
     * 归并排序---分解的过程
     *
     * @param arr
     * @param left
     * @param right
     * @param temp
     */
    private static void mergeSort(int[] arr, int left, int right, int[] temp) {
        if (left < right) {
            //得到中间值
            int mid = (left + right) / 2;
            //向左递归进行分解
            mergeSort(arr, left, mid, temp);
            //向右递归进行分解
            mergeSort(arr, mid + 1, right, temp);
            //到合并的时候
            merge(arr, left, mid, right, temp);
        }
    }

    /**
     * 归并排序---合并的过程
     *
     * @param arr   排序的原始数组
     * @param left  左边有序序列的初始索引
     * @param mid   中间索引
     * @param right 右边索引
     * @param temp  中转数组
     */
    private static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        //初始化 i，表示的是左边有序序列的初始索引位置
        int i = left;
        //初始化 j，表示的是右边有序序列的初始索引位置
        int j = mid + 1;
        //指向temp数组的当前索引
        int t = 0;

        //第一步：将左右两边的有序数据列表按照规则填充到temp数组中，直到有一边数据处理完毕。
        while (i <= mid && j <= right) {
            //如果左边的当前数据小于右边的当前数据，即将左边的当前数据拷贝到temp数组中
            if (arr[i] < arr[j]) {
                temp[t] = arr[i];
                t++;
                i++;
            } else {
                temp[t] = arr[j];
                j++;
                t++;
            }
        }

        //第二步：将有剩余的一边的数据依次填充到temp中
        while (i <= mid) {
            //说明左边的有序序列还有剩余的元素
            temp[t] = arr[i];
            i++;
            t++;
        }
        while (j <= right) {
            //说明右边的有序序列还有剩余的元素
            temp[t] = arr[j];
            j++;
            t++;
        }

        //第三步：将temp数组中的数据拷贝到原始数组中
        t = 0;
        int tempLeft = left;
        while (tempLeft <= right) {
            arr[tempLeft] = temp[t];
            t++;
            tempLeft++;
        }
    }

}

```

### 4.9 基数排序

> **1）基本思想**
>
> - **基数排序（radix sort）**属于 **分配式排序(disruibution sort)**，又称 **桶排序（bucket sort）**，它是通过键值的各个位的值，将要排序的元素分配到各个 **桶** 中，达到排序的作用。
> - 基数排序法属于 **稳定性排序算法**。
> - 基数排序法是 桶排序 的扩展。
> - 实现方式是**将整数按位数切割成不同的数字，然后按每个位数分别比较。**
> - 将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。
>
> **2）图解：**将数组 {53, 3, 542, 748, 14, 214} 使用基数排序, 进行升序排序。
>
> ![1561359147481](assets/1561359147481.png)
>
> ![1561359162703](assets/1561359162703.png)
>
> ![1561359173634](assets/1561359173634.png)

代码实现：

```java
package radix;

import java.util.*;

/**
 * @author ${张世林}
 * @date 2019/06/24
 * 作用：基数排序
 */
public class RadixSort {

    public static void main(String[] args) {
//        int[] arr = {53, 3, 542, 748, 14, 214};
//        radixSort1(arr);

        int[] arr = new int[8000000];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 8000000);
        }
        long before = System.currentTimeMillis();
        radixSortByArray(arr);
        long end = System.currentTimeMillis();
        System.out.println((end - before));

    }


    /**
     * 基数排序：典型的空间换时间的算法
     *  使用数组的方式
     * @param arr
     */
    private static void radixSortByArray(int[] arr) {
        int max = 0;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        //得到最大的一个数的长度
        int length = String.valueOf(max).length();
        //创建一个二维数组，来作为桶
        int[][] bucket = new int[10][arr.length];
        //创建一个一维数组，拥于保存对应的二维数组每个桶的长度
        int[] indexArr = new int[10];

        //循环遍历最长的那个数据的长度
        for (int i = 0, div = 1; i < length; i++, div *= 10) {
            //将数据存放到桶中
            for (int j = 0; j < arr.length; j++) {
                int val = arr[j] / div % 10;
                bucket[val][indexArr[val]] = arr[j];
                indexArr[val] += 1;
            }

            //将数据从桶中取出
            int index = 0;
            for (int j = 0; j < indexArr.length; j++) {
                if (indexArr[j] != 0) {
                    for (int k = 0; k < indexArr[j]; k++) {
                        arr[index++] = bucket[j][k];
                    }
                }
                //将数据清空
                indexArr[j] = 0;
            }
        }
    }

    /**
     * 基数排序，使用集合的方式
     * @param arr
     */
    private static void radixSort1(int[] arr) {
        //根据最大数据来确定循环几次，排序
        //得到数组中最大的一个数据，先假设最大的数是第一个数
        int max = 0;
        for (int i = 0; i < arr.length; i++) {
            if (max < arr[i]) {
                max = arr[i];
            }
        }
        int length = String.valueOf(max).length();

        for (int i = 0, div = 1; i < length; i++, div *= 10) {
            Map<Integer, List<Integer>> map = new HashMap<Integer, List<Integer>>(10);
            for (int j = 0; j < arr.length; j++) {
                //取出每一个元素的个位数
                Integer integer = Integer.valueOf(arr[j] / div % 10);
                if (!map.containsKey(integer)) {
                    List<Integer> list = new ArrayList<Integer>();
                    list.add(arr[j]);
                    map.put(integer, list);
                } else {
                    List<Integer> list = map.get(integer);
                    list.add(arr[j]);
                }
            }

            int index = 0;
            for (Integer integer : map.keySet()) {
                List<Integer> list = map.get(integer);
                for (int p = 0; p < list.size(); p++) {
                    arr[index++] = list.get(p);
                }
            }
        }
    }
}
```















