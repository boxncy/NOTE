## M_STUDIO RPG PROJECT STUDY NOTE

#### 1.Unity编辑器扩展，用到了 RequireComponent

如果脚本A上加了[RequireComponent(typeof(B))]，则当AddComponent<A>()时,如果当前没有添加组件B，则组件B会被自动添加。

#### 2.相机效果用Cinemacine，基本具有需要的所有效果

#### 3.Navigation，烘焙后选择目标点即可实现gameobject移动，具有属性isstop

#### 4.鼠标设置，Cursor的图标、位置等的设置

#### 5.ShaderGrap，实现遮挡剔除，如：显示树木后的player、enemy等 

#### 		<!--*PS:没学会，再看一下 lesson09*-->

- [ ] ####   已解决？

#### 6.简单的状态用 switch

#### 		<!--复杂的用fsm有限状态机，M_STUDIO的2D课程有-->

- [ ] ####   已解决？

#### 7.physics一般和碰撞有关，如检查一定范围内是否有player等

#### 8.navmesh：SamplePosition可以判断一个点是否是烘培后可移动到的

#### 9.lesson15.，创建脚本对象资源（ScriptableObject）到asset，保存数据

用来存储数据的一个**资源文件**，像是JSON、XML、文本文件这样的存储文件，可以用来存储数据。因为他是资源文件，所以他有着资源文件的特性，我们Resource.Load他就可以使用他了。
其实可以简单的理解为把你所有的数据都用变量在一个类中声明，然后我们使用的时候，直接实例化这个类就好了。作为资源文件，可以引用到脚本上

##### 

