# 数据结构

## 一、链表

定义：

> 链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的。
>
> 链表是一种**递归**的数据结构，它或者为空(nil)，或者是指向一个节点（node）的引用，该节点含有一个泛型的元素和一个指向另一条链表的引用。

链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。

每个结点包括两个部分：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。

### 1.链表的基本实现

#### 1.1单链表

单向，一直往下一个结点进行寻找，直到为 `nil`。

```go
//定义链表结点结构体
type LinkNode struct {
   Val int
   Next *LinkNode
}
```

初始化函数及验证

```go
//对结点进行初始化的函数，也可以使用new()申请
func InitLinkNode()*LinkNode{
   return &LinkNode{}
}

func main(){
   node1:=InitLinkNode()
   node1.Val=11

   node2:=InitLinkNode()
   node1.Next=node2
   node2.Val=22

   node3:=InitLinkNode()
   node2.Next=node3
   node3.Val=33

   thisNode:=InitLinkNode()
   thisNode.Next=node1
   thisNode.Val=0
    
   for thisNode!=nil{							//循环遍历节点
      fmt.Printf("当前节点值：%d\n",thisNode.Val)
      thisNode=thisNode.Next
   }
}
```

输出如下：

```go
当前节点值：0
当前节点值：11
当前节点值：22
当前节点值：33
```

#### 1.2双向链表

双向链表，每个结点及可以找到它之前的一个节点，也可以找到后一个节点。

```
//定义链表结点结构体
type LinkNode struct {
   Val int
   Pre,Next *LinkNode
}
```

LRU缓存结构就是通过双向链表+哈希表的形式实现的。

[面试题 16.25. LRU 缓存 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/lru-cache-lcci/)

#### 1.3循环链表

循环链表，就是它一直往下找数据节点，最后回到了自己那个节点，形成了一个回路。

```go
//定义链表结点结构体
type CircleNode struct {
   Val int
   Next *CircleNode
}
```

初始化一个空的循环链表

```go
// 初始化空的循环链表，指向自己，因为是循环的
func Init(n int) *CircleNode {
	head:=new(CircleNode)
	head.Val=1
	head.Next=nil
	circle:=head
	for i:=2;i<=n;i++{
		node:=new(CircleNode)
		node.Val=i
		node.Next=nil
		circle.Next=node
		circle=circle.Next
	}
	circle.Next=head
	return head
}
```

### 2.LeetCode链表经典题

[206. 反转链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/reverse-linked-list/)

[21. 合并两个有序链表 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

[83. 删除排序链表中的重复元素 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

## 二、队列

### 1.介绍

队列(queue)， 是一种先进先出的数据结构。通常用数据或者链表来实现队列。 队列只允许在后端插入（进队），首端删除（出队）。

在日常生活中非常常见:比如去银行排队办理业务. 在计算机中也极其常见, 很多情况下,当有大量任务需要完成时, 就会把任务暂时加入到 任务队列中, 执行一个删除一个,继续执行下一个任务.

特点：

- 先进先出
- 只能从队尾插入数据，从头部删除数据

在Go语言中可以用切片实现队列。

```go
//队列结构体
type Queue struct{
   data []interface{}    //空接口类型的切片
   size int            //队列长度
}

//初始化一个空间为n，元素全为空的队列
func InitQueue(n int)*Queue{
   return &Queue{
      data:make([]interface{},n),
      size: n,
   }
}

//清空或者创建一个空队列
func (myqueue *Queue)Clear(){
   myqueue.data =make([]interface{},0)  //开辟内存
   myqueue.size=0
}

//入队、出队操作
func (myqueue *Queue)EnterQueue(n interface{}){
   myqueue.data=append(myqueue.data,n)
   myqueue.size++
}

func (myqueue *Queue)DeQueue()interface{}{
   if myqueue.size==0{
      return nil
   }
   ret:=myqueue.data[0]
   myqueue.data=myqueue.data[1:]
   myqueue.size--
   return ret
}


func(myqueue *Queue) Size()int   {
   return  myqueue.size //大小
}

func(myqueue *Queue) Front()interface{} {
   if myqueue.Size()==0{ //判断是否为空
      return nil
   }
   return myqueue.data[0]
}

func(myqueue *Queue) End()interface{} {
   if myqueue.Size()==0{ //判断是否为空
      return nil
   }
   return myqueue.data[myqueue.size-1]
}

func(myqueue *Queue) IsEmpty() bool{
   return  myqueue.size==0
}
```

### 2.LeetCode队列题

[232. 用栈实现队列 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

[102. 二叉树的层序遍历 - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

## 三.栈（Stack）

栈(stack)，是一种特殊的线性表，仅能在线性表的一端操作，栈顶允许操作，栈底不允许操作。

栈的特性：后进先出

![img](https://img-blog.csdnimg.cn/20181218091809482)

### 1.栈的数组实现

数组实现的栈结构体。

```go
//栈的数组实现
type ArrayStack struct {
   size int                  //记录当前栈中含有的元素个数
   valStorage []interface{}      //存储每个元素具体值
}
```

`valStorage []interface{}`切片的头部为栈底，尾部为栈顶。

栈的初始化、Push、Pop操作

```go
//初始化栈的函数
func InitStack(n int)*ArrayStack{
   stack:=new(ArrayStack)
   stack.size=0                        //初始化栈中没有元素，size=0
   stack.valStorage=make([]interface{},0,n)   //存储元素值的列表为[]
   return stack
}

//进栈操作Push
func (this *ArrayStack)Push(x interface{}){
   if this.size==cap(this.valStorage){          //判断当前栈内元素个数是否已满
      return
   }
   this.valStorage=append(this.valStorage,x)   //把x放入栈中,即存储切片的最后一个位置
   this.size++
}

func (this *ArrayStack)Pop()(x interface{}){
   if this.size==0{
      return nil
   }                 //栈内为空，返回nil
   n:=len(this.valStorage)
   ans:=this.valStorage[n-1]              //栈不为空，返回存储切片中的最后一个元素
   this.valStorage=this.valStorage[:n-1]     //取最后一个元素后，要将原切片修改，删除最后一个元素
   this.size--
   return ans
}

func main(){
   demo:=InitStack(5)
   for i:=0;i<5;i++{
      demo.Push(i)
   }
   x:=demo.Pop()
   fmt.Println(x)
}
```

### 2.栈的链表实现

链表节点的定义。

```go
//栈的链表实现
type LinkNodeStack struct {
   val interface{}
   next *LinkNodeStack
}
```

链表实现的Push、Pop操作。要注意，头结点不在栈内，头结点一直指向栈顶结点。

```go
//初始化头结点（指向栈顶）
func InitLinkStack()*LinkNodeStack{
   head:=new(LinkNodeStack)
   return head
}

func (head *LinkNodeStack)PushNode(value interface{}){
   newNode:=InitLinkStack()            //构造一个新的节点
   newNode.val=value
   newNode.next=head.next
   head.next=newNode                 //把新节点压入，放在头结点后一位
}

func (head *LinkNodeStack)PopNode()interface{}{
   if head.next==nil{                //判断是否为空
      return nil
   }
   ans:=head.next.val
   tmp:=head.next                   //删除要弹出的节点，并把头节点指向栈顶第二个节点
   head.next=head.next.next
   tmp.next=nil
   return ans
}
```

## 
