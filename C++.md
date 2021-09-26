# C++

## 1.关于c++中的内存区，代码区、常量区、栈区、堆区。

其中常量区有全局变量、静态变量、常量（全局常量const，字符串常量）。

栈区是局部变量以及形参等，注意**不要返回局部变量地址**。

释放堆区，如果是数组需要加[] 如：delete[] arr。

------



## 2.关于模板

### 2.1 函数模板

- **关键字<u>template</u>**   

- **关键字<u>typename</u>**    **T**   
- **typename**可换成<u>**class**</u>

```c++
template<typename T>
void myFunc(T& a,T& b)
{
    /////////模板方法//////////
}

int a,b
//自动类型推导  //必须推导出一致的数据类型T  //必须确定出T的类型
myFunc(a,b);
//显示指定类型
myFunc<int>(a,b);

//模板提高了复用性，将类型参数化

```

------



#### 2.1.1 普通函数与函数模板的区别

- 普通函数可以**隐式类型转换**
- 函数模板的**自动类型推导**不可以**隐式类型转换**
- 函数模板的**显式指定类型**可以**隐式类型转换**

比如：

```c++
int a=10;
char c='c';

int myAdd(int a, int b){}
myAdd(a,c);//这是一个普通函数，结果为109，c的asic码为99，可以隐式类型转换

template<class T>
void myAdd1(T& a,T& b){}
myAdd1(a,c);//这个就不行，模板自动类型推导不能隐式转换
myAdd1<int>(a,c);//这个行，显式指定类型可以隐式类型转换
```

***总结：建议使用显式指定类型***

------



#### 2.1.2 普通函数与函数模板的选择与调用规则

- 模板重载普通函数，导致都能用的情况，编译器优先调用**优先普通函数**，哪怕普通函数只有声明，就会出现报错
- 空模板参数列表，可强制调用函数模板
- 模板也可以重载
- 模板可以更好匹配时（参数类型），编译器**优先调用模板**

```c++
void add（int a,int b);

template<class T>
void add(T& a,T& b){}

int a,b=10;
add(a,b);//此时运行会报错，提示声明未实现
add<>(a,b);//空模板列表，强制使用模板，不会报错

template<class T>
void add(T& a,T& b,T& c){}
int c=10;
add(a,b,c);//模板也可以重载

char d,e='123';
add(d,e);//相比于参数为int的普通函数，模板更好的匹配char类型参数，此时会调用模板
```

***总结：使用了模板，就尽量不要使用普通函数***

------



#### 2.1.3 函数模板对自定义类型需要重载

- 比较大小的函数，对内置类型int等可以比较，但对**自定义类型**如：person则不好比较

```c++
template<class T>
bool Compare(T a,T b)
{
    if(a==b) return true;
    else return false;
}

class Person(string name,int age)
{
    ////////
}

int a,b=1;
Person c,d=Person("tom",10);

Compare(a,b);//正确
Compare（c,d)//报错，无法对比
    
template<> bool Compare(Person& p1,Persom& p2)//对自定义类型单独重载，注意写法
{
    if（p1.age==p2.age) return true;
    else return false;
}

Compare(c,d);//正确，编译器调用了Compare的person重载版本
```

**总结：具体化的模板，解决 <u>*模板通用化问题*</u>**

------



### 2.2 类模板

- **template**<typename T>
- **class** XXX

```c++
template<class NameType,class AgeType>
class Person
{
    public:
    	Person(NameType name,AgeType age)
        {
            this->name=name;
            this->age=age;
        }
    NameType name;
    AgeType age;
};

Person<string,int>p1("qtx",88);//< >内声明模板需要的类型
```

**总结：用法与函数模板类似，下面写个类**

------



#### 2.2.1 类模板与函数模板区别

- 类模板没有自动类型推导
- 类模板参数列表可以设置默认值

