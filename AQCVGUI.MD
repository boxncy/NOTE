## Tool

### Tool_Factory

-   使用前，需要由用户显示调用接口进行全局的注册
-   不可重复注册，可以取消注册
-   提供获得所有工具类型的接口
-   通过工具类型，创建具体工具

### Tool_Factory接口

```cpp
/**
   * @brief 获得所有工具类型
   * @return const vector<std::string>&
*/
  static const vector<std::string>& all_types();
```



```cpp
/**
   * @brief 子类注册
*/
  static void regist();
/**
   * @brief 获得具体工厂
   * @param const std::string& tool_type
   * @return AQToolFactory*
*/
  static AQToolFactory* get_factory(const std::string& tool_type)
/**
   * @brief 取消注册具体工具
   * @param const std::string& tool_type
*/
  static void unload_regist(const std::string& tool_type);
/**
   * @brief 工具是否注册
   * @param const std::string& tool_type
   * @return bool
*/
  static bool registed(const std::string& tool_type);
```

AQTool继承于QObject，资源由画布管理

```cpp
/**
   * @brief 创建具体工具
   * @return AQTool*
*/
  tool:AQTool* create_tool();
```

### Tool_Factory demo

```cpp
namesapce tool{
    const vector<std::string> all_types ={}

    class AQToolFactory {
    public:
        virtual AQTool *create_tool() = 0;
        static AQToolFactory* get_factory(const std::string& tool_type)
        {
            AQToolFactory* instance = check(tool_type);
            return instance;
        }
    	static bool registed(const std::string& tool_type)
    	{
            bool result = false;
            if(check(tool_type)){
                result = true;
            }
            return result;
        }
    
    protected: 
        static void Register(const std::string& tool_type, AQToolFactory* func)
        {
            AQToolFactory* regist = check(tool_type); // 防止重复注册
            if (regist != NULL)
            {
                return;
            }
            else
            {
                registry.insert(std::make_pair(tool_type, func));
            }
        }
    
        static AQToolFactory* check(const std::string& tool_type)
        {
            std::map<std::string, AQToolFactory*>::iterator iter = registry.find(tool_type);
            if (iter != registry.end())
                return iter->second;
            else
                return nullptr;        
        }
    private:
    	static std::map<std::string, AQToolFactory*> registry = nullptr;
	};

    class AQPenToolFactory : public AQToolFactory{
	public:
    	virtual AQTool *create_tool(){
    		return new pen_tool();
    	}
        static void regist(){AQToolFactory::Register("AQPenToolFactory", new AQPenToolFactory());}  
	};
}

custom_regist()
{
    AQRectToolFactory::regist();
    ...
}

main()
{
    custom_regist();
    AQToolFactory* factory = nullptr;
    for(auto tool_type : tool_types)
    {
        factory = AQToolFactory::GetInstance(tool_type);
        if (factory != nullptr)
        {
            AQTool* tool = factory->create_tool();
        }
    }
}
```

### AQTool

-   工具与画布绑定，拦截画布事件，处理在画布绘制过程与特殊鼠标事件
-   返回序列化的最终绘制过程信息，由外部完成图元的创建
-   画布与工具绑定，内部有画布的指针，完成绘制过程临时图元的显示
-   工具继承QObject，tool资源由画布控制

![img](https://cdn.nlark.com/yuque/0/2022/jpeg/22574189/1662064992387-df8c8104-f7ed-4298-b661-0a4e4dbdc695.jpeg)

## Canvas

-   由AQGraphicsview和AQGraphicscene组成
-   event_dispatcher拦截转发事件
-   UndoStack处理图元undo/redo

![img](https://cdn.nlark.com/yuque/0/2022/jpeg/22574189/1662064832278-39972fbb-3186-4e4a-848f-ac0aac87bd51.jpeg)

## Item

### Item_Factory

-   与Tool_Factory类似，区别是在构造函数中隐式注册。

```cpp
class AQRectItemFactory : public AQItemFactory{
	public:
    	void AQRectItemFactory(){
            AQItemFactory::Register("AQRectGraphicsItem", new AQRectGraphicsItem());
        }
    	virtual AQRectGraphicsItem *create_item(){
    		return new AQRectGraphicsItem();
    	}
		virtual AQRectGraphicsItem *create_item(const std::string& param,const std::string& custom_info){
    		return new AQRectGraphicsItem();
    	}
};
```

### item的构造参数

-   实现一个参数构造类，提供参数构造接口如

```cpp
ParamCreator::create_rect_param(Point topleft,double width,double  height,std::string& param)
```

-   提供方法，允许输入json格式的键值对,作为item的动态属性，并序列化为std::string

### AQGraphicsItem

-   实现各种图元的绘制、鼠标事件
-   每种图元实现自己的UndoCommand
-   Handles作为item拉伸和旋转操作点的显示

![img](https://cdn.nlark.com/yuque/0/2022/jpeg/22574189/1662064945652-51bdfcd7-7c27-4232-8003-50e8dacde3d8.jpeg)

## undo/redo

-   对图元的操作，以undocommand*的形式push到画布的undostack中，实现撤销回滚
-   实现commmand : add、delete、move、resize、rotate、move_merge

![img](https://cdn.nlark.com/yuque/0/2022/jpeg/22574189/1662083615940-24112820-68c0-4b7e-8b48-f95ea988da1b.jpeg)

## c#调用方式