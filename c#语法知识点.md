# 关于c#中值类型和引用类型

成员变量值类型的默认值：

  int类型的默认值是0

  String类型的默认值是null

  double类型的默认值是0.0d

  Integer类型的默认值是null

  Long类型的默认值是null

  long类型的默认值是0L

  float类型的默认值是0.0f

  char类型的默认值是\u0000

  byte类型的默认值是(byte)0

  short类型的默认值是(short)0

## 	值类型 

​			储存于栈上



### 		数值类型

#### 			结构体 

​				1.简单类型

​					整型、浮点型、bool型、字符类型（char）

​				2.用户自定义类型

#### 			枚举类型



### 		可空类型？

就是将int，double，bool等不可设为null的类型设为null的类型

nullable类



## 	引用类型  

​			引用类型在栈中存储一个地址引用，实际存储位置在堆中



### 		类

#### 			用户自定义类

#### 			object基类

#### 			字符串类

字符串不可改变

```
string 转换成 Char[]
　　string ss = "abcdefg";
　　char[] cc = ss.ToCharArray();

Char[] 转换成string
　　string s = new string(cc);

此外,byte[] 与 string 之间的装换
　　byte[] bb = Encoding.UTF8.GetBytes(ss);
　　string s = Encoding.UTF8.GetString(bb);

 

我们也可以利用 StringBuilder 来进行数组 与 string 间的转换

using System.Text;

StringBuilder sb = new StringBuilder();
foreach(char c in cc)
{
    sb.Append(c);
}
string s = sb.ToString();
```



### 		接口

interface

### 		委托

degelate

事件是委托的包装器，只对外提供+=和-=

### 		数组

数组的元素，无论值类型还是引用类型，都存在堆上，数组的容量是固定的，您只能一次获取或设置一个元素的值，而ArrayList或List<T>的容量可根据需要自动扩充、修改、删除或插入数据。

#### 				Array

```
string[] s=new string[2];  
  
//赋值  
s[0]="a";  
s[1]="b";  
//修改  
s[1]="a1";  
```

#### 				ArrayList

```
ArrayList list1 = new ArrayList();  
```

```c#
//新增数据  
list1.Add("cde");  
list1.Add(5678);  
  
//修改数据  
list1[2] = 34;  
  
//移除数据  
list1.RemoveAt(0);  
  
//插入数据  
list1.Insert(0, "qwe");  
```

#### 				List< T>

```
List<string> list = new List<string>();  
Var list1 =new List<string>();  
//新增数据  
list.Add(“abc”);  
  
//修改数据  
list[0] = “def”;  
  
//移除数据  
list.RemoveAt(0);  

list.Sort();// 升序排序
list.Reverse();// 反转顺序

1.添加元素

(1)List. Add(T item)   添加一个元素

mList.Add("John");

(2)List. AddRange(IEnumerable<T>collection)   添加一组元素，

string[] temArr = { "Ha","Hunter", "Tom","Lily", "Jay","Jim", "Kuku",  "Locu"};

mList.AddRange(temArr);

(3)在index位置添加一个元素， 

mList.Insert(1, "Hei");

2.删除元素

(1)删除一个值

mList.Remove("Hunter");

(2)删除下标为index的元素

mList.RemoveAt(0);

(3)从下标index开始，删除count个元素

mList.RemoveRange(3, 2);

3.排序元素

(1)默认是元素第一个字母按升序

mList.Sort();

(2)顺序反转

List. Reverse ()   

4.查找元素

(1)List.Find 方法：搜索与指定谓词所定义的条件相匹配的元素，并返回整个 List 中的第一个匹配元素。

Predicate是对方法的委托，如果传递给它的对象与委托中定义的条件匹配，则该方法返回 true。当前 List 的元素被逐个传递给Predicate委托，并在 List 中向前移动，从第一个元素开始，到最后一个元素结束。当找到匹配项时处理即停止。

public T Find(Predicate<T>match);



关于数组的合并

int[] b7 = new int[] { 1, 2, 3, 4, 5 };
int[] b8 = new int[] { 6, 7, 8, 9 };
int[] b9 = new int[b1.Length + b2.Length];
string[] b10 = new string[] { "1", "2", "3", "4", "5" };
string[] b11 = new string[] { "6", "7", "8", "9" };
string[] b12 = new string[b1.Length + b2.Length];


b7.CopyTo(b9, 0);//这种方法适用于所有数组
b8.CopyTo(b9, b7.Length);

b6 = b4.Concat(b5).ToArray();//这种linq方法适用于所有数组，狠，一句话搞定。这里可能是返回字符串，所以加了toarray


关于数组去重

public class Solution {
    public bool ContainsDuplicate(int[] nums) {
       HashSet<int> intSet = new HashSet<int>(nums);
            return (intSet.Count != nums.Length);
    }
}



public class Solution {
    public bool ContainsDuplicate(int[] nums) {
      return (nums.Distinct().Count() != nums.Length);
    }
}


```

​		

List<string> strList = strArr.ToList();



 HashSet<string> hs = new HashSet<string>(li1); //此时已经去掉重复的数据保存在hashset中



var dic=new Dictionary<int,int>();

dic.ContainsKey(n)