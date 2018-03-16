# Android面试题

Android面试题除了Android基础之外，更多的问的是一些源码级别的、原理这些等。所以想去大公司面试，一定要多看看源码和实现方式，常用框架可以试试自己能不能手写实现一下，锻炼一下自己。

### 一、Android基础知识点


现在面试Android会问：热修复，插件化，性能优化，事件机制，自定义控件，设计模式，算法等


* 四大组件是什么
* 四大组件的生命周期和简单用法
* Activity之间的通信方式
* Activity各种情况下的生命周期
* 横竖屏切换的时候，Activity 各种情况下的生命周期
* Activity与Fragment之间生命周期比较
* Activity上有Dialog的时候按Home键时的生命周期
* 两个Activity 之间跳转时必然会执行的是哪几个方法？
* 前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命值周期回调方法。
* Activity的四种启动模式对比
* Activity状态保存于恢复
* fragment各种情况下的生命周期
* Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用？
* 如何实现Fragment的滑动？
* fragment之间传递数据的方式？
* Activity 怎么和Service 绑定？
* 怎么在Activity 中启动自己对应的Service？
* service和activity怎么进行数据交互？
* Service的开启方式
* 请描述一下Service 的生命周期
* 谈谈你对ContentProvider的理解
* 说说ContentProvider、ContentResolver、ContentObserver 之间的关系
* 请描述一下广播BroadcastReceiver的理解
* 广播的分类
* 广播使用的方式和场景
* 在manifest 和代码中如何注册和使用BroadcastReceiver?
* 本地广播和全局广播有什么差别？
* BroadcastReceiver，LocalBroadcastReceiver 区别
* AlertDialog,popupWindow,Activity区别
* Application 和 Activity 的 Context 对象的区别
* Android属性动画特性
* 如何导入外部数据库?
* LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。
* 谈谈对接口与回调的理解
* 回调的原理
* 写一个回调demo
* 介绍下SurfView

		`


		SurfaceView继承之View，但拥有独立的绘制表面，即它不与其宿主窗口共享同一个绘图表面，可以单独在一个线程进行绘制，并不会占用主线程的资源。这样，绘制就会比较高效，游戏，视频播放，还有最近热门的直播，都可以用SurfaceView

		SurfaceView和View的区别：

		View主要适用于主动更新的情况下，而SurfaceView主要适用于被动更新，例如频繁地刷新
		View在主线程中对画面进行刷新，而SurfaceView通常会通过一个子线程来进行页面的刷新
		View在绘图时没有使用双缓冲机制，而SufaceView在底层实现机制中就已经实现了双缓冲机制


		SurfaceView使用过程有一套模板代码，大部分的SurfaceView都可以套用

		3步走套路：
		
		创建SurfaceView
		初始化SurfaceView
		使用SurfaceView


		
		`	

* RecycleView的使用
* 序列化的作用，以及Android两种序列化的区别
* 差值器
* 估值器
* Android中数据存储方式

### 二、Android源码相关分析

* Android动画框架实现原理

	
	`http://blog.csdn.net/harrain/article/details/53726960`

--

	`Android 平台提供了两类动画，一类是 Tween 动画，即通过对场景里的对象不断做图像变换 ( 平移、缩放、旋转 ) 产生动画效果；第二类是 Frame 动画，即顺序播放事先做好的图像，跟电影类似。


	https://www.jianshu.com/p/3683a69c38ea

	Animation的分类
	
	-Tweend动画即通过对场景里的对象不断做图像变换 ( 平移、缩放、旋转 ) 产生动画效果
	
	-Frame 动画即顺序播放事先做好的图像，跟电影类似


	因为整个 View 的布局就是一棵树，所以绘制的时候也是按照树形结构遍历来让每个 View 进行绘制。ViewRoot.java 中的 draw 函数准备好 Canvas 后会调用 mView.draw(canvas)，其中 mView 就是调用 ViewRoot.setView 时设置的 DecorView。然后看一下 View.java 中的 draw 函数：
	递归的绘制整个窗口需要按顺序执行以下几个步骤：
	绘制背景；
	如果需要，保存画布（canvas）的层为淡入或淡出做准备；
	绘制 View 本身的内容，通过调用 View.onDraw(canvas) 函数实现，通过这个我们应该能看出来 onDraw 函数重载的重要性，onDraw 函数中绘制线条 / 圆 / 文字等功能会调用 Canvas 中对应的功能。下面我们会 drawLine 函数为例进行说明；
	绘制自己的孩子（通常也是一个 view 系统），通过 dispatchDraw(canvas) 实现，参看 ViewGroup.Java 中的代码可知，dispatchDraw->drawChild->child.draw(canvas) 这样的调用过程被用来保证每个子 View 的 draw 函数都被调用，通过这种递归调用从而让整个 View 树中的所有 View 的内容都得到绘制。在调用每个子 View 的 draw 函数之前，需要绘制的 View 的绘制位置是在 Canvas 通过 translate 函数调用来进行切换的，窗口中的所有 View 是共用一个 Canvas 对象
	如果需要，绘制淡入淡出相关的内容并恢复保存的画布所在的层（layer）
	绘制修饰的内容（例如滚动条），这个可知要实现滚动条效果并不需要 ScrollView，可以在 View 中完成的，不过有一些小技巧，具体实现可以参看我们的 TextViewExample 示例代码

	
	`

