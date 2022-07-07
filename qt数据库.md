### QT数据库

#### QtSql类的分层

​	由低到高

1. 驱动层：主要实现数据库和接口层之间的连接
   - QSqlDriver
   - QSqlDriverCreator
   - ......
2. SQL接口层：实现了对数据库的访问
   - QSqlDatabase：创建连接
   - QsqlQuery：使用SQL语句与数据库交互，以下四个类用于支持前两个
   - QSqlError
   - QSqlField
   - QSqlIndex
   - QSqlRecord
3. 用户接口层：将数据链接到窗口上，model/view实现
   - QSqlQueryModel
   - QSqlTableModel
   - QSqlRelationalTableModel

#### 数据库的使用

##### 	示例

```c++
//.pro 添加
QT += core sql
```

```c++
#include <QSqlDatabase>
#include <QSqlDatabase>

//添加数据库驱动，qt支持的数据库可以通过 QSqlDatabase::drivers() 查看
QSqlDatabase db = QSqlDatabase::addDatabase("QSQLITE");
//设置数据库名称
db.setDatabaseName("Database173.db");//“:memory:”表示在内存中建立
//检查数据库链接
if(!db.open()){
    return false;
}

//开始执行sql语句
QSqlQuery query;
//新建表，设置id为主键，添加path项，path项不大于450个字符
query.exec("create table image(id int primary key,path varchar(450))");
//向表中插入数据
query.exec("insert into image values(1,'D:\PROTECT\UnsuperClassify_0\source\1.png')");
query.exec("insert into image values(2,'D:\PROTECT\UnsuperClassify_0\source\2.png')");
query.exec("insert into image values(3,'D:\PROTECT\UnsuperClassify_0\source\3.png')");
//查询操作
query.exec("select id,path from where id >=2");
//query.next()指向查询到的第一条记录，每次后移一个
while(query.next()){
    int value_id = query.value(0).toInt();
    qstring value_path = query.value(1).toString();
}
```

##### 更详细的query操作

```c++
query.exec("select * from image");
```

- 结果集其实就是查询到的所有记录的集合，记录是从0开始编号的

最常用的操作有：

- `seek(int n)` ：`query`指向结果集的第n条记录；
- `first()` ：`query`指向结果集的第一条记录；
- `last()` ：`query`指向结果集的最后一条记录；
- `next()` ：`query`指向下一条记录，每执行一次该函数，便指向相邻的下一条记录；
- `previous()` ：`query`指向上一条记录，每执行一次该函数，便指向相邻的上一条记录；
- `record()` ：获得现在指向的记录；
- `value(int n)` ：获得属性的值。其中`n`表示你查询的第n个属性，比方上面我们使用`“select * from image`”就相当于`“select id, path from image”`，那么`value(0)`返回`id`属性的值，`value(1)`返回`path`属性的值。该函数返回`QVariant`类型的数据。
- `at()` ：获得现在`query`指向的记录在结果集中的编号。

**注意：**刚执行完`query.exec("select *from image");`这行代码时，`query`是指向结果集以外的，我们可以利用`query.next()`使得 `query`指向结果集的第一条记录。

```c++
void MainWindow::on_pushButton_clicked()
{
    QSqlQuery query;
    query.exec("select * from image");
    //开始就先执行一次next()函数，那么query指向结果集的第一条记录
    if(query.next())
    {
       //获取query所指向的记录在结果集中的编号
       int rowNum = query.at(); // 0
       //获取每条记录中属性（即列）的个数
       int columnNum = query.record().count(); // 2
       //获取"path"属性所在列的编号，列从左向右编号，最左边的编号为0
       int fieldNo = query.record().indexOf("path"); // 1
       //获取id属性的值，并转换为int型
       int id = query.value(0).toInt(); // 0
       //获取path属性的值
       QString path = query.value(fieldNo).toString(); // 填入的路径
    }
//定位到结果集中编号为2的记录，即第三条记录，因为第一条记录的编号为0
    if(query.seek(2))
    {
       ...
    }
//定位到结果集中最后一条记录
    if(query.last())
    {
       ...
    }
//占位符操作
    query.exec(QString("select path from image where id =%1").arg(id));
```

##### ODBC表示方法

###### 占位表示方法

```c++
void MainWindow::on_pushButton_clicked()
{
    QSqlQuery query;
    //prepare方法,:id, :path用于表示占位
    query.prepare("insert into image (id, path) "
                  "values (:id, :path)");
    //bindvalue() 给id、path两个属性赋值
    //编号0和1分别代表“:id”和“:path”，就是说按照prepare()函数中出现的属性从左到右编号，最左边是0 。
    query.bindValue(0, 4);
    query.bindValue(1, path_4);
    //最后一定要执行exec()函数，所做的操作才能被真正执行
    query.exec();
}

//也可以利用addBindValue()函数，这样就可以省去编号，它是按顺序给属性赋值的，如下：
query.addBindValue(4);
query.addBindValue(path_4);
//当用ODBC的表示方法时，我们也可以将编号用实际的占位符代替，如下：
query.bindValue(":id", 4);
query.bindValue(":path", path_4);
```

###### 批处理

```c++
void MainWindow::on_pushButton_clicked()
{
    QSqlQuery q;
    q.prepare("insert into image values (?, ?)");
    QVariantList ints;
    ints << 1 << 2 << 3 << 4;
    q.addBindValue(ints);
    QVariantList paths;
    //最后一个是空字符串(结果就是只有id没有path)，应与前面的格式相同
    paths << path_1 << path_2<< path_3 << QVariant(QVariant::String);
    q.addBindValue(names);
    if (!q.execBatch()) //进行批处理，如果出错就输出错误
}
```

##### 事务操作

1.   事务可以保证一个复杂的操作的原子性，就是对于一个数据库操作序列，要么全部做完，要么一条也不做，它是一个不可分割的工作单位。

2.   在Qt中，如果底层的数据库引擎支持事务，那么`QSqlDriver::hasFeature(QSqlDriver::Transactions)`会返回`true`。

3.   使用`QSqlDatabase::transaction()`来启动一个事务，然后编写SQL语句。

4.   最后调用`QSqlDatabase::commit()`或者`QSqlDatabase::rollback()`。

5.   当使用事务时必须在创建查询以前就开始事务。

```c++
//查询是否可以使用事务
QSqlDatabase::database().transaction();
QSqlQuery query;
query.exec("SELECT id FROMemployee WHERE name = 'Torild Halvorsen'");
if (query.next()) {
    int employeeId = query.value(0).toInt();
    query.exec("INSERT INTO project(id, name, ownerid) "
               "VALUES (201, 'ManhattanProject', "
               + QString::number(employeeId) + ')');
}
QSqlDatabase::database().commit();
```

#### QSqlModel

提供了更简单的数据库操作，暂不讨论
