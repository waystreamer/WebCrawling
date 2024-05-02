# 声明:

    **以下所有内容仅供学习交流,相关链接已删除，严禁用于商业用途和非法用途，否则由此产生的一切后果均与作者无关，若有侵权，请联系我立即删除！**

# 日志更新

### 2024-04-01

- 对某易云的params和encSecKey两个参数进行逆向

### 2024-04-02

- 加速乐的逆向分析
- **再度声明：加速乐的网站基本都是gov官网，不要乱爬！！！此仓库的全部链接均已删除，严禁用于商业用途和非法用途，否则由此产生的一切后果均与作者无关，若有侵权，请联系我立即删除！**

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

### 2024-04-15

- 对QQ音乐的逆向分析，比较简单

- 进行了webpack扣取，扣取量也不算很大

- 这里需要补环境。使用补环境框架时发现只需要补两处地方，所以这里就不用到补环境框架，直接自己手动补上就可以了

### 2024-04-16

- 对酷狗和酷我网页端的爬取

- 这两个网站都没有加密值，只有酷狗里面有一个时间戳的参数。其余的都不用关注，直接请求爬取即可

- 到此我自己的简易音乐软件所需要爬取的网页已经全部完成

- 新版百度翻译的爬取，旧版的百度翻译还有一个token加密参数需要逆向，新版的只需要直接请求接口爬取即可

### 2024-04-16

- 今日头条的逆向分析

- 有jsvmp，这里我还是选择补环境的做法

- 检测了navigator.plugins、使用document.createEvent('touchevent')会报错等等

- 在运行python环境下运行代码时，我还是选择使用nodejs去运行来返回结果（这个方法真的很香，不会出现报错并且返回的值一定正确）

- 这次补环境没有之前困难了。在之前逆向分析需要补环境的网站时，已经把我的补环境框架逐步完善了。所以在补这个网站的环境时只需要添加一些接口的属性和方法，修改一些值就可以过这个网站了

### 2024-04-18

- 巨量算数的逆向分析

- 需要逆向msToken、X-Bogus、_signature这三个参数

- msToken这个参数,可以理解成Message Token，相当于每次消息请求的令牌，主要用于请求统计。具有反爬虫的机制，如果相同msToken请求太多，也会被定义成恶意请求，这时候会出现验证码校验。可以自己伪造生成这个msToken

- X-Bogus、_signature的逆向存在jsvmp，和今日头条一样都是抖音旗下的产品，补环境也大差不差，总体来说比较简单。下一个逆向的是抖音，不知道逆向的参数会不会和这两个一样

- 这里还有最后一步待解决，就是返回的数据是加密的，这里需要进行解密。但是不管我怎么找都找不到他解密的地方。网上查找了解密的入口函数，进去后是平坦流，要自己插桩看打印日志，查看使用了什么加密方法。这里我对这种方法不是很熟悉，也不知道要如何操作。如果有大佬了解且会解决此问题，可以提交issue与我沟通，不胜感激!

### 2024-04-19

- 抖音的a_bogus参数逆向，这个参数逆向出来的值有错误

- 这里我也是通过补环境的方法把a_bogus这个参数给逆向出来了，但是长度只有158。正确的长度应该是160。这两天一直在看吐出来的环境，该补的都补上了，实在是不知道他检测了哪个环境。如果有大佬有逆向过这个网站的话，希望您能提交issue与我一起沟通。不胜感激！

### 2024-04-20

- boss直聘cookie中的__zp_token__逆向

- 今天重新补了一下环境，发现里面还检测了document.all，同时检测了 document.all 和 appendchild 之间的强关系。这里我尝试了很久都过不了这个检测。这里看别人都是通过自己写node插件来实现这个document.all。我也不会写这个插件😥😥😥

### 2024-04-24

- 大众点评的逆向分析

- 大众点评里面有两个参数存在加密逆向，分别是_token和mtgsig。但是_token这个参数他没有校验。但这里我全部都逆向分析出来了

- _token检测了浏览器环境，这里补环境不难，直接补出来再和网站加密前的参数进行比较。如果全部一致，则这个_token参数就逆向出来了

- 最重要的是mtgsig参数，折磨了我整整三天。
  
  - 全局搜索mtgsig搜不到，想找到他的入口函数也找不到。后面把H5guard.js的全部代码扣下来运行，把该补的环境全部补了。补完后发现window下面有了H5guard这个方法，里面有初始化的函数和一个sign函数。这里大胆推测是这个sign函数会生成mtgsig。
  
  - 进入sign函数会有个异步操作，跟代码逻辑找到生成的函数he()进行导出。he()函数需要传入一个Object对象的参数才能生成，这个参数后面发现只要固定就可以了。
  
  - 在he方法里面再进入一个函数内部才会生成mtgsig，里面是vmp，这里有补环境所以不管先。在vmp里面找到最后生成的mtgsig，对这个数值导出。但是！！！！出来的值进行网页的爬取返回的403。
  
  - 这里发现mtgsig中的a6的值不一样。在此我反复对比生成这个a6所需要的参数，最后发现是一个值的一个字母错了！网页生成的fK.k50是'048eea200'，但是我这里生成的值是'040eea200'，最后导致过不了。这里我推测大概率是浏览器环境的问题，但是我也过了几遍环境，能补的我都补了但就是过不了。这里我的解决方法是在生成a6前面直接暴力把fK.k50的值直接固定。最后生成出来的mtgsig是正确的了🤗🤗🤗