* Android各个版本API的区别
* Requestlayout，onlayout，onDraw，DrawChild区别与联系
* invalidate和postInvalidate的区别及使用

		`

		http://blog.csdn.net/ziwang_/article/details/65690751

		区别与联系

	postInvalidate() 方法在非 UI 线程中调用，通知 UI 线程重绘。 
	invalidate() 方法在 UI 线程中调用，重绘当前 UI。


		其实 posInvalidate() 源码内部其实也就是调用了 invalidata()
	
	`	
	
* Activity-Window-View三者的差别


		`Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图） LayoutInflater像剪刀，Xml配置像窗花图纸。

		在Activity中调用attach，创建了一个Window
		创建的window是其子类PhoneWindow，在attach中创建PhoneWindow
		在Activity中调用setContentView(R.layout.xxx)
		其中实际上是调用的getWindow().setContentView()
		调用PhoneWindow中的setContentView方法
		创建ParentView： 作为ViewGroup的子类，实际是创建的DecorView(作为FramLayout的子类）
		将指定的R.layout.xxx进行填充 通过布局填充器进行填充【其中的parent指的就是DecorView】
		调用到ViewGroup
		调用ViewGroup的removeAllView()，先将所有的view移除掉
		添加新的view：addView()
		fragment 特点
		
		Fragment可以作为Activity界面的一部分组成出现；
		可以在一个Activity中同时出现多个Fragment，并且一个Fragment也可以在多个Activity中使用；
		在Activity运行过程中，可以添加、移除或者替换Fragment；
		Fragment可以响应自己的输入事件，并且有自己的生命周期，它们的生命周期会受宿主Activity的生命周期影响。

		`

* 谈谈对Volley的理解
	

	`

		http://blog.csdn.net/guolin_blog/article/details/17482095

		1.创建 requestQueue:

		 Volley框架的源码，发现它在HTTP请求的使用上比较有意思，在Android 2.3及以上版本，使用的是HttpURLConnection，而在Android 2.2及以下版本，使用的是HttpClient

		 android  网络请求 是使用HttpClient 还是HttpUrlConnection 

	
	`
* android 网络请求使用 HttpClient 还是HttpUrlConnection

	`
		http://blog.csdn.net/guolin_blog/article/details/12452307

		大多数的Android应用程序都会使用HTTP协议来发送和接收网络数据，而Android中主要提供了两种方式来进行HTTP操作，HttpURLConnection和HttpClient。这两种方式都支持HTTPS协议、以流的形式进行上传和下载、配置超时时间、IPv6、以及连接池等功能	

		在Android 2.2版本之前，HttpClient拥有较少的bug，因此使用它是最好的选择。



		而在Android 2.3版本及以后，HttpURLConnection则是最佳的选择。它的API简单，体积较小，因而非常适用于Android项目。压缩和缓存机制可以有效地减少网络访问的流量，在提升速度和省电方面也起到了较大的作用。对于新的应用程序应该更加偏向于使用HttpURLConnection，因为在以后的工作当中我们也会将更多的时间放在优化HttpURLConnection上面。
		
	`
* 如何优化自定义View
* 低版本SDK如何实现高版本api？
* 描述一次网络请求的流程
* HttpUrlConnection 和 okhttp关系
* Bitmap对象的理解


