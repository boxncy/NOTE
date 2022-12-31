CMAKE
操作系统虚拟内存、运行时进程内存布局
c++多线程

## qt

===========Qtcreator中常用快捷键总结=========================

F1     查看帮助

F2     跳转到函数定义（和Ctrl+鼠标左键一样的效果）
Shift+F2  声明和定义之间切换
F4     头文件和源文件之间切换
——————————————————————————-
Alt+0     显示或者隐藏侧边条，编辑模式下起作用（有时写的函数太长，屏幕不够大，就用这个）
Ctrl+Space  自动补全（貌似会和输入法的切换冲突）

——————————————————————————-

ESc     切换到编辑模式

Alt(按住)+ Enter  将光标移动到h文件中的方法声明。按Alt(按住),再按回车键将在cpp中添加该函数的定义。

——————————————————————————-

Ctrl+I     自动对齐
Ctrl+/     注释行，取消注释行

——————————————————————————-

Ctrl+B     编译工程
Ctrl+R     运行工程
F5        开始调试
Shift+F5  停止调试
F9        设置和取消断点
F10      单步前进
F11      单步进入函数

Shift + F11 单步跳出函数

——————————————————————————-







============Qtcreator中常用小技巧========================





- 1 .Ctrl(按住)+ Tab快速切换已打开的文件



