1.优化滚轮缩放效果

2.log日志增加版本信息

3.新增键盘事件

4.新增鼠标样式

5.新增存图质量参数l

6.新增保存渲染图接口

修复bug

1.修复modify图元崩溃

2.修复gui标注工具崩溃

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <memory>

using namespace std;

// 抽象产品类
class Product {
public:
    virtual void operation() = 0;
    virtual ~Product() {}
};

// 具体产品类A
class ConcreteProductA : public Product {
public:
    void operation() override {
        cout << "This is ConcreteProductA." << endl;
    }
};

// 具体产品类B
class ConcreteProductB : public Product {
public:
    void operation() override {
        cout << "This is ConcreteProductB." << endl;
    }
};

// 抽象工厂类
class Factory {
public:
    virtual unique_ptr<Product> createProduct() = 0;
    virtual ~Factory() {}
};

// 具体工厂类A
class ConcreteFactoryA : public Factory {
public:
    unique_ptr<Product> createProduct() override {
        return make_unique<ConcreteProductA>();
    }
};

// 具体工厂类B
class ConcreteFactoryB : public Factory {
public:
    unique_ptr<Product> createProduct() override {
        return make_unique<ConcreteProductB>();
    }
};

// 工厂注册表
class FactoryRegistry {
public:
    static FactoryRegistry& instance() {
        static FactoryRegistry registry;
        return registry;
    }

    void registerFactory(const string& key, unique_ptr<Factory> factory) {
        factories_[key] = move(factory);
    }

    unique_ptr<Product> createProduct(const string& key) {
        return factories_[key]->createProduct();
    }

private:
    unordered_map<string, unique_ptr<Factory>> factories_;
};

// 注册具体工厂类
void registerFactories() {
    FactoryRegistry::instance().registerFactory("A", make_unique<ConcreteFactoryA>());
    FactoryRegistry::instance().registerFactory("B", make_unique<ConcreteFactoryB>());
}

// 主函数
int main() {
    registerFactories();

    // 读入用户输入的字符串，创建相应的产品对象
    string input;
    cin >> input;
    unique_ptr<Product> product = FactoryRegistry::instance().createProduct(input);
    product->operation();

    return 0;
}
```

解释这段代码

```cmake
cmake_minimum_required(VERSION 3.0)
project(pro1)

set(CMAKE_DEBUG_POSTFIX "d")

add_library(project2 SHARED project2.cpp)

set_target_properties(project2 PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/build"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin"
    ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${CMAKE_CURRENT_BINARY_DIR}/build/debug"
    LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_CURRENT_BINARY_DIR}/bin/debug"
    RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_CURRENT_BINARY_DIR}/bin/debug"
    ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${CMAKE_CURRENT_BINARY_DIR}/build/release"
    LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_CURRENT_BINARY_DIR}/bin/release"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_CURRENT_BINARY_DIR}/bin/release")

target_include_directories(project2 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/project2/include)

target_link_libraries(pro1 project2)

install(TARGETS pro1 project2
        ARCHIVE DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/build
        LIBRARY DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/bin
        RUNTIME DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/bin)

install(DIRECTORY project2/include/
        DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/include)