* looper架构
* ActivityThread，AMS，WMS的工作原理
* 自定义View如何考虑机型适配
* 自定义View的事件
* AstncTask+HttpClient 与 AsyncHttpClient有什么区别？
* LaunchMode应用场景
* AsyncTask 如何使用?
* SpareArray原理

	` SparseArray(稀疏数组).他是Android内部特有的api,标准的jdk是没有这个类的.在Android内部用来替代HashMap<Integer,E>这种形式,使用SparseArray更加节省内存空间的使用,SparseArray也是以key和value对数据进行保存的.使用的时候只需要指定value的类型即可.并且key不需要封装成对象类型.


		1.数据结构：

		HashMap: 链式结构+数组结构
		SparseArray: 数组结构  稀疏数组  是单纯的数组  对数据的保存没有额外的开支

		2. 正序插入：

			 <---Map的插入时间--->914
 			 <---Map占用的内存--->28598272
			
			<---Sparse的插入时间--->611
			<---Sparse占用的内存--->23281664

		3. 倒序插入：
			
			  //执行后的结果：
			  <------------- 数据量100000 Map 倒序插入--------------->
			  <---Map的插入时间--->836<---Map占用的内存--->28598272

			  //执行后的结果
				<------------- 数据量100000 SparseArray 倒序插入--------------->
				<---Sparse的插入时间--->20222<---Sparse占用的内存--->23281664


			 通过上面的运行结果,我们仍然可以看到,SparseArray与HashMap无论是怎样进行插入,数据量相同时,前者都要比后者要省下一部分内存,但是效率呢？我们可以看到,在倒序插入的时候,SparseArray的插入时间和HashMap的插入时间远远不是一个数量级.由于SparseArray每次在插入的时候都要使用二分查找判断是否有相同的值被插入.因此这种倒序的情况是SparseArray效率最差的时候.


			` public void put(int key, E value) {
        int i = ContainerHelpers.binarySearch(mKeys, mSize, key); //二分查找.

        if (i >= 0) {  //如果当前这个i在数组中存在,那么表示插入了相同的key值,只需要将value的值进行覆盖..
            mValues[i] = value;
        } else {  //如果数组内部不存在的话,那么返回的数值必然是负数.
            i = ~i;  //因此需要取i的相反数.
            //i值小于mSize表示在这之前. mKey和mValue数组已经被申请了空间.只是键值被删除了.那么当再次保存新的值的时候.不需要额外的开辟新的内存空间.直接对数组进行赋值即可.
            if (i < mSize && mValues[i] == DELETED) {
                mKeys[i] = key;
                mValues[i] = value;
                return;
            }
            //当需要的空间要超出,但是mKey中存在无用的数值,那么需要调用gc()函数.
            if (mGarbage && mSize >= mKeys.length) {
                gc();
                
                // Search again because indices may have changed.
                i = ~ContainerHelpers.binarySearch(mKeys, mSize, key);
            }
            //如果需要的空间大于了原来申请的控件,那么需要为key和value数组开辟新的空间.
            if (mSize >= mKeys.length) {
                int n = ArrayUtils.idealIntArraySize(mSize + 1);
                //定义了一个新的key和value数组.需要大于mSize
                int[] nkeys = new int[n];
                Object[] nvalues = new Object[n];

                // Log.e("SparseArray", "grow " + mKeys.length + " to " + n);
                //对数组进行赋值也就是copy操作.将原来的mKey数组和mValue数组的值赋给新开辟的空间的数组.目的是为了添加新的键值对.
                System.arraycopy(mKeys, 0, nkeys, 0, mKeys.length);
                System.arraycopy(mValues, 0, nvalues, 0, mValues.length);
                //将数组赋值..这里只是将数组的大小进行扩大..放入键值对的操作不在这里完成.
                mKeys = nkeys;
                mValues = nvalues;
            }
            //如果i的值没有超过mSize的值.只需要扩大mKey的长度即可.
            if (mSize - i != 0) {
                // Log.e("SparseArray", "move " + (mSize - i));
                System.arraycopy(mKeys, i, mKeys, i + 1, mSize - i);
                System.arraycopy(mValues, i, mValues, i + 1, mSize - i);
            }
            //这里是用来完成放入操作的过程.
            mKeys[i] = key;
            mValues[i] = value;
            mSize++;
        }
   		 }



		 这就是SparseArray插入函数的源码.每次的插入方式都需要调用二分查找.因此这样在倒序插入的时候会导致情况非常的糟糕,效率上绝对输给了HashMap学过数据结构的大家都知道.Map在插入的时候会对冲突因子做出相应的决策.有非常好的处理冲突的方式.不需要遍历每一个值.因此无论是倒序还是正序插入的效率取决于处理冲突的方式,因此插入时牺牲的时间基本是相同的.



		SparseArray是android v4包里提供的工具类，在某些场景下可以用来替代Hashmap进行对象的存储，其内部实现了一个矩阵压缩算法，可以进行矩阵压缩，大大减少了存储空间，节约内存。此外它的查找算法是二分法，提高了查找的效率。 
		SparseArray的value可以是任意类型，但key只能是Integer、Long类型，所以key是Integer、Long场景下，SparseArray比Hashmap更合适

		 `

			 
			 
			 


		

			


	`	

	
	

