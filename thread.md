## 单线程

### detach与join

使用**detach**时，尽量不要使用引用，绝对不能使用指针，原因是内存的生命周期问题

**join**可以传指针从而解决跨线程函数把参数拷贝一份到新的线程栈中的性能损耗

```cpp
#include <iostream>
#include <thread>
#include <string>
using namespace std;

void myPrint(const int i, const string& pmybuf)
{
    cout << std::this_thread::get_id() << endl;
    cout << i << endl;
    cout << pmybuf << endl;
}

int main()
{
	int mvar = 1;
	int& mvary = mvar;
	char mybuf[] = "this is a test";
	//如果detach了，这样仍然是不安全的
	//因为存在主线程运行完了，mybuf被回收了，系统采用mybuf隐式类型转换成string
    thread myThread(myPrint, mvar, mybuf); //错误，这样会有问题 ⬇
	//推荐创建一个临时对象，因为隐式转换（拷贝构造）可能发生在当前作用域结束后
    //这种方式，会有一次构造一次拷贝（函数参数传递），都发生在主线程
    //myPrint内部参数为const &,不会为函数内变量pmybuf产生额外的一次构造
	thread myThread(myPrint, mvar, string(mybuf));
	myThread.join();
	//myThread.detach();

	cout << "Hello World!" << endl;
}
```

###  关于传递对象

跨线程函数实际上会把参数拷贝一份到新的线程栈中（即使是const &），而不是直接共享原始A对象的内存地址。所以会多一次拷贝

```cpp
#include <iostream>
#include <thread>
using namespace std;

class A {
public:
	mutable int m_i; //m_i即使实在const中也可以被修改
	A(int i) :m_i(i) {}
};

void myPrint(const A& pmybuf)
{
	pmybuf.m_i = 199;
	cout << "子线程myPrint的参数地址是" << &pmybuf << "thread = " << std::this_thread::get_id() << endl;
}

int main()
{
	A myObj(10);
	//即使是传递的const引用，但在子线程中还是会调用拷贝构造函数构造一个新的对象
	//如果希望子线程中修改m_i的值影响到主线程，可以用thread myThread(myPrint, std::ref(myObj));
	//这样const就是真的引用了，myPrint定义中的const就可以去掉了，类A定义中的mutable也可以去掉了
	thread myThread(myPrint, myObj);
	myThread.join();
	//myThread.detach();

	cout << "Hello World!" << endl;
}
```

### 关于传递智能指针

注意原始内存的释放时机

```cpp
#include <iostream>
#include <thread>
#include <memory>
using namespace std;

void myPrint(unique_ptr<int> ptn)
{
	cout << "thread = " << std::this_thread::get_id() << endl;
}

int main()
{
	unique_ptr<int> up(new int(10)); //主线程中new，主线程中delete
	//unique_ptr通过move()、release() + reset()传递
	//传递后up就指向空，新的ptn指向原来的内存
	//这时就不能用detach了，因为如果主线程先执行完，ptn指向的对象就被释放了
	thread myThread(myPrint, std::move(up));
	myThread.join();
	//myThread.detach();

	return 0;
}
```

### 关于使用类成员函数

```cpp
#include <iostream>
#include <thread>
using namespace std;

class A {
public:
	mutable int m_i;
	A(int i) :m_i(i) {}
    void do_something();
};

int main()
{
	A myObj(10);
    //注意myObj的析构位置，谨慎用detach()
	thread myThread(&A::do_something(), &myObj);
	myThread.join();
}
```

### 关于使用operator()

```cpp
#include <iostream>
#include <thread>
using namespace std;

class A {
public:
	mutable int m_i;
	A(int i) :m_i(i) {}
    void do_something();
    void operator()(int num){}
};

int main()
{
	A myObj(10);
    //注意myObj的析构位置，谨慎用detach()
	thread myThread(std::ref(myObj) ,15);
	myThread.join();
}
```

## 多线程

### 创建和等待多个线程

```cpp
#include <iostream>
#include <thread>
#include <string>
using namespace std;

void myPrint()
{
    cout << "我是线程" << this_thread::get_id() << endl;
    cout << "线程" << this_thread::get_id() << "执行结束" << endl; 
}

int main()
{
    vector<thread> threads;
    for(int i = 0; i < 10; i++){
        threads.push_back(thread(myprint)); //创建且开始执行,但是执行顺序不确定，由OS控制
    }
	for(auto thd = threads.begin(); thd != threads.end; ++thd){
        thd->join(); //等待结束
    }

}
```

### 数据共享问题

#### 只读是安全稳定的