### 2024-04-25

- 重新再过了一下抖音，现在生成出来的长度是正确的了，自己生成的参数对比了网页上的也大概相似，但还是返回不了数据

- 上次长度不正确主要是document.body的clientHeight和clientWidth。因为在生成这个对象的时候我忘记挂上了代理，所以就没有补上这个值。

- 虽然生成结果类似，但是就是返回不了数据。大概是有某个检测点还没过，但是我还未发现

### 2024-04-26

- 猫眼票房逆向分析

- 需要逆向的参数有User-Agent，signKey

- 这两个参数的逆向都比较简单。User-Agent就是浏览器的ua进行base64加密，这个其实可以直接固定。signKey就是简单的md5加密返回的值

### 2024-04-27

- 京东逆向分析

- 需要逆向的参数只有h5st，该版本为4.7。后面爬取数据时发现这个h5st竟然不检测

- 在逆向这个参数的时候，发现和大众点评的差不多。都是在全局作用域里面定义一个对象，然后进行初始化，再通过异步来调用生成所需要的值。所以第一步直接把所关联的代码全部扣取运行，补环境先跑成功先。这里环境检测不是很多，也不难。接着就是找到h5st的生成位置然后进行导出。代码里面全是控制流，很恶心。这里我误打误撞，只打了十来个断点就直接找到生成的位置

```js
{
    key: s,
    value: function(e, t, r, n) {
    return ["" + r, "" + this._fingerprint, "" + this._appId, "" + (this._isNormal ? this._token : this._defaultToken), "" + e, "" + this._version, "" + t, "" + n].join(";")
    }
}
```

### 2024-04-30

- 瑞数四代逆向分析

- 初识瑞数四代，一开始还是有点无从下手，查阅相关的博客才了解瑞数四的基本流程，发现和加速乐很相似

- 瑞数四网站的基本发包流程：第一次访问页面（返回202和cookie_S）->获取js，解密生成cookie_T->第二次带上cookie访问页面（返回200和内容）

- 这里我使用的方法是补环境过这个瑞数四。论补环境的难度，算不上太难，很多接口的方法都没有调用，只是检测了是否有这个方法而已。补环境的时候也要会去跟堆栈，在网站里面运行到相关位置看看做了什么环境检测，然后自己再进行补环境。

- 我这里的补环境框架是直接过了这个瑞数4，基本不用补环境，只需要改改里面的变量即可

- 我主要遇到的困难是他的整个代码流程逻辑。我看别人的博客的做法后自己陷入了混乱，他们是进行了映射换代码的操作，这里我搞了很久才明白他们的意思。但是我这里没有用他们的方法。我这里是直接暴力拼接代码解决。首先要了解明白整个流程的逻辑和代码逻辑。因为他返回的是动态的代码，所以这里进行动态的拼接。然后在vm2沙箱环境中运行这个代码并且返回所需要的cookie_T。最后就是带上cookie再请求这个页面，返回数据

- 要说瑞数难吧，其实也不怎么难，会补环境并且理清他的逻辑基本很快就完成了

- 后续还会去逆向瑞数5、5.5、6代

### 2024-05-01:

- 淘宝逆向分析

- 需要逆向的参数只有sign，这个参数逆向比较简单，只是简单的把字符串拼接然后进行md5加密

- 加密位置如下，其中d.token这个参数经过测试是固定的，i是时间戳，g是headers中的appKey，c.data是headers中的data。将这些字符串拼接之后进行md5加密就是sign了

```js
, j = h(d.token + "&" + i + "&" + g + "&" + c.data)
, k = {
    jsv: w,
    appKey: g,
    t: i,
    sign: j
}
```

### 2024-05-02

- 得物逆向分析

- 需要逆向的参数只有sign，这里的sign逆向很简单，这个sign甚至可以固定

- 这里主要难的是找到加密的位置，下面的代码可以直接定位到加密的位置

- 里面加密的方法是md5，由两个字符串组成。一个是请求时需要携带的data，还有一个是他自己固定的字符串。如果只是单纯请求这个接口的话，这个sign就是一个固定值。

- 使用加密方法的代码进行扣取时，涉及到了webpack扣取。一开始我以为这个md5是魔改的，结果扣到后面发现就是原生的md5，可以直接调用库解决

```js
return t ? e.url += "?sign=".concat(n) : e.params.sign = n,
```

# 未解决

- [ ] **巨量算数中返回值加密data未进行解密**

- [ ] **抖音的a_bogus参数逆向**

- [ ] **boss直聘的cookie中的__zp_token__**

上述问题如有大佬能够解决，希望能够提交issue与我沟通，不胜感激!