* 请介绍下ContentProvider 是如何实现数据共享的？
* AndroidService与Activity之间通信的几种方式
* IntentService原理及作用是什么？
* 说说Activity、Intent、Service 是什么关系
* ApplicationContext和ActivityContext的区别
* SP是进程同步的吗?有什么方法做到同步？

	`
		
		http://blog.csdn.net/u010208788/article/details/50541902
		
		1. SharedPreferences不支持进程同步

		一个进程的情况，经常采用SharePreference来做，但是SharePreference不支持多进程，它基于单个文件的，默认是没有考虑同步互斥，而且，APP对SP对象做了缓存，不好互斥同步.
		MODE_MULTI_PROCESS的作用是什么?
		在getSharedPreferences的时候, 会强制让SP进行一次读取操作，从而保证数据是最新的. 但是若频繁多进程进行读写 . 若某个进程持有了一个外部sp对象, 那么不能保证数据是最新的. 因为刚刚被别的进程更新了.
	
		2.考虑用ContentProvider来实现SharedPreferences的进程同步.
	
		ContentProvider基于Binder，不存在进程间互斥问题，对于同步，也做了很好的封装，不需要开发者额外实现。 
		
		另外ContentProvider的每次操作都会重新getSP. 保证了sp的一致性.`	



* 谈谈线程

		`

		`在 Java 中实现多线程有两种手段，一种是继承 Thread 类，另一种就是实现 Runnable 接口。下面我们就分别来介绍这两种方式的使用。`

		多线程的状态变化：

		要想实现多线程，必须在主线程中创建新的线程对象。任何线程一般具有5种状态，即创建，就绪，运行，阻塞，终止。下面分别介绍一下这几种状态：


		在此提出一个问题，Java 程序每次运行至少启动几个线程？

		回答：至少启动两个线程，每当使用 Java 命令执行一个类时，实际上都会启动一个 JVM，每一个JVM实际上就是在操作系统中启动一个线程，Java 本身具备了垃圾的收集机制。所以在 Java 运行时至少会启动两个线程，一个是 main 线程，另外一个是垃圾收集线程

		`

* 谈谈多线程在Android中的使用


	`
	Android提供了四种常用的操作多线程的方式，分别是：
	1. Handler+Thread
	2. AsyncTask
	3. ThreadPoolExecutor
	4. IntentService
	
	下面分布对四种方式进行介绍。

	1.Handler+Thread
		
		有post 方式和 sendMessage的方式


	1. Handler用法简单明了，可以将多个异步任务更新UI的代码放在一起，清晰明了
	2. 处理单个异步任务代码略显多

	3. 适用范围是多个任务更新UI


	2.AsyncTask

		AsyncTask是android提供的轻量级的异步类,可以直接继承AsyncTask，在类中实现异步操作，并提供接口反馈当前异步执行的程度(可以通过接口实现UI进度更新)，最后反馈执行的结果给UI主线程。

		. 处理单个异步任务简单，可以获取到异步任务的进度
		2. 可以通过cancel方法取消还没执行完的AsyncTask
		3. 处理多个异步任务代码显得较多


	3.ThreadPoolExecutor 线程池

		ThreadPoolExecutor提供了一组线程池，可以管理多个线程并行执行。这样一方面减少了每个并行任务独自建立线程的开销，另一方面可以管理多个并发线程的公共资源，从而提高了多线程的效率。所以ThreadPoolExecutor比较适合一组任务的执行。Executors利用工厂模式对ThreadPoolExecutor进行了封装，使用起来更加方便。


		1. Executors.newFixedThreadPool()
		   创建一个定长的线程池，每提交一个任务就创建一个线程，直到达到池的最大长度，这时线程池会保持长度不再变化
		2. Executors.newCachedThreadPool()
		   创建一个可缓存的线程池，如果当前线程池的长度超过了处理的需要时，它可以灵活的回收空闲的线程，当需要增加时，
		    它可以灵活的添加新的线程，而不会对池的长度作任何限制
		3. Executors.newScheduledThreadPool()
		   创建一个定长的线程池，而且支持定时的以及周期性的任务执行，类似于Timer
		4. Executors.newSingleThreadExecutor()
		   创建一个单线程化的executor，它只创建唯一的worker线程来执行任务


	4.IntentService

		`IntentService继承自Service，是一个经过包装的轻量级的Service，用来接收并处理通过Intent传递的异步请求。客户端通过调用startService(Intent)启动一个IntentService，利用一个work线程依次处理顺序过来的请求，处理完成后自动结束Service。
		

		`

		






	`