```

你可以通过在QWidget的paintEvent函数中进行绘制来实现这个效果。具体实现步骤如下：

1.  在paintEvent函数中创建一个QPainter对象，并设置好矩形框的绘制样式，比如线条颜色、线条宽度等。
2.  调用QPainter的drawRect函数来绘制矩形框。矩形框的位置可以通过QWidget的rect函数获取到，然后再在外框向内偏移10像素的位置进行绘制。
3.  在绘制矩形框的下一行，通过QPainter的drawText函数绘制文字。文字的位置可以通过矩形框的位置和大小计算得出，然后再向上偏移相同的距离。
4.  如果想要被文字覆盖的位置不绘制矩形框，可以先保存当前QPainter的状态，然后通过QPainter的setClipRect函数设置一个裁剪区域，使得矩形框只绘制在不被文字覆盖的区域。绘制完成后，通过QPainter的restore函数恢复之前的状态。

下面是一个示例代码，其中假设QWidget的名称为MyWidget，需要显示的文字为"Hello World!"，矩形框的颜色为红色，线条宽度为2像素。

```cpp
cppCopy codevoid MyWidget::paintEvent(QPaintEvent *event)
{
    QPainter painter(this);

    // 设置矩形框的绘制样式
    painter.setPen(QPen(Qt::red, 2));

    // 计算矩形框的位置和大小
    QRect rect = this->rect().adjusted(10, 10, -10, -10);

    // 绘制矩形框
    painter.drawRect(rect);

    // 计算文字的位置
    QRect textRect = QRect(rect.x(), rect.y() - painter.fontMetrics().height() - 2, rect.width(), painter.fontMetrics().height());

    // 绘制文字
    painter.drawText(textRect, Qt::AlignCenter, "Hello World!");

    // 保存当前状态
    painter.save();

    // 设置裁剪区域
    painter.setClipRect(textRect.adjusted(-2, -2, 2, 2));

    // 绘制矩形框
    painter.drawRect(rect);

    // 恢复之前的状态
    painter.restore();
}
```

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

```
AQImportIncrementDialog
```

```
"json diffirent, cover?"
get_is_increment
```

导入后:

​	 获取当前模块train index vector

​	 修改本地路径train dataset,以及四个参数





`b.take(key)` 和 `b.insert(key, a.value(key))` 可以用 `b[key] = a.value(key)` 来代替。这是因为，当我们使用 `b[key] = a.value(key)` 时，如果 b 中不包含 key 这个属性，那么在执行该语句后，系统会自动将 key 添加到 b 中，并将它的值设置为 a 中对应的值。

因此，下面给出一个修改版的代码，这个版本将 b 中 value 和 a 中 key 相同的字段用 a 中的 value 进行覆盖操作，同时不会添加新字段到 b 中：

```cpp
c++复制代码QJsonParseError error;
QJsonDocument doc_a = QJsonDocument::fromJson(json_a.toUtf8(), &error);
QJsonObject a = doc_a.object();

QJsonDocument doc_b = QJsonDocument::fromJson(json_b.toUtf8(), &error);
QJsonObject b = doc_b.object();

replaceFields(a, b);

void replaceFields(const QJsonObject& a, QJsonObject& b) {
    for (auto key : a.keys()) {
        if (b.contains(key)) {
            if (a.value(key).isObject() && b.value(key).isObject()) { // 如果当前属性都是 JSON 对象，递归处理
                replaceFields(a.value(key).toObject(), b.value(key).toObject());
            } else {
                b[key] = a.value(key); // 用 a 中的值替换 b 中的值
            }
        }
    }
}
```

在上述代码中，如果 a 与 b 中都有这个字段，则直接使用 `b[key] = a.value(key)` 将 b 中的值替换为 a 中的值。因此，这个版本的代码更加简洁。

经过这个处理，就可以实现将 b 中与 a 相同 key 的字段用 a 的 value 进行覆盖操作，并且不会添加新字段到 b 中。

//////////\



在 Qt 中实现可以使用 `QJsonParseError` 和 `QJsonDocument` 类解析和操作 JSON 数据。下面是一个示例代码，其中 `updateJson` 函数用于实现更新两个 JSON 字符串。

```
cpp复制代码#include <QCoreApplication>
#include <QDebug>
#include <QJsonDocument>
#include <QJsonObject>
#include <QJsonArray>
#include <QJsonValue>
#include <QJsonParseError>

