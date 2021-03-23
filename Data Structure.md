数据结构：

1. 集合
2. 线性结构
3. 树形结构
4. 图状结构或网状结构

* 逻辑结构：数据元素之间逻辑关系的整体
* 物理结构：数据结构在计算机中的表示（即映像）



通常数据元素在计算机中有两种不同的表示方法：

* 顺序映像：借助数据元素在存储器中的仙姑地位之来表示数据元素之间的逻辑关系，所以顺序存储就是把逻辑上相邻的数据元素存储在物理位置相邻的存储单元中
* 非顺序映像：借助指示数据元素存储地址的指针表示数据元素之间的逻辑关系，所以链式存储就是用一组任意的存储单元存储数据元素

------

# 线性表

## 线性表的类型定义

**线性表**：由n个数据元素（节点）$a_1,a_2,...,a_n$组成的有限序列。当n=0时成为空表，常常将非空的线性表记作：$(a_1,a_2,...a_n)$

* 对非空的线性表，有且仅有一个开始结点$a_1$，它没有直接前趋，而今有一个直接后继$a_2$
* 有且仅有一个终端结点$a_n$，它没有直接后继，而今有一个直接前趋$a_{n-1}$
* 其余的内部节点$a_i(2\leq i\leq n-1)$都有且仅有一个直接前趋$a_{i-1}$和一个直接后继$a_{i+1}$

## 线性表的顺序表示和实现

顺序表：把线性表的结点按逻辑顺序依次存放在一组地址连续的存储单元里

$Loc(a_i)=(i-1)*m+Loc(a_1)=Loc(a_1)-m+i*m$



顺序表的类定义1（采用方式一定义的一维数组）:

```c++
const int MaxSize = 100;
typedef int ElemType;　//ElemType代表数据元素的类型
class SqList_s{  //顺序表类SqList_s
　　private:
　　ElemType elem_array[MaxSize]; //一维数组的容量，MaxSize为可存放的数据元素的最大个数
　　int length；
　　public：
　　　　…//12个基本操作对应的成员函数
};
```

顺序表的类定义2（采用方式二定义的一维数组）：

```c++
const int LISTINCREMENT=10;   
typedef int ElemType;　//ElemType代表数据元素的类型
class SqList_d{　//顺序表类SqList_d
　　private:
　　ElemType *elem; 	//指针elem指向一维数组的第一个元素
　　int length;		//线性表的长度，表中数据元素的实际个数
　　int MaxSize; 
　　public：
　　…//12个基本操作对应的成员函数
};
```

初始化顺序表

```c++
SqList_d::SqList_d(int n){
	elem = new int[n];
    length = 0;
    MaxSize = n;
}
//O(n)
```

销毁顺序表

```c++
SqLsit_d::~SqList_d(){
    delete [] elem;
    length = 0;
    MaxSize = 0;
}
//O(1)
```

顺序插入数据元素

```c++
SqList_d::SqList_insert(int i,int e){// 在第i个位置插入数据元素
	if(i<1||i>length+1) return; //插入位置异常
	if(length>=MaxSize){//顺序表已满,分配增量空间
        int *elem1=new [MaxSize+LISTINCREMENT];
    	for(int i=0;i<length;i++)
            elem1[i]=elem[i];
        delete elem[];
        elem=elem1;
        MaxSize+=LISTINCREMENT;
        int *p=&elem[i-1],*q=&elem[length-1];
        for(;q>=p;q--)
            *(q+1)=*q;
        *p=e;
        length++;
    }
}
//O(n)
```

顺序表删除数据元素

```c++
SqList_d::SqList_delete(int i,int &e){// 在删除第i个数据元素
    if(i<1||i>length) return; //插入位置异常
　 int *p=&elem[i-1],*q=&elem[length-1];
     e=*p; 
     for(;p<q;p++) *p=*(p+1);
     length--;
}
//O(n)
```

顺序表返回数据元素的位置

```c++
int SqList_d::Locatex (int x){// 数据元素定位，若找到，返回该数据元素
                                                    //在表中的位序；末找到，返回0。
     for(int i=0;i<length;i++)
        if(elem[i]==x)  return i+1;
     return 0;
  }	
```

特点：

1. 节省存储空间。由于结点之间的相邻逻辑关系可以用物理位置上的相邻关系表示，因此不需增加额外的存储空间来表示此关系（如链表则需利用指针来表示逻辑相邻关系）
2. 随机存取
3. 插入和删除操作需要移动大量的数据

## 线性表的链式表示和实现

### 线性链表