* 进程和 Application 的生命周期


		`
		

		android系统会为每个程序运行时创建一个Application类的对象且仅创建一个，所以Application可以说是单例 (singleton)模式的一个类.且application对象的生命周期是整个程序中最长的，它的生命周期就等于这个程序的生命周期。因为它是全局 的单例的，所以在不同的Activity,Service中获得的对象都是同一个对象。所以通过Application来进行一些，数据传递，数据共享 等,数据缓存等操作。

		SDK中的描述：Application类是为了那些需要保存全局变量设计的基本类，你可以在AndroidManifest.xml的<application>标签中进行自己的实现，这样的结果是：当你的       application或者包被建立的时候将引起那个类被建立。

		理解：就是说application是用来保存全局变量的，并且是在package创建的时候就跟着存在了。所以当我们需要创建全局变量的时候，不需 要再像j2se那样需要创建public权限的static变量，而直接在application中去实现。只需要调用Context的getApplicationContext或者Activity的getApplication方法来获得一个application对象，再做出相应 的处理。


		      onCreate();

			   这个函数是当我们的应用开始之时就被调用了，比应用中的其他对象创建的早，这个实现尽可能的快一点，因为这个时间直接影响到我们第一个activity/service


			  onTerminate():


		`

		
		`Android系统会尽可能长的延续一个应用程序进程，但在内存过低的时候，仍然会不可避免需要移除旧的进程。为决定保留或移除一个进程，Android将每个进程都放入一个“重要性层次”中，依据则是它其中运行着的组件及其状态。重要性最低的进程首先被消灭，然后是较低的，依此类推。重要性共分五层，依据重要性列表如下：

		（1）前台进程
		
		是用户操作所必须的，任一时间下，仅有少数进程会处于前台，这样的进程拥有一个在屏幕上显示并和用户交互的 activity 或者它的一个IntentReciver 正在运行。仅当内存实在无法供给它们维持同时运行时才会被杀死。一般来说，在这种情况下，设备依然处于使用虚拟内存的状态，必须要杀死一些前台进程以用户界面保持响应。
		
		（2）可视进程
		
		没有前台组件，但仍可被用户在屏幕上所见。当满足如下任一条件时，进程被认为是可视的：
		
		●它包含着一个不在前台，但仍然为用户可见的activity（它的onPause()方法被调用）。这种情况可能出现在以下情况：比如说，前台activity是一个对话框，而之前的Activity位于其下并可以看到。
		
		●它包含了一个绑定至一个可视的activity的服务。
		
		 可视进程依然被视为是很重要的，非到不杀死它们便无法维持前台进程运行时，才会被杀死。
		
		（3）服务进程
		
		是由startService() 方法启动的服务，它不会变成上述两类。尽管服务进程不会直接为用户所见，但它们一般都在做着用户所关心的事情（比如在后台播放mp3或者从网上下载东西）。所以系统会尽量维持它们的运行，除非系统内存不足以维持前台进程和可视进程的运行需要。
		
		（4）背景进程
		
		包含目前不为用户所见的activity（Activity对象的onStop() 方法已被调用）。这些进程与用户体验没有直接的联系，可以在任意时间被杀死以回收内存供前台进程、可视进程以及服务进程使用。一般来说，会有很多背景进程运行，所以它们一般存放于一个LRU（最后使用）列表中以确保最后被用户使用的activity最后被杀死。如果一个activity正确的实现了生命周期方法，并捕获了正确的状态，则杀死它的进程对用户体验不会有任何不良影响。
		
		（5）空进程
		
		不包含任何活动应用程序组件。这种进程存在的唯一原因是做为缓存以改善组件再次于其中运行时的启动时间。系统经常会杀死这种进程以保持进程缓存和系统内核缓存之间的平衡

		二、在Service中新开线程和直接新开线程的区别

		（1）若我们直接在Activity中新开一条线程来做耗时操作，当该Activity退出到桌面或其他情况时将成为一个背景进程。
		
		（2）若我们在Service中新启动线程，则此时Android会依据进程中当前活跃组件重要程度，将其判断为服务进程，优先级比（1）高。

		`