```
Person p1("qtx",88);//报错，无法自动类型推导
Person<string,int> p1("qtx",88);//< >内声明模板需要的类型


template<class NameType,class AgeType=int>//设置AgeType默认值为int
class Pig
{
    public:
    	Person(NameType name,AgeType age)
        {
            this->name=name;
            this->age=age;
        }
    NameType name;
    AgeType age;
};

Pig<string> p2("pp",99);//设置了默认值int，所以< >中不需要声明模板参数类型
```

**总结：注意类模板要使用指定的类型**，**同时可以有默认类型参数**

------



#### **2.2.2** **类模板中成员函数创建时机**

- 普通类一开始就创建
- 类模板调用时才创建

------



#### **2.2.3** **类模板对象做函数参数**

- 指定传入类型
- 参数模板化
- 整个类模板化

```c++
template<class NameType,class AgeType>
class Person
{
    public:
    	Person(NameType name,AgeType age)
        {
            this->name=name;
            this->age=age;
        }
    NameType name;
    AgeType age;
};

Person<string,int> p("qtx",88);
func01(p);
void func01(Person<string,int>& person){}//指定传入类型,将类模板类型传入

func02(p);
template<class T1,class T2>
void func02(Person<T1,T2>& person){}//参数模板化，将这个方法参数类型模板化

func03(p);
template<class T>
void func03(T& person){};//将整个类模板化，当成参数传递，模板自动推导

```

**总结**：**一般用第一种**，**直接指定传入的类型**

------



#### 2.2.4 类模板与继承

- 子类继承模板父类时，必须要知道父类中的T类型具体是什么，因为它需要分配内存空间
- 如果想灵活指定T的类型，子类也需要为类模板

```c++
template<class T>
class Father
{
    T m;
}

class Son:public Father//错误，必须指定T的类型
    
class Son:public Father<int>//正确
{
    
}

template<class T1,class T2>//正确，子类也为类模板
class Son2:public Father<T1>
{
    T2 obj;
}
```

**总结：继承类模板需要指定类型，或者子类也为类模板**

------



#### 2.2.5 类模板成员函数类外实现

- 需要指明作用域以及模板参数

```c++
template<class T1,class T2>
class Person
{
    public:
    	Person(T1 name,T2 age)
        {
            this->name=name;
            this->age=age;
        }
    	
    	void Func();
    T1 name;
    T2 age;
};

template<class T1,class T2>
void Person<T1,T1>::Func(){}//声明了Person作用域 与 <T1,T1>模板参数列表
```

总结：注意写法，类外**实现**（类模板内已声明）类模板的成员函数需要**<u>*指定类模板作用域与模板参数*</u>**

------



#### **2.2.6** **类模板分文件编写**

- **问题：类模板分成.h与.cpp文件后，类模板成员函数在调用阶段创建，分文件时链接不到**

  （未创建，找不到调用的类模板的成员函数）

  **报错：无法解析的外部命令**

  **解决：直接包含cpp源文件（指的是，#include .cpp文件为不是.h文件)**

  ​	#include .cpp文件与.h文件的区别是：include .cpp时，会看cpp的代码，此时类模板成员函数已经创建,第一行是#include .h文件，所以会去.h看一遍声明，然后回到.cpp看完具体实现,

  ​	<u>一般不这么做</u>

  **解决：声明与实现放在一个文件中，改后缀为hpp后#include .hpp**

  ​	<u>常用方式</u>

------



## 3 STL

内容是**容器**、**算法**、**迭代器**、**仿函数**、**适配器**、**空间配置器**

STL基本都采用了模板，**容器**和**算法**通过**迭代器**来连接

仿函数：类似函数，可作为算法的策略

适配器：修饰容器或仿函数或迭代器接口的东西

空间配置器：负责空间配置与管理

序列式容器：无序

关联式容器：有序

质变式算法：拷贝、添加、删除等

非质变式算法：查找、计数、遍历等

