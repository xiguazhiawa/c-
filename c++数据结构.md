[TOC]

# 顺序表

## 数组的增删查改

1. **插入操作**：`insert`函数可在数组的指定位置插入新元素。要是索引不合法，就会抛出异常。插入时，它会把插入位置及之后的元素依次后移一位，从而腾出空间放置新元素。
2. **查找操作**：`find`函数用于在数组里查找特定值。若找到，就返回该值首次出现的索引；若没找到，则返回 - 1。
3. **删除操作**：`DeleteArray`函数能够删除数组中指定位置的元素。删除后，它会把后续元素依次前移一位。
4. **更新操作**：`updateArray`函数可修改数组中指定位置的元素值。若索引不合法，会抛出异常。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;
//在数组的下标位置插入元素，i为插入元素的下标，value为插入的值，size为插入之前元素的个数
void insert(int a[],int i,int value,int *size){
    if (i<0||i>*size){
        throw std::invalid_argument("Invalid index");
    }
    //插入，之前最后一个元素的下标是size-1，首先第一次循环是需要将size-1的元素赋值到size位置，循环到i位置的值存储到i+1位置，i位置的值赋值新值value，size++
    for (int j=*size;j>i;j--){
        a[j]=a[j-1];
    }
    a[i]=value;
    (*size)++;
}
int find(int a[],int value,int *size){
    for (int i=0;i<*size;i++){
        if(a[i]==value){
            return i;
        }
    }
    return -1;
}
//把i位置的值进行删除，i位置的值空了，循环第一次让a[i]=a[i+1],直到a[size-2]=a[size-1]之后循环截止，size--
void DeleteArray(int a[],int i,int *size){
    for (int j=i;j<*size-1;j++){
        a[j]=a[j+1];
    }
    (*size)--;
}
void updateArray(int a[],int i,int value,int *size){
    if(i<0||i>=*size){
        throw std::invalid_argument("Invalid index");
    }
    a[i]=value;
}
int main(){
    int a[100]{1,2,3,4,5,6,7};
    int ArraySize=7;
    int* size =&ArraySize;
    insert(a,4,4,size);
    for (int i=0;i<*size;i++){
        cout <<a[i] <<" ";
    }
    DeleteArray(a,4,size);
    cout <<endl;
    for (int i=0;i<*size;i++){
        cout <<a[i] <<" ";
    }
    cout <<endl;
    int index=find(a,3,size);
    cout <<index <<endl;
    updateArray(a,5,10,size);
    for (int i=0;i<*size;i++){
        cout <<a[i] <<" ";
    }
    
}
```

## 顺序表的增删改查

1. 初始化与销毁
   - `initializeList`：创建一个指定容量的顺序表，并将`size`置为 0。
   - `destroylist`：释放顺序表占用的内存，防止内存泄漏。
2. 状态查询
   - `size`：返回当前顺序表的大小。
   - `isempty`：判断顺序表是否为空。
3. 插入操作
   - `insert`：在指定位置插入元素。若插入位置等于当前容量，则自动扩容（容量翻倍），并将原数据复制到新数组。
4. 删除操作
   - `deleteElement`：删除指定位置的元素，后续元素前移。
5. 查找与访问
   - `findElement`：查找元素并返回其第一次出现的索引，未找到返回 - 1。
   - `getElement`：获取指定位置的元素值。
6. 更新操作
   - `updateElement`：修改指定位置的元素值。

```C++
#include <iostream>
using namespace std;
#define eleType int
struct sqlist{
    eleType *elements;
    int size;   //当前顺序表存储数据的大小 即存储的下标等于size-1
    int capacity;  //当前顺序表存储数据的容量
};
//初始化顺序表，创建一个结构体，让结构体的指针指向数组
void initializeList(sqlist *list,int capacity){
    list->elements=new eleType[capacity];
    list->size=0;
    list->capacity=capacity;
}
//销毁所创建的数组
void destroylist(sqlist* list){
    delete [] list ->elements;
}
//获取当前顺序表存储数据大小
int size(sqlist *list){
    return list->size;
}
bool isempty(sqlist *list){
    return list->size==0;
}
//传入index 插入数据的下标和插入的数据
void insert(sqlist*list, int index,eleType element){
    if(index<0||index>list->size){
        throw std::invalid_argument("Invalid index"); 
    } 
    if(index==list->capacity){
        int new_capacity =list->capacity*2;
        eleType* new_elements= new eleType[new_capacity];
        for (int i=0;i<list->size;i++){
            new_elements[i]=list->elements[i];
        }
        delete [] list->elements;
        list ->elements=new_elements;
        list ->capacity=new_capacity;
    }
    //必须从后往前访问
    for (int i=list->size;i>index;i--){
        list->elements[i]=list->elements[i-1];
    }
    list->elements[index]=element;
    list->size++;
}
void deleteElement(sqlist *list ,int index){
    if(index<0||index>=list->size){
        throw std::invalid_argument("Invalid index"); 
    }
    for (int i = index;i<list->size-1;i++){
        list->elements[i]=list->elements[i+1];
    }
    list->size--;
}
//查找
int findElement(sqlist *list ,eleType element){
    for(int i=0;i<list->size;i++){
        if ( list->elements[i]==element)
        {
            return i;
        }
        
    }
    return -1;
}
//知道下标，找下标所对应的数值
eleType getElement(sqlist *list, int index){
    if (index <0||index >=list->size){
        throw std::invalid_argument("Invalid index");
    }
    return list->elements[index];
}
//将下标所对应的元素进行修改
void updateElement( sqlist *list ,int index ,eleType element){
    if (index <0||index >=list->size){
        throw std::invalid_argument("Invalid index");
    }
    list->elements[index]=element;
}
int main(){
    sqlist myList;
    initializeList(&myList,10);
    for (int i=0;i<10;i++){
        insert(&myList,i,i+2);
    }
    cout <<"size:" <<size(&myList) <<endl;
    cout << isempty(&myList) <<endl;
    for (int i=0;i<myList.size;i++){
        cout << getElement(&myList,i) <<" ";
    }
    cout <<endl;
    deleteElement(&myList,2);
    for (int i=0;i<myList.size;i++){
    cout << getElement(&myList,i) <<" ";
    }
    cout <<endl;
    updateElement(&myList,2,4);
     for (int i=0;i<myList.size;i++){
    cout << getElement(&myList,i) <<" ";
    }
}
```

## vector

​	是STL中常用的类，该类中的元素可以是任意的类型

```C++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // 创建并初始化vector
    vector<int> nums = {5, 2, 8, 1};

    // 添加元素
    nums.push_back(10);
    nums.push_back(3);

    // 排序
    sort(nums.begin(), nums.end());

    // 遍历并输出
    cout << "Sorted elements: ";
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;

    // 删除第一个元素
    nums.erase(nums.begin());

    // 使用迭代器遍历
    cout << "After erase: ";
    for (auto it = nums.begin(); it != nums.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;

    return 0;
}
```

# 链表

[【数据结构】链表(单链表实现+详解+原码)-CSDN博客](https://blog.csdn.net/Edward_Asia/article/details/120876314)



链表是一种物理存储结构上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针链接次序实现的 。
只根据文字描述还是比较抽象的，直接上图来观察：

![img](https://i-blog.csdnimg.cn/blog_migrate/061b5ffce79596f4445207f850c0db6e.png)

图中2.3.4.5都是结构体，称之为结点，与顺序表不同的是，链表中的每个结点不是只单纯的存一个数据。而是一个结构体，结构体成员包括一个所存的数据，和下一个结点的地址。

phead指针中存放的是第一个结点的地址，那么根据指着地址我们就可以找到这个结构体，又因为这个结构体中存放了下一个结构体的地址，所以又可以找到第二个结构体，循环往复就可以找到所有的结点，直到存放空地址的结构体。

**注：**图中的箭头实际上是不存在的，这里只是为了能够方便理解。

####  单向或者双向

![img](https://i-blog.csdnimg.cn/blog_migrate/be8ea4f69ef95e499d7d604bc45c2212.png)

#### 带头或不带头

![img](https://i-blog.csdnimg.cn/blog_migrate/1819ff5b0bc9d584be11f70677cae433.png)

#### 循环或非循环

![img](https://i-blog.csdnimg.cn/blog_migrate/475e361777d816c2c1e1999f0854580a.png)

**虽然有这么多的链表的结构，但是我们实际中最常用还是两种结构：** 

![img](https://i-blog.csdnimg.cn/blog_migrate/1aad6ea72e3cefaed378b4fe34334490.png)

## 单向链表



```c++
#include <iostream>
#include <stdexcept>  // 用于异常处理（如out_of_range）
using namespace std;

// 宏定义元素类型，方便后续修改链表存储的数据类型（如改为double）
#define eleType int

// 链表节点结构体：存储数据和指向下一个节点的指针
struct LinkNode{
    eleType element;  // 节点存储的数据
    LinkNode* next;   // 指向下一个节点的指针
    // 节点构造函数：初始化数据，next默认指向空
    LinkNode(eleType x):element(x),next(nullptr){}
};

// 链表类：封装链表的操作方法
class LinkedList{
private:
    LinkNode* head;  // 头指针：指向链表第一个节点（无头结点设计）
    int size;        // 链表长度：记录节点数量，方便边界检查

public:
    // 链表构造函数：初始化空链表（头指针为空，长度为0）
    LinkedList():head(nullptr),size(0){}
    ~LinkedList();   // 析构函数：释放所有节点内存，防止泄漏

    // 核心操作函数声明
    void insert(int i, eleType value);  // 在位置i插入节点
    void remove(int i);                 // 删除位置i的节点
    LinkNode* find(eleType value);      // 查找值为value的节点
    LinkNode* get(int i);               // 获取位置i的节点
    void update(int i, eleType value);  // 更新位置i的节点值
    void print();                       // 打印链表所有元素
};

// 析构函数：遍历链表并释放所有节点内存
LinkedList::~LinkedList(){
    LinkNode* curr = head;  // 从头部开始遍历
    while (curr){           // 循环直到所有节点被释放
        LinkNode* tmp = curr;  // 暂存当前节点地址
        curr = curr->next;     // 移动到下一个节点
        delete tmp;            // 释放当前节点内存
    }
}