* 封装View的时候怎么知道view的大小

		`


		http://blog.csdn.net/fwt336/article/details/52979876

		在自定义view的时候，其实很简单，只需要知道3步骤：
		1.测量——onMeasure()：决定View的大小
		2.布局——onLayout()：决定View在ViewGroup中的位置
		3.绘制——onDraw()：如何绘制这个View。
		而第3步的onDraw系统已经封装的很好了，基本不用我们来操心，只需要专注到1,2两个步骤就中好了。

		Measure()：
		Measure的中文意思就是测量。所以它的作用就是测量View的大小。
		而决定View的大小只需要两个值：宽详细测量值（widthMeasureSpec）和高详细测量值（heightMeasureSpec）。也可以把详细测量值理解为视图View想要的大小说明（想要的未必就是最终大小）。
		对于详细测量值（measureSpec）需要两样东西来确定它，那就是大小（size）和模式（mode）。而measureSpec，size，mode他们三个的关系，都封装在View类中的一个内部类里，名叫MeasureSpec。
		
		MeasureSpec：
		因为MeasureSpec类很小，而且设计的很巧妙，所以我贴出了全部的源码并进行了详细的标注。（掌握MeasureSpec的机制后会对整个Measure方法有更深刻的理解。）


	
		`


		`

		/** 
		 * MeasureSpec封装了父布局传递给子布局的布局要求，每个MeasureSpec代表了一组宽度和高度的要求 
		 * MeasureSpec由size和mode组成。 
		 * 三种Mode： 
		 * 1.UNSPECIFIED 
		 * 父不没有对子施加任何约束，子可以是任意大小（也就是未指定） 
		 * (UNSPECIFIED在源码中的处理和EXACTLY一样。当View的宽高值设置为0的时候或者没有设置宽高时，模式为UNSPECIFIED 
		 * 2.EXACTLY 
		 * 父决定子的确切大小，子被限定在给定的边界里，忽略本身想要的大小。 
		 * (当设置width或height为match_parent时，模式为EXACTLY，因为子view会占据剩余容器的空间，所以它大小是确定的) 
		 * 3.AT_MOST 
		 * 子最大可以达到的指定大小 
		 * (当设置为wrap_content时，模式为AT_MOST, 表示子view的大小最多是多少，这样子view会根据这个上限来设置自己的尺寸) 
		 *  
		 * MeasureSpecs使用了二进制去减少对象的分配。 
		 */  
		public class MeasureSpec {  
        // 进位大小为2的30次方(int的大小为32位，所以进位30位就是要使用int的最高位和倒数第二位也就是32和31位做标志位)  
        private static final int MODE_SHIFT = 30;  
          
        // 运算遮罩，0x3为16进制，10进制为3，二进制为11。3向左进位30，就是11 00000000000(11后跟30个0)  
        // (遮罩的作用是用1标注需要的值，0标注不要的值。因为1与任何数做与运算都得任何数，0与任何数做与运算都得0）  
        private static final int MODE_MASK  = 0x3 << MODE_SHIFT;  
  
        // 0向左进位30，就是00 00000000000(00后跟30个0)  
        public static final int UNSPECIFIED = 0 << MODE_SHIFT;  
        // 1向左进位30，就是01 00000000000(01后跟30个0)  
        public static final int EXACTLY     = 1 << MODE_SHIFT;  
        // 2向左进位30，就是10 00000000000(10后跟30个0)  
        public static final int AT_MOST     = 2 << MODE_SHIFT;  
  
        /** 
         * 根据提供的size和mode得到一个详细的测量结果 
         */  
        // measureSpec = size + mode；   (注意：二进制的加法，不是十进制的加法！)  
        // 这里设计的目的就是使用一个32位的二进制数，32和31位代表了mode的值，后30位代表size的值  
        // 例如size=100(4)，mode=AT_MOST，则measureSpec=100+10000...00=10000..00100  
        public static int makeMeasureSpec(int size, int mode) {  
            return size + mode;  
        }  
  
        /** 
         * 通过详细测量结果获得mode 
         */  
        // mode = measureSpec & MODE_MASK;  
        // MODE_MASK = 11 00000000000(11后跟30个0)，原理是用MODE_MASK后30位的0替换掉measureSpec后30位中的1,再保留32和31位的mode值。  
        // 例如10 00..00100 & 11 00..00(11后跟30个0) = 10 00..00(AT_MOST)，这样就得到了mode的值  
        public static int getMode(int measureSpec) {  
            return (measureSpec & MODE_MASK);  
        }  
  
        /** 
         * 通过详细测量结果获得size 
         */  
        // size = measureSpec & ~MODE_MASK;  
        // 原理同上，不过这次是将MODE_MASK取反，也就是变成了00 111111(00后跟30个1)，将32,31替换成0也就是去掉mode，保留后30位的size  
        public static int getSize(int measureSpec) {  
            return (measureSpec & ~MODE_MASK);  
        }  
  
        /** 
         * 重写的toString方法，打印mode和size的信息，这里省略 
         */  
        public static String toString(int measureSpec) {  
            return null;  
        }  
		}  

		`


		