​		链表是指用一组任意的存储单元来依次存放线性表的结点，这组存储单元即可以是连续的，也可以是不连续的，甚至是零散分布在内存中的任意位置上的。因此，链表中结点的逻辑次序和物理次序不一定相同。为了能正确表示结点间的逻辑关系，在存储每个结点值的同时，还必须存储指示其后继结点的地址（或位置）信息，这个信息称为指针(pointer)或链(link)。这两部分组成了链表中的结点结构：

1. 存储数据元素自身新写的部分，称为数据域
2. 存储与前驱或后继结点的逻辑关系，称为指针域

单链表的类定义

```c++
typedef int ElemType;	  //ElemType代表数据元素的类型
struct Node{
   ElemType data;
   Node *next;      
};
class LinkList{
   private:
   Node *Head;
   public:
       LinkList() ;//构造一个空的线性表L
      ~LinkList();//销毁线性表L
      void CreateList1(int n);//头插法创建具有n个数据元素的线性链表
      void CreateList2(int n);//尾插法创建具有n个数据元素的线性链表
      void ListInsert(int i,int e);//在表中第i个位置插入数据元素
      int ListDelete(int i);//删除表中第i个位置上的数据元素
      int GetElem(int i);//获取第i个数据元素的
      int LocateElem(int e);//在链表中查找是否存在数据元素e
      int ListLength();//计算表长
};
```



插入运算：首先找到$a_{i-1}$的存储位置p，然后生成一个数据域为x的新结点，并令q指针指向该新结点，新结点的指针域指向结点$a_i$。

```c++
void  insertnode(head, x, i)
{
	LNode  * p，*q;
    p=getnode(head，i-1);
    if(p==NULL){
        cout<<“position  error”<<endl;
	return ERROR;
    }
    q=new LNode;
	q–>data=x;
	q–>next=p–>next;
	p–>next=q;
}
```



删除运算“首先找到$a_{i-1}$的存储位置p，然后令$p\to next$指向$a_i$的直接后继结点，最后释放结点$a_i$的空间。

```c++
void  deletelist(head, i)
{
    LNode *p， *r;
    p=getnode(head，i-1);
    if(p= =NULL || p–>next= =NULL)
        return ERROR;
    r=p–>next;
    p–>next=r–>next;
    delete r;
}
```



按序号查找：从链表的头指针出发，顺链域next逐个结点往下搜素，直到搜索到第i个结点为止。

```c++
LNode  * getnode( head ，i)
//在链表head中取第i个数据链表有头结点
{
    p=head;
    j=0;  //计数用
    while(p–>next && j<i){
        p=p–>next;
        j++;
    }
    if (i==j)
        return p;
    else return NULL;
}
```

**静态链表**：用一维数组来实现线性链表，这种用一维数组表示的线性链表，称为静态链表。

---

### 循环链表

循环链表是一种头尾相接的链表。其特点是无需增加存储量，仅对表的链接方式稍作改变，即可使得表处理更加方便灵活。由于循环链表中没有NULL指针，都涉及遍历操作时，其终止条件为判断它们是否等于某一指定指针，如头指针或尾指针等。

---

### 双向链表

**双向链表**：在单链表的每个结点里再增加一个指向其直接前趋的指针域prior，这样就形成的链表中有里那个方向不同的链，称为双向链表。

```c++
struct DuLNode{
    datatype data;
    DuLNode *prior, *next;
};
```



---

### 一元多项式的表示和相加

用线性链表表示。增加头结点，每个结点有coef（系数），exp（指数），next（指针），其中头结点的exp为-1。

相加运算规则：指数相同的系数相加。

步骤：分别对两个链表ha、hb进行扫描，设工作指针pa、pb分别指向两个多项式当前进行比较的结点，q指针指向pa的前趋，初始：q = ha， pa = ha->next， pb = hb->next

* 若pa，pb都不为空，则比较两者指数
* * pa系数 > pb系数：q，pa后移
  * pa系数 = pb系数：将pb所知节点的系数加到pa所指结点的系数上；若和为0，则pa所指结点删除。q，pa，pb调整
  * pa系数 < pb系数：从表hb中复制pb所指节点的coef，exp，，并将其插入到ha表pa所指结点之前
* 若pb不空：hb表中从pb开始的结点插入到ha表尾上

```c++
int comp(int a,int b){
    if(a>b) 
        return -1;
    if(a==b) 
        return 0;
    else 
        return 1;  }

PloyAdd(ha,hb){
    q=ha;
    pa=ha->next;  
    pb=hb->next;
    while(pa!=NULL && pb!=NULL)
        switch(comp(pa->exp,pb->exp)){
                -1:    q=pa; pa=pa->next;
                break;
                0:   pa->coef+=pb->coef;
                if(pa->coef==0) { 
                    q->next=pa->next;
                    delete pa;
                    pa=q;
                }
                else q=pa;
                pa=pa->next;
                pb=pb->next;
                break;
                1:       r=new  Node;
                r->coef=pb->coef;
                r->exp=pb->exp;
                q->next=r;
                r->next=pa;
                q=r;
                pb=pb->next;
                break
        }
    while(pb!=NULL){  
        r=new Node;
        r->coef=pb->coef;
        r->exp=pb->exp;
        q->next=r;
        r->next=pa;
        q=r;
        pb=pb->next; 
    }
}
```