```c++
#include <algorithm> //stl算法头文件

vector<int> v;
//迭代器可以理解为一个指针
vector<int>::iterator itBegin = v.begin();//指向容器第一个位置
vector<int>::iterator itEnd = v.end();//注意：指向容器最后的下一个位置
//遍历输出容器内容
for(vector<int>::iterator it=v.begin();it!=v.end();it++)
{
    cout<<*it<<endl;
}
//第二种方式  stl遍历算法
void myPrint(val)
{
    cout<<val<<endl;
}
for_each(v.begin(),v.end(),myPrint);//第三个参数（函数名）利用回调
```

### 3.1 string

**本质：**

- string是一个**类**

**string和char*的区别：**

- char*是指针
- string类内封装char*
- 是一个char*型的容器

**特点：**

- 内部许多方法如：find、copy、delete、replace、insert
- string管理char*分配内存，不用担心复制越界、取值越界等

#### 3.1.1 string构造方法

- string ();  //  string s;
- string<const char* s);  //  const char* str="daff";  string s(str);
- string<const string& str);  //  string s1=string(s);
- string<int n,char c);  //  string s(5,'a');

#### 3.1.2 string赋值操作 

- string s ="dawda";
- string s= str1; 

#### 3.1.3 string拼接

- string s  +=  "fsefse";
- string s.append("fsfs");
- string s.append("fsfs",2);//表示拼接前两个
- string s.append("fsfs",2，1);//表示第二个开始，拼接一个

#### 3.3.4 string查找和替换

-  str1.find("de");//查找后返回int，表示字符串起始位置下标，若无返回-1
- str1.rfind("de");//从右向左查找

## 4 指针与内存管理

**常量指针**：指向的值为常量，可以理解为const *类型

```c++
int const* ptr;
```

**指针常量**：指向的地址为常量，区别为*的位置

```c++
int *const ptr;
```

**void指针**与c++11指针类型转换：c中指针都是void无格式指针

```c++
//其他类型指针可直接转换为void*
int num=1;
void* void_ptr=&num;

//void*转其他类型需要特殊转换，否则报错
int* ptr =void_ptr;//XXXX错误
int* ptr =static_cast<int*>(void_ptr);//增加了验证，不会转换const
int* ptr =（int*）void_ptr;//会转换const
int* ptr =const_cast<int*>(void_ptr);//会转换const,区别是你知道他转化了const

//重新定义类型
unsigned char* char_ptr=new unsigned char[1024];
auto ptr=reinterpret_cast<int*>(char_ptr);

```

### 4.1 **智能指针**

**解决的问题**：空指针野指针，重复释放，new delete不匹配，内存泄漏



#### 4.1.1 **std::unique_ptr**

 **独占指针，所指内存独有，只有这个只能能指向这个内存，不支持拷贝、赋值**

```c++
//必须直接初始化，未初始化时为空指针
std::unique_ptr<int> ptr1(new int(10));
std::unique_ptr<int> ptr2 = std::move(ptr1);//ptr2为唯一的指针
ptr2.reset();//释放ptr2所指向的内存空间
//reset的其他用法
int* ptr=new int(10);
ptr2.reset(ptr);//释放之前的，并指向ptr
int *p = ptr2.release();//释放ptr2的控制权，并不会释放内存
```



#### 4.1.2 **std::shared_ptr** 

允许多个该类型指针指向同一个堆内存 **“引用计数”**实现，记录指向同一内存的share-ptr个数，如果个数为零，该堆内存自动删除，支持赋值和复制

