越多的东西被封装，就有越大的弹性去改变

### 条款3：CONST

```c++
class Box
{
public:
	Box(double length, double height, const string& owner) //多用const&（速度快，同时进行类型检查）
		:length_(length), height_(height),owner_(owner) //初始化列表
	{ }
	double get_length() const { return length_; } //函数不改变成员，相当于传入的this指针为const
	const string& get_owner() const { return owner_; } //返回const
private:
	double length_ , height_;
	const string& owner_;
};
-----------------------------ps--------------------------------
1.使用const代替#define对于常量的定义
2.常量指针与指针常量主要看const后接的是类型名还是指针对象名
    const string* str;//指针常量
	string* const str;//常量指针
3.如果函数定义没有加const，使用时不可以返回const
	（X） const box.get_length(); //错误，定义没加const，返回值不能为const
4.常量迭代器，如果希望迭代器所指的值不改动，使用const_iterator
    std::vector<int>::const_iterator cIter = vec.begin();
5.函数常量性不同时（函数后的const，函数不改变成员）,可以被重载,常量对象不能调用非常量函数
6.注意，给值类型形参加上const是没有意义的，本身就是拷贝过来的
7.非const成员函数中，this指针是一个类类型的；在const成员函数中，this指针是一个const类类型的；
```

### 条款4：确保对象的初始化

- 尽量使用初始化列表，内置类型手工初始化
- 初始化时就赋值，代替default构造，再赋值
- 成员初始化次序，bass先于derived，bass中成员按照声明次序
- 不同编译单元初始化次序不固定，可以用单例static解决

```c++
class FileSystem{}
FileSystem(){
    static FileSystem fs;
    return fs;
}
```

### 条款5、6：编译器做的事

- 空类中自动生成的函数：构造函数、析构函数、copy操作符，都为public

- 默认的copy会将原对象的non-local成员变量拷贝，面对const和引用时，编译器拒绝，需自己完成

- 当你要阻止默认函数时，声明函数为explicit private并不去实现

  ```c++
  class Month(){
  public:
  	static Month Jan(){return Month(1);}
      ...
  private:
      explicit Month(int month){};
  }
  ```

  

### 条款7：基类虚析构

- 子类对象经由基类指针删除时，如果non-virtual析构，结果未有定义，可能局部销毁、内存泄漏
- 基类声明virtual析构后，正常（析构顺序由外到内）
- 任何基类都应该有virtual析构

### 条款8：析构函数不能吐出异常

- 析构函数吐出异常可能会导致同时出现两个异常导致崩溃
- 将导致异常的函数从析构中拿出处理

### 条款9：构造析构中，不使用virtual函数

- base的构造函数中，virtual并不是virtual，原因是base构造早于derived

### 条款10：operator= 返回reference to *this

```c++
Widget& operator= (int rfs){
    ...
    return *this
}
```

### 条款11：在operator= 中处理自我赋值

```c++
Widget& Widget::operator= (const Widget& rhs){
	if(this == &rhs) retuen *this //自我赋值判断
	
	delete pb;
	pb = new Bitmap(*rhs.pb);
	return *this;
}
```

### 条款12：拷贝对象应确保所有成员全部拷贝

- 自己实现copy时应保证，所有成员都复制，以及base class成员都复制

### 条款13：以对象管理资源

- 对象资源创建后，立即使用智能指针管理，而不是手动delete

### 条款15：资源管理类提供原始资源访问

- 对原始资源访问可能需要隐式转换（客户方便）或显式转换（智能指针的get，安全）
- 提供原始资源访问方法

### 条款16：new&delete形式

- new [] ;   delete [ ];

### 条款17：以独立语句将new对象放入智能指针

- 智能指针储存时，使用单独的语句，可避免可能存在的调用顺序问题

### 条款18：正确设计接口

- 接口一致性，与内置类型操作相似
- 阻止误用，限制类型和值来消除误用的可能

### 条款20：传引用代替传值

- 除内置类型与stl容器外，尽量传const 引用类型，可避免切割问题

### 条款21：关于reference与value的取舍

- 看到reference声明时，立即思考他本名是什么
- 不要返回stack临时对象的reference和pointer，也不要返回一个heap-allocated的reference，或者指向的static对象可能需要多个的情况

```c++
//这个类计算相乘操作
class Rational{
public:
    Rational(int numberator = 0,int denominator = 1);
    const Rational operator*(const Rational& lhs,const Rational& ths);
private:
    int n,d;
}
//如果方法中返回的是引用，会有一个问题，必须提前有一个Rational对象接收这个值，需要提前构造一
//个对象，那么返回引用的作用是什么呢？而且这个对象也必须等于你返回的引用的值.
//如果在函数中构造stack对象，那么就是返回临时对象引用了
//如果在函数中构造heap对象，在方法中new出来，返回去指针，那么何时delete？很难确定
//所以并不是一定要返回const reference
```

### 条款22：成员变量声明为private

- public和protected中的变量意味着他们被使用的地方过多而不可以再改变（低弹性）
- protected并不比public更有封装性
- private保证数据的一致性、访问控制、约束条件、类的实现弹性

### 条款23：用non-member、non-friend代替member函数

- non-member封装性高于member，原因是封装性是由能访问private部分的函数数量决定的
- 将non-member作为便利函数的做法是，将其放到同一个命名空间下

### 条款24：隐式转换与non-member函数

- 尽量使用显示转换代替隐式转换
- 如果一个函数所有参数都要进行类型转换，它必须是non-member

### 条款25：c++模板特化与non-member

- c++名称查找法则，会确保找到T类型命名空间专属的方法，然后再从std中查找

