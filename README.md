# 声明:

    **以下所有内容仅供学习交流，严禁用于商业用途和非法用途，否则由此产生的一切后果均与作者无关，若有侵权，请联系我立即删除！**

# 日志更新

### 2024-04-01

- 对某易云的params和encSecKey两个参数进行逆向

### 2024-04-02

- 加速乐的逆向分析

### 2024-04-04

- 极验三代滑块逆向分析

- 现在的滑块需要把三个加密参数全部逆向并且放入请求头中，不然最后会返回失败

- 在逆向过程中有一个滑块时间参数，需要和模拟轨迹后所需的时间一致

- 在逆向过程中有一个随机生成的key值，如果存在后面使用的时候这个key就是第一次生成的参数，所以要注意保持一致

### 2024-04-05

- 拼多多的anti_content逆向分析

- 需要补环境，环境检测比较简单

- 考察异步调试和webpack的扣取

### 2024-04-06

- 同花顺的cookie中的加密参数v

- 需要补环境

### 2024-04-07

- 看准网逆向分析

- webpack打包

- 返回的参数进行了加密

**对于返回参数加密有以下方法可以找到参数加密的位置：**

1. 搜索json.parse（优先使用，搜索结果会很多，需要自己慢慢找）

2. 搜索decrypt关键字

3. 搜索拦截器

4. 下断点到类似.then的回调方法

### 2024-04-08

- 有道翻译逆向分析

- 返回的data数据是加密的

- AES和MD5加密

- 使用了Buffer.alloc的方法创建了新的缓冲区对象

- 使用了crypto.createHash的方法进行调用md5或者aes进行加密。crypto需要引入才能使用

- 我对上面两个方法不是很熟悉，一开始是直接把全部代码扣取，发现很麻烦并且还有许多报错。后面直接调用所需要的方法直接生成

### 2024-04-09

- 中国观鸟网的逆向分析

- 这里使用execjs一直报文件编码错误，此博客[https://blog.csdn.net/hihell/article/details/109528220]() 可以解决

- 此网站返回的data数据是加密的，按照上面所说的找到加密参数的方法进行解密

### 2024-04-10

- 对知乎的X-zse-96参数进行逆向分析

- 初识vmp，jsvmp的逆向方法有三种，分别是RPC远程调用，补环境，日志断点还原算法。使用纯算法的话不管这个jsvmp怎么更新都能使用，但是纯算法还原对于现阶段的我来说还是有点难度。所以这里我是通过补环境的方法来实现的，中间也是尝试进行插桩输出查看调用了什么环境

- 逆向过程中有一个报错很让我不解，明明作用域中是有__g这个函数的，但是在vm2沙箱中到了某个case中执行eval('__g')时却发生__g is not define的情况。只能把__g函数的定义和使用到的方法放到最外层作用域中.在不使用vm2沙箱运行代码时却能正常过这个报错

- jsvmp基本都设置了运行时长，如果超过这个时间代码会直接报错，防止了单步调试

### 2024-04-13

- 对小红书的请求头X-S逆向分析

- 有jsvmp算法，我使用的还是补环境的方法，里面有很多检测点，比如对Object中的一些方法的检测。还会通过修改navigator中的属性再重新取值来检测navigator是否被冻结

- 这里使用pycharm运行时，发现使用execjs运行js文件返回的结果不正确。所以直接使用nodejs运行后获取返回值。具体如此博客：[https://blog.csdn.net/lhys666/article/details/120677584]()