```cpp
#include <iostream>
#include <thread>
#include <string>
using namespace std;
vector<int> vi ={1,2,3}; //共享数据

void myPrint()
{
    cout << vi[0] << vi[1] << vi[2] << endl;
    cout << "线程" << this_thread::get_id() << "执行结束" << endl; 
}

int main()
{
    //只读
    vector<thread> threads_p;
    for(int i = 0; i < 10; i++){
        threads_p.push_back(thread(myprint)); //创建且开始执行
    }
	for(auto thd = threads_p.begin(); thd != threads_p.end; ++thd){
        thd->join(); //等待结束
    }
}
```

#### **互斥量 mutex**

1.  **运行原理**：作为一个类对象，同一时刻只可以被一个线程lock()，继续运行，其他线程不断尝试lock();

2.  **使用规则**：将保护的共享资源封装在一个类中，使其不被并发访问；lock unlock成对使用；

    ```cpp
    std::mutex.lock();
    //use data 
    std::mutex.unlock();
    ```

3.  **保证最少锁定**：注意保护的代码不多不少，多了影响效率，只关注共享数据；建议将需要lock部分封装；

4.  **底层原理**：mutex 通常使用操作系统提供的内核对象**`[1]`**来实现。当一个线程调用 lock 函数时，它会尝试获取 mutex 对应的内核对象所有权；如果 mutex 已经被占用，会释放之前获取的 mutex，这样做的目的是为了避免死锁，并进入内核等待队列等待 mutex 可用。当一个线程调用 unlock 函数时，它会释放 mutex 对应的内核对象，并向已经加入操作系统维护的线程等待队列发送通知信号，从而由操作系统线程调度机制决定唤醒等待该 mutex 的其他线程。

>   **`[1]`**在 Windows 操作系统中mutex 的内核对象是指在操作系统内核中对 mutex 的实现，mutex 的内核对象是通过 CreateMutex 函数创建的。CreateMutex 函数返回一个句柄，该句柄指向一个互斥对象。在使用 mutex 时，我们需要使用 Lock 和 Unlock 函数分别来获取和释放 mutex 的所有权，并使用 CloseHandle 函数关闭互斥对象的句柄

##### **lock_guard类模板**

1.  **运行原理**：作为一个临时对象，构造函数执行了mutex::lock();在作用域结束时，调用析构函数，执行mutex::unlock();

2.  **使用规则**：

    ```cpp
    std::lock_guard<std::mutex> m_lock_guard(m_mutex);
    ```

3.  **std::lock_guard的std::adopt_lock参数**：加入adopt_lock后，在调用lock_guard的构造函数时，不再进行lock()；这玩意一般配合std::lock；

    ```cpp
    std::lock(mutex1,mutex2); 
    std::lock_guard<std::mutex> m_guard(mutex1,std::adopt_lock);
    std::lock_guard<std::mutex> m_guard(mutex2,std::adopt_lock);
    ```

##### unique_lock类模板

1.  **与lock_guard的区别**：可以完全取代lock_guard、更加灵活、效率略低，重点在于第二个参数；
2.  **unique_lock第二个参数**、**几种策略锁**：

-   ==std::adopt_lock==：表示mutex已经lock，与lock_guard一样；
-   ==std::try_to_lock==：尝试锁定mutex，没成功则返回bool，不会阻塞等待拿到mutex，所以这里要注意的是不能提前lock，否则两次lock导致卡死，使用owns_lock()判断是否拿到

```cpp
#include <iostream>
#include <mutex>
#include <thread>
#include <chrono>
//一般的用法是希望在当前拿不到锁的情况下，让线程在将来某个时刻可以拿到锁并执行
//通常与 std::this_thread::yield() 或 std::this_thread::sleep_for() 
//配合使用，以避免过度占用 CPU 资源
std::mutex mtx;
void print_hello() {
    std::unique_lock<std::mutex> lock(mtx, std::try_to_lock);
    while (!lock.owns_lock()) {
        // 让出 CPU 时间片，避免过度占用资源
        std::this_thread::sleep_for(std::chrono::milliseconds(10)); 
        //std::this_thread::yield(); // 主动让出 CPU 时间片,等同于milliseconds(0)，实际上是等一次调度时间，几微秒
        lock.try_lock(); // 再次尝试加锁
        if (lock.owns_lock()) {
            //拿到锁了，干点事
        } else {
            //拿不到锁，干点别的
        }
    }
}

int main() {
    std::thread t1(print_hello);
    std::thread t2(print_hello);
    t1.join();
    t2.join();

    return 0;
}
```



-   ==std::defer_lock==：不能先lock，初始化一个没加锁的mutex，像没有一样，得手动lock，比直接用裸的mutex的优势⬇

>   ​       *1*. **异常安全**：如果在加锁和解锁之间的代码抛出异常，std::unique_lock 会在析构时自动解锁，确保互斥量不会被永久锁定。

