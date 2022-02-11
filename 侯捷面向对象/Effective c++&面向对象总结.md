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