void updateJson(QString& oldJson, QString& newJson)
{
    // 解析旧的 JSON
    QJsonParseError error;
    QJsonDocument oldJsonDoc = QJsonDocument::fromJson(oldJson.toUtf8(), &error);
    if (error.error != QJsonParseError::NoError) {
        qWarning() << "Parse old JSON failed:" << error.errorString();
        return;
    }

    // 解析新的 JSON
    QJsonDocument newJsonDoc = QJsonDocument::fromJson(newJson.toUtf8(), &error);
    if (error.error != QJsonParseError::NoError) {
        qWarning() << "Parse new JSON failed:" << error.errorString();
        return;
    }

    if (!oldJsonDoc.isObject() || !newJsonDoc.isObject()) {
        qWarning() << "JSON format error";
        return;
    }

    // 递归合并 JSON 对象
    QJsonObject oldObj = oldJsonDoc.object();
    QJsonObject newObj = newJsonDoc.object();

    for (auto it = oldObj.begin(); it != oldObj.end(); ) {
        auto key = it.key();
        auto value = it.value();

        // 新 JSON 中不存在当前键，则将其删除
        if (!newObj.contains(key)) {
            it = oldObj.erase(it);
        }
        else {
            if (value.isObject() && newObj[key].isObject()) {
                // 如果当前键对应的值是 JSON 对象，则递归合并
                updateJson(value.toObject(), newObj[key].toObject());
            }
            else {
                // 否则直接替换旧值
                it.value() = newObj[key];
            }
            ++it;
        }
    }

    // 将合并后的 JSON 对象转回字符串格式
    QJsonDocument updatedDoc(oldObj);
    QString updatedJson = updatedDoc.toJson(QJsonDocument::Compact);
    oldJson.swap(updatedJson);
}

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);

    QString oldJson = "{\"g_common\": {\"annotate\": \"模型训练经常需要调节的参数\", \"cn_name\": \"通用参数\", \"data_type\": \"ParamHelper\", \"id\": 0, \"meta_type\": 2, \"permission\": 0, \"value\": {\"batch_size\": {\"annotate\": \"网络训练每一次迭代时参与训练的图像数量\", \"cn_name\": \"训练批次\", \"ctrl_info\": {\"max\": 512, \"min\": 1, \"precision\": 1}, \"data_type\": \"ph_Int\", \"id\": 1, \"meta_type\": 11, \"permission\": 1, \"value\": 4}}}}}";
    QString newJson = "{\"g_common\": {\"annotate\": \"模型训练经常需要调节的参数\", \"cn_name\": \"通用参数\", \"data_type\": \"ParamHelper\", \"id\": 0, \"meta_type\": 2, \"permission\": 0, \"value\": {\"batch_size\": {\"annotate\": \"网络训练每一次迭代时参与训练的图像数量\", \"cn_name\": \"训练批次\", \"ctrl_info\": {\"max\": 1024, \"min\": 1, \"precision\": 1}, \"data_type\": \"ph_Int\", \"id\": 1, \"meta_type\": 11, \"permission\": 1, \"value\": 8}}}}}";

    qDebug() << "Old JSON:" << oldJson;
    qDebug() << "New JSON:" << newJson;

    updateJson(oldJson, newJson);

    qDebug() << "Updated JSON:" << oldJson;

    return a.exec();
}
```

上述代码中，`updateJson` 函数首先解析旧的 JSON 和新的 JSON，然后递归合并两个 JSON 对象（注意只合并 `value` 字段），最后将合并后的 JSON 对象转成字符串格式返回。在递归合并 JSON 对象时，如果当前键对应的值是 JSON 对象，则继续递归合并；否则直接替换旧值。

```cpp
int row = mark_table_view_->currentIndex().row();
    int column = mark_table_view_->currentIndex().column();
qDebug()<<"row"<<row;
    qDebug()<<"delete_mark_list_data";
    int model_row_count = mark_table_view_->model()->rowCount();
    row = row >= 1 ? row : 1;
    row = row < model_row_count ? row : model_row_count - 1;
    QModelIndex index = mark_table_view_->model()->index(row - 1, column);
    mark_table_view_->setCurrentIndex(index);
    slt_click_item(index);
    qDebug()<<"index"<<index;
```