// 在位置i插入值为value的新节点（i从0开始，允许插入到末尾）
void LinkedList::insert(int i, eleType value){
    // 边界检查：插入位置不能为负，也不能超过当前长度（允许等于size，即插入到末尾）
    if(i < 0 || i > size){
        throw std::out_of_range("Invalid position: 插入位置越界");
    }

    // 1. 创建新节点（自动调用LinkNode构造函数初始化）
    LinkNode* new_Node = new LinkNode(value);

    // 2. 分情况插入：
    if (i == 0){  // 情况1：插入到头部（特殊处理，需更新头指针）
        new_Node->next = head;  // 新节点指向原来的第一个节点（空链表时head为null，也适用）
        head = new_Node;        // 头指针更新为新节点（新节点成为第一个节点）
    }
    else{  // 情况2：插入到中间或尾部
        // 找到插入位置的前一个节点（i-1位置）
        LinkNode* curr = head;
        for (int j = 0; j < i - 1; j++){  // 循环i-1次，移动到前驱节点
            curr = curr->next;
        }
        // 插入逻辑：先让新节点指向后继节点，再让前驱节点指向新节点
        new_Node->next = curr->next;  // 新节点连接到原i位置的节点
        curr->next = new_Node;        // 前驱节点连接到新节点
    }

    size++;  // 插入成功后，链表长度+1
}

// 删除位置i的节点（i从0开始）
void LinkedList::remove(int i){
    // 边界检查：删除位置不能为负，且不能超过等于当前长度（有效范围0~size-1）
    if(i < 0 || i >= size){
        throw std::out_of_range("Invalid position: 删除位置越界");
    }

    // 分情况删除：
    if(i == 0){  // 情况1：删除头节点（需更新头指针）
        LinkNode* temp = head;    // 暂存头节点地址
        head = head->next;        // 头指针指向原第二个节点（空链表时已被边界检查拦截）
        delete temp;              // 释放原头节点内存
    }else{  // 情况2：删除中间或尾部节点
        // 找到待删除节点的前驱节点（i-1位置）
        LinkNode* curr = head;
        for (int j = 0; j < i - 1; j++){  // 循环i-1次，移动到前驱节点
            curr = curr->next;
        }
        // 删除逻辑：暂存待删除节点，更新前驱节点的next指针，释放内存
        LinkNode* temp = curr->next;  // 待删除节点是前驱节点的下一个
        curr->next = temp->next;      // 前驱节点跳过待删除节点，直接连接后继节点
        delete temp;                  // 释放待删除节点内存
    }

    size--;  // 删除成功后，链表长度-1
}

// 查找值为value的节点，返回节点指针（未找到返回nullptr）
LinkNode* LinkedList::find(eleType value){
    LinkNode* curr = head;  // 从头部开始遍历
    // 循环条件：当前节点不为空，且节点值不等于目标值
    while(curr && curr->element != value){
        curr = curr->next;  // 移动到下一个节点
    }
    return curr;  // 找到则返回节点指针，否则返回nullptr
}

// 获取位置i的节点（i从0开始），用于后续访问或修改节点值
LinkNode* LinkedList::get(int i){
    // 边界检查：同remove函数，确保i在有效范围内
    if(i < 0 || i >= size){
        throw std::out_of_range("Invalid position: 获取位置越界");
    }

    LinkNode* curr = head;  // 从头部开始遍历
    for(int j = 0; j < i; j++){  // 循环i次，移动到目标节点
        curr = curr->next;
    }
    return curr;  // 返回目标节点指针
}

// 更新位置i的节点值为value（依赖get函数的边界检查）
void LinkedList::update(int i, eleType value){
    // 先通过get函数获取目标节点，再修改其element值
    get(i)->element = value;
}

// 打印链表所有元素（从头部到尾部）
void LinkedList::print(){
    LinkNode* curr = head;  // 从头部开始遍历
    while(curr){  // 循环直到所有节点被访问
        cout << curr->element << " ";  // 输出当前节点值
        curr = curr->next;             // 移动到下一个节点
    }
    cout << endl;  // 打印结束后换行
}

// 主函数：测试链表功能
int main(){
    LinkedList list;  // 创建空链表

    // 测试插入功能：在位置0~5依次插入10~15
    list.insert(0,10);
    list.insert(1,11);
    list.insert(2,12);
    list.insert(3,13);
    list.insert(4,14);
    list.insert(5,15);
    cout << "插入后链表元素：";
    list.print();  // 输出：10 11 12 13 14 15

    // 测试删除功能：删除位置1的节点（值为11）
    list.remove(1);
    cout << "删除位置1后元素：";
    list.print();  // 输出：10 12 13 14 15

    // 测试更新功能：将位置2的节点值改为80（原位置2是13）
    list.update(2,80);
    cout << "更新位置2后元素：";
    list.print();  // 输出：10 12 80 14 15

    // 测试查找功能：查找值为14的节点
    LinkNode* temp = list.find(14);
    if(temp) cout << "找到值为14的节点" << endl;  // 输出：找到值为14的节点

    // 测试获取功能：获取位置3的节点值（当前链表位置3是14）
    cout << "位置3的元素值：" << list.get(3)->element << endl;  // 输出：14

    return 0;
}
```

## 指针指向的链表

### **1. 数据结构定义**

- 节点结构体（`Node`）

  每个节点包含两个成员：

  - `data`：存储节点的值（类型为`eleType`，通过宏定义为`int`）；
  - `next`：指向链表中下一个节点的指针；
    构造函数`Node(int val)`用于初始化节点值，并将`next`默认设为`nullptr`。

### **2. 核心操作函数**

#### **（1）插入操作**

- **头部插入（`insertAtHead`）**：
  在链表头部新增节点，新节点的`next`指向原头节点，返回新的头指针。
  
- **尾部插入（`insertAtTail`）**：
  遍历链表至最后一个节点，将新节点链接到其`next`，若链表为空则直接返回新节点作为头。
- **指定位置插入（`insertAtPosition`）**：
  - 位置`pos < 0`：视为无效，直接返回原链表；
  - 位置`pos == 0`：调用头部插入；
  - 位置超出链表长度：自动插入到尾部；
  - 正常位置：找到第`pos-1`个节点，将新节点插入其后（新节点`next`指向原`pos`位置节点，前节点`next`指向新节点）。

#### **（2）删除操作（`deleteNode`）**

- 功能：删除第一个值为`val`的节点。
- 步骤：
  - 空链表直接返回`nullptr`；
  - 若头节点值为`val`：删除头节点，返回新头节点（原头节点的`next`）；
  - 其他节点：遍历找到待删除节点的前一个节点（`prev`），将`prev->next`指向待删除节点的`next`，释放待删除节点内存。
    

#### **（3）查找操作（`findNode`）**

- 遍历链表，若找到值为`val`的节点，返回该节点指针；未找到则返回`nullptr`。

#### **（4）辅助操作**

- **打印链表（`printList`）**：遍历链表，依次输出每个节点的`data`，以空格分隔。
- **释放内存（`destroyList`）**：通过循环遍历链表，逐个删除节点并释放内存，避免内存泄漏。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;
#define eleType int

// 定义链表节点
struct Node {
    eleType data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

// 在链表头部插入节点
Node* insertAtHead(Node* head, int val) {
    Node* newNode = new Node(val);
    newNode->next = head;
    return newNode;  // 返回新的头指针
}

// 在链表尾部插入节点
Node* insertAtTail(Node* head, int val) {
    Node* newNode = new Node(val);
    if (!head) return newNode;  // 空链表直接返回新节点
    
    Node* current = head;
    while (current->next) {
        current = current->next;
    }
    current->next = newNode;
    return head;  // 头指针不变
}

// 在指定位置插入节点（位置从0开始）
Node* insertAtPosition(Node* head, int val, int pos) {
    if (pos < 0) return head;  // 无效位置
    
    // 插入到头部的情况
    if (pos == 0) {
        return insertAtHead(head, val);
    }
    
    Node* current = head;
    int count = 0;
    
    // 找到第pos-1个节点
    while (current && count < pos - 1) {
        current = current->next;
        count++;
    }
    
    // 位置超出链表长度，插入到尾部
    if (!current) {
        return insertAtTail(head, val);
    }
    
    // 正常插入
    Node* newNode = new Node(val);
    newNode->next = current->next;
    current->next = newNode;
    return head;
}

// 删除值为val的第一个节点
Node* deleteNode(Node* head, int val) {
    if (!head) return nullptr;  // 空链表
    
    // 删除头节点的情况
    if (head->data == val) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return head;  // 返回新的头指针
    }
    
    // 查找待删除节点的前一个节点
    Node* prev = head;
    Node* current = head->next;
    while (current && current->data != val) {
        prev = current;
        current = current->next;
    }
    
    // 如果找到，删除节点
    if (current) {
        prev->next = current->next;
        delete current;
    }
    return head;  // 头指针不变
}

// 查找值为val的节点
Node* findNode(Node* head, int val) {
    Node* current = head;
    while (current) {
        if (current->data == val) return current;
        current = current->next;
    }
    return nullptr;  // 未找到
}

// 打印链表
void printList(Node* head) {
    Node* current = head;
    while (current) {
        cout << current->data << " ";
        current = current->next;
    }
    cout << endl;
}

// 释放链表内存  将我们的头指针往后移，删除刚刚的
void destroyList(Node* head) {
    while (head) {
        Node* temp = head;
        head = head->next;
        delete temp;
    }
}

// 示例用法
int main() {
    Node* head = nullptr;  // 空链表
    
    // 插入节点
    head = insertAtHead(head, 10);     // 链表: 10
    head = insertAtHead(head, 20);     // 链表: 20 -> 10
    head = insertAtTail(head, 30);     // 链表: 20 -> 10 -> 30
    head = insertAtPosition(head, 15, 1); // 链表: 20 -> 15 -> 10 -> 30
    
    cout << "初始链表: ";
    printList(head);  // 输出: 20 15 10 30
    
    // 删除节点
    head = deleteNode(head, 10);  // 链表: 20 -> 15 -> 30
    cout << "删除10后的链表: ";
    printList(head);  // 输出: 20 15 30
    
    // 查找节点
    Node* found = findNode(head, 30);
    if (found) cout << "找到节点: " << found->data << endl;
    else cout << "未找到节点" << endl;
    
    // 测试越界插入
    head = insertAtPosition(head, 40, 100); // 插入到尾部
    cout << "越界插入后的链表: ";
    printList(head);  // 输出: 20 15 30 40
    
    // 释放内存
    destroyList(head);
    return 0;
}
```

### **带头结点来实现链表**

###  **数据结构定义**

- 节点结构体（`Node`）

  每个节点包含两个成员：

  - `data`：存储节点的整型数据；
  - `next`：指向链表中下一个节点的指针。

### **2. 核心操作函数**

#### **（1）链表的创建与销毁**

- **创建新节点（`createNode`）**：
  动态分配内存生成新节点，初始化`data`为指定值，`next`为`nullptr`。
- **创建带头结点的链表（`createList`）**：
  生成一个头结点（数据域初始化为 0，无实际意义），作为链表的起始标识，简化空链表操作。
- **销毁链表（`destroyList`）**：
  从头部开始遍历，逐个释放所有节点的内存，避免内存泄漏。

