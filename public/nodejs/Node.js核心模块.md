#### Node.js核心模块

#### 1.console
>global.console 用于向stdout和stderr输出日志信息

>Console class  console模块中的Console构造函数，用于向任意指定的输出流（文件）中输入信息。

#### 2.Query String模块

>提供了处理URL中“查询字符串”部分的相关操作

>parse()	//从查询字符串中解析出数据对象，参数为一个查询字符串

>stringify()	//将一个数据对象反向解析为一个查询字符串格式，
参数1，为一个数据对象；
可选参数2，指定键值对之间的分隔符；
可选参数3，指定键和值之间的分隔符。

#### 3.URL模块

>提供了处理url中不同部分的相关操作

>parse()	//解析出url的各个组成部分，参数1，要解析的url字符串；
可选参数2，若为true，可以将查询字符串部分解析为对象

>format()	//将对象反向格式化为url格式，参数为一个对象

>resolve() 	//根据基地址和相对地址解析出目标地址，参数1，基地址；参数2，相对地址

#### 4.Path模块

>提供了对文件路径进行操作的相关方法

>>parse()	//解析一个路径，参数为路径字符串

>>format()	//将路径对象格式化为路径字符串，参数为对象

>>join()	//用于连接路径，会正确使用当前系统的路径分隔符

>>resolve()	//根据基础路径，解析出目标路径的绝对路径

>>relative()	//根据基础路径，获取目标路径与其的相对关系

#### 5.DNS模块

>提供了域名和IP地址的双向解析功能。

>>lookup()	//把一个域名解析成一个IP地址，从操作系统中查询（缓存）

>>resolve() 	//把一个域名解析为一个DNS的记录解析数组，从DNS服务器中查询

>>reverse()	//把一个IP地址反向解析为一组域名
	
#### 6.Util 工具模块
>>format()	//使用带占位符的方式格式化字符串

>>inspect()	//返回一个对象的字符串表示

>>inherits()	//实现构造方法之间的继承

#### 7.Buffer
>缓冲区，一块专用于存储数据的内存区域，与string类型相对应，用于存储二进制数据。

>Buffer对象的实例，可以通过读取文件获得，也可以手动创
建。

>Buffer是Global对象的成员，无需require引入。

#### 8.fs模块——文件系统模块

>fs模块提供了文件的读写、更名、删除、遍历目录等操作。

>fs模块中大多数方法都带有同步和异步两种

>所有的异步方法都有一个回调函数，此回调函数的第一个参数都是一个错误对象。

>异步方法中如果有错误的话，会静默失败，不会自己抛出error，通常情况下都需要手动处理。

>常用Class:

>>fs.Stats	//文件或目录的统计信息描述对象

>>fs.ReadStream	//stream.Readable接口的实现对象

>>fs.WriteStream	//streamWriteable接口的实现对象

>>fs.FSWatcher	//可用于监视文件修改的文件监视器对象

	
>常用的方法：

>>fs.stat()	//用于返回一个文件或目录的统计信息对象（fs.Stats对象）

>>fs.mkdir()	//创建目录

>>fs.rmdir()	//删除目录

>>fs.readdir()//读取目录下的内容

>>fs.readFile()  //读取文件内容

>>fs.writeFile() //向文件中写入内容

>>fs.appendFile()  //向文件末尾追加内容

>>fs.unlink()	    //删除文件

>>fs.rename    //重命名文件


>>fs.stat()   &    fs.statSync()	//用于返回一个文件或目录的统计信息对象（fs.Stats对象）

>>fs.Stats对象的方法：

>>stats.isFile()	//是否为文件

>>stats.isDirectory()	//是否为目录

>操作目录：

>>fs.mkdir()	 &  fs.mkdirSync()	//创建目录

>>fs.rmdir()	 &  fs.rmdirSync()	//删除目录

>>fs.readdir()&  fs.readdirSync()	//读取目录下的内容