![img](http://hiphotos.baidu.com/exp/pic/item/30ecd5ef76094b3672ae59dba1cc7cd98d109d8d.jpg)



.

- 2 .快速添加方法实体(.cpp)声明,

将光标移动到h文件中的方法声明。按Alt(按住)+ Enter,再按回车键将在cpp中添加该函数的声明。

![img](http://hiphotos.baidu.com/exp/pic/item/1f569482b9014a909b31048bab773912b31beebe.jpg)



.

- 3 .修改变量名,并应用到所有使用该变量的地方。

将光标移动到需要更改的变量上,按Ctrl + Shift + R,当前变量名称外框为红色时,表示已经已激活全局修改功能,当修改此处变量名称时将一同修改代码中所有使用该变量的变量名。

![img](http://hiphotos.baidu.com/exp/pic/item/d872d695d143ad4b84bc04f680025aafa40f0632.jpg)



.

- 4 .快速打开输出窗口

按Alt +数字键(1-7)可以快速打开对应的输出窗口。

![img](http://hiphotos.baidu.com/exp/pic/item/4e83cb628535e5dd16a3e7e974c6a7efce1b623e.jpg)



.

- 5 .书签功能

Qt Creator中有一个叫做书签功能,即在某行代码处进行标记,方便以后找到。书签也可以添加文字标注。Qt中

按Ctrl + M 添加/删除书签,

按Ctrl + . 查找并移动到下一个标签

![img](http://hiphotos.baidu.com/exp/pic/item/d0526df082025aaffdb93cfdf9edab64034f1a39.jpg)



.

- 6 .分栏显示

这个功能只要用 Qt Creator开发基本上都会用到。这个快捷键操作方法比较特别:

先按Ctrl + e后松开再按2添加上下布局的分栏

先按Ctrl + e后松开再按3添加上下布局的分栏

先按Ctrl + e后松开再按1删除所有的分栏

![img](http://hiphotos.baidu.com/exp/pic/item/c9bdddcec3fdfc03a7fda18ed63f8794a4c2263b.jpg)



.

- 7 .其他重要快捷键

F2 快速切换到 光标选中对象 的源码。

F4 在 头文件(.h) 和 实现文件(.cpp) 之间进行切换。

Ctrl + / 注释/取消注释选定内容。

Ctrl + i 自动缩进选中代码。

Ctrl + shift + up 将当前行的代码向上移动一行。

Ctrl + shift + down 将当前行的代码向下移动一行。



=============Qtcreator中常用小技巧=======================



代码片段 snippets

  根据输入触发代码片段，节省敲代码的时间

  响应时间可以调整 QT Creator中的Tool->Option中的Text Editor->Completion 里的 “Activate completion”设置

**split**

```c++
String str = "dr__awedr4";
QStringList sl =str.split("w");
```

**wordwrap**

wordWrap属性用于控制视图中数据项文本的换行策略。如果此属性为True，则在数据项文本中分词的适当处进行换；否则数据项文本不进行换行处理。默认情况下，此属性为True。

请注意，即使启用了换行，单元格也不会展开以适合所有文本，如果数据项的空间无法展示所有内容，则会根据textElideMode设定的省略号模式在文本中插入省略号。

该属性可以通过wordWrap()和setWordWrap(bool wordWrap)来进行访问和设置。

**QWidget、QLabel、QPushButton的elidemode（省略模式）**

QString QFontMetrics::elidedText(const QString & text, Qt::TextElideMode mode, int width, int flags = 0) ，有没有很熟悉呢。第一个参数text：是需要省略处理的文本，第二个mode：省略模式，Qt::ElideLeft省略号在左侧、Qt::ElideRight右侧、Qt::ElideMiddle中间、Qt::ElideNone不出现省略号。第三个参数width：用来显示文本的宽度。会根据这个宽度判断是否文本超出。

**qshortcut**

1、autoRepeat：bool 访问函数：bool autoRepeat() const; void setAutoRepeat(bool); 描述快捷键是否可自动重复，若为 true，则只要系统启用了键盘自动重复，则快捷键启用 自动重复。默认为 true。

2、context：Qt::ShortcutContext 访问函数：Qt::ShortcutContext context() const; void setContext(Qt::ShortcutContext); 描述在什么情况下允许激活(或触发)快捷键，即快捷键的激活方式，默认为 Qt::WindowShortcut。其中 Qt::ShortcutContext 枚举见下表。

![img](https://img-blog.csdnimg.cn/20200805113813245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NzgyMzgw,size_16,color_FFFFFF,t_70)

3、enabled：bool 访问函数：bool isEnabled() const; void setEnabled(bool); 描述是否启用快捷键。若启用快捷键，则当产生 QShortcutEvent 事件时，会发送 activated() 或 activatedAmbiguously()信号，若程序处于 WhatsThis 模式下，则快捷键不会发送信号， 而是使用"Whats This"的文字代替，默认为 true。

4、key：QKeySequence 访问函数：QKeySequence key() const; void setKey(const QKeySequence&); 获取和设置键序列。默认为一个空的键序列。

5、whatsThis：QString 访问函数：QString whatsThis() const; void setWhatsThis( const QString&); 描述快捷键的 What’s This 帮助文本。当应用程序在 WhatsThis 模式且用户键入的快捷键 时，显示该文本。默认为一个空字符串。
**QShortcut 类中的函**数
1、QShortcut(QWidget* parent);
QShortcut(const QKeySequence& key, QWidget* parent, const char* member = Q_NULLPTR, const char* ambiguousMember = Q_NULLPTR, Qt::ShortcutContext context = Qt::WindowShortcut);
各参数意义为：key，表示键序列。parent，表示快捷键的父部件。若用户按下的键与 key 匹配，则调用 member 函数，若用户按下的键是不明确的，则调用 ambiguousMember 函数。 参数 context 表示在什么情况下允许激活(或触发)快捷键，详见 context 属性。注意：member 和ambiguousMember 需使用 SLOT 或SIGNAL 宏指定。

2、int id() const; //返回快捷键的 ID。

3、QWidget* parentWidget() const; //返回快捷键的父部件。

4、void activated(); //信号 当用户按下的键与快捷键的键序列匹配时，发送此信号。

5、void activatedAmbiguously(); //信号 若用户按下的键序列是不明确的，则发送该信号，此时不会发送 activated()信号。不明确 的键序列是指，该键序列是一个或多个其他快捷键的开始。

快捷键的激活方式

 #include<QtWidgets>

 int main(int argc, char *argv[])
 {    
	QApplication aa(argc,argv); 
	QWidget w;    
	QPushButton *pb2=new QPushButton("XXX",&w);
	pb2->move(99,77);    
	 
```c++
QCheckBox *pc=new QCheckBox("FFF",&w);  
pc->move(22,77); //使按钮 pw 成为其他按钮的父部件，因为这样 pw 就可以获取焦点。  
 
QPushButton *pw=new QPushButton(&w);    
pw->resize(222,55);     
 
QPushButton *pb=new QPushButton("Show",pw); //点击该按钮显示对话框     
QPushButton *pb1=new QPushButton("Enable",pw); 
pb1->move(77,0); //点击该按钮启用复选框 pc //创建一个对话框     
 
QDialog *pd=new QDialog(pw);    
pd->resize(222,111);     
 
QPushButton *pb3=new QPushButton("Disable",pd);  //点击该按钮禁用复选框 pc 
 
//创建 4 个快捷键并分别对应 4 种不同的     
QShortcut *ps=new QShortcut(QKeySequence("Ctrl+D"),pb1,SLOT(click())); 
//以下 3 个快捷键都可禁用复选框 pc     
QShortcut *ps1=new QShortcut(QKeySequence("Ctrl+E"),pw);     
QShortcut *ps2=new QShortcut(QKeySequence("Ctrl+F"),pw); 
QShortcut *ps3=new QShortcut(QKeySequence("Ctrl+G"),pw); 

//设置各快捷键的激活方式     
ps->setContext(Qt::ApplicationShortcut);    
ps1->setContext(Qt::WindowShortcut);     
ps2->setContext(Qt::WidgetShortcut);    
ps3->setContext(Qt::WidgetWithChildrenShortcut); 
 
//激活快捷键 ps1、ps2、ps3 时调用 pb3 的 click 槽函数(即相当于点击 pb3)     
QObject::connect(ps1,&QShortcut::activated,pb3,&QPushButton::click);     
QObject::connect(ps2,&QShortcut::activated,pb3,&QPushButton::click);      	
QObject::connect(ps3,&QShortcut::activated,pb3,&QPushButton::click); //点击 pb3 禁用复选框 pc     
QObject::connect(pb3,&QPushButton::clicked,pc,&QCheckBox::setEnabled); //点击 pb1 启用复选框 pc     
QObject::connect(pb1,&QPushButton::clicked,pc,&QCheckBox::setDisabled); //点击 pb 显示对话框        
QObject::connect(pb,&QPushButton::clicked,pd,&QDialog::show);    

w.resize(333,222);    
w.show();    
return aa.exec(); 
```
1、Ctrl+E、Ctrl+F、Ctrl+G 都是禁用复选框 FFF 的快捷键
2、Ctrl+D 可重新启用复选框 FFF
3、把焦点移至 pw(即 show 和 Enable 的父部件)，此时按下 Ctrl+E、Ctrl+F、Ctrl+G 都可禁用复选框 FFF。
4、把焦点移至按钮 Show，此时按下 Ctrl+F 不能禁用 FFF，因 为按钮 Show不是快捷键 Ctrl+F 的父部件。
5、把焦点移至按钮 XXX，此时按下 Ctrl+F、Ctrl+G 不能禁用 FFF。因为 XXX即不是Ctrl+G的父部件也不是他的子部件。 Ctrl+F 的原因同第 4点
6、点击按钮 Show 弹出对话框，使对话框成为活动的，此时按下 Ctrl+E、Ctrl+F、Ctrl+G 都不能禁 用复选框 FFF。因为 Ctrl+E 的父部件 pw 不是活动顶层窗口 pd 的子部件。Ctrl+F 的原因同第 4 点，Ctrl+G 的原因同第 5点
7、只要该程序的任一窗口是活动的，按下Ctrl+D 都能启用 FFF
————————————————
版权声明：本文为CSDN博主「一只老虎跑的贼快」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_37782380/article/details/107811537

**Qt：正确判断文件、文件夹是否存在的方法**

准确判断文件是否存在

1.用QFileInfo::isFile()方法

 

准确判断文件夹是否存在
1.用QFileInfo::isDir()方法
2.用QDir::exists()方法

 

不确定字符串是文件还是文件夹路径
1.用QFileInfo::exists()方法
2.用QFile::exists()方法

**qdebug**

使用 " qDebug() << "一定要添加头文件 #include <QDebug>

然而

 int num = 20;
 char str[20]="hello world";
 qDebug("如果只写在括号里，是不需要QDebug头文件的 %d %s", num, str);

**setStyleSheet**

Qt中设置按钮或QWidget的外观是，可以使用QT Style Sheets来进行设置，非常方便。
可以用setStyleSheet("font: bold; font-size:20px; color: rgb(241, 70, 62); background-color: green");来进行设置，

其他的样式介绍如下：


font: bold; 是否粗体显示

font-family:""; 来设定字体所属家族，

font-size:20px; 来设定字体大小

font-style: nomal; 来设定字体样式

font-weight:20px; 来设定字体深浅

color：black ;字体颜色

 

border: 1px solid gray;边框大小，样式，颜色
border-image:""; 用来设定边框的背景图片。
border-radius:5px; 用来设定边框的弧度。可以设定圆角的按钮
border-width: 1px； 边框大小

 

 background-color: green; 设置背景颜色
background:transparent; 设置背景为透明
color:rgb(241, 70, 62); 设置前景颜色
selection-color:rgb(241, 70, 62); 用来设定选中时候的颜色

可以使用border-top，border-right，border-bottom，border-left分别设定按钮的上下左右边框，
同样有border-left-color, border-left-style, border-left-width.等分别来设定他们的颜色，样式和宽度

#### 获取类名

```c++
bool res = obj->metaObject()->className() == QStringLiteral("需要对比的类名称");
```



#### CSS基本功能

CSS的强大在于组合功能的强大，这里只是简单介绍基本功能，将简单功能组合起来才能实现好看的效果。

##### CSS 背景属性（Background）

| 属性                                                         | 描述                                             | CSS  |
| ------------------------------------------------------------ | ------------------------------------------------ | ---- |
| [background](http://www.w3school.com.cn/cssref/pr_background.asp) | 在一个声明中设置所有的背景属性。                 | 1    |
| [background-attachment](http://www.w3school.com.cn/cssref/pr_background-attachment.asp) | 设置背景图像是否固定或者随着页面的其余部分滚动。 | 1    |
| [background-color](http://www.w3school.com.cn/cssref/pr_background-color.asp) | 设置元素的背景颜色。                             | 1    |
| [background-image](http://www.w3school.com.cn/cssref/pr_background-image.asp) | 设置元素的背景图像。                             | 1    |
| [background-position](http://www.w3school.com.cn/cssref/pr_background-position.asp) | 设置背景图像的开始位置。                         | 1    |
| [background-repeat](http://www.w3school.com.cn/cssref/pr_background-repeat.asp) | 设置是否及如何重复背景图像。                     | 1    |
| [background-clip](http://www.w3school.com.cn/cssref/pr_background-clip.asp) | 规定背景的绘制区域。                             | 3    |
| [background-origin](http://www.w3school.com.cn/cssref/pr_background-origin.asp) | 规定背景图片的定位区域。                         | 3    |
| [background-size](http://www.w3school.com.cn/cssref/pr_background-size.asp) | 规定背景图片的尺寸。                             | 3    |

##### CSS 边框属性（Border 和 [Outline](https://www.baidu.com/s?wd=Outline&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)）

| 属性                                                         | 描述                                                         | CSS  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| [border](http://www.w3school.com.cn/cssref/pr_border.asp)    | 在一个声明中设置所有的边框属性。                             | 1    |
| [border-bottom](http://www.w3school.com.cn/cssref/pr_border-bottom.asp) | 在一个声明中设置所有的下边框属性。                           | 1    |
| [border-bottom-color](http://www.w3school.com.cn/cssref/pr_border-bottom_color.asp) | 设置下边框的颜色。                                           | 2    |
| [border-bottom-style](http://www.w3school.com.cn/cssref/pr_border-bottom_style.asp) | 设置下边框的样式。                                           | 2    |
| [border-bottom-width](http://www.w3school.com.cn/cssref/pr_border-bottom_width.asp) | 设置下边框的宽度。                                           | 1    |
| [border-color](http://www.w3school.com.cn/cssref/pr_border-color.asp) | 设置四条边框的颜色。                                         | 1    |
| [border-left](http://www.w3school.com.cn/cssref/pr_border-left.asp) | 在一个声明中设置所有的左边框属性。                           | 1    |
| [border-left-color](http://www.w3school.com.cn/cssref/pr_border-left_color.asp) | 设置左边框的颜色。                                           | 2    |
| [border-left-style](http://www.w3school.com.cn/cssref/pr_border-left_style.asp) | 设置左边框的样式。                                           | 2    |
| [border-left-width](http://www.w3school.com.cn/cssref/pr_border-left_width.asp) | 设置左边框的宽度。                                           | 1    |
| [border-right](http://www.w3school.com.cn/cssref/pr_border-right.asp) | 在一个声明中设置所有的右边框属性。                           | 1    |
| [border-right-color](http://www.w3school.com.cn/cssref/pr_border-right_color.asp) | 设置右边框的颜色。                                           | 2    |
| [border-right-style](http://www.w3school.com.cn/cssref/pr_border-right_style.asp) | 设置右边框的样式。                                           | 2    |
| [border-right-width](http://www.w3school.com.cn/cssref/pr_border-right_width.asp) | 设置右边框的宽度。                                           | 1    |
| [border-style](http://www.w3school.com.cn/cssref/pr_border-style.asp) | 设置四条边框的样式。                                         | 1    |
| [border-top](http://www.w3school.com.cn/cssref/pr_border-top.asp) | 在一个声明中设置所有的上边框属性。                           | 1    |
| [border-top-color](http://www.w3school.com.cn/cssref/pr_border-top_color.asp) | 设置上边框的颜色。                                           | 2    |
| [border-top-style](http://www.w3school.com.cn/cssref/pr_border-top_style.asp) | 设置上边框的样式。                                           | 2    |
| [border-top-width](http://www.w3school.com.cn/cssref/pr_border-top_width.asp) | 设置上边框的宽度。                                           | 1    |
| [border-width](http://www.w3school.com.cn/cssref/pr_border-width.asp) | 设置四条边框的宽度。                                         | 1    |
| [outline](http://www.w3school.com.cn/cssref/pr_outline.asp)  | 在一个声明中设置所有的轮廓属性。                             | 2    |
| [outline-color](http://www.w3school.com.cn/cssref/pr_outline-color.asp) | 设置轮廓的颜色。                                             | 2    |
| [outline-style](http://www.w3school.com.cn/cssref/pr_outline-style.asp) | 设置轮廓的样式。                                             | 2    |
| [outline-width](http://www.w3school.com.cn/cssref/pr_outline-width.asp) | 设置轮廓的宽度。                                             | 2    |
| [border-bottom-left-radius](http://www.w3school.com.cn/cssref/pr_border-bottom-left-radius.asp) | 定义边框左下角的形状。                                       | 3    |
| [border-bottom-right-radius](http://www.w3school.com.cn/cssref/pr_border-bottom-right-radius.asp) | 定义边框右下角的形状。                                       | 3    |
| [border-image](http://www.w3school.com.cn/cssref/pr_border-image.asp) | 简写属性，设置所有 border-image-* 属性。                     | 3    |
| [border-image-outset](http://www.w3school.com.cn/cssref/pr_border-image-outset.asp) | 规定边框图像区域超出边框的量。                               | 3    |
| [border-image-repeat](http://www.w3school.com.cn/cssref/pr_border-image-repeat.asp) | 图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched)。 | 3    |
| [border-image-slice](http://www.w3school.com.cn/cssref/pr_border-image-slice.asp) | 规定图像边框的向内偏移。                                     | 3    |
| [border-image-source](http://www.w3school.com.cn/cssref/pr_border-image-source.asp) | 规定用作边框的图片。                                         | 3    |
| [border-image-width](http://www.w3school.com.cn/cssref/pr_border-image-width.asp) | 规定图片边框的宽度。                                         | 3    |
| [border-radius](http://www.w3school.com.cn/cssref/pr_border-radius.asp) | 简写属性，设置所有四个 border-*-radius 属性。                | 3    |
| [border-top-left-radius](http://www.w3school.com.cn/cssref/pr_border-top-left-radius.asp) | 定义边框左上角的形状。                                       | 3    |
| [border-top-right-radius](http://www.w3school.com.cn/cssref/pr_border-top-right-radius.asp) | 定义边框右下角的形状。                                       | 3    |
| box-decoration-break                                         |                                                              | 3    |
| [box-shadow](http://www.w3school.com.cn/cssref/pr_box-shadow.asp) | 向方框添加一个或多个阴影。                                   | 3    |

##### Box 属性

| 属性                                                         | 描述                                                        | CSS  |
| ------------------------------------------------------------ | ----------------------------------------------------------- | ---- |
| [overflow-x](http://www.w3school.com.cn/cssref/pr_overflow-x.asp) | 如果内容溢出了元素内容区域，是否对内容的左/右边缘进行裁剪。 | 3    |
| [overflow-y](http://www.w3school.com.cn/cssref/pr_overflow-y.asp) | 如果内容溢出了元素内容区域，是否对内容的上/下边缘进行裁剪。 | 3    |
| [overflow-style](http://www.w3school.com.cn/cssref/pr_overflow-style.asp) | 规定溢出元素的首选滚动方法。                                | 3    |
| [rotation](http://www.w3school.com.cn/cssref/pr_rotation.asp) | 围绕由 rotation-point 属性定义的点对元素进行旋转。          | 3    |
| [rotation-point](http://www.w3school.com.cn/cssref/pr_rotation-point.asp) | 定义距离上左边框边缘的偏移点。                              | 3    |

##### CSS 字体属性（Font）

| 属性                                                         | 描述                                   | CSS  |
| ------------------------------------------------------------ | -------------------------------------- | ---- |
| [font](http://www.w3school.com.cn/cssref/pr_font_font.asp)   | 在一个声明中设置所有字体属性。         | 1    |
| [font-family](http://www.w3school.com.cn/cssref/pr_font_font-family.asp) | 规定文本的字体系列。                   | 1    |
| [font-size](http://www.w3school.com.cn/cssref/pr_font_font-size.asp) | 规定文本的字体尺寸。                   | 1    |
| [font-size-adjust](http://www.w3school.com.cn/cssref/pr_font_font-size-adjust.asp) | 为元素规定 aspect 值。                 | 2    |
| [font-stretch](http://www.w3school.com.cn/cssref/pr_font_font-stretch.asp) | 收缩或拉伸当前的字体系列。             | 2    |
| [font-style](http://www.w3school.com.cn/cssref/pr_font_font-style.asp) | 规定文本的字体样式。                   | 1    |
| [font-variant](http://www.w3school.com.cn/cssref/pr_font_font-variant.asp) | 规定是否以小型大写字母的字体显示文本。 | 1    |
| [font-weight](http://www.w3school.com.cn/cssref/pr_font_weight.asp) | 规定字体的粗细。                       | 1    |

##### CSS 外边距属性（Margin）

| 属性                                                         | 描述                             | CSS  |
| ------------------------------------------------------------ | -------------------------------- | ---- |
| [margin](http://www.w3school.com.cn/cssref/pr_margin.asp)    | 在一个声明中设置所有外边距属性。 | 1    |
| [margin-bottom](http://www.w3school.com.cn/cssref/pr_margin-bottom.asp) | 设置元素的下外边距。             | 1    |
| [margin-left](http://www.w3school.com.cn/cssref/pr_margin-left.asp) | 设置元素的左外边距。             | 1    |
| [margin-right](http://www.w3school.com.cn/cssref/pr_margin-right.asp) | 设置元素的右外边距。             | 1    |
| [margin-top](http://www.w3school.com.cn/cssref/pr_margin-top.asp) | 设置元素的上外边距。             | 1    |

##### CSS 字体属性（Font）

| 属性                                                         | 描述                                   | CSS  |
| ------------------------------------------------------------ | -------------------------------------- | ---- |
| [font](http://www.w3school.com.cn/cssref/pr_font_font.asp)   | 在一个声明中设置所有字体属性。         | 1    |
| [font-family](http://www.w3school.com.cn/cssref/pr_font_font-family.asp) | 规定文本的字体系列。                   | 1    |
| [font-size](http://www.w3school.com.cn/cssref/pr_font_font-size.asp) | 规定文本的字体尺寸。                   | 1    |
| [font-size-adjust](http://www.w3school.com.cn/cssref/pr_font_font-size-adjust.asp) | 为元素规定 aspect 值。                 | 2    |
| [font-stretch](http://www.w3school.com.cn/cssref/pr_font_font-stretch.asp) | 收缩或拉伸当前的字体系列。             | 2    |
| [font-style](http://www.w3school.com.cn/cssref/pr_font_font-style.asp) | 规定文本的字体样式。                   | 1    |
| [font-variant](http://www.w3school.com.cn/cssref/pr_font_font-variant.asp) | 规定是否以小型大写字母的字体显示文本。 | 1    |
| [font-weight](http://www.w3school.com.cn/cssref/pr_font_weight.asp) | 规定字体的粗细。                       | 1    |

##### CSS 内边距属性（Padding）

| 属性                                                         | 描述                             | CSS  |
| ------------------------------------------------------------ | -------------------------------- | ---- |
| [padding](http://www.w3school.com.cn/cssref/pr_padding.asp)  | 在一个声明中设置所有内边距属性。 | 1    |
| [padding-bottom](http://www.w3school.com.cn/cssref/pr_padding-bottom.asp) | 设置元素的下内边距。             | 1    |
| [padding-left](http://www.w3school.com.cn/cssref/pr_padding-left.asp) | 设置元素的左内边距。             | 1    |
| [padding-right](http://www.w3school.com.cn/cssref/pr_padding-right.asp) | 设置元素的右内边距。             | 1    |
| [padding-top](http://www.w3school.com.cn/cssref/pr_padding-top.asp) | 设置元素的上内边距。             | 1    |

##### CSS 定位属性（Positioning）

| 属性                                                         | 描述                                                   | CSS  |
| ------------------------------------------------------------ | ------------------------------------------------------ | ---- |
| [bottom](http://www.w3school.com.cn/cssref/pr_pos_bottom.asp) | 设置定位元素下外边距边界与其包含块下边界之间的偏移。   | 2    |
| [clear](http://www.w3school.com.cn/cssref/pr_class_clear.asp) | 规定元素的哪一侧不允许其他浮动元素。                   | 1    |
| [clip](http://www.w3school.com.cn/cssref/pr_pos_clip.asp)    | 剪裁绝对定位元素。                                     | 2    |
| [cursor](http://www.w3school.com.cn/cssref/pr_class_cursor.asp) | 规定要显示的光标的类型（形状）。                       | 2    |
| [display](http://www.w3school.com.cn/cssref/pr_class_display.asp) | 规定元素应该生成的框的类型。                           | 1    |
| [float](http://www.w3school.com.cn/cssref/pr_class_float.asp) | 规定框是否应该浮动。                                   | 1    |
| [left](http://www.w3school.com.cn/cssref/pr_pos_left.asp)    | 设置定位元素左外边距边界与其包含块左边界之间的偏移。   | 2    |
| [overflow](http://www.w3school.com.cn/cssref/pr_pos_overflow.asp) | 规定当内容溢出元素框时发生的事情。                     | 2    |
| [position](http://www.w3school.com.cn/cssref/pr_class_position.asp) | 规定元素的定位类型。                                   | 2    |
| [right](http://www.w3school.com.cn/cssref/pr_pos_right.asp)  | 设置定位元素右外边距边界与其包含块右边界之间的偏移。   | 2    |
| [top](http://www.w3school.com.cn/cssref/pr_pos_top.asp)      | 设置定位元素的上外边距边界与其包含块上边界之间的偏移。 | 2    |
| [vertical-align](http://www.w3school.com.cn/cssref/pr_pos_vertical-align.asp) | 设置元素的垂直对齐方式。                               | 1    |
| [visibility](http://www.w3school.com.cn/cssref/pr_class_visibility.asp) | 规定元素是否可见。                                     | 2    |
| [z-index](http://www.w3school.com.cn/cssref/pr_pos_z-index.asp) | 设置元素的堆叠顺序。                                   |      |

##### CSS 文本属性（Text）

| 属性                                                         | 描述                                                    | CSS  |
| ------------------------------------------------------------ | ------------------------------------------------------- | ---- |
| [color](http://www.w3school.com.cn/cssref/pr_text_color.asp) | 设置文本的颜色。                                        | 1    |
| [direction](http://www.w3school.com.cn/cssref/pr_text_direction.asp) | 规定文本的方向 / 书写方向。                             | 2    |
| [letter-spacing](http://www.w3school.com.cn/cssref/pr_text_letter-spacing.asp) | 设置字符间距。                                          | 1    |
| [line-height](http://www.w3school.com.cn/cssref/pr_dim_line-height.asp) | 设置行高。                                              | 1    |
| [text-align](http://www.w3school.com.cn/cssref/pr_text_text-align.asp) | 规定文本的水平对齐方式。                                | 1    |
| [text-decoration](http://www.w3school.com.cn/cssref/pr_text_text-decoration.asp) | 规定添加到文本的装饰效果。                              | 1    |
| [text-indent](http://www.w3school.com.cn/cssref/pr_text_text-indent.asp) | 规定文本块首行的缩进。                                  | 1    |
| text-shadow                                                  | 规定添加到文本的阴影效果。                              | 2    |
| [text-transform](http://www.w3school.com.cn/cssref/pr_text_text-transform.asp) | 控制文本的大小写。                                      | 1    |
| [unicode-bidi](http://www.w3school.com.cn/cssref/pr_unicode-bidi.asp) | 设置文本方向。                                          | 2    |
| [white-space](http://www.w3school.com.cn/cssref/pr_text_white-space.asp) | 规定如何处理元素中的空白。                              | 1    |
| [word-spacing](http://www.w3school.com.cn/cssref/pr_text_word-spacing.asp) | 设置单词间距。                                          | 1    |
| [hanging-punctuation](http://www.w3school.com.cn/cssref/pr_hanging-punctuation.asp) | 规定标点字符是否位于线框之外。                          | 3    |
| [punctuation-trim](http://www.w3school.com.cn/cssref/pr_punctuation-trim.asp) | 规定是否对标点字符进行修剪。                            | 3    |
| text-align-last                                              | 设置如何对齐最后一行或紧挨着强制换行符之前的行。        | 3    |
| [text-emphasis](http://www.w3school.com.cn/cssref/pr_text-emphasis.asp) | 向元素的文本应用重点标记以及重点标记的前景色。          | 3    |
| [text-justify](http://www.w3school.com.cn/cssref/pr_text-justify.asp) | 规定当 text-align 设置为 "justify" 时所使用的对齐方法。 | 3    |
| [text-outline](http://www.w3school.com.cn/cssref/pr_text-outline.asp) | 规定文本的轮廓。                                        | 3    |
| [text-overflow](http://www.w3school.com.cn/cssref/pr_text-overflow.asp) | 规定当文本溢出包含元素时发生的事情。                    | 3    |
| [text-shadow](http://www.w3school.com.cn/cssref/pr_text-shadow.asp) | 向文本添加阴影。                                        | 3    |
| [text-wrap](http://www.w3school.com.cn/cssref/pr_text-wrap.asp) | 规定文本的换行规则。                                    | 3    |
| [word-break](http://www.w3school.com.cn/cssref/pr_word-break.asp) | 规定非中日韩文本的换行规则。                            | 3    |
| [word-wrap](http://www.w3school.com.cn/cssref/pr_word-wrap.asp) | 允许对长的不可分割的单词进行分割并换行到下一行。        | 3    |

![img](https://img-blog.csdnimg.cn/img_convert/8e4a510830e2991b94e0b169210dfcf7.png)

**qobject_cast<xxx*>(sender()) 用法**

sender()在一个槽函数中，返回发出这个信号的qobject，qobject_cast将qobject转成xxx

这样我们就拿到了发出信号的控件，可以利用该控件中的私有协议类型，进一步传递信号

```c++
AQCMDButton::AQCMDButton(const AQItemIndexList::EAction_item action_item, const QString &text, QWidget *parent)
    :AQCMDButton(text, parent)
{
    action_item_ = action_item;
}

void AQItemIndexPanel::slt_cmd_transfer() 
{
    AQCMDButton* button = qobject_cast<AQCMDButton*>(sender());
    lst_itemindex_->transfer_action(button->get_action_item());
}

void AQItemIndexList::transfer_action(AQItemIndexList::EAction_item action_item) 
{
    foreach(QAction* a, actions_list_) {
        if (a->data() == action_item){
            emit a->triggered(true);
            break;
        }
    }
}
```

![image-20211103173310295](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211103173310295.png)

![image-20211103173334647](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20211103173334647.png)

#### Qtabelwidget

在 QTableWidget 表格中，每一个单元格是一个 QTable Widgetltem 对象，可以设置文字内容、字体、前景色、背景色、图标，也可以设置编辑和显示标记。每个单元格还可以存储一个 QVariant 数据，用于设置用户自定义数据。

表格的第 1 行称为行表头，用于设置每一列的标题，第 1 列称为列表头，可以设置其标题，但一般使用缺省的标题，即为行号。行表头和列表头一般是不可编辑的。

除了行表头和列表头之外的表格区域是内容区，内容区是规则的网格状，如同一个二维数组，每个网格单元称为一个单元格。每个单元格有一个行号、列号，图 1 表示了行号、列号的变化规律。


##### 表头

**一次性 顺序设置水平/竖直表头**

```c++
QTableWidget:: setHorizontalHeaderLabels(QstringList)

QTableWidget:: setVerticalHeaderLabels(QstringList)
    
//属性
tablewidget->horizontalHeader()->setVisible(bool); //表头是否可见
tablewidget->verticalHeader()->setVisible(bool);
```

1.  该方法无返回值

2.  该方法设置的多个节是按第0节开始的逻辑顺序设置，不论中间是否出现隐藏节或交换节或者是否已设置标签

3.  如果标签列表超出实际表头节数将忽略多出的，前面的按给定顺序设置

4.  如果标签列表少于实际表头节数也能设置前面对应节数的标签

5.  设置了标签的节自动会创建该节对应的项

   

**访问对应项**

表头的一个节实际上对应一个项，项的类型与表格部件的项类型相同，都是QTableWidgetItem实例对象。水平节对应项可以通过方法horizontalHeaderItem和setHorizontalHeaderItem方法访问，调用语法如下：

```c++
QTableWidgetItem QTableWidget:: horizontalHeaderItem(int column)

QTableWidgetItem QTableWidget:: setHorizontalHeaderItem(int column, QTableWidgetItem item)

QTableWidgetItem QTableWidget:: verticalHeaderItem(int row)

QTableWidgetItem QTableWidget:: setVerticalHeaderItem(int row, QTableWidgetItem item)
```

注：

1. column参数对应水平表头节的序号，从0开始，其数值必须小于表格部件的总列数，否则设置调用无效，查询返回None；
2. 调用setHorizontalHeaderItem将替换原有节对应的项，标签显示为项的文本；
3. 如果一个节没有设置标签也没有通过setHorizontalHeaderItem设置项，则对应表头节没有对应项。
   

**QTableWidget取下表头节对应项并返回**

```c++
QTableWidgetItem QTableWidget:: takeHorizontalHeaderItem(int column)

QTableWidgetItem QTableWidget:: takeVerticalHeaderItem(int row)

```



##### 编辑项 editItem

QTableWidget提供了触发项编辑的方法，调用语法如下：
**QTableWidget** :: editItem(QTableWidgetItem item)

设置QTableWidget某一单元格处于**编辑状态，指定单元格处于光标闪烁状态**。

```c++
m_TableWidget->setFocus();
m_TableWidget->editItem(m_TableWidget->item(1,1));

//选中单元格或者行
table->setCurrentCell(row, column, QItemSelectionModel::Select);
table->setCurrentCell(row, QItemSelectionModel::Select);

```

注意：

1.  editItem方法生效必须设置项的标记flags为可编辑
2.  editItem一次只能触发一个项进行编辑，一旦退出编辑状态（如改变焦点），除非再次调用
3. editItem或设置editTriggers触发编辑或打开永久编辑器否则对应项不能再编辑
    连续多次调用editItem，中间没有触发事件处理，则只有第一次调用生效，后续调用无效

##### 单元格内容：Qtablewidgetitem

```c++
//new一个Qtablewidgetitem  第一个参数是item的内容 第二个为节点类型
Qtablewidgetitem::Qtablewidgetitem(const Qstring &str,int type = Type);
item = new Qtablewidgetitem("niu niu",MainWindow::ctName);

//设置单元格内容
table_widget_->setitem(row,col,item);

//单元格属性设置 字体/对齐/前景/背景/图标/勾选/属性标记等
setTextAlignment()...
    
//获取某一单元格
table_widget_->item(row,col);
```

##### 获取当前单元格

```c++
currentColumn();
currentRow();
//当单元格切换时，会发射currentCellCahanged(); currentItemChanged();两个信号
```

##### 合并单元格

```c++
//其参数为： 要改变单元格的 1行数 2列数 要合并的 3行数 4列数
tableWidget->setSpan(0, 0, 3, 1);
```

##### 设置单元格大小

```c++
tableWidget->setColumnWidth(3,200);
tableWidget->setRowHeight(3,60);
```

##### 删除/插入/添加行

```c++
insertRow(int row);//row 前插入一行
removeRow(int row);
```

##### 自动调整行高列宽

```c++
resizeColumnToContents();				//调整所有列宽
resizeColumnToContents(int column);		//调整指定列宽

resizeRowToContents();					//调整所有行高
resizeRowToContents(int column);		//调整指定行高

horizontalHeader()->setResizeMode(QHeaderView::Stretch);  //自适应宽度

// PS:实际为tablewidget父类 tableview的函数
```

##### 其他属性

```c++
// PS:实际为tablewidget父类 tableview的函数
//其他属性
//选择模式 单元格、行、列、多选
tablewidget->setSelectionBehavior(QAbstractItemView::SelectItems/Rows/Columns);
//编辑属性 是否可编辑、单机双击等触发编辑的方式
tableWidget->setEditTriggers(QAbstractItemView::NoEditTriggers);
//QtableWidgetItem转Qstring
QTableWidgetItem->data(Qt::DisplayRole)->toString();
    
verticalHeader()->setDefaultSectionSize(30);    		//设置默认行高
horizontalHeader()->setClickable(false);    	//设置表头不可点击（默认点击后进行排序
horizontalHeader()->setFont(font);						//设置字体
horizontalHeader()->setStretchLastSection(true);   		//设置充满表宽度
setFrameShape(QFrame::NoFrame);      					//设置无边框
setShowGrid(false);                						//设置不显示格子线
verticalHeader()->setVisible(false); 					//设置垂直头不可见
setSelectionMode(QAbstractItemView::ExtendedSelection); //可多选
setItemDelegate(new NoFocusDelegate());             	//虚线边框去除
setFocusPolicy(Qt::NoFocus);   							//去除选中虚线框
horizontalHeader()->setHighlightSections(false); //点击表时不对表头行光亮（获取焦点）
setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);	//去掉水平滚动条
```



##### 删除layout中所有控件（递归）

最近一个项目开发，需要动态的添加/删除控件，下面记录一下方法，该方法参考了网上的方法，但是没有针对layout中嵌套layout的做处理：

```c++
void deleteAllitemsOfLayout(QLayout* layout){
    QLayoutItem *child;
    while ((child = layout->takeAt(0)) != nullptr)
    {
        ///setParent为NULL，防止删除之后界面不消失
        if(child->widget())
        {
            child->widget()->setParent(nullptr);
        }else if(child->layout()){
            deleteAllitemsOfLayout(child->layout());
        }
        delete child;
    }
}

//迭代器 auto关键字遍历
printf("使用auto关键字\n");
    for (auto iter = points.begin(); iter != points.end(); iter++) {
        printf("  x: %.2f, y: %.2f\n",
               (*iter).x, (*iter).y);
    }
```

#### 两种窗口的模态

```c++
setWindowModality(Qt::WindowModal);
setWindowModality(Qt::ApplicationModal);
```

#### setWindowFlags ()

```c++
Qt::FrameWindowHint:没有边框的窗口
Qt::WindowStaysOnTopHint://总在最上面的窗口
Qt::CustomizeWindowHint://自定义窗口标题栏,以下标志必须与这个标志一起使用才有效,否则窗口将有默认的标题栏
Qt::WindowTitleHint:显示窗口标题栏
Qt::WindowSystemMenuHint://显示系统菜单
Qt::WindowMinimizeButtonHint://显示最小化按钮
Qt::WindowMaximizeButtonHint://显示最大化按钮
Qt::WindowMinMaxButtonsHint://显示最小化按钮和最大化按钮
Qt::WindowCloseButtonHint://显示关闭按钮 
Qt::Drawer://去掉窗口左上角的图标，右上角的最大化最小化按钮（好像关闭按钮会变个样。。。）
```

#### 正则（linedit）

```c++
QRegExp rx = QRegExp("[^\\\\/<>*|?:\"\\s]*"); //任意个[]中的字符
QRegExpValidator *validator = new QRegExpValidator(rx);
linedit->setValidator(validator);
linedit->setMaxLength(5); //设置最大输入限制5
```



### QGraphicsScene/View/Item

**事件传递顺序：view--scene--subitem--item**

setPos的坐标是**父类坐标系的坐标，一般对于item位于scene中的应用场景**。

translate()将当前视图坐标原点平移，从而实现显示图像的平移变换。由于默认场景的左上角顶点与视图坐标原点对齐，translate()将坐标原点平移，也就实现了将场景的平移。

rotate()将当前视图围绕视图坐标原点旋转，从而实现显示图像的旋转变换

#### 三种坐标系

Item坐标系

- Items位于它们自己的坐标系中。它的坐标都以点(0,0)为中心点，这也是所有变换的中心点
- 子坐标与父坐标之间的差值等于在父坐标系下，父item与子item之间的距离

场景坐标系

- 场景坐标系统描述了每个最顶级item的位置

- 场景中的每个item有场景位置与包围矩形，场景位置描述了item在场景坐标下的位置，

  场景包围矩形则用于QGraphicsScene决定场景中哪块区域发生了变化。

- 场景中的变化通过QGraphicsScene::changed()信号来通知，它的参数是场景矩形列表

视图坐标系

- 视图坐标是widget的坐标，视图坐标中每个单位对应一个像素。

- 视图坐标不会被所观察的场景所影响。QGraphicsView的视口的左上角总是（0，0），

  右下角总是(视口宽，视口高）。

- 所有的鼠标事件与拖拽事件，最初以视图坐标表示，应该把这些坐标映射到场景坐标以便与item交互。

  

#### 坐标映射

在场景与item之间，item与item之间，视图与场景之间进行坐标映射，形状映射是非常有用的。

**例：当你在QGraphicsView的视口中点击鼠标时，获知光标下是场景中的哪个item？**

1. 调用QGraphicsView::mapToScence()；坐标映射；
2. 调用QGraphicsScene::itemAt()；获取item；

**例：你想获知一个item位于视口中的什么位置？**

1. item上调用QGraphicsItem::mapToScene()；坐标映射；
2. 调用QGraphicsView::mapFromScene()；坐标映射；

**例：你想在一个视图椭圆中有哪些items？**

1. QPainterPath传递到mapToScene()；坐标映射；
2. 映射后的路径传递到QGraphicsScene::items()；获取item；

**PS:**

- 调用QGraphicsItem::mapToScene()与QGraphicsItem::mapFromScene()；

  在item与场景之间进行坐标与形状的映射。

- 在item与其父item之间通过，

  QGraphicsItem::mapToParent()；QGraphicsItem::mapFromItem()；进行映射。

- 所有映射函数可以包括点，矩形，多边形，路径。视图与场景之间的映射也与此类似。

- 对于从视图与item之间的映射，你应该首先映射到场景，然后再从场景向item进行映射。



#### QGraphticsScene

场景——QGraphticsScene管理items，可受多个QGraphicsView监视，提供操作/获取items的接口

- 将场景中的事件传递到item中（如点击位置的item）
- 控制item的focus，获得当前focus_item
- 将部分场景传递到绘图设备进行渲染



#### QGraphicsView

可视化场景中的内容，多视图可连接一个场景，为一份数据提供多个视口，是一个滚动区域

GraphicsView::setViewport()来把一个QGLWidget设为视口。

- 视图输入鼠标键盘等事件，传入到场景中时，自动将视口坐标转化为场景坐标
- 坐标转换的函数：QGraphicsView::mapToScene()； QGraphicsView::mapForScene()；
- QGraphicsView默认带有内边距和边界

**旋转与缩放**

- QGraphicsView通过QGraphicsView::setMatrix()支持同QPainter一样的仿射变换
- 当对视图变换时，QGraphicsView会对视图中心进行校正。

**拖拽**

- QGraphicsView继承自QWidget,它也提供了像QWidget那样的拖拽功能
- 当视图接收到拖拽事件，它可翻译为QGraphicsSceneDragDropEvent,再发送到场景
- 场景接管这个事件，把它发送到光标下接受拖拽的第一个item
- 一个item开始拖拽时，创建一个QDrag对象，传递开始拖拽的那个widget的指针
-  mousePressEvent()或mouseMoveEvent()中，你可以从事件中得到那个原始的widget指针

```c++
 void CustomItem::mousePressEvent(QGraphicsSceneMouseEvent *event) 
 { 
     QMimeData *data=new QMimeData; 
     data->setColor(Qt::green); 
     QDrag *drag=new QDrag(event->widget()); 
     drag->setMimeData(data); 
     drag->start(); 
 } 
```

-  为了在场景中载取拖拽事件，你应重新实现

  QGraphicsScene::dragEnterEvent();

  和在QGraphicsItem的子类里任何与你特定场景需要的事件处理器。

- items也可以通过调用QGraphicsItem::setAcceptDrops()获得拖拽支持,

  为了处理将要进行的拖拽，你需要重新实现,

  QGraphicsItem::dragEnterEvent();

  QGraphicsItem::dragMoveEvent();

  QGraphicsItem::dragLeaveEvent();

  QGraphicsItem::dropEvent();

#### QGraphicsItem

图形items的基类，提供了一些典型的形状如，矩形、椭圆、文本，接收许多事件

- 具有本地坐标系，同时提供多种坐标转换接口
- 通过matrix();进行旋转、缩放，通过shape();返回形状
- 碰撞检测colludeWith();可以重写

**item组**

- 通过把一个item做为另一个item的孩子，你可以得到item组的大多数本质特性。
- 这些items会一起移动，所有变换 会从父到子传递。
- QGraphicsItem也可以为它的孩子处理所有的事件，这样就允许以父亲代表它所有的孩子，可以有效地把所有的items看作一个整体。 
- 另外，QGraphicsItemGroup是一个特殊的item,它既对孩子事件进行处理又有一个接口把items从一个组中增加和删除。把一个item加到 QGraphicsItemGroup仍会保留item的原始位置与变换。
- 而给一个item重新指定父item则会让item根据其新的父亲重新定位。可以用QGraphicsScene::createItemGroup()建组。



### QUndoStack

### QEvent

### QVariant

```c++
QMap<EnumPipelineType,QVector<EnumClassType>> map;
map.insert()....
QVariant var = QVariant::fromValue(map);

QMap<EnumPipelineType,QVector<EnumClassType>> mapConfigInfo = var.value<QMap<EnumPipelineType,QVector<EnumClassType>>>();
```

### ToolBar

| addAction()      | 添加具有文本或图标的工具按钮                     |
| ---------------- | ------------------------------------------------ |
| addSeperator()   | 分组显示工具按钮                                 |
| addWidget()      | 添加工具栏中按钮以外的控件                       |
| addToolBar()     | 使用QMainWindow类的方法添加一个新的工具栏        |
| setMovale()      | 工具栏变得可移动                                 |
| setOrientation() | 工具栏的方向可以设置为Qt.Horizontal或Qt.vertical |



qstringlist&vector<qstring>等价

### 设置鼠标中心点缩放

```c++
    setTransformationAnchor(QGraphicsView::AnchorUnderMouse);
    setResizeAnchor(QGraphicsView::AnchorUnderMouse);
```



### QBrush坑

如果 qbrush初始化为空时，需要设置Qt::SolidPattern，否则设置颜色无效

```C++
 ItemDetail(){
        brush_.setStyle(Qt::SolidPattern);
    }
    int type_ = 0;
    ItemStyle style_;
    QPen pen_;
    QBrush brush_;
    bool handles_visible_ = false;
    bool sample_handles = true;
    QList<QSharedPointer<AQHandleItem>> handles_;
    QPainterPath label_path_;
    QSharedPointer<AQHandleItem> current_handle_;
```

### 黑白棋盘背景

```c++

#include "QtWidgetsApplication2.h"
#include<QPixmap>
#include<QPainter>
QtWidgetsApplication2::QtWidgetsApplication2(QWidget *parent)
    : QWidget(parent)
{
    ui.setupUi(this);
    QPixmap pm(20, 20);
    QPainter pmp(&pm);
    pmp.fillRect(0, 0, 10, 10, Qt::lightGray);
    pmp.fillRect(10, 10, 10, 10, Qt::lightGray);
    pmp.fillRect(0, 10, 10, 10, Qt::darkGray);
    pmp.fillRect(10, 0, 10, 10, Qt::darkGray);
    pmp.end();
    QPalette pal = palette();
    pal.setBrush(backgroundRole(), QBrush(pm));
    setAutoFillBackground(true);
    setPalette(pal);
}
```

不要边setpos边mapto

### View派生类中的paintEvent:绘制设备返回的引擎== 0，类型:1

同样画黑白棋盘

```c++
//报错
QWidget::paintEngine: Should no longer be called
QPainter::begin: Paint device returned engine == 0, type: 1
```

```c++
//由于View是QAbstractScrollArea的子类，因此应在其视口中打开QPainter:
QTableView::paintEvent(event);
```









