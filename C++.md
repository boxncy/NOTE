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
- ：：作用域限制符 全局查找模板 如 ::max(),不加有可能调用std：：max（）

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

STL基本都采用了模板，**容器**和**算法**通过**迭代器**来连接,**所有不支持随机访问的容器，不可以用标准算法，只能用内部算法**

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

for(vector<int>::iterator elem : vector1)
for(auto elem:vector_1)  //一种遍历容器方式，可以是任何容器，注意auto主要是可以代替迭代器那种比较长的写法
{
    
}
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

#### 3.1.4 string查找和替换

-  str1.find("de");//查找后返回int，表示字符串起始位置下标，若无返回-1
-  str1.rfind("de");//从右向左查找
-  str1.replace(1,3,"1111");//从1开始3个字符，替换为1111

#### 3.1.5 string字符串比较

**比较是通过逐个ascll码比较**

- int compare(const string &s) const; // 0 = ,1 > , -1 <
- str1.compare(str2)==0;

#### 3.1.6 string字符存取，访问单个字符

- str1[i]="s"
- str1.at[i]="s"

#### 3.1.7 string插入删除

- str1.insert(1,"111");  //1位置后插入111
- str1.erase(1,"111");  //1位置删除111

#### 3.1.8 string子串

- substr = str1.substr(1,3);  // 1 位置截取3个

------

### 3.2 vector

单端数组，可以动态扩展，找到大空间，把原数据拷到新空间，释放原空间

![image-20210930141702656](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20210930141702656.png)

#### 3.2.1 vector构造

```c++
 vector<int> v;
 vector(v.begin(),v.end());  //前闭后开，end指向最后一个元素下一个位置
 vector(n,elem);			 //n个elem
 vector(const vector &v);	 //拷贝

例：
    vector<int>v2(v1);
```

#### 3.2.2 vector赋值

```c++
vector<int>v1;
vector<int>v2=v1;  //等号赋值
vector<int>v3.assign(v1.begin(),v1.end());  //迭代器
vector<int>v4.assign(10,100)				//10个100

for(vector<int>::iterator it=v.begin();it!=v.end();it++)
```

#### 3.2.3 vector容量和大小

- empty();
- capacity();
- size();
- resize(int num); //重新指定长度，比原先长则默认值填充，比原先短则删除后面的元素
- resize(int num , elem);   //重新指定长度，比原先长则elem填充比原先短则删除后面的元素

####  3.2.4 vector插入删除

- push_back(elem);
- pop_back();
- insert(const_iterator pos , elem);
- insert(const_iterator pos , int count , elem);
- erase(const_iterator pos);
- erase(const_iterator start , const_iterator end);
- clear();

#### 3.2.5 vector数据存取

 **注意[]和at都是随机访问，只能访问连续空间的容器**

- at(int idx);  //返回索引处数据
- []
- front();        //返回第一个
- back();         //返回最后一个

#### 3.2.5 vector互换容器

- swap(vector);

```c++
for(int i=0;i<100000;i++)
{
    v1.push_back(i);
}

//v.capacity此时大于100000

v.resize(3);
//此时还是大于100000，size=3

vector<int>(v).swap(v);
//vector<int>(v)创建匿名对象，大小容量为3，与v交换，匿名对象此行后系统回收
```

#### 3.2.6 vector预留空间

- reserve(int len);  //分配len长度内存，但无初始值，不可访问

------



### 3.3 deque

双端数组，头尾插入删除；

vector头部操作慢，deque快；

支持随机访问，但随机访问vector快；

![image-20211004160353306](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211004160353306.png)

deque工作原理：并不是全部连续的内存空间，有一个**中控器**，其中的缓存区存放真实数据，中控器中放缓存区地址；

![image-20211004160724715](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211004160724715.png)

#### 3.3.1 deque构造函数

```c++
include<deque>

deque<int>d1();    //默认
deque<int>d2(d1.begin(),d2.end());    //区间
deque<int>d3(10,1);    //确定元素个数
deque<int>d4(d1);    //拷贝
```

