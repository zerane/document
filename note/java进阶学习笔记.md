# 前言 #

因为书找不到，突然想到应该列一个内容清单，与java比较相关的放在这里

- 数据结构
- 并发
- 网络
- 国际化
- mvc
- web
- 动态性
- io
- 异常
- GUI（这个感觉还是略看下）
- java7更新
- java8更新
- 虚拟机基础 
	- 对象生命周期
- 安全
- java源码               

后续内容记得也放在这里



- 《深入理解java虚拟机》
- 《java语言程序设计（进阶篇）》   

# 正文 #
## 数据结构 ##
### 泛型 ###
基本的泛型类就不写了

泛型函数：（可脱离泛型类存在），

- 泛型标记的作用域貌似和变量类似
- 泛型函数可以通过extends或者super来标记类型的一定功能，应该说是对类型做一些限定，功能是给出限定范围后才确定的
- 泛型函数使用的时候不需要声明类型，但是需要满足接口条件
 
例子：

    public static <XA extends Comparable> void templateFunc(XA inpara1,XA inpara2){
    	System.out.println(inpara1.compareTo(inpara2));
    }

?,? extends T,? super T

将类带入?决定是否正确

编译中会将泛型消除，全部替换为实际使用的类型，所以instanceof并不区分泛型

### 集合类 ###
感觉java语言程序设计p22的图更清楚点，这个有点乱
![](http://i.imgur.com/tWc6uWe.jpg)

ListIterator双向迭代

compareable接口，sort函数这个写法可以借鉴以实现实体类的比较

	public class Solution {
		public static void main(String[] args) throws IOException{
			List<Integer> l1 = new LinkedList<Integer>();
        	l1.sort(new Solution().new Toc());
		}

		public class Toc implements Comparator<Integer>{
			@Override
			public int compare(Integer o1, Integer o2) {
				// TODO Auto-generated method stub
				return 0;
			}
		}
	}

也可以这么写：(这个写法需要思考下)
看起来这是在创建实例的时候重载了某个函数，具体写法叫啥暂时不知道

		Comparator<Interval> intervalOrder =  new Comparator<Interval>(){
			@Override  
            public int compare(Interval i1, Interval i2) {  
                // TODO Auto-generated method stub  
                return 0;
            }
        };
        Queue<Interval> priorityQueue =  new PriorityQueue<Interval>(intervalOrder);

Collections中有对集合或者列表常用的一系列函数，注意collections是一个工具类，collection是接口或者说抽象类

具体类除了优先队列都能clone和序列化，注意是具体类，所以需要转成LinkedList之类的才能用

vector和stack是java早期版本遗留，据说vector效率较低，不知道stack如何，queue有个抽象子类叫deque（de：double-ended），从图中看在类库中只能实例化为linkedlist，队列只有这个和优先队列，貌似内容不是很多

HashSet中可以定义初始大小和负载，ArrayList中只能定义初始大小，其他集合没看，不过感觉和hash有关的都能定义负载？

LinkedHashSet和TreeSet都是要在Hash环境下保持与一个顺序，前者顺序是插入顺序，后者是自定义的顺序，而且treeset可以干一些奇奇怪怪的事，比如ceil，floor啥的

### 各种数据结构的实现 ###
具体实现貌似不难，看看有啥感悟。好吧原来还有avl树
#### 线性表 ####
arraylist扩容1.5倍，vector扩容2倍，之后可以看看源码

arraylist和linkedlist有些自己的方法，比如ensurecapacity、getfirst、getlast啥的，可能在某些场景下可以提高点效率

linkedlist的增删效率也是O(n)因为需要先找到目标节点

思考一下栈和队列分别用数组和链表如何实现。好像也就数组实现队列扩容的时候需要处理下

#### 二叉树 ####
虽然有点二还是把名词写下：二叉查找树、完全二叉树

二叉查找树删节点，保证树结构正确，把左子树中最大的元素提上来

实现iterable可以再某些场合自动调用.iterator()，比如foreach

#### AVL树 ####
                                                                                                                        
AVL树关键在于旋转，节点记录俩子树树高之差（±1,0），在此数值到2的时候分情况对树旋转

#### 散列 ####

java默认的hashcode竟然返回的是内存地址。（自定义类）改hashCode和equal记得一起改
   
hashset和map中，先hashCode，再用equals判断相等，若两者皆相等则视为同一key

util的api中hash的装填因子是0.75，一般认为，链地址<0.9,开放地址<0.5

put返回值是原有的value

put一般流程：获取（hashcode（key的hashCode方法和自身的hash）+equals）更新，无原有则检查空间扩展新增，扩展系数2.0,扩增方式是将元素添加到新的table中，hash操作中有将原


## 并发 ##
并发并行不解释

### 线程 ###
线程在lang里

extend Thread impl runable

Thread.currentThread()

thread中的run竟然不是抽象函数

### 线程池 ###
util里有线程池

	ExecutorService eService = Executors.newFixedThreadPool(10);
    eService.execute(thread);
    Future<?> future = eService.submit(thread);

callable和runable，callable的调用带返回值，并且提交给线程池后可以通过future获取返回值或者控制线程运行

这个范围内，runable和callable都能提交给executor运行，但是callable不能通过thread封装运行，runable提交给executor后有future，但future get到的是null，可以理解，这个future感觉只起管理功能

线程池空间不够时后面的线程会一直等着

### synchronized ###

修饰对象或者代码块，加锁目标为对象，当然也能对类加锁（synchronized(SyncThread.class)），范围等同于目标作用域，函数上的synchronized关键字修饰的是对象并且不会被继承。

### ReentrantLock ###

	ReentrantLock reentrantLock = new ReentrantLock();

然后就是正常的lock和unlock，可以在外部检测isLocked或者内部检查tryLock

	Condition condition = reentrantLock.newCondition();

condition可以通过await和signal控制线程运行

### 阻塞队列 ###

	interface BlockingQueue

有三个具体子类：ArrayBlockingQueue,LinkedBlockingQueue,PriorityBlockingQueue，看名字想差别，存取使用put和take

### 信号量 ###

	Semaphore semaphore = new Semaphore(10);
    semaphore.acquire(1);
    semaphore.release(1);

信号量并没有什么其他内容

### 线程状态及操作 ###
线程状态：新建、就绪、运行、阻塞、结束

sleep、join、wait进阻塞

wait()：object函数，等待资源，但需要持有线程，就是说wait需要在sychronized块（函数也行）里写，并且wait会释放锁资源

sleep():thread函数，static，当前线程等待

notify():object函数，唤醒wait，需要在synchronized里写。

yield():thread函数，让出cpu资源，理解为排到队伍末尾？

interrupt():thread函数，不是用来停止的，调用后如果线程在wait、join或者sleep则唤醒并处理interrupt的exception，如果在就绪和运行，则设置中断标志为true，新建的话无影响

join():thread函数，a线程调用b.join()，a阻塞等待b执行完成，b.join函数中主要就是,这里b是示意，线程结束后会调用notifyall使其跳出循环。

	while(b.isalive()){
		b.wait(0);
	}

### 同步集合 ###

Collections中提供synchronizedlist()\synchronizedmap()之类的方法来转换出线程安全的集合，但是实际使用中发生读写冲突的时候还是会丢exception，所以需要手动加锁。

### fork join框架 ###

看起来感觉用的有点麻烦，优势在于数量难以控制的并发处理，比如归并排序。这里forkjoin的都是线程，通过fork和join函数来控制子线程进度。另：task带返回值action不带

## 网络 ##

服务器端：

	ServerSocket serverSocket;
	Socket socket;
	OutputStream oStream = null;
	InputStream iStream;
	try {
		serverSocket = new ServerSocket(10086);
		socket = serverSocket.accept();
		oStream =  socket.getOutputStream();
		iStream = socket.getInputStream();
		DataOutputStream dataOutputStream = new DataOutputStream(oStream);
		dataOutputStream.writeInt(12306);
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}

客户端：cli的端口由客户端自动选取

	try {
		Socket cliSocket = new Socket("127.0.0.1", 10086);
	} catch (UnknownHostException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}

socket.getInetAddress();获取对方的连接信息

其他内容感觉在框架中看好点

## 国际化 ##
## mvc ##
## web ##
## 动态性 ##
### 脚本语言API ###
这部分内容在javax中，可以通过这类api在java中使用其他语言，其余内容就是对象的绑定、方法调用和脚本是否编译（javax.script.Compilable接口）
### 反射 ###
反射与之前理解的看来比较类似，通过名称去直接获取类中的一些内容，可以是函数、变量之类的

obj.getClass获取class之后可以

- 调用方法，与field类似，私有的调用前需要设置accessible为true
- 获取构造函数，创建实例
- 获取域getfield是当前类、其父类的所有public，getdeclaredfield可以获取当前类的public、protected、package、private，有对应的getset
- 数组通过java.lang.reflect.Array操作，这里的意思应该是直接对数组获取其构造函数、变量啥的，不过书上只讲了构造函数，试了试好像并没有什么用- -

java7新特性：反射相关的异常都继承自ReflectiveOperationException，可以直接抓这个
### 动态代理 ###
暂时看不懂等设计模式看完后看
## io ##
## 异常 ##
![](http://i.imgur.com/1Uil7EB.jpg)

非受检异常（runtimeException和Error）

受检异常（其他）：需要显式捕获和处理

自己设计异常，可以捕获记录一些错误信息（或许可以用来记log？），非受检异常继承runtimeException，受检异常继承Exception，个人也觉得还是runtime优先，简化代码提高可读性

异常丢失：在catch中丢出异常，finally执行中又出现异常导致catch中的异常丢失，可以通过finally中再套一个try，或者用java7的addsuppressed记录（需要的时候可以看看）

java7特性：catch中可以用|连接，连接方式类似case，父子类关系注意

    try {
			
	} catch (ArrayStoreException | ArithmeticException e) {
			// TODO: handle exception
	}

抛出异常的精确性，这本书上写的有点绕.....大概意思是java7编译器会做更精确的异常类型判断，比如丢一个exceptionA编译器会自动判断为其子类exceptionAsub1，如果之前这个实例确实是这个子类的实例的话

## GUI ##
## java7更新 ##
switch可以用字符串了，但是编译器会转化为if或者hashcode什么的进行计算。

0b二进制，数字内容间（不包括边界、进制）可以加下划线类似1,000,000,000中的逗号,位置不限制

@SafeVarargs注解，看到这觉得得把注解看一下
## java8更新 ##
## 虚拟机基础 ##
## 安全 ##
## java源码 ##
## 不知道怎么分类的内容 ##
object.hashCode()：

> - 地址无关
> - 内容相关，实例无关
> - 看起来应该是直接将类的内容做一个hash，修改内容实例不变hashcode还是会变化，所以可以用于判断相似
 
enum：地位等同class（貌似就是class）

> 看起来用起来和c++并没有什么不一样，但是这是通过继承java.lang.Enum实现的，所以不能extend，不过可以implement其他接口，而且有构造函数（需要是private）可以写其他函数
> 
> 这么说可以enum线程？闲着无聊可以试试
> 
> 附代码，不过感觉构造函数不能调的话函数返回都是固定值好像没啥用- -

    public enum mytype{
		AAA(1,"123"),BBB(2,"456");
		private int val1;
		private String str1;
		private mytype(int i,String s) {
			val1 = i;
			str1 = s;
		}
		public String getSth() {
			return str1+val1;
		}
	};

0x：十六进制，0：八进制，0b：二进制

> - 输出是十进制
> - 越界报错
> - 不区分大小写
> - 0b是java7+，

try-with-resource:

> 类似于异常处理中的内容，try中的资源会被自动释放，但需要实现java.lang.AutoCloseable接口
> 
> 这么说能用在共用资源上？