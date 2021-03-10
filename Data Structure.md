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

### 线性表的类型定义

**线性表**：由n个数据元素（节点）$a_1,a_2,...,a_n$组成的有限序列。当n=0时成为空表，常常将非空的线性表记作：$(a_1,a_2,...a_n)$

* 对非空的线性表，有且仅有一个开始结点$a_1$，它没有直接前趋，而今有一个直接后继$a_2$
* 有且仅有一个终端结点$a_n$，它没有直接后继，而今有一个直接前趋$a_{n-1}$
* 其余的内部节点$a_i(2\leq i\leq n-1)$都有且仅有一个直接前趋$a_{i-1}$和一个直接后继$a_{i+1}$

### 线性表的顺序表示和实现

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

### 线性表的链式表示和实现

#### 线性链表

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



#### 循环链表

#### 双向链表

### 一元多项式的表示和相加