---

# 栈和队列

## 栈

**栈（*stack*）**是限制在表的一端进行插入和删除运算线性表，通常称插入、删除的这一端为**栈顶**，另一端为**栈底**。栈的修改是按后进先出的原则进行的。

### 顺序存储栈

栈的顺序存储结构简称为顺序栈，可用数组来实现顺序栈。因为栈底位置是固定不变的，所以可以将栈底位置设置在数组的两端的任何一个断电；栈顶位置是随着进栈和退栈操作二变化的，故需用一个整形变量**top（栈顶指针）**来指出栈顶。一般使用数组的最后一个元素。

顺序栈的定义和基本实现

```c++
class SqStack{
    private:
    int *base;//栈底指针
    int top;//栈顶
    int stacksize;//栈容量
    public:
    SqStack(int m);//构建一个长度为m的栈
    ~SqStack(){delete [] base;
               top=-1;
               stacksize=0;
              }//栈销毁
    void push(int e);//入栈
    int pop(int &e);//出栈
    int getTop();//获取栈顶元素
    int stackEmpty();//测栈空
    void stackTranverse();//显示栈中元素
};
```

### 栈的链式存储结构及实现

栈的链式存储结构称为栈链，插入和删除操作仅限制在链头位置上进行，栈顶指针就是链表的头指针。

```c++
Struct Node{
    int data;
    Node *next;
};
class LinkStack{
    private:
    Node *top;                      //栈顶指针即链栈的头指针
    public:
    LinkStack(){top =NULL;}         //构造函数，置空链栈
    ~LinkStack();                   //析构函数，释放链栈中各结点的存储空间
    void push(int e);                 //将元素e入栈
    void pop(int &e);                       //将栈顶元素出栈
    int getTop() {
        if(top!=NULL) 
            return top->data;
    }  //取栈顶元素
    int empty(){
        top==NULL? return 1：return 0;
    }   //判断链栈是否为空栈
};
 
LinkStack::~LinkStack(){
   while(top){
      p=top->next;
      delete top;
      top=p;
   }
}

void LinkStack::push(int e){
      s = new Node; 
      if(!s){ 
        cout<<“内存分配失败”;
        return;
       }
    s->data =e;
    s->next = top;
    top = s;
 }

void LinkStack::pop(int &e){
     if (top==NULL){
         cout<<“溢出”;
         return ;
     }
    e=top->data;       //暂存栈顶元素
    p=top; 
    top = top->next;  
    delete p;
}
```

---

## 队列

队列是线性表的特例。它将元素排列成队，有入口（队尾）和出口（队头），数据元素只能从队尾入队，从队头离队，FIFO和LILO。

### 循环队列

将存储队列的数组看成是头尾相接的圆环，并成为循环存储空间，即允许队列直接从数组中下标最大的位置延续到下标最小的位置，这个操作可以通过取模运算实现。队列的这种头尾相接的顺序存储结构称为**循环队列**（***Circular Queue***）。

在循环队列中进行入队、出队操作时，头尾指针仍然需要加1，朝前移动。

循环队列定义：

```c++
class CQueue {
    private:
	int *base;// 存储空间基址
	int front;// 队头指针
	int rear;// 队尾指针
	int queuesize;// 队容量
    public:
    CQueue(){base=new int[queuesize]; front=rear=0;}//构造空队列
	~CQueue();// 析构函数，释放链队各结点的存储空间
	void EnQueue(int e);// 元素e入队
	void DeQueue(int &e);// 队顶元素出队
	int GetHead();// 取队头元素
	int GetLast();//取队尾元素
	void QueueDisplay();//遍历队，输出队列元素
};
```

循环队列入队算法：

```c++
void CQueue::EnQueue(int e){
    if((rear+1)%(queuesize) == front){
        cout<<"上溢，无法入队";
        return;
    }
    base[rear]=e;
    rear=(rear+1)%queuesize;
}
```

循环队列出队算法：

```c++
void DeQueue(int &e){
    if(front == rear){
        cout<<"下溢，不能出队";
        return ;
    }
    e=base[front];
    front=(front+1)%queuesize;
}
```

### 链队列

