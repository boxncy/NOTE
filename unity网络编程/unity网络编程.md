# unity网络编程

## socket

### socket中的信息：

- 协议 tcp/udp；
- 本地IP：192.168.1.12；
- 本地端口号：8000；
- 远程IP：xxx.xxx.x.xx;
- 远程端口号：xxxx

### socket通信流程：

1. 开启链接前，创建socket （使用API Socket），并绑定本地端口<u>（客户端连接时使用API Connect可以自动分配端口，省去绑定步骤）？</u>；
2. 服务端监听（API Listen），等待客户端接入<u>（其实这个应该是第一步）</u>；
3. 客户端连接服务端（API Connect）；
4. 服务端接受连接（API Accept）；<u>（此时通过以上步骤已经建立连接，可以通信）</u>；
5. 客户端和服务端通过（API Send）（API Receive）发送消息，<u>操作系统自动完成数据确认和重传？</u>；
6. 某一方关闭连接（API Close），执行四次挥手，断开连接；

![](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 094230.png)

### 简单示例：Echo.cs

<u>向服务端发送文本信息；接收服务端发来的信息；</u>

`using System.Net.Sockets;`

`using UnityEngine.UI;`

`public class Echo:MonoBehaviour`

`{`

​	`Socket socket;`

​	`public InputField inputField;`

​	`public Text text;`

<u>//点击连接按钮</u>

​	`public void Connection`()

​		`{`

<u>//Socket初始化</u>

​			`socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.TCP);`

<!--InterNetwork指代使用IPv4/IPv6-->

<!--SocketType是套接字类型,此处使用stream字节流-->

<!--ProtocolType指明协议类型-->

​			`socket.Connect("127.0.0.1",8888)`

<!--Connect是一个阻塞方法--><u>所以会用线程？</u>

​		`}`

<u>//点击发送按钮</u>

`public void Send()`

​		`{`

<u>//发送</u>

​			`string sendStr = InputField.text;`

​			`byte[ ] sendBytes = System.Text.Encoding.Default.GetBytes(sendStr);`

​			`socket.Send(sendBytes);`

<!--Send是一个阻塞方法-->

<!--接收byte[]类型参数，Send方法会指明数据长度，发送给服务端-->

<u>//接收</u>

​			`byte[] readBuff = new byte[1024];`

​			`int count = socket.Receive(readBuff);`

<!--Receive是一个阻塞方法-->

<!--byte[]类型参数储存数据，Receive方法会返回数据长度-->

​			`string recvStr = System.Text.Encoding.Default.GetString(readBuff,0,count);`

​			`text.text = recvStr;`

​			`socket.Close();`

<!--关闭连接-->

​		`}`

`}`

## 异步服务端

### 用户信息 ClientState

`class ClientState`

`{`

​	`public Socket socket;`

​	`public byte[] readBuff = new byte[1024];`

`}`

ClientState用于存socket和缓冲区数据；



### 用户列表 Dictionary clients

`static Dictionary<Socket,ClientState> clients `

`= new Dictionary<Socket,ClientState>();`

Dictionary<Socket,ClientState> clients用此结构 key value 获取客户端信息；



### 异步方法 beginxxx；endxxx；

![](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-20 155044.png)

![](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 151431.png)

![屏幕截图 2021-03-18 151505](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 151505.png)

![屏幕截图 2021-03-18 151611](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 151611.png)



![屏幕截图 2021-03-18 151809](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 151809.png)

![屏幕截图 2021-03-18 154322](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 154322.png)

![屏幕截图 2021-03-18 154332](C:\Users\boxnc\Desktop\GRATH\unity网络编程\屏幕截图 2021-03-18 154332.png)

## Static NetManger

### 消息队列 msgList

```c#
 //消息列表
    static List<string> msgList = new List<string>();
```

​	从服务端收到的消息，socket异步存到list中，在Update()中读取，使用协议调用回调方法；

![image-20210320160908422](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20210320160908422.png)

### 监听列表  listeners

```c#
//定义delegte
public delegate void MsgListener(string str);
//listeners
private static Dictionary<string, MsgListener> listeners 
    = new Dictionary<string, MsgListener>();
//添加监听
public static void AddListener(string msgName, MsgListener listener)
	{
   	 	listeners[msgName] = listener;
  	}
```

​	从msgList取出的消息，通过协议分割，调用回调函数

```c#
public static void Update()
    {
        if (msgList.Count <= 0) return;
        string msgStr = msgList[0];
        msgList.RemoveAt(0);
        string[] split = msgStr.Split('|');
        string msgName = split[0];
        string msgArgs = split[1];
        //监听回调
        if(listeners.ContainsKey(msgName))
        {
            Debug.Log("加入监听list" + msgName + msgArgs);
            listeners[msgName](msgArgs);
        }
    }
```

​	在客户端使用AddListener()绑定回调方法

```c#
void Start()
    {
        NetManager.AddListener("Enter", OnEnter);
        NetManager.Connect("127.0.0.1", 8888);
    }

private void OnEnter(string msg)
    {
        Debug.Log("OnEnter" + msg);
    }

void Update()
    {
        NetManager.Update();
    }
```

![image-20210320163301095](C:\Users\boxnc\AppData\Roaming\Typora\typora-user-images\image-20210320163301095.png)



### 协议的实例

#### Enter

​	服务端将客户端发来的消息解析，再使用委托完成处理

```c#
string[] split = recvStr.Split('|');
                string msgName = split[0];
                string msgArgs= split[1];
                string funName = "Msg" + msgName;
                MethodInfo mi = typeof(MsgHandler).GetMethod(funName);
                object[] o = { state, msgArgs };
                mi.Invoke(null, o);
```

```c#
public static void MsgEnter(ClientState c, string msgArgs)
        {
            //解析
            Console.WriteLine("MsgEnter" + msgArgs);
            string[] split = msgArgs.Split(',');
            string desc = split[0];
            float x = float.Parse(split[1]);
            float y = float.Parse(split[2]);
            float z = float.Parse(split[3]);
            float euly = float.Parse(split[4]);
            int hp = int.Parse(split[5]);
            //更新
            c.hp = hp;
            c.x = x;
            c.y = y;
            c.z = z;
            c.euly = euly;
            //发到每个player
            string sendStr = "Enter|" + msgArgs;
            byte[] sendByte = System.Text.Encoding.Default.GetBytes(sendStr);
            foreach (ClientState cs in MainClass.clients.Values)
            {
                cs.socket.Send(sendByte);
            }
        }
```

#### List

​	同理，使用list发出读取列表的请求

```c#
public static void MsgList(ClientState c, string msgArgs)
        {
            string sendStr = "List|";
            foreach(ClientState cs in MainClass.clients.Values)
            {
                sendStr += cs.socket.RemoteEndPoint.ToString() + ",";
                sendStr += cs.x.ToString() + ",";
                sendStr += cs.y.ToString() + ",";
                sendStr += cs.z.ToString() + ",";
                sendStr += cs.euly.ToString() + ",";
                sendStr += cs.hp.ToString() + ",";
            }

            byte[] sendByte = System.Text.Encoding.Default.GetBytes(sendStr);
            foreach (ClientState cs in MainClass.clients.Values)
            {
                cs.socket.Send(sendByte);
            }
        }
```





























