#### **（2）状态查询**

- **判断链表是否为空（`isEmpty`）**：
  若头结点的`next`为`nullptr`（即无有效节点），则返回`true`。
- **获取链表长度（`getLength`）**：
  从头结点的下一个节点开始遍历，统计有效节点的数量并返回。

#### **（3）插入操作**

- **尾部添加元素（`append`）**：
  遍历至链表末尾（最后一个节点的`next`为`nullptr`），将新节点链接到尾部。
  示例：原链表`10 -> 20` → 追加`30`后变为`10 -> 20 -> 30`。
- **指定位置插入（`insert`）**：
  - 先检查位置合法性（`pos < 0`或`pos > 长度`则失败）；
  - 找到目标位置的前一个节点，将新节点插入其后方（新节点`next`指向原后续节点，前节点`next`指向新节点）。
    示例：在`10 -> 20 -> 30`的`pos=1`插入`15` → 变为`10 -> 15 -> 20 -> 30`。

#### **（4）删除操作（`remove`）**：

- 检查位置合法性（`pos < 0`或`pos >= 长度`则失败）；
- 找到目标位置的前一个节点，跳过待删除节点（将前节点`next`指向待删除节点的`next`），释放待删除节点内存。
  示例：删除`10 -> 15 -> 20 -> 30`中`pos=2`的节点 → 变为`10 -> 15 -> 30`。

#### **（5）查询与访问**

- **获取指定位置元素（`get`）**：
  检查位置合法性，遍历至目标位置，返回对应节点的`data`；若越界则抛出异常。
- **查找元素位置（`find`）**：
  遍历链表，返回目标元素第一次出现的位置（从 0 开始）；未找到则返回`-1`。

#### **（6）打印链表（`printList`）**：

从头结点的下一个节点开始遍历，依次输出所有有效节点的`data`，以空格分隔。

```C++
#include <iostream>
#include <cstdlib>

// 链表节点结构体
struct Node {
    int data;           // 存储节点数据
    Node* next;         // 指向下一个节点的指针
};

// 创建新节点
Node* createNode(int value) {
    Node* newNode = new Node;
    newNode->data = value;
    newNode->next = nullptr;
    return newNode;
}

// 创建带头结点的空链表
Node* createList() {
    return createNode(0);  // 头结点数据域可以不使用，初始化为0
}

// 销毁链表
void destroyList(Node* head) {
    Node* current = head;
    while (current != nullptr) {
        Node* next = current->next;
        delete current;
        current = next;
    }
}

// 判断链表是否为空
bool isEmpty(Node* head) {
    return head->next == nullptr;
}

// 获取链表长度
int getLength(Node* head) {
    int length = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始遍历
    while (current != nullptr) {
        length++;
        current = current->next;
    }
    return length;
}

// 在链表尾部添加元素
void append(Node* head, int value) {
    Node* newNode = createNode(value);
    Node* current = head;
    
    // 找到最后一个节点
    while (current->next != nullptr) {
        current = current->next;
    }
    
    current->next = newNode;  // 将新节点连接到尾部
}

// 在指定位置插入元素（位置从0开始）
bool insert(Node* head, int pos, int value) {
    if (pos < 0 || pos > getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* newNode = createNode(value);
    Node* current = head;
    
    // 移动到插入位置的前一个节点
    for (int i = 0; i < pos; i++) {
        current = current->next;
    }
    
    newNode->next = current->next;  // 新节点的next指向当前节点的下一个节点
    current->next = newNode;        // 当前节点的next指向新节点
    return true;
}

// 删除指定位置的元素（位置从0开始）
bool remove(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* current = head;
    
    // 移动到要删除节点的前一个节点
    for (int i = 0; i < pos; i++) {
        current = current->next;
    }
    
    Node* toDelete = current->next;  // 要删除的节点
    current->next = toDelete->next;  // 跳过要删除的节点
    delete toDelete;                 // 释放内存
    return true;
}

// 获取指定位置的元素（位置从0开始）
int get(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        throw std::out_of_range("位置超出范围");
    }
    
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    // 移动到指定位置
    for (int i = 0; i < pos; i++) {
        current = current->next;
    }
    
    return current->data;
}

// 查找元素第一次出现的位置
int find(Node* head, int value) {
    int pos = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != nullptr) {
        if (current->data == value) {
            return pos;  // 找到元素，返回位置
        }
        current = current->next;
        pos++;
    }
    
    return -1;  // 未找到元素
}

// 打印链表
void printList(Node* head) {
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != nullptr) {
        std::cout << current->data << " ";
        current = current->next;
    }
    
    std::cout << std::endl;
}

// 示例用法
int main() {
    Node* list = createList();  // 创建带头结点的链表
    
    append(list, 10);           // 添加元素
    append(list, 20);
    append(list, 30);
    
    std::cout << "链表内容: ";
    printList(list);            // 输出: 10 20 30
    
    insert(list, 1, 15);        // 在位置1插入15
    std::cout << "插入后: ";
    printList(list);            // 输出: 10 15 20 30
    
    remove(list, 2);            // 删除位置2的元素
    std::cout << "删除后: ";
    printList(list);            // 输出: 10 15 30
    
    std::cout << "位置0的元素: " << get(list, 0) << std::endl;  // 输出: 10
    std::cout << "元素30的位置: " << find(list, 30) << std::endl;  // 输出: 2
    
    destroyList(list);          // 销毁链表
    return 0;
}
```



### **带头结点的循环链表**

### **数据结构定义**

- 节点结构体（`Node`）

  每个节点包含两个成员：

  - `data`：存储节点的整型数据；
  - `next`：指向链表中下一个节点的指针。
    循环链表的核心特点是：最后一个节点的`next`指向头结点，形成 “首尾相连” 的闭环。

### **2. 核心操作函数**

#### **（1）链表的创建与销毁**

- **创建循环链表（`createList`）**：
  生成一个头结点，其`next`指针指向自身（`head->next = head`），形成初始的空循环链表（仅有头结点，无有效数据节点）。
- **销毁链表（`destroyList`）**：
  从头部的下一个节点开始遍历，逐个释放所有数据节点的内存，最后释放头结点，避免内存泄漏。遍历终止条件是 “当前节点等于头结点”（即回到循环起点）。

#### **（2）状态查询**

- **判断链表是否为空（`isEmpty`）**：
  若头结点的`next`指向自身（`head->next == head`），表示无有效数据节点，返回`true`。
- **获取链表长度（`getLength`）**：
  从头结点的下一个节点开始遍历，统计有效数据节点的数量（遍历终止条件：当前节点等于头结点），返回统计结果。

#### **（3）插入操作**

- **尾部添加元素（`append`）**：
  遍历找到链表的尾节点（尾节点的`next`指向头结点），将新节点插入尾部：新节点的`next`指向头结点，尾节点的`next`指向新节点，维持循环特性。
  示例：原链表`10 -> 20`（尾节点`20`的`next`指向头结点）→ 追加`30`后，`20->next=30`，`30->next=head`，链表变为`10->20->30`。
- **指定位置插入（`insert`）**：
  - 先检查位置合法性（`pos < 0`或`pos > 长度`则失败）；
  - 找到目标位置的前一个节点，将新节点插入其后方：新节点的`next`指向原后续节点，前节点的`next`指向新节点，不破坏循环结构。
    示例：在`10->20->30`的`pos=1`插入`15` → 变为`10->15->20->30`。

#### **（4）删除操作（`remove`）**：

- 检查位置合法性（`pos < 0`或`pos >= 长度`则失败）；
- 找到目标位置的前一个节点，跳过待删除节点（前节点的`next`指向待删除节点的`next`），释放待删除节点的内存，维持循环特性。
  示例：删除`10->15->20->30`中`pos=2`的节点（值为`20`）→ 变为`10->15->30`（`15->next=30`，`30->next=head`）。

#### **（5）查询与访问**

- **获取指定位置元素（`get`）**：
  检查位置合法性后，从头节点的下一个节点开始遍历，移动到目标位置，返回对应节点的`data`；越界时抛出异常。
- **查找元素位置（`find`）**：
  从头节点的下一个节点开始遍历，若找到目标值，返回其位置（从 0 开始）；遍历完所有节点（回到头节点）仍未找到，返回`-1`。

#### **（6）打印链表（`printList`）**：

从头节点的下一个节点开始遍历，依次输出所有有效节点的`data`，直至回到头节点终止，输出结果为线性序列（忽略循环的闭环展示）。

```C++
#include <iostream>
#include <cstdlib>

// 链表节点结构体
struct Node {
    int data;           // 存储节点数据
    Node* next;         // 指向下一个节点的指针
};

// 创建带头结点的空循环链表
Node* createList() {
    Node* head = new Node;
    head->next = head;  // 头结点的next指向自身，形成循环
    return head;
}

// 销毁链表
void destroyList(Node* head) {
    Node* current = head->next;  // 从头结点的下一个节点开始
    while (current != head) {    // 遍历到链表尾部（即再次回到头结点）
        Node* next = current->next;
        delete current;
        current = next;
    }
    delete head;  // 最后删除头结点
}

// 判断链表是否为空
bool isEmpty(Node* head) {
    return head->next == head;  // 头结点的next指向自身表示空链表
}

// 获取链表长度
int getLength(Node* head) {
    int length = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        length++;
        current = current->next;
    }
    
    return length;
}

// 在链表尾部添加元素
void append(Node* head, int value) {
    Node* newNode = new Node;
    newNode->data = value;
    
    Node* tail = head;
    while (tail->next != head) {  // 找到尾节点
        tail = tail->next;
    }
    
    tail->next = newNode;  // 尾节点的next指向新节点
    newNode->next = head;  // 新节点的next指向头结点，形成循环
}

// 在指定位置插入元素（位置从0开始）
bool insert(Node* head, int pos, int value) {
    if (pos < 0 || pos > getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* newNode = new Node;
    newNode->data = value;
    
    Node* current = head;
    for (int i = 0; i < pos; i++) {  // 移动到插入位置的前一个节点
        current = current->next;
    }
    
    newNode->next = current->next;  // 新节点的next指向当前节点的下一个节点
    current->next = newNode;        // 当前节点的next指向新节点
    return true;
}

// 删除指定位置的元素（位置从0开始）
bool remove(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* current = head;
    for (int i = 0; i < pos; i++) {  // 移动到要删除节点的前一个节点
        current = current->next;
    }
    
    Node* toDelete = current->next;  // 要删除的节点
    current->next = toDelete->next;  // 跳过要删除的节点
    delete toDelete;                 // 释放内存
    return true;
}

// 获取指定位置的元素（位置从0开始）
int get(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        throw std::out_of_range("位置超出范围");
    }
    
    Node* current = head->next;  // 从头结点的下一个节点开始
    for (int i = 0; i < pos; i++) {  // 移动到指定位置
        current = current->next;
    }
    
    return current->data;
}

// 查找元素第一次出现的位置
int find(Node* head, int value) {
    int pos = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        if (current->data == value) {
            return pos;  // 找到元素，返回位置
        }
        current = current->next;
        pos++;
    }
    
    return -1;  // 未找到元素
}

// 打印链表
void printList(Node* head) {
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        std::cout << current->data << " ";
        current = current->next;
    }
    
    std::cout << std::endl;
}

// 示例用法
int main() {
    Node* list = createList();  // 创建带头结点的循环链表
    
    append(list, 10);           // 添加元素
    append(list, 20);
    append(list, 30);
    
    std::cout << "链表内容: ";
    printList(list);            // 输出: 10 20 30
    
    insert(list, 1, 15);        // 在位置1插入15
    std::cout << "插入后: ";
    printList(list);            // 输出: 10 15 20 30
    
    remove(list, 2);            // 删除位置2的元素
    std::cout << "删除后: ";
    printList(list);            // 输出: 10 15 30
    
    std::cout << "位置0的元素: " << get(list, 0) << std::endl;  // 输出: 10
    std::cout << "元素30的位置: " << find(list, 30) << std::endl;  // 输出: 2
    
    destroyList(list);          // 销毁链表
    return 0;
}
```