队列的链式存储结构称为**链队列**（***Linked Queue***）。根据队列先进先出的特性，链队列是仅在表头删除元素和表尾插入元素的单链表。为了操作方便，链队列用有头结点的链表表示，并设置队头指针指向链队列的头结点，队尾指针指向终端结点。

链队列的类定义和基本操作：

```c++
struct Node{
   int data;
   Node *next;
 };
class LinkQueue{
    private:
    Node *front;//队头指针，链表头为队头
    Node *rear;//队尾指针，链表尾为队尾
    public:
    LinkQueue(){front=rear=new Node;front->next=NULL;}//构造空队列
    ~LinkQueue();// 析构函数，释放链队各结点的存储空间
    void EnQueue(int e);// 元素x入队
    void DeQueue(int &e);// 队顶元素出队
    int emptyQueue(){if(front==rear)return 1;else return 0;}//队列判空
    int GetHead();// 取队头元素
    int GetLast();//取队尾元素
    void QueueDisplay();
};
```

链队列入队算法：

```c++
void LinkQueue::EnQueue(int e){
    Node *s;
    s=new Node; s->data=e; 
    s->next=rear->next;
    rear->next=s; 
    rear=s; 
}
```

链队列出队算法：

```c++
void LinkQueue::DeQueue(int &e){
    Node *p;
    if(rear==front){
        cout<<"下溢";return;
    }//队空，则下溢
    p=front->next;
    e=p->data;
    front->next=p->next;
    if(p->next==NULL)
        rear=front;
    delete p;
}
```

---

## 串

### 串类型的定义

**串**(***String***)是零个或多个字符组成的有限序列。一般记作$S=“a_1a_2a_3…a_n”$，其中S是串名，双引号括起来的字符序列是串值；$a_i(1\leq i\leq n)$可以是字母、数字或其它字符。

空白串：由空格组成

空串：长度为零

**子串**：串中任意个连续字符组成的子序列，包含字串的串称为**主串**

通常将子串在主串中首次出现时的该子串的首字符在主串中的位置，定义为子串在主串中的序号（或位置）。

* 串长：`int strlen(char *s);`
* 串复制：`char *strcpy(char *to, char *from)`
* 串连接：`char strcat(char *to, char *from)`

求子串：

```c++
void substr(string sub,string s,int pos,int len){
    if(pos<0 || pos>strlen(s)-1 || len<0)
        return ;
    strncpy(sub,s+pos,len);
}
```

串的定位：

```c++
index(s, t, pos){
    if(pos>0){
        n=strlen(s);    
        m=strlen(t);  
        i=pos;
        while(i<n-m+1){
            substr(sub,s,i,m);
            if(strcmp(sub,t)!=0)
                ++i;
            else return i;
        }
    }
    return  0;
}
```



### 串的表示和实现

#### 定长顺序存储表示

定长顺序存储表示，也称为静态存储分配的顺序表，数组的上界预先给出

```c++
#define MAXSTRLEN 256
typedef char sstring[MAXSTRLEN];
sstring s;   //s是一个可容纳255个字符的顺序串。
```

#### 堆分配存储表示

这种存储表示的特点是，仍以一组地址连续的存储单元存放串值字符序列，但它们的存储空间是在程序执行过程中动态分配而得。

#### 串的链式存储结构

可用单链表方式来存储串值，串的这种链式存储结构简称为链串。一个链串由头指针唯一确定。

---

### 串的模式匹配算法

一般将主串称为目标串，子串称为模式串

#### Brute Force算法

```c++
//在主串s中定位子串t,不存在返回-1 
int Index(char s[],char t[]){
    int i=0,j=0;
    int slen=strlen(s)-1;//s最大下标号
    int tlen=strlen(t)-1; //t最大下标号
    while(i<=slen && j<=tlen){
        if(s[i]==t[j]){ 
            ++i; 
            ++j;
        }
        else{ 
            i=i-j+1;
            j=0;  
        }
    }
    if(j>tlen)
        return i-tlen-1;
    else 
        return -1;
}
```

#### KMP算法

失败函数
$$
f[j]=\left\{
\begin{aligned}
i \quad 使p_1...p_i = p_{j-1+1}...p_j\\
0 \quad 其它
\end{aligned}
\right.
$$

$$
next[j]=\left\{
\begin{aligned}
0\quad 当j=1时 \\
f[j-1]+1
\end{aligned}
\right.
$$

$next[i]$给出了当与模式串的第i个字符不匹配时，应该重新开始比较的字符序号。

```c++
int Index_KMP(s,t){ //下标计数从1开始
   i=1;j=1;
   while(i<=s串长&&j<=t串长){ 
       if(j= =0 || s[i]= =t[j]){ 
           i++;
           j++;}
       else 
           j=next[j];
       if(j>t串长) 
           return i-t串长;
       else
           return 0;
}
```

---