#### 3.3.2 deque赋值

```c++
deque<int>v1;
deque<int>v2=v1;  //等号赋值
deque<int>v3.assign(v1.begin(),v1.end());  //迭代器
deque<int>v4.assign(10,100)				//10个100
```

#### 3.3.3 deque大小操作

- empty();
- **capacity();    （X)     deque没有容量的概念**
- size();
- resize(int num); //重新指定长度，比原先长则默认值填充，比原先短则删除后面的元素
- resize(int num , elem);   //重新指定长度，比原先长则elem填充比原先短则删除后面的元素

#### 3.3.4 deque插入删除

**两端操作**

- push_back(elem);

- push_front(elem);

- pop_back();

- pop_front();

  

- insert(const_iterator pos , elem);                             //返回新数据位置

- insert(const_iterator pos , int count , elem);          //无返回

- insert(const_iterator pos , begin,end);                    //插入区间元素，无返回

- erase(const_iterator pos);

- erase(const_iterator start , const_iterator end);    //返回下一个数据位置

- clear();                                                                           //返回下一个数据位置

#### 3.3.5 deque数据存取

- at(int idx);  //返回索引处数据
- []
- front();        //返回第一个
- back();         //返回最后一个

#### 3.3.6 deque排序

- sort(begin，end)；//从小到大，vector也可用，包含头文件algorithm

------



### 3.4 stack

栈，先进后出；

只有栈顶元素可以访问，所以不能遍历；

可以判断是否为空；

可以返回个数；

![image-20211004163058299](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211004163058299.png)

#### 3.4.1 常用接口

- stack<T> stk;
- stack(const stack &stk);   //拷贝

赋值

- stack = const stack &stk;

数据存取

- push(elem);
- pop();
- top();  //返回栈顶元素

大小操作

- empty();
- size();

### 3.5 queue

队列，先进先出；

只有队头队尾可以访问，随意不能遍历；



![image-20211004164042179](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211004164042179.png)

#### 3.5.1 queue常用接口

- queue<T> queue;
- queue(const queue &queue);   //拷贝

赋值

- queue= const stack &queue;

数据存取

- push(elem);
- pop();
- back(); 
- front();

大小操作

- empty();
- size();

------



### 3.6 list

stl中是双向循环链表

链表，由**结点**组成；

结点，**数据域**储存数据元素，**指针域**储存下一个结点地址；

快速插入删除，且迭代器不失效，vector失效；

空间大，遍历慢，不可随机访问；

![image-20211004165107752](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211004165107752.png)

#### 3.6.1 list构造

```c++
include<list>

list<int>l1();    //默认
list<int>l2(l1.begin(),l2.end());    //区间
list<int>l3(10,1);    //确定元素个数
list<int>l4(l1);    //拷贝
```

#### 3.6.2 list赋值交换

```c++
list<int>v1;
list<int>v2=v1;  //等号赋值
list<int>v3.assign(v1.begin(),v1.end());  //迭代器
list<int>v4.assign(10,100)				//10个100
list<int>v4.swap(v1)				//交换
```

#### 3.6.3 deque大小操作

- empty();
- **capacity();    （X)     deque没有容量的概念**
- size();
- resize(int num); //重新指定长度，比原先长则默认值填充，比原先短则删除后面的元素
- resize(int num , elem);   //重新指定长度，比原先长则elem填充比原先短则删除后面的元素

#### 3.6.4 list插入删除

- push_back(elem);

- push_front(elem);

- pop_back();

- pop_front();

  

- insert(const_iterator pos , elem);                             //返回新数据位置

- insert(const_iterator pos , int count , elem);          //无返回

- insert(const_iterator pos , begin,end);                    //插入区间元素，无返回

- erase(const_iterator pos);

- erase(const_iterator start , const_iterator end);    //返回下一个数据位置

- clear();                                                                           //返回下一个数据位置

- remove(elem)                                                              //删除所有等于elem的元素

#### 3.6.5 list数据存取

- front();
- back();

#### 3.6.6 list反转和排序

- reverse();
- sort();   //有一个重载，可以自定义方法排序