>   ​	   *2*. **RAII（资源获取即初始化）**：std::unique_lock 遵循 RAII 原则，这意味着在对象的生命周期内，资源（这里是锁）的管理是自动的。当 std::unique_lock 对象离开作用域时，它会自动解锁互斥量。这使得代码更简洁、易于维护。

>   ​	   *3*. **灵活性**：std::unique_lock 提供了更多的控制选项，例如使用 std::defer_lock 策略来延迟加锁。这使得你可以在不同的代码段中灵活地控制锁的生命周期。此外，std::unique_lock 还支持条件变量（std::condition_variable）等高级同步原语。

```cpp
std::mutex mtx;
void print_hello() {
    std::unique_lock<std::mutex> lock(mtx, std::defer_lock);
    //先不锁，干点事
    lock.lock();
    //多线程干点事
    lock.unlock();
    //解锁，干点事
}
```

3.   **成员函数**：

```cpp
unique_lock<mutex> m_uniLock(mtx， defer_lock);
//lock()
m_uniLock.lock();
//unlock()
m_uniLock.unlock();
//try_lock()
if(m_uniLock.try_lock()){
//release() 解除绑定，返回mutex指针
    mutex* ptx = m_uniLock.release();
    ptx.unlock();
}
```

​	4.**传递所有权**：

```cpp
std::mutex mtx;
//返回临时变量
unique_lock<mutex> func(){
    unique_lock<mutex> m_ulock(mtx);
    return m_ulock;
}
//move
unique_lock<mutex> m_ulock1(mtx);
unique_lock<mutex> m_ulock2(std::move(m_ulock1));
```



##### **死锁**

1.  **发生条件**：至少两个锁，线程A已有锁1，去锁2，线程B已有锁2，去锁1。

2.  **解决方案**：保证多个互斥量上锁的顺序一样

3.  **std::lock()**：一次锁定多个互斥量，如果互斥量中一个没锁住，它就等着，等所有互斥量都锁住，才能继续执行。如果有一个没锁住，就会把已经锁住的释放掉（要么互斥量都锁住，要么都没锁住，防止死锁）

    ```cpp
    std::lock(mutex1,mutex2); 
    ```

#### 有读有写，使用互斥量

读写互斥，只能执行一个

```cpp
#include <iostream>
#include <thread>
#include <string>
#include <list>
#include <mutex>
using namespace std;

class PlayerMsgHandler {
public:
	PlayerMsgHandler() {}
    //写消息
    void insert_queue(int id){
        cout << "insert_queue" << endl; 
        m_mutex.lock();
        msgPlayerQueue.push_back(id);
        m_mutex.unlock();
    }
    //读消息
    void out_queue(){
        cout << "out_queue" << endl; 
        //只关注共享数据,不要lock整个循环，或者将需要lock部分封装
        while(!msgPlayerQueue.empty()){
            {
                std::lock_guard<std::mutex> m_lock_guard(m_mutex);
                int msg = msgPlayerQueue.front(); //取首元素，不检查
            	msgPlayerQueue.pop_front(); //移除首元素，不返回
            }
            cout << msg << endl; 
        }
    }
private:
    std::list<int> msgPlayerQueue;
    std::mutex m_mutex;
};

int main()
{
    PlayerMsgHandler msg_handler;
    std::thread in_msg_thread(&PlayerMsgHandler::insert_queue ,&msg_handler ,1);
    std::thread out_msg_thread(&PlayerMsgHandler::out_queue ,&msg_handler);
    in_msg_thread.join();
    out_msg_thread.join();
}
```

#### 单例模式的共享数据

```cpp
#include <iostream>	
#include <mutex>
using namespace	std;

mutex m_mutex;
//懒汉模式
class Singleton
{
public:
    static Singleton* get_instance(){
        if(instance == nullptr){
            lock_guard<mutex> m_lock(m_mutex);
            if(instance == nullptr){
                instance = new Singleton;
                static CGarhuishou c1;
            }
        }
        return instance;
    }
    //单例类中，释放单例的类
    class CGarhuishou {
	public:
		~CGarhuishou()
		{
			if (Singleton::instance)
			{
				delete Singleton::instance;
				Singleton::instance = nullptr;
			}
		}
	};
private:
    //构造私有
    Singleton(){}
    static Singleton* instance;
};
Singleton* Singleton::instance = nullptr;
```

#### std::call_once()

1.  **运行原理**：保证多线程下，该函数只调用一次

2.  **使用规则**：

    ```cpp
    std::once_flag g_flag;//注意作用域，要作为所有线程共享对象
    std::call_once(g_flag, get_instance);
    ```

3.  **std::call_once的std::once_flag参数**：once_flag是一个结构体，`call_once()`函数通过改变once_flag标记，来保证该函数只调用一次