## 不带头结点的循环链表

### **一、数据结构基础**

- **节点（`Node`结构体）**：
  链表的基本组成单位，每个节点包含：
  - `data`：存储整数数据（例如 10、20 等）；
  - `next`：指针，指向链表中的下一个节点。
- **循环链表的特点**：
  与普通单链表的区别在于：**最后一个节点的`next`指针不指向`nullptr`，而是指向头结点**，形成 “首尾相连” 的闭环（例如`A→B→C→A`）。
  这里的 “头结点” 是一个特殊节点，不存储有效数据，仅用于简化链表操作（如空链表判断、插入删除的统一处理）。

### **二、核心功能函数详解**

#### **1. 链表的创建与销毁**

- **`createList()`：创建空循环链表**
  - 生成一个头结点，让其`next`指针指向自身（`head->next = head`），此时链表为空（只有头结点，无有效数据）。
  - 作用：初始化链表，提供一个统一的起点。
- **`destroyList(Node\* head)`：销毁链表**
  - 从 “头结点的下一个节点” 开始遍历，逐个删除所有数据节点，最后删除头结点。
  - 遍历终止条件：`current == head`（回到头结点，说明已遍历完所有节点）。
  - 作用：释放动态分配的内存，避免内存泄漏。

#### **2. 状态查询**

- **`isEmpty(Node\* head)`：判断链表是否为空**
  - 若头结点的`next`指向自身（`head->next == head`），说明没有数据节点，返回`true`；否则返回`false`。
- **`getLength(Node\* head)`：计算链表长度**
  - 从头结点的下一个节点开始遍历，统计所有数据节点的数量（每遇到一个节点，长度 + 1）。
  - 遍历终止条件：`current == head`（回到头结点，停止计数）。

#### **3. 插入操作**

- **`append(Node\* head, int value)`：在尾部添加元素**
  - 步骤：
    1. 新建节点，存入数据`value`；
    2. 遍历找到 “尾节点”（尾节点的`next`指向头结点）；
    3. 让尾节点的`next`指向新节点，新节点的`next`指向头结点（维持循环特性）。
  - 示例：原链表`10→20→head` → 追加 30 后变为`10→20→30→head`。
- **`insert(Node\* head, int pos, int value)`：在指定位置插入元素**
  - 步骤：
    1. 检查位置合法性（`pos`不能小于 0 或大于当前长度）；
    2. 找到插入位置的前一个节点（例如插入到`pos=1`，需找到第 0 个节点）；
    3. 新节点的`next`指向 “前节点的下一个节点”，前节点的`next`指向新节点（插入中间）。
  - 示例：在`10→20→30`的`pos=1`插入 15 → 变为`10→15→20→30`。

#### **4. 删除操作**

- `remove(Node* head, int pos)`：删除指定位置的元素
  - 步骤：
    1. 检查位置合法性（`pos`不能小于 0 或大于等于长度）；
    2. 找到待删除节点的前一个节点；
    3. 让前节点的`next`指向 “待删除节点的下一个节点”（跳过待删除节点），然后释放其内存。
  - 示例：删除`10→15→20→30`中`pos=2`的节点（值为 20）→ 变为`10→15→30`。

#### **5. 查找与访问**

- **`get(Node\* head, int pos)`：获取指定位置的元素**
  - 遍历到`pos`对应的节点，返回其`data`；若`pos`越界（如负数或超过长度），抛出异常。
- **`find(Node\* head, int value)`：查找元素的位置**
  - 从第一个数据节点开始遍历，若找到`data`等于`value`的节点，返回其位置（从 0 开始）；若遍历完所有节点（回到头结点）仍未找到，返回`-1`。

#### **6. 打印链表**

- **`printList(Node\* head)`**：
  从头结点的下一个节点开始，依次输出每个数据节点的`data`，直到回到头结点停止（避免无限循环）。
  例如循环链表`10→15→30→head`，打印结果为`10 15 30`。

```C++
#include <iostream>
#include <cstdlib>

// 链表节点结构体
struct Node {
    int data;           // 存储节点数据
    Node* next;         // 指向下一个节点的指针
};

// 创建带头结点的空循环链表
Node* createList() {
    Node* head = new Node;
    head->next = head;  // 头结点的next指向自身，形成循环
    return head;
}

// 销毁链表
void destroyList(Node* head) {
    Node* current = head->next;  // 从头结点的下一个节点开始
    while (current != head) {    // 遍历到链表尾部（即再次回到头结点）
        Node* next = current->next;
        delete current;
        current = next;
    }
    delete head;  // 最后删除头结点
}

// 判断链表是否为空
bool isEmpty(Node* head) {
    return head->next == head;  // 头结点的next指向自身表示空链表
}

// 获取链表长度
int getLength(Node* head) {
    int length = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        length++;
        current = current->next;
    }
    
    return length;
}

// 在链表尾部添加元素
void append(Node* head, int value) {
    Node* newNode = new Node;
    newNode->data = value;
    
    Node* tail = head;
    while (tail->next != head) {  // 找到尾节点
        tail = tail->next;
    }
    
    tail->next = newNode;  // 尾节点的next指向新节点
    newNode->next = head;  // 新节点的next指向头结点，形成循环
}

// 在指定位置插入元素（位置从0开始）
bool insert(Node* head, int pos, int value) {
    if (pos < 0 || pos > getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* newNode = new Node;
    newNode->data = value;
    
    Node* current = head;
    for (int i = 0; i < pos; i++) {  // 移动到插入位置的前一个节点
        current = current->next;
    }
    
    newNode->next = current->next;  // 新节点的next指向当前节点的下一个节点
    current->next = newNode;        // 当前节点的next指向新节点
    return true;
}

// 删除指定位置的元素（位置从0开始）
bool remove(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        return false;  // 位置不合法
    }
    
    Node* current = head;
    for (int i = 0; i < pos; i++) {  // 移动到要删除节点的前一个节点
        current = current->next;
    }
    
    Node* toDelete = current->next;  // 要删除的节点
    current->next = toDelete->next;  // 跳过要删除的节点
    delete toDelete;                 // 释放内存
    return true;
}

// 获取指定位置的元素（位置从0开始）
int get(Node* head, int pos) {
    if (pos < 0 || pos >= getLength(head)) {
        throw std::out_of_range("位置超出范围");
    }
    
    Node* current = head->next;  // 从头结点的下一个节点开始
    for (int i = 0; i < pos; i++) {  // 移动到指定位置
        current = current->next;
    }
    
    return current->data;
}

// 查找元素第一次出现的位置
int find(Node* head, int value) {
    int pos = 0;
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        if (current->data == value) {
            return pos;  // 找到元素，返回位置
        }
        current = current->next;
        pos++;
    }
    
    return -1;  // 未找到元素
}

// 打印链表
void printList(Node* head) {
    Node* current = head->next;  // 从头结点的下一个节点开始
    
    while (current != head) {    // 遍历到链表尾部
        std::cout << current->data << " ";
        current = current->next;
    }
    
    std::cout << std::endl;
}

// 示例用法
int main() {
    Node* list = createList();  // 创建带头结点的循环链表
    
    append(list, 10);           // 添加元素
    append(list, 20);
    append(list, 30);
    
    std::cout << "链表内容: ";
    printList(list);            // 输出: 10 20 30
    
    insert(list, 1, 15);        // 在位置1插入15
    std::cout << "插入后: ";
    printList(list);            // 输出: 10 15 20 30
    
    remove(list, 2);            // 删除位置2的元素
    std::cout << "删除后: ";
    printList(list);            // 输出: 10 15 30
    
    std::cout << "位置0的元素: " << get(list, 0) << std::endl;  // 输出: 10
    std::cout << "元素30的位置: " << find(list, 30) << std::endl;  // 输出: 2
    
    destroyList(list);          // 销毁链表
    return 0;
}
```



### **题目 1：反转单链表**

**要求**：将一个单链表反转（不带头结点）

```C++
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

// 创建新节点
Node* createNode(int val) {
    Node* node = new Node;
    node->data = val;
    node->next = nullptr;
    return node;
}

// 反转单链表
Node* reverseList(Node* head) {
    Node* prev = nullptr;   // 前一个节点
    Node* curr = head;      // 当前节点
    Node* next = nullptr;   // 下一个节点

    while (curr != nullptr) {
        next = curr->next;  // 保存下一个节点
        curr->next = prev;  // 反转当前节点的指针
        prev = curr;        // 前指针后移
        curr = next;        // 当前指针后移
    }
    return prev;  // 反转后prev成为新头节点
}

// 打印链表
void printList(Node* head) {
    Node* curr = head;
    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }
    cout << endl;
}

// 测试
int main() {
    // 构建链表：1->2->3->4->5
    Node* head = createNode(1);
    head->next = createNode(2);
    head->next->next = createNode(3);
    head->next->next->next = createNode(4);
    head->next->next->next->next = createNode(5);

    cout << "原链表: ";
    printList(head);  // 1 2 3 4 5

    Node* reversed = reverseList(head);
    cout << "反转后: ";
    printList(reversed);  // 5 4 3 2 1

    // 释放内存（略）
    return 0;
}
```

### **合并两个有序链表**

将两个升序单链表合并为一个新的升序单链表（不带头结点）。