------

### 3.7 set

关联式容器，自动排序，底层为二叉树（红黑树）；

set不允许重复元素，muitiset允许；

#### 3.7.1 set构造和赋值

```c++
set<int> st;
set<int>st2(st);

set<int> st3 = st;
```

#### 3.7.1 set大小和交换

无resize，因为不能有重复元素

- size();
- empty();
- swap();

#### 3.7.2 set插入删除

- insert(elem);
- clear();
- erase(pos);
- erase(begin,end);
- erase(elem);

#### 3.7.2 set查找和统计

- find(key);  //查找key是否存在，存在则返回该元素迭代器，否则返回set.end();
- count(key);

#### 3.7.3 set与multiset

- set不可重复，multiset可以
- set插入同时返回插入结果(一个对组，第二个值为bool），multiset则无

#### 3.7.4 pair对组

- pair<type,type> p (value1,value2);
- pair<type,type> p =  make_pair(value1,value2);

#### 3.7.5 set排序

set本身是从大到小，利用仿函数改变规则

```c++
class MyCompare
{
    public:
    	bool operator()(int v1,int v2)
        {
            return v1>v2;//这是降序
		}
}

set<int,MyCompare>S2;
```

------

### 3.8 map/multimap

map中都是键值对<key,value>;

根据元素key自动排序；

关联式容器，底层为二叉树；

map中key不可重复，multimap可以；

key为map.first;   value: map.second;

#### 3.8.1 map构造赋值

```c++
map<T1,T2> MP;
map<T1,T2> MP2(MP);

mp3= mp;
```

#### 3.8.2 map大小和交换

- size();
- empty();
- swap();

#### 3.8.3 map插入删除

- insert(elem);   
- clear();
- erase(pos);
- erase(begin,end);
- erase(key);

```c++
mp.insert(pair<int,int>(10,10));
mp.insert(make_pair(10,10));
mp[1]=10;  //插入少用，如果该key没有对应值，会产生一个默认值，访问时用

mp.erase(1);
mp.erase(mp.begin());
```

#### 3.8.4 map查找和统计

- find(key);  //查找key是否存在，存在则返回该元素迭代器，否则返回map.end();
- count(key);

#### 3.8.5 map排序

set本身是从大到小，利用仿函数改变规则

```c++
class MyCompare
{
    public:
    	bool operator()(int v1,int v2)
        {
            return v1>v2;//这是降序
		}
}

map<int,int,MyCompare>mp;
```

------

### 右值引用

在将亡值离开其作用域（作为参数或者返回值传递），会调用拷贝构造，如果此时有重载的移动构造，将自动调用移动构造

创建临时对象a时，使用如：a = std::move(b); 将调用移动构造

forward则是保持变量的类型，完美转发

### 四种强制类型转换

#### static_cast

没有类型检查，所以用于安全性较高的类型转换，如基本数据类型。

(1) 基类与派生类的转换，上行转换是安全的，反之则不是。

(2) 基本数据类型的转换

(3) 空指针转换成目标类型空指针

(4) 任意类型的表达式转换为void

#### const_cast

去掉对象的**指针或引用**的常量特性，不能去掉变量的

```c++
const int a = 10;
int b = const_cast<int>(a); //error

const int* p = &a;
int * q = const_cast<int*>(p); //const_cast
```

#### reinterpret_cast

**比特拷贝**

(1) 不同类型的引用与指针之间的类型转换

(2) 将指针或引用转换成整型

(3) 将整形转换为指针或引用类型

#### dynamic_cast

**运行时转换，类型检查**，在基类与派生类的转换中，比static_cast要安全，

但转换的基类必须要包含虚函数（若无虚函数，基类与派生类的转换没有意义，类型检查需要运行时类型信息，存在虚函数表中），不能转换非多态的的基类与派生类，

**转换失败时返回 NULL**

```c++
Base* a;
InBase b;

a = dynamic_cast<InBase*> (&b);
Base& c = dynamic_cast<InBase&> (b);

if(a == NULL){
    ......
}
```