* RecycleView原理
* AndroidManifest的作用与理解

	`

	该AndroidManifest文件的作用有两点：一是向系统声明该app包含了哪些类型的组件，二是申请权限。	
	
	`

### 三、常见的一些原理性问题

* Handler机制和底层实现
* Handler、Thread和HandlerThread的差别
* handler发消息给子线程，looper怎么启动？
* 关于Handler，在任何地方new Handler 都是什么线程下?
* ThreadLocal原理，实现及如何保证Local属性？
* 请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系
* 请描述一下View事件传递分发机制
* Touch事件传递流程
* 事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
* View和ViewGroup分别有哪些事件分发相关的回调方法
* View刷新机制
* View绘制流程
* 自定义控件原理
* 自定义View如何提供获取View属性的接口？
* Android代码中实现WAP方式联网
* AsyncTask机制
* AsyncTask原理及不足
* 如何取消AsyncTask？
* 为什么不能在子线程更新UI？
* ANR产生的原因是什么？
* ANR定位和修正
* oom是什么？
* 什么情况导致oom？
* 有什么解决方法可以避免OOM？
* Oom 是否可以try catch？为什么？
* 内存泄漏是什么？
* 什么情况导致内存泄漏？
* 如何防止线程的内存泄漏？
* 内存泄露场的解决方法
* 内存泄漏和内存溢出区别？
* LruCache默认缓存大小
* ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)
* 如何通过广播拦截和abort一条短信？
* 广播是否可以请求网络？
* 广播引起anr的时间限制是多少？
* 计算一个view的嵌套层级
* Activity栈
* Android线程有没有上限？
* 线程池有没有上限？
* ListView重用的是什么？
* Android为什么引入Parcelable？
* 有没有尝试简化Parcelable的使用？

### 四、开发中常见的一些问题

* ListView 中图片错位的问题是如何产生的?
* 混合开发有了解吗？
* 知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，H5，小程序，WPA等。做Android的了解一些前端js等还是很有好处的)；
* 屏幕适配的处理技巧都有哪些?
* 服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？
* 动态布局的理解
* 怎么去除重复代码？
* 画出 Android 的大体架构图
* Recycleview和ListView的区别
* ListView图片加载错乱的原理和解决方案
* 动态权限适配方案，权限组的概念
* Android系统为什么会设计ContentProvider？
* 下拉状态栏是不是影响activity的生命周期
* 如果在onStop的时候做了网络请求，onResume的时候怎么恢复？
* Bitmap 使用时候注意什么？
* Bitmap的recycler()
* Android中开启摄像头的主要步骤
* ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？
* 点击事件被拦截，但是想传到下面的View，如何操作？
* 微信主页面的实现方式
* 微信上消息小红点的原理
* CAS介绍（这是阿里巴巴的面试题，我不是很了解，可以参考博客: [CAS简介](http://blog.csdn.net/jly4758/article/details/46673835)）