```C++
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

// 创建新节点
Node* createNode(int val) {
    Node* node = new Node;
    node->data = val;
    node->next = nullptr;
    return node;
}

// 合并两个有序链表
Node* mergeTwoLists(Node* l1, Node* l2) {
    Node* dummy = createNode(0);  // 临时 dummy 节点（简化操作）
    Node* curr = dummy;

    while (l1 != nullptr && l2 != nullptr) {
        if (l1->data <= l2->data) {
            curr->next = l1;  // 取 l1 节点
            l1 = l1->next;
        } else {
            curr->next = l2;  // 取 l2 节点
            l2 = l2->next;
        }
        curr = curr->next;
    }

    // 拼接剩余节点
    curr->next = (l1 != nullptr) ? l1 : l2;

    Node* result = dummy->next;
    delete dummy;  // 释放 dummy 节点
    return result;
}

// 打印链表
void printList(Node* head) {
    Node* curr = head;
    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }
    cout << endl;
}

// 测试
int main() {
    // 构建链表1：1->3->5
    Node* l1 = createNode(1);
    l1->next = createNode(3);
    l1->next->next = createNode(5);

    // 构建链表2：2->4->6
    Node* l2 = createNode(2);
    l2->next = createNode(4);
    l2->next->next = createNode(6);

    Node* merged = mergeTwoLists(l1, l2);
    cout << "合并后: ";
    printList(merged);  // 输出：1 2 3 4 5 6

    // 释放内存（略）
    return 0;
}
```

### **删除链表的倒数第 k 个节点**

给定一个单链表，删除倒数第 k 个节点（不带头结点）。

```C++
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

// 创建新节点
Node* createNode(int val) {
    Node* node = new Node;
    node->data = val;
    node->next = nullptr;
    return node;
}

// 删除倒数第k个节点
Node* removeNthFromEnd(Node* head, int k) {
    // 边界处理
    if (head == nullptr || k <= 0) return head;

    // 快慢指针：快指针先走k步
    Node* dummy = createNode(0);  // 用dummy简化头节点删除
    dummy->next = head;
    Node* fast = dummy;
    Node* slow = dummy;

    // 快指针先走k步
    for (int i = 0; i < k; i++) {
        if (fast->next == nullptr) return head;  // k超过链表长度
        fast = fast->next;
    }

    // 快慢指针同步移动，直到快指针到尾
    while (fast->next != nullptr) {
        fast = fast->next;
        slow = slow->next;
    }

    // 删除slow的下一个节点（倒数第k个）
    Node* toDelete = slow->next;
    slow->next = toDelete->next;
    delete toDelete;

    Node* result = dummy->next;
    delete dummy;
    return result;
}

// 打印链表
void printList(Node* head) {
    Node* curr = head;
    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }
    cout << endl;
}

// 测试
int main() {
    // 构建链表：1->2->3->4->5
    Node* head = createNode(1);
    head->next = createNode(2);
    head->next->next = createNode(3);
    head->next->next->next = createNode(4);
    head->next->next->next->next = createNode(5);

    cout << "原链表: ";
    printList(head);  // 1 2 3 4 5

    Node* afterDelete = removeNthFromEnd(head, 2);  // 删除倒数第2个（4）
    cout << "删除后: ";
    printList(afterDelete);  // 1 2 3 5

    // 释放内存（略）
    return 0;
}
```

# 栈

栈是一种“特殊”的线性存储结构，它的特殊之处体现在以下两个地方：
1、元素进栈和出栈的操作只能从一端完成，另一端是封闭的，如下图所示：

![栈存储结构示意图](https://i-blog.csdnimg.cn/img_convert/d7d6d7067ba7aa52b8bd2ae4cc235fdd.gif)

通常，我们将元素进栈的过程简称为“入栈”、“进栈”或者“压栈”；将元素出栈的过程简称为“出栈”或者“弹栈”。

2、栈中无论存数据还是取数据，都必须遵循“先进后出”的原则，即最先入栈的元素最先出栈。以图 1 的栈为例，很容易可以看出是元素 1 最先入栈，然后依次是元素 2、3、4 入栈。在此基础上，如果想取出元素 1，根据“先进后出”的原则，必须先依次将元素 4、3、2 出栈，最后才能轮到元素 1 出栈。

我们习惯将栈的开口端称为栈顶，封口端称为栈底。例如在图 1 中，元素 4 一侧为栈顶，元素 1 一侧为栈底，如图 2 所示。

<div align="center">   <img src="https://i-blog.csdnimg.cn/img_convert/e371a9fae5d264afa9c6c414b49499a4.gif" alt="栈顶和栈底"> </div>



## 使用顺序表来实现栈

### **1. 数据结构定义**

- 栈结构体（`Stack<T>`，模板类）

  采用动态数组实现，包含三个核心成员：

  - `data`：指向动态数组的指针，用于存储栈元素（类型由模板参数`T`指定，如`int`、`double`等）；
  - `capacity`：栈的最大容量（当前动态数组的长度）；
  - `topIndex`：栈顶元素的索引（`-1`表示空栈，`0`表示栈底，数值越大越靠近栈顶）。

### **2. 核心操作函数**

#### **（1）初始化与销毁**

- **`initStack`**：初始化栈，分配初始容量（默认 10）的动态数组，将`topIndex`设为`-1`（空栈状态）。
- **`destroyStack`**：释放动态数组占用的内存，避免内存泄漏，并将`data`指针置为`nullptr`。

#### **（2）状态查询**

- **`isEmpty`**：判断栈是否为空，若`topIndex == -1`返回`true`，否则返回`false`。
- **`size`**：返回栈中当前元素的个数，计算公式为`topIndex + 1`（例如`topIndex=2`表示有 3 个元素）。

#### **（3）扩容操作**

- **`resize`**：当栈满（`topIndex == capacity - 1`）时自动扩容，新容量为原容量的 2 倍。
  步骤：分配新数组 → 复制原元素到新数组 → 释放原数组内存 → 更新`data`指针和`capacity`。

#### **（4）栈的核心操作**

- **`push`（入栈）**：
  在栈顶添加元素。若栈已满，先调用`resize`扩容，再将`topIndex`加 1，并将新元素存入`data[topIndex]`。
  示例：栈中原有`10、20`（`topIndex=1`），入栈`30`后，`topIndex=2`，栈元素为`10、20、30`。
- **`pop`（出栈）**：
  移除并返回栈顶元素。若栈为空（`topIndex=-1`），抛出`underflow_error`异常；否则返回`data[topIndex]`并将`topIndex`减 1。
  示例：栈顶元素为`30`（`topIndex=2`），出栈后`topIndex=1`，返回`30`。
- **`top`（获取栈顶元素）**：
  返回栈顶元素的引用（可修改），若栈为空则抛出异常。提供常量版本（`const T& top`）用于只读访问。

#### **（5）辅助操作**

- **`clear`**：清空栈，只需将`topIndex`设为`-1`（无需实际删除元素，后续入栈会覆盖旧值）。
- **`print`**：从栈顶到栈底打印所有元素（例如栈元素为`10、20、30`，打印结果为`30 20 10`）。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;

// 定义栈结构体
template <typename T>
struct Stack {
    T* data;        // 动态数组存储栈元素
    int capacity;   // 栈的最大容量
    int topIndex;   // 栈顶元素的索引（-1表示空栈）
};

// 初始化栈
template <typename T>
void initStack(Stack<T>& s, int initCapacity = 10) {
    s.data = new T[initCapacity];
    s.capacity = initCapacity;
    s.topIndex = -1;
}

// 释放栈内存
template <typename T>
void destroyStack(Stack<T>& s) {
    delete[] s.data;
    s.data = nullptr;
}

// 判断栈是否为空
template <typename T>
bool isEmpty(const Stack<T>& s) {
    return s.topIndex == -1;
}

// 返回栈的当前元素个数
template <typename T>
int size(const Stack<T>& s) {
    return s.topIndex + 1;
}

// 扩容函数
template <typename T>
void resize(Stack<T>& s) {
    int newCapacity = s.capacity * 2;
    T* newData = new T[newCapacity];
    
    // 复制原栈元素到新数组
    for (int i = 0; i <= s.topIndex; i++) {
        newData[i] = s.data[i];
    }
    
    // 释放原数组内存并更新指针和容量
    delete[] s.data;
    s.data = newData;
    s.capacity = newCapacity;
}

// 入栈操作
template <typename T>
void push(Stack<T>& s, const T& value) {
    if (s.topIndex == s.capacity - 1) {
        resize(s);
    }
    s.data[++s.topIndex] = value;
}

// 出栈操作
template <typename T>
T pop(Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");
    }
    return s.data[s.topIndex--];
}

// 获取栈顶元素
template <typename T>
T& top(Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");
    }
    return s.data[s.topIndex];
}

// 获取栈顶元素（常量版本）
template <typename T>
const T& top(const Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");
    }
    return s.data[s.topIndex];
}

// 清空栈
template <typename T>
void clear(Stack<T>& s) {
    s.topIndex = -1;
}

// 打印栈元素
template <typename T>
void print(const Stack<T>& s) {
    cout << "Stack (top to bottom): ";
    for (int i = s.topIndex; i >= 0; i--) {
        cout << s.data[i] << " ";
    }
    cout << endl;
}