- 对于std函数需要特化的情况，提供成员函数以及一个非成员函数，并使用using，保证不特化时使用std版本

- 对于class

  ```c++
  class Widget
  {
  public:
      void swap(Widget& b){
          using std::swap;	//using std版本
          swap(this,b){};		//swap的特化，提升效率
      }
  private:
      ...
  }
  namespace std{				//std 特化版本
      template<>
      void swap<Widget> (Widget& a,Widget& b){
          swap(a,b);
      }
  }
  ```

- 对于class templates

  ```c++
  template<typename T>
  class Widget
  {
  public:
      template<typename T>
      void swap(Widget<T>& a,Widget<T>& b){}; //特化版本
  private:
      ...
  }
  template<typename T>
  void swap(Widget<T>& a,Widget<T>& b){
      using std::swap;
      swap(a,b);
  }
  ```

### 条款26：尽可能延后变量定义的时刻

- 尽量延后变量的定义至其有实际值的时刻，减少default构造和无效的构造析构带来的成本

### 条款27：少做转型操作

- 如非必要，少用类型转换，c++具有指针的偏移量，使用类型安全容器或者空的虚函数代替
- 一定要转型，要把类型转换封装
- 对dynamic_cast保持警惕，他有较大的消耗

#### C风格类型转换

​	(T)expression;

#### C++四种强制类型转换

#### static_cast

**没有类型检查**，所以用于安全性较高的类型转换，如基本数据类型。

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

**无视种族隔离**

(1) 不同类型的引用与指针之间的类型转换

(2) 将指针或引用转换成整型

(3) 将整形转换为指针或引用类型

#### dynamic_cast

======================================================================

##### *为什么要用dynamic_cast？*

是因为你要在一个你认定为子类的对象上调用子类方法，但你手上只有一个父类的对象

======================================================================

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

### 条款28：避免返回handle指向对象内部成员

两点原因（handle是用来表示号码牌，取得某个对象）

- 内部成员可能是你不想改变的，就算声明了私有，也可能通过handle取得并更改
- 生命周期不可控，如果对象被销毁，而指向该对象的成员还在某处工作，危

解决

- 若必须返回内部成员，注意const有效性，成员变量的封装性最多只等于返回reference的访问级别

### 条款29（补充）

### 条款30：inline函数

- inline函数将对函数的每一个调用都以函数本体代之
- 免除函数调用成本，增加了内存压力
- 将inline限制在小型的、频繁调用的函数上

### 条款31：（补充）

### 虚函数表和override（写在这里）

overide作用:在派生类中提醒自己要重写这个同参数函数,不写则报错

虚函数表是编译器生成的，程序运行时被载入内存

每一个有虚函数的类（或有虚函数的类的派生类）都有一个虚函数表，指示虚函数位置

子类若没有该方法，虚函数表中存的就是父类函数地址

析构时，若父类有虚析构，子类会自动生成虚析构，调用子类虚析构会自动执行父类虚析构

### 条款32：public继承达到is-a关系

- public继承意味 is-a（derived is a base），一句话就是base class上的任何一件事都适用于derived class上

### 条款33：避免继承遮掩名称

- derived与base中有同名的non-virtual函数，会将base中的覆盖，违反了is-a关系
- 使用using声明式声明base方法（public继承）或转交函数（private继承）解决上述问题

### 条款34：接口继承与实现继承

- 区分pure virtual函数、simple virtual函数、non-virtual函数
- pure virtual函数只继承接口，需自己实现
- simple virtual函数有接口和一份缺省实现
- non-virtual函数继承了接口和一份强制实现，注意它代表的是不变性和凌驾特异性，不应被重写

### 条款35：virtual函数其他替代方法（补充）

- NVI:以public non-virtual方法包装virtual方法，限制virtual前后环境，不限制实现

### 条款36：不重写继承而来的non-virtual函数

- 若子类有必要使用不同的函数，那就应定义为virtual函数
- 若子类有必要使用不同的函数，且父类确实不需要进行修改，应该使用private继承（has-a）
- non-virtual是静态绑定的

### 条款37：不重新定义继承而来的缺省参数

- 缺省值依赖静态类型，调用的函数依赖动态类型，这可能导致错误，也不要重新定义
- 需要缺省值的virtual时，使用NVI设计

### 条款38：通过复合实现has-a

- private下使用一个需要复合的对象指针，封装对象的方法而不是用继承

### 条款39：谨慎使用private继承

- derived以private形式继承base，是指其采用base某些特性，而不是与base有任何关系
- private继承在设计层面没有意义，其意义只存于实现层面
- 尽量使用复合
- 想让base具有独特的函数，可以使用复合（private成员）
- 类内只保存一个指向对象的指针，使用前置声明，减少不必要的include
- 重新定义继承而来的virtual函数，或者访问protect base class时，使用private继承？？
- private可造成empty base最优化？？

### 条款40：谨慎使用多重继承（补充）

- 函数名，成员名歧义，要指定类名
- 钻石型多重继承，成员变量重复继承
- virtual继承，若virtual base不带任何数据，将最具实用价值？？

### 条款41-47模板：（补充）

### 条款49：了解new-handler行为（补充）

- std::set_new-handler(new-handler p),参数为函数指针，new操作失败时调用

### 条款50：替换缺省的new和delete（补充）

- 检测运行错误
- 分配额外空间，收集分配内存的信息
- 增加分配和归还的速度
- 降低缺省内存分配的额外开销
- 替换为最佳齐位

### 条款51-52：（补充）

### 条款53：注意编译器的警告（补充）

### 条款54-55：(补充)





## 面向对象