```c++
std::share_ptr<int> ptr1(new int(10));
std::share_ptr<int> ptr2=ptr1;
std::cout<<"引用个数："<<ptr2.use_count()<<std::endl;//2

ptr1.reset();//此时，引用数-1

//安全的使用方法，make_share方法，动态分配一个对象的内存，并返回以一个指向此对象的share_ptr;需要指定创建的对象的类型，或者用auto接。
share_ptr<int> ptr3=make_share<int>(10);
auto ptr4 = make_share<vector<int>>();

//一些操作
shared_ptr<T> p;
//空智能指针，可指向类型是T的对象
 
if(p)
 //如果p指向一个对象,则是true
 
(*p)
//解引用获取指针所指向的对象
 
p -> number == (*p).number;
 
p.get();
//返回p中保存的指针
 
swap(p,q);
//交换p q指针
 
p.swap(q);
//交换p,q指针
 
make_shared<T>(args) 
//返回一个shared_ptr的对象，指向一个动态类型分配为T的对象，用args初始化这个T对象
 
shared_ptr<T> p(q)
//p 是q的拷贝，q的计数器++，这个的使用前提是q的类型能够转化成是T*
  
p =q;
//p的引用计数-1，q的+1,p为零释放所管理的内存
 
p.unique();
//判断引用计数是否是1，是，返回true
 
p.use_count();
//返回和p共享对象的智能指针数量

```



#### 4.1.3 **std::weak_ptr** 

辅助share_ptr使用，可从share_ptr/weak_ptr构造,weak_ptr**不会影响引用计数**，指向share_ptr内存，但不拥有该内存，所以不可以直接使用内存资源，除非通过weak_ptr构造一个share_ptr, 成员lock可返回指向内存对应的share_ptr。

```c++
std::share_ptr<int> ptr1(new int(10));
std::weak_ptr<int> wp = ptr1;//无拥有权
std::share_ptr<int> ptr2 = wp.lock();
```



**为什么使用weak_ptr**？

解决share_ptr循环引用问题

如下图，创建ptr_a,ptr_b指向两个对象，m_ptr_a,m_ptr_b又指向互相，当1，3销毁时2，4会导致引用计数不为0，从而导致无法释放。

**解决**：将m_ptr_a,m_ptr_b设置为weak_ptr后，不会增加引用计数，可以正常销毁


![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5raW5nd2F5LmZ1bi9JTUdNYXRyaXgvYmxvZy9jcHAvYzExMDAxLnBuZw)

#### 4.1.4 智能指针的实现

```c++
1 #include <iostream>
 2 #include <memory>
 3 
 4 template<typename T>
 5 class SmartPointer {
 6 private:
 7     T* _ptr;
 8     size_t* _count;
 9 public:
10     SmartPointer(T* ptr = nullptr) :
11             _ptr(ptr) {
12         if (_ptr) {
13             _count = new size_t(1);
14         } else {
15             _count = new size_t(0);
16         }
17     }
18 
19     SmartPointer(const SmartPointer& ptr) {
20         if (this != &ptr) {
21             this->_ptr = ptr._ptr;
22             this->_count = ptr._count;
23             (*this->_count)++;
24         }
25     }
26 
27     SmartPointer& operator=(const SmartPointer& ptr) {
28         if (this->_ptr == ptr._ptr) {
29             return *this;
30         }
31 
32         if (this->_ptr) {
33             (*this->_count)--;
34             if (this->_count == 0) {
35                 delete this->_ptr;
36                 delete this->_count;
37             }
38         }
39 
40         this->_ptr = ptr._ptr;
41         this->_count = ptr._count;
42         (*this->_count)++;
43         return *this;
44     }
45 
46     T& operator*() {
47         assert(this->_ptr == nullptr);
48         return *(this->_ptr);
49 
50     }
51 
52     T* operator->() {
53         assert(this->_ptr == nullptr);
54         return this->_ptr;
55     }
56 
57     ~SmartPointer() {
58         (*this->_count)--;
59         if (*this->_count == 0) {
60             delete this->_ptr;
61             delete this->_count;
62         }
63     }
64 
65     size_t use_count(){
66         return *this->_count;
67     }
68 };
69 
70 int main() {
71     {
72         SmartPointer<int> sp(new int(10));
73         SmartPointer<int> sp2(sp);
74         SmartPointer<int> sp3(new int(20));
75         sp2 = sp3;
76         std::cout << sp.use_count() << std::endl;
77         std::cout << sp3.use_count() << std::endl;
78     }
79     //delete operator
80 }
```