// 示例用法
int main() {
    Stack<int> s;
    initStack(s);

    try {
        // 测试入栈
        push(s, 10);
        push(s, 20);
        push(s, 30);
        print(s);  // 输出: 30 20 10

        // 测试出栈
        cout << "Popped: " << pop(s) << endl;  // 输出: 30
        print(s);  // 输出: 20 10

        // 测试获取栈顶元素
        cout << "Top element: " << top(s) << endl;  // 输出: 20

        // 测试修改栈顶元素
        top(s) = 25;
        cout << "Modified top: " << top(s) << endl;  // 输出: 25

        // 测试栈状态
        cout << "Size: " << size(s) << endl;  // 输出: 2
        cout << "Is empty: " << (isEmpty(s) ? "Yes" : "No") << endl;  // 输出: No

        // 测试清空栈
        clear(s);
        cout << "After clearing: ";
        print(s);  // 输出: 空

        // 测试异常
        pop(s);  // 触发异常
    } catch (const exception& e) {
        cerr << "Exception: " << e.what() << endl;
    }

    // 释放栈内存
    destroyStack(s);
    return 0;
}
```

## 使用链表来实现栈

### **1. 数据结构定义**

- **节点结构体（`Node<T>`，模板类）**：
  链表的基本组成单位，每个节点包含：
  - `data`：存储栈元素的值（类型由模板参数`T`指定，如`int`、`string`等）；
  - `next`：指针，指向链表中的下一个节点（在栈中即 “栈顶下方” 的元素）。
- **栈结构体（`Stack<T>`，模板类）**：
  管理整个栈的核心结构，包含：
  - `top`：指向栈顶节点的指针（链表的头指针，栈顶为链表头部）；
  - `size`：记录栈中当前元素的数量（避免每次统计时遍历链表）。

### **2. 核心操作函数**

#### **（1）初始化与销毁**

- **`initStack`**：初始化空栈，将栈顶指针`top`设为`nullptr`，`size`设为 0。
- **`destroyStack`**：释放栈中所有节点的内存（从栈顶开始遍历，逐个删除节点），最后重置`top`和`size`，避免内存泄漏。

#### **（2）状态查询**

- **`isEmpty`**：判断栈是否为空，若`top`为`nullptr`（无栈顶节点），返回`true`，否则返回`false`。
- **`size`**：直接返回`size`成员，即当前栈中元素的个数。

#### **（3）栈的核心操作**

- **`push`（入栈）**：
  在栈顶添加元素。步骤：创建新节点 → 新节点的`next`指向原栈顶 → 更新`top`为新节点 → `size`加 1。
  示例：栈中原有`10、20`（栈顶为`20`），入栈`30`后，栈顶变为`30`，链表为`30→20→10`。
- **`pop`（出栈）**：
  移除并返回栈顶元素。步骤：若栈为空，抛出`underflow_error`异常 → 暂存栈顶节点 → 保存节点数据 → 更新`top`为下一个节点 → 删除原栈顶节点 → `size`减 1 → 返回数据。
  示例：栈顶为`30`，出栈后栈顶变为`20`，返回`30`。
- **`top`（获取栈顶元素）**：
  返回栈顶元素的引用（可直接修改），若栈为空则抛出异常。提供常量版本（`const T& top`）用于只读访问。

#### **（4）辅助操作**

- **`clear`**：清空栈，复用`destroyStack`释放所有节点，再调用`initStack`重置栈状态（保留栈结构，可重新使用）。
- **`print`**：从栈顶到栈底打印所有元素（遍历链表，依次输出每个节点的`data`）。
  示例：栈元素为`30→20→10`，打印结果为`30 20 10`。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;

// 链表节点结构体（存储栈元素）
template <typename T>
struct Node {
    T data;           // 节点存储的数据
    Node<T>* next;    // 指向下一个节点的指针（栈中即下一个元素）
    Node(const T& value) : data(value), next(nullptr) {}
};

// 栈结构体（链式栈的核心控制结构）
template <typename T>
struct Stack {
    Node<T>* top;     // 栈顶指针（指向链表头部）
    int size;         // 当前栈中元素的数量
};

// 初始化空栈
template <typename T>
void initStack(Stack<T>& s) {
    s.top = nullptr;  // 栈顶指针置空
    s.size = 0;       // 初始元素数量为0
}

// 释放栈内存（销毁所有节点）
template <typename T>
void destroyStack(Stack<T>& s) {
    Node<T>* curr = s.top;
    while (curr != nullptr) {
        Node<T>* temp = curr;  // 暂存当前节点
        curr = curr->next;     // 移动到下一个节点
        delete temp;           // 释放当前节点内存
    }
    s.top = nullptr;  // 栈顶指针置空
    s.size = 0;       // 元素数量清零
}

// 判断栈是否为空
template <typename T>
bool isEmpty(const Stack<T>& s) {
    return s.top == nullptr;  // 栈顶指针为空即栈空
}

// 返回栈的当前元素个数
template <typename T>
int size(const Stack<T>& s) {
    return s.size;  // 直接返回记录的元素数量
}

// 入栈操作（在链表头部插入新节点）
template <typename T>
void push(Stack<T>& s, const T& value) {
    Node<T>* newNode = new Node<T>(value);  // 创建新节点
    newNode->next = s.top;           // 新节点指向原栈顶
    s.top = newNode;                 // 更新栈顶指针为新节点
    s.size++;                        // 元素数量加1
}

// 出栈操作（删除链表头部节点）
template <typename T>
T pop(Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");  // 空栈出栈抛异常
    }
    Node<T>* temp = s.top;  // 暂存栈顶节点
    T value = temp->data;   // 保存栈顶元素值
    s.top = s.top->next;    // 栈顶指针后移（指向新栈顶）
    delete temp;            // 释放原栈顶节点内存
    s.size--;               // 元素数量减1
    return value;           // 返回出栈元素值
}

// 获取栈顶元素（返回引用，可修改）
template <typename T>
T& top(Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");
    }
    return s.top->data;  // 返回栈顶节点的数据引用
}

// 获取栈顶元素（常量版本，不可修改）
template <typename T>
const T& top(const Stack<T>& s) {
    if (isEmpty(s)) {
        throw std::underflow_error("Stack is empty");
    }
    return s.top->data;  // 返回栈顶节点的常量数据引用
}

// 清空栈（释放所有节点，保留栈结构）
template <typename T>
void clear(Stack<T>& s) {
    destroyStack(s);  // 直接复用销毁函数（释放所有节点）
    initStack(s);     // 重新初始化栈（栈顶为空，size为0）
}

// 打印栈元素（从栈顶到栈底）
template <typename T>
void print(const Stack<T>& s) {
    cout << "Stack (top to bottom): ";
    Node<T>* curr = s.top;
    while (curr != nullptr) {
        cout << curr->data << " ";  // 依次打印节点数据
        curr = curr->next;          // 移动到下一个节点
    }
    cout << endl;
}

// 示例用法
int main() {
    Stack<int> s;
    initStack(s);  // 初始化栈

    try {
        // 测试入栈
        push(s, 10);
        push(s, 20);
        push(s, 30);
        print(s);  // 输出: 30 20 10

        // 测试出栈
        cout << "Popped: " << pop(s) << endl;  // 输出: 30
        print(s);  // 输出: 20 10

        // 测试获取栈顶元素
        cout << "Top element: " << top(s) << endl;  // 输出: 20

        // 测试修改栈顶元素
        top(s) = 25;
        cout << "Modified top: " << top(s) << endl;  // 输出: 25

        // 测试栈状态
        cout << "Size: " << size(s) << endl;  // 输出: 2
        cout << "Is empty: " << (isEmpty(s) ? "Yes" : "No") << endl;  // 输出: No

        // 测试清空栈
        clear(s);
        cout << "After clearing: ";
        print(s);  // 输出: 空

        // 测试异常
        pop(s);  // 触发异常
    } catch (const exception& e) {
        cerr << "Exception: " << e.what() << endl;  // 输出: Stack is empty
    }

    // 释放栈内存
    destroyStack(s);
    return 0;
}
```

### **用栈实现括号匹配**

判断一个字符串中的括号（`()`、`[]`、`{}`）是否匹配（正确嵌套）。

```C++
#include <iostream>
#include <cstring>
#include <stdexcept>
using namespace std;

// 栈节点结构
struct StackNode {
    char data;
    StackNode* next;
};

// 初始化空栈
StackNode* initStack() {
    return nullptr;
}

// 入栈
void push(StackNode*& top, char c) {
    StackNode* newNode = new StackNode;
    newNode->data = c;
    newNode->next = top;
    top = newNode;
}

// 出栈
char pop(StackNode*& top) {
    if (top == nullptr) {
        throw underflow_error("栈空");
    }
    StackNode* temp = top;
    char c = temp->data;
    top = top->next;
    delete temp;
    return c;
}

// 获取栈顶元素
char getTop(StackNode* top) {
    if (top == nullptr) {
        return '\0'; // 栈空返回空字符
    }
    return top->data;
}

// 判断栈空
bool isEmpty(StackNode* top) {
    return top == nullptr;
}

// 括号匹配判断
bool isMatch(const char* s) {
    StackNode* stack = initStack();
    int len = strlen(s);
    
    for (int i = 0; i < len; i++) {
        char c = s[i];
        // 左括号入栈
        if (c == '(' || c == '[' || c == '{') {
            push(stack, c);
        }
        // 右括号判断匹配
        else if (c == ')' || c == ']' || c == '}') {
            if (isEmpty(stack)) {
                // 右括号多了
                delete stack;
                return false;
            }
            char topChar = pop(stack);
            // 检查是否匹配
            if ((c == ')' && topChar != '(') || 
                (c == ']' && topChar != '[') || 
                (c == '}' && topChar != '{')) {
                delete stack;
                return false;
            }
        }
    }
    
    // 栈空则完全匹配，否则左括号多了
    bool result = isEmpty(stack);
    delete stack; // 释放内存
    return result;
}

// 测试
int main() {
    const char* s1 = "({[]})"; // 匹配
    const char* s2 = "([)]";   // 不匹配
    const char* s3 = "(()";    // 不匹配
    
    cout << s1 << " 是否匹配: " << boolalpha << isMatch(s1) << endl; // true
    cout << s2 << " 是否匹配: " << boolalpha << isMatch(s2) << endl; // false
    cout << s3 << " 是否匹配: " << boolalpha << isMatch(s3) << endl; // false
    return 0;
}
```

# 队列

和[顺序表](https://xiecoding.cn/view/288.html)、[链表](https://xiecoding.cn/view/290.html)相比，队列的特殊性体现在以下两个方面：
1、元素只能从队列的一端进入，从另一端出去，如下图所示：

![队列存储结构](https://i-blog.csdnimg.cn/img_convert/11cbe82c4e8fafa21a24fb2d4a72d12b.gif)

我们将元素进入队列的一端称为“队尾”，进入队列的过程称为“入队”；将元素从队列中出去的一端称为“队头”，出队列的过程称为“出队”。

2、队列中各个元素的进出必须遵循“先进先出”的原则，即最先入队的元素必须最先出队

队列的基本操作（入队和出队）
1) 顺序队列的基本操作
顺序队列指的是用顺序表模拟实现的队列存储结构。

我们知道，队列具有以下两个特点：

数据从队列的一端进，从另一端出；
数据的入队和出队遵循"先进先出"的原则；
在顺序表的基础上，只要元素进出的过程遵循以上两个规则，就能实现队列结构。
用顺序表模拟实现队列，必然要先定义一个足够大的数组。不仅如此，为了遵守队列中数据从 "队尾进，队头出" 且 "先进先出" 的规则，还需要定义两个变量（top 和 rear）分别记录队头和队尾的具体位置，如图 1 所示：

![顺序队列实现示意图](https://i-blog.csdnimg.cn/img_convert/eaf1dd23493d0d006c9991105398728a.gif)

初始状态下，顺序队列中没有任何元素，因此 top 和 rear 重合，都位于 a[0] 处

**实现入队**

在图 1 的基础上，当有新元素入队时，依次执行以下两步操作：

1. 将新元素存储在 rear 记录的位置；
2. 更新 rear 的值（rear+1），记录下一个空闲空间的位置，为下一个新元素入队做好准备。

在图 1 基础上将 `{1,2,3,4}` 用顺序队列存储的实现操作如图 2 所示：

![数据进顺序队列的过程实现示意图](https://i-blog.csdnimg.cn/img_convert/660ec7975b1430d487c67ceae2883056.gif)

当有元素出队时，根据“先进先出”的原则，目标元素以及在它之前入队的元素要依次从队头出队。

出队操作的实现方法很简单，就是更新 top 的值（top+1）。例如，在图 2 基础上，顺序队列中元素逐个队列的过程如图 3 所示：

![数据出顺序队列的过程示意图](https://i-blog.csdnimg.cn/img_convert/e3ee63f2f6ef8c812a9ef6ea76dcb668.gif)

图 3 数据出顺序队列的过程示意图

注意，虽然数组中仍保存着 1、2、3、4 这些元素，但队列中的元素是依靠 top 和 rear 来判别的，因此图 3b) 显示的队列中确实不存在任何元素。

## 顺序链表

### **1. 数据结构定义**

- 队列结构体（`Queue<T>`，模板类）

  ：

  采用动态数组实现，包含 5 个核心成员：

  - `data`：指向动态数组的指针，存储队列元素（类型由模板参数`T`指定，如`int`、`string`等）；
  - `capacity`：队列的最大容量（动态数组的当前长度）；
  - `front`：队首索引，指向队列中第一个有效元素；
  - `rear`：队尾索引，指向队尾元素的**下一个位置**（新元素将插入此处）；
  - `count`：当前队列中的元素个数（简化状态判断）。

### **2. 核心操作函数**

#### **（1）初始化与销毁**

- **`initQueue`**：初始化队列，分配初始容量（默认 10）的动态数组，`front`和`rear`均置为 0，`count`置为 0（空队列）。
- **`destroyQueue`**：释放动态数组的内存，将`data`指针置为`nullptr`，避免内存泄漏。

#### **（2）状态查询**

- **`isEmpty`**：判断队列是否为空，若`count == 0`（无元素），返回`true`，否则返回`false`。
- **`size`**：直接返回`count`，即当前队列中元素的个数。

#### **（3）扩容操作**

- `resize`

  ：当队列满自动扩容，新容量为原容量的 2 倍。

  ```
  count == capacity
  ```

  步骤：

  1. 分配新数组（容量为原 2 倍）；
  2. 按顺序复制原队列的有效元素（从`front`到`rear-1`）到新数组；
  3. 释放原数组内存，更新`data`指针和`capacity`，重置`front = 0`、`rear = count`（新队列的队尾为元素个数位置）。

#### **（4）队列的核心操作**

- **`enqueue`（入队）**：
  在队尾添加元素。步骤：若队列满则先扩容 → 将新元素存入`data[rear]` → `rear`直接加 1（非循环，只向后移动） → `count`加 1。
  示例：队列原有`10、20`（`rear=2`），入队`30`后，`rear=3`，元素为`10、20、30`。
- **`dequeue`（出队）**：
  移除并返回队首元素。步骤：若队列为空，抛出`underflow_error`异常 → 保存`data[front]`的值 → `front`直接加 1（队首后移，不复用之前的空间） → `count`减 1 → 返回保存的值。
  - 优化逻辑：当`front`超过容量的一半时，将所有元素整体前移（覆盖`front`前的空闲空间），重置`front=0`、`rear=count`，避免`front`无限增长。
    示例：队首为`10`（`front=0`），出队后`front=1`，返回`10`，剩余元素为`20、30`。
- **`peek`（获取队首元素）**：
  返回队首元素的引用（可修改），若队列为空则抛出异常。提供常量版本（`const T& peek`）用于只读访问。

#### **（5）辅助操作**

- **`clear`**：清空队列，直接重置`front=0`、`rear=0`、`count=0`（无需删除元素，后续入队会覆盖旧值）。
- **`print`**：从队首到队尾打印所有元素（遍历`[front, rear)`范围的元素），为空时输出空序列。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;

// 单向队列结构体（基于动态数组）
template <typename T>
struct Queue {
    T* data;      // 动态数组存储元素
    int capacity; // 队列最大容量
    int front;    // 队首索引（指向第一个有效元素）
    int rear;     // 队尾索引（指向最后一个有效元素的下一个位置）
    int count;    // 当前元素个数
};

// 初始化队列
template <typename T>
void initQueue(Queue<T>& q, int initCapacity = 10) {
    q.data = new T[initCapacity];
    q.capacity = initCapacity;
    q.front = 0;    // 初始队首在0位置
    q.rear = 0;     // 初始队尾在0位置（无元素）
    q.count = 0;    // 元素个数为0
}

// 释放队列内存
template <typename T>
void destroyQueue(Queue<T>& q) {
    delete[] q.data;
    q.data = nullptr;
}

// 判断队列是否为空
template <typename T>
bool isEmpty(const Queue<T>& q) {
    return q.count == 0;
}

// 返回队列元素个数
template <typename T>
int size(const Queue<T>& q) {
    return q.count;
}

// 扩容（容量翻倍）
template <typename T>
void resize(Queue<T>& q) {
    int newCapacity = q.capacity * 2;
    T* newData = new T[newCapacity];
    
    // 复制元素（只复制有效元素，从front到rear-1）
    for (int i = 0; i < q.count; i++) {
        newData[i] = q.data[q.front + i];  // 非循环，直接按顺序复制
    }
    
    delete[] q.data;
    q.data = newData;
    q.front = 0;              // 新队首重置为0
    q.rear = q.count;         // 新队尾为元素个数位置
    q.capacity = newCapacity;
}

// 入队（队尾添加）
template <typename T>
void enqueue(Queue<T>& q, const T& value) {
    if (q.count == q.capacity) {  // 队列满则扩容
        resize(q);
    }
    q.data[q.rear] = value;       // 队尾位置存入元素
    q.rear++;                     // 队尾指针后移（非循环，直接+1）
    q.count++;
}

// 出队（队首删除）
template <typename T>
T dequeue(Queue<T>& q) {
    if (isEmpty(q)) {
        throw underflow_error("Queue is empty");
    }
    T value = q.data[q.front];    // 保存队首元素
    q.front++;                    // 队首指针后移（不复用前面的空间）
    q.count--;
    
    // 优化：当队首指针过大时，整体前移元素（可选，避免front无限增长）
    if (q.front > q.capacity / 2) {  // 例如front超过容量一半时
        for (int i = 0; i < q.count; i++) {
            q.data[i] = q.data[q.front + i];  // 元素整体前移
        }
        q.rear -= q.front;  // 队尾同步前移
        q.front = 0;        // 重置front为0
    }
    
    return value;
}

// 获取队首元素
template <typename T>
T& peek(Queue<T>& q) {
    if (isEmpty(q)) {
        throw underflow_error("Queue is empty");
    }
    return q.data[q.front];
}

// 常量版本获取队首元素
template <typename T>
const T& peek(const Queue<T>& q) {
    if (isEmpty(q)) {
        throw underflow_error("Queue is empty");
    }
    return q.data[q.front];
}

// 清空队列
template <typename T>
void clear(Queue<T>& q) {
    q.front = 0;
    q.rear = 0;
    q.count = 0;
}

// 打印队列（队首到队尾）
template <typename T>
void print(const Queue<T>& q) {
    cout << "Queue (front to rear): ";
    for (int i = q.front; i < q.rear; i++) {  // 非循环，直接遍历[front, rear)
        cout << q.data[i] << " ";
    }
    cout << endl;
}

// 示例测试
int main() {
    Queue<int> q;
    initQueue(q);

    try {
        enqueue(q, 10);
        enqueue(q, 20);
        enqueue(q, 30);
        print(q);  // 输出：10 20 30

        cout << "Dequeued: " << dequeue(q) << endl;  // 输出：10
        print(q);  // 输出：20 30

        cout << "Front: " << peek(q) << endl;  // 输出：20
        peek(q) = 25;
        print(q);  // 输出：25 30

        cout << "Size: " << size(q) << endl;  // 输出：2

        clear(q);
        print(q);  // 输出空

        dequeue(q);  // 触发异常
    } catch (const exception& e) {
        cerr << "Exception: " << e.what() << endl;
    }

    destroyQueue(q);
    return 0;
}
```



## 顺序表循环队列 

- ### **1. 数据结构定义**

  - 队列结构体（`Queue<T>`，模板类）

    ：

    采用动态数组结合 “循环结构” 实现，包含 5 个核心成员：

    - `data`：指向动态数组的指针，存储队列元素（类型由模板参数`T`指定，如`int`、`string`等）；
    - `capacity`：队列的最大容量（动态数组的长度）；
    - `front`：队首元素的索引（指向队列中第一个有效元素）；
    - `rear`：队尾元素的**下一个位置**的索引（新元素将插入此处）；
    - `count`：当前队列中的元素个数（简化状态判断，避免通过`front`和`rear`计算）。

  ### **2. 核心操作函数**

  #### **（1）初始化与销毁**

  - **`initQueue`**：初始化队列，分配初始容量（默认 10）的动态数组，将`front`和`rear`置为 0（初始位置），`count`置为 0（空队列）。
  - **`destroyQueue`**：释放动态数组占用的内存，将`data`指针置为`nullptr`，避免内存泄漏。

  #### **（2）状态查询**

  - **`isEmpty`**：判断队列是否为空，若`count == 0`（无元素），返回`true`，否则返回`false`。
  - **`size`**：直接返回`count`，即当前队列中元素的个数。

  #### **（3）扩容操作**

  - `resize`

    ：当队列满（

    ```
    count == capacity
    ```

    ）时自动扩容，新容量为原容量的 2 倍。

    步骤：

    1. 分配新数组（容量为原 2 倍）；
    2. 复制原队列元素到新数组（按`front`到`rear`的顺序，通过`(front + i) % capacity`计算原索引）；
    3. 释放原数组内存，更新`data`指针、`capacity`，并重置`front = 0`、`rear = count`（新队列的队尾为元素个数位置）。

  #### **（4）队列的核心操作**

  - **`enqueue`（入队）**：
    在队尾添加元素。步骤：若队列满则先扩容 → 将新元素存入`data[rear]` → 更新`rear`为`(rear + 1) % capacity`（循环特性，避免越界） → `count`加 1。
    示例：队列原有`10、20`（`front=0`，`rear=2`），入队`30`后，`rear=3`，`count=3`，元素为`10、20、30`。
  - **`dequeue`（出队）**：
    移除并返回队首元素。步骤：若队列为空，抛出`underflow_error`异常 → 保存`data[front]`的值 → 更新`front`为`(front + 1) % capacity`（循环移动队首） → `count`减 1 → 返回保存的值。
    示例：队首为`10`（`front=0`），出队后`front=1`，返回`10`，剩余元素为`20、30`。
  - **`peek`（获取队首元素）**：
    返回队首元素的引用（可修改），若队列为空则抛出异常。提供常量版本（`const T& peek`）用于只读访问。

  #### **（5）辅助操作**

  - **`clear`**：清空队列，直接重置`front=0`、`rear=0`、`count=0`（无需删除元素，后续入队会覆盖旧值）。
  - **`print`**：从队首到队尾打印所有元素（通过`(front + i) % capacity`遍历元素）。

```C++
#include <iostream>
#include <stdexcept>
using namespace std;

// 定义队列结构体
template <typename T>
struct Queue {
    T* data;      // 动态数组存储队列元素
    int capacity; // 队列的最大容量
    int front;    // 队首元素的索引
    int rear;     // 队尾元素的下一个位置索引
    int count;    // 当前队列中的元素个数
};

// 初始化队列
template <typename T>
void initQueue(Queue<T>& q, int initCapacity = 10) {
    q.data = new T[initCapacity];
    q.capacity = initCapacity;
    q.front = 0;
    q.rear = 0;
    q.count = 0;
}

// 释放队列内存
template <typename T>
void destroyQueue(Queue<T>& q) {
    delete[] q.data;
    q.data = nullptr;
}

// 判断队列是否为空
template <typename T>
bool isEmpty(const Queue<T>& q) {
    return q.count == 0;
}

// 返回队列的当前元素个数
template <typename T>
int size(const Queue<T>& q) {
    return q.count;
}

// 扩容函数
template <typename T>
void resize(Queue<T>& q) {
    int newCapacity = q.capacity * 2;
    T* newData = new T[newCapacity];
    
    // 复制原队列元素到新数组
    for (int i = 0; i < q.count; i++) {
        newData[i] = q.data[(q.front + i) % q.capacity];
    }
    
    // 释放原数组内存并更新指针和索引
    delete[] q.data;
    q.data = newData;
    q.front = 0;
    q.rear = q.count;
    q.capacity = newCapacity;
}

// 入队操作
template <typename T>
void enqueue(Queue<T>& q, const T& value) {
    if (q.count == q.capacity) {
        resize(q);
    }
    q.data[q.rear] = value;
    q.rear = (q.rear + 1) % q.capacity;
    q.count++;
}

// 出队操作
template <typename T>
T dequeue(Queue<T>& q) {
    if (isEmpty(q)) {
        throw std::underflow_error("Queue is empty");
    }
    T value = q.data[q.front];
    q.front = (q.front + 1) % q.capacity;
    q.count--;
    return value;
}

// 获取队首元素
template <typename T>
T& peek(Queue<T>& q) {
    if (isEmpty(q)) {
        throw std::underflow_error("Queue is empty");
    }
    return q.data[q.front];
}

// 获取队首元素（常量版本）
template <typename T>
const T& peek(const Queue<T>& q) {
    if (isEmpty(q)) {
        throw std::underflow_error("Queue is empty");
    }
    return q.data[q.front];
}

// 清空队列
template <typename T>
void clear(Queue<T>& q) {
    q.front = 0;
    q.rear = 0;
    q.count = 0;
}

// 打印队列元素
template <typename T>
void print(const Queue<T>& q) {
    cout << "Queue (front to rear): ";
    for (int i = 0; i < q.count; i++) {
        cout << q.data[(q.front + i) % q.capacity] << " ";
    }
    cout << endl;
}

// 示例用法
int main() {
    Queue<int> q;
    initQueue(q);

    try {
        // 测试入队
        enqueue(q, 10);
        enqueue(q, 20);
        enqueue(q, 30);
        print(q);  // 输出: 10 20 30

        // 测试出队
        cout << "Dequeued: " << dequeue(q) << endl;  // 输出: 10
        print(q);  // 输出: 20 30

        // 测试获取队首元素
        cout << "Front element: " << peek(q) << endl;  // 输出: 20

        // 测试修改队首元素
        peek(q) = 25;
        cout << "Modified front: " << peek(q) << endl;  // 输出: 25

        // 测试队列状态
        cout << "Size: " << size(q) << endl;  // 输出: 2
        cout << "Is empty: " << (isEmpty(q) ? "Yes" : "No") << endl;  // 输出: No

        // 测试清空队列
        clear(q);
        cout << "After clearing: ";
        print(q);  // 输出: 空

        // 测试异常
        dequeue(q);  // 触发异常
    } catch (const exception& e) {
        cerr << "Exception: " << e.what() << endl;
    }

    // 释放队列内存
    destroyQueue(q);
    return 0;
}
```

## 队列的链式存储

### **1. 数据结构定义**

- **节点结构体（`Node`）**：
  队列的基本组成单位，每个节点包含：
  - `data`：存储整型元素值（如 10、20 等）；
  - `next`：指针，指向队列中的下一个节点（队尾方向）。
- **队列结构体（`Queue`）**：
  管理队列的核心结构，包含两个指针：
  - `front`：指向队首节点（出队操作的起点）；
  - `rear`：指向队尾节点（入队操作的终点）。

### **2. 核心操作函数**

#### **（1）初始化与状态判断**

- **`initQueue`**：初始化空队列，返回一个`front`和`rear`均为`nullptr`的队列结构体。
- **`isEmpty`**：判断队列是否为空，若`front`为`nullptr`（无队首节点），返回`true`，否则返回`false`。

#### **（2）入队与出队**

- **`enqueue`（入队）**：
  在队尾添加元素，时间复杂度为`O(1)`。
  - 步骤：创建新节点 → 若队列为空（`front`和`rear`均为`nullptr`），则`front`和`rear`均指向新节点；若队列非空，原队尾节点的`next`指向新节点，更新`rear`为新节点。
  - 示例：队列原有`10`（`front=rear=10`），入队`20`后，`10->next=20`，`rear=20`，队列变为`10→20`。
- **`dequeue`（出队）**：
  移除并返回队首元素，时间复杂度为`O(1)`。
  - 步骤：若队列为空，抛出`underflow_error`异常 → 暂存队首节点 → 保存节点数据 → 更新`front`为下一个节点 → 若出队后队列为空（`front`为`nullptr`），同步更新`rear`为`nullptr` → 删除原队首节点 → 返回数据。
  - 示例：队首为`10`，出队后`front=20`，返回`10`，队列变为`20`。

#### **（3）查询与辅助操作**

- **`getFront`**：返回队首元素的值（不删除），若队列为空则抛出异常，时间复杂度`O(1)`。
- **`getSize`**：计算队列中元素的个数，通过遍历链表计数，时间复杂度`O(n)`（`n`为元素个数）。
- **`clearQueue`**：清空队列，通过反复调用`dequeue`删除所有节点，释放内存，避免泄漏。
- **`printQueue`**：从队首到队尾打印所有元素（遍历链表），为空时输出 “队列为空”。

```C++
#include <iostream>
#include <stdexcept>  // 用于异常处理
using namespace std;

// 链表节点结构体（队列的每个元素对应一个节点）
struct Node {
    int data;       // 存储队列元素值
    Node* next;     // 指向下一个节点的指针
};

// 队列结构体（包含队首和队尾指针）
struct Queue {
    Node* front;    // 队首指针（出队端）
    Node* rear;     // 队尾指针（入队端）
};

// 初始化空队列
Queue initQueue() {
    Queue q;
    q.front = nullptr;  // 队首为空
    q.rear = nullptr;   // 队尾为空
    return q;
}

// 判断队列是否为空
bool isEmpty(Queue q) {
    return q.front == nullptr;  // 队首为nullptr则队列为空
}

// 入队操作（在链表尾部插入节点，O(1)）
void enqueue(Queue& q, int value) {  // 用引用接收队列，可能修改队首/队尾
    Node* newNode = new Node;        // 创建新节点
    newNode->data = value;
    newNode->next = nullptr;         // 新节点为尾节点，next为nullptr
    
    if (isEmpty(q)) {                // 队列为空时，新节点既是队首也是队尾
        q.front = newNode;
        q.rear = newNode;
    } else {                         // 队列非空时，将新节点连接到队尾
        q.rear->next = newNode;      // 原队尾的next指向新节点
        q.rear = newNode;            // 更新队尾指针为新节点
    }
}

// 出队操作（删除链表头部节点，O(1)）
int dequeue(Queue& q) {
    if (isEmpty(q)) {
        throw underflow_error("队列空，无法出队");  // 空队列抛异常
    }
    
    Node* temp = q.front;           // 暂存队首节点
    int value = temp->data;         // 保存队首元素值
    
    q.front = q.front->next;        // 队首指针后移
    
    if (q.front == nullptr) {       // 如果出队后队列为空
        q.rear = nullptr;           // 队尾指针也置空
    }
    
    delete temp;                    // 释放原队首节点内存
    return value;                   // 返回出队元素值
}

// 获取队首元素（不删除，O(1)）
int getFront(Queue q) {
    if (isEmpty(q)) {
        throw underflow_error("队列空，无队首元素");  // 空队列抛异常
    }
    return q.front->data;  // 返回队首节点的值
}

// 获取队列的大小（元素个数，O(n)）
int getSize(Queue q) {
    int size = 0;
    Node* curr = q.front;
    while (curr != nullptr) {  // 遍历链表计数
        size++;
        curr = curr->next;
    }
    return size;
}

// 清空队列（释放所有节点内存）
void clearQueue(Queue& q) {
    while (!isEmpty(q)) {
        dequeue(q);  // 反复出队直到队列为空
    }
}

// 打印队列元素（从队首到队尾）
void printQueue(Queue q) {
    if (isEmpty(q)) {
        cout << "队列为空" << endl;
        return;
    }
    
    cout << "队列（首->尾）: ";
    Node* curr = q.front;
    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }
    cout << endl;
}

// 示例用法
int main() {
    Queue queue = initQueue();  // 初始化空队列
    
    try {
        // 测试入队
        enqueue(queue, 10);
        enqueue(queue, 20);
        enqueue(queue, 30);
        printQueue(queue);  // 输出：队列（首->尾）: 10 20 30 
        
        // 测试队列状态
        cout << "队列大小: " << getSize(queue) << endl;  // 输出：3
        cout << "队首元素: " << getFront(queue) << endl;  // 输出：10
        
        // 测试出队
        cout << "出队元素: " << dequeue(queue) << endl;  // 输出：10
        printQueue(queue);  // 输出：队列（首->尾）: 20 30 
        
        // 继续出队至空
        dequeue(queue);
        dequeue(queue);
        cout << "全部出队后，队列是否为空: " << boolalpha << isEmpty(queue) << endl;  // 输出：true
        
        // 测试空队列异常
        dequeue(queue);  // 触发异常
    } catch (const exception& e) {
        cerr << "错误: " << e.what() << endl;  // 输出：队列空，无法出队
    }
    
    // 清空队列（示例中已空，实际使用时需调用）
    clearQueue(queue);
    return 0;
}
```

