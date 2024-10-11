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

- 网站:`aHR0cDovL3d3dy5mYW5nZGkuY29tLmNuL25ld19ob3VzZS9uZXdfaG91c2UuaHRtbA==`

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

### 2024-05-03

- B站首页的逆向爬取

- 这里需要逆向的参数只有w_rid，这里还需要注意dm_img_list、dm_img_str、dm_cover_img_str、dm_img_inter这四个参数。

- dm_img_list：里面记录的是鼠标轨迹，可以固定

- dm_img_str：取的WebGL的版本，可以固定

- dm_cover_img_str：取的显卡信息进行加密，可以固定

- dm_img_inter：推测是取浏览器里面的环境进行加密，这个也可以固定

- w_rid是md5加密，这里我直接调用库解决

- 这里请求次数多了会出现-352错误，显示风控校验失败。我这里也没测试哪个参数会和风控校验有关

### 2024-05-08

- B站评论区的全部评论爬取

- 这里有个加密参数，就是我之前弄的B站首页的那个加密参数，但这里我没用js去进行加密。因为是比较简单的md5加密，所以这里我直接按照他的代码逻辑自己用Python实现了这个参数的加密。也是为了后面实现用Scrapy框架爬取评论区做准备（这里用Python实现的原因是考虑到使用Scrapy框架进行爬取时。如果有大量的数据的话，我需要一直打开js文件生成加密参数再关闭这个js文件，他的io操作是非常庞大的）

- 这里有父评论和子评论，子评论就是回复父评论的评论（有点小饶）

- 全部评论的爬取就要进行翻页，这里B站是进行加载的形式，一开始一直搞不懂他是怎么获取后面加载的评论。因为从第二次加载评论开始，后面发包的请求参数都一模一样，就只有一个加密参数和时间戳发生了变化。

- 在我爬取的网页中，需要获取多页的情况一般都是在请求参数进行变化，比如会有一个请求参数page，进行下一页的请求就会把page+1。后面自己想了想是不是在同一个session对这个接口反复请求他就会实现翻页的操作，事实也是如此。需要注意的就是时间戳

- 最后需要判断评论是否全部爬完。这里回到网站，拉到底没有评论时会请求同一个接口，然后返回的数据中data['replies']是一个[]空数组。所以判断是不是空数组就可以知道是否是评论已经爬取完毕

- 接下来还要实现对子评论的爬取，未完待续...

### 2024-05-09

- 使用Selenium实现B站的登入

- 这个东西挺早之前就写了的，还是比较简单的，水一水GitHub

### 2024-05-10

- 完成B站评论区爬取的全部功能

- 实现了对子评论区的全部爬取

- 实现把全部的数据存储到数据库，还可以把数据打包成csv格式。这里我没有实现，可以自己扩展

- 后续我将会依照这个代码逻辑去使用Scrapy框架再实现这个B站评论区的爬取，发挥Scrapy框架的优点

### 2024-05-28

- 携程的逆向分析

- 这里有首页数据和评论的url，一共有三个参数

- 爬取首页数据需要的参数有_fxpcqlniredt和x-traceID。_fxpcqlniredt是cookie中的一个参数，x-traceID是_fxpcqlniredt、时间戳、一个随机生成的字符串组合而成

- 爬取评论区需要的参数是textab。这个会遇到vmp，这里我还是用补环境的方法去过这个vmp。这里的检测点和小红书的检测有点相似，全部补上即可。这里的vmp还是动态的，需要请求接口去返回动态js

### 2024-05-29

- 喜马拉雅逆向分析

- 需要逆向的参数是headers中的xm-sign，这个参数目前网站没有校验

- xm-sign由一个md5加密字符串和随机生成的数字和时间戳组成

### 2024-06-05

- 基于websocket实现实时长连接B站直播并获取弹幕
- 这里连接服务器时需要发送鉴权包，这个鉴权包是以二进制数据进行传输的。这个二进制数据转换前是一个json数据，里面包含了直播间的id，自己账户的uid，还有token（这里token我看B站直播信息流中说明这个token可以不携带）等等
- 服务器返回的数据也是一个二进制数据，需要自己转换。转换后就是一个json数据，按自己需求进行获取需要的内容。这里我只获取了有关弹幕的信息
- 这里不知道为什么获取弹幕时，会有部分弹幕获取不到。如果有大佬知道如何解决，希望您能与我沟通，谢谢
- 相关信息可以查完下面的文档

B站直播信息流文档：https://socialsisteryi.github.io/bilibili-API-collect/docs/live/message_stream.html

### 2024-06-09

- 瑞数五代逆向分析
- 网站：`aHR0cDovL3d3dy5uaGMuZ292LmNuL3dqdy9nZnh3amovbGlzdC5zaHRtbA==`
- 瑞数5和瑞数4整体流程基本都是一样的，只是比瑞数4多了一些环境检测。所以这里可以直接用瑞数4的代码进行修改运行即可
- 这里会出现一个生成出来的cookie长度不一样的问题。因为在网站中会有取 window.localStorage里面的某些值进行计算的步骤，这些值就是取浏览器 canvas 等指纹生成的，指纹随机就能并发，通常访问单独的一个 html 页面是不校验指纹的，生成的短 cookie 就能通过。如果短的cookie请求没有返回数据，那么就是有些必要的环境没有补上

### 2024-07-06

- 完成抖音的a_bogus参数逆向
- 这里排查了一下问题，其实之前补的环境没有错误，主要是没有登入后的cookie。直接请求能够成功，但是不会返回数据
- 环境检测不多，重点是有关window、screen和document.body的相关长度全都给补了。还有_sdkGlueVersionMap、onwheelx和xmst全都给补上去。再把一些常见的环境检测补上去，这个参数就是正确的了

### 2024-08-03

- 瑞数六代逆向分析
- 网站` aHR0cHM6Ly93d3cuY3JlZGl0Y2hpbmEuZ292LmNuLw==`

- 瑞数的流程不在赘述，主要注意4和5第一次请求html页面中是一个自执行函数，而6是相反的。并且代码不要格式化，这里会检测代码是否格式化。
- 这里我使用自己的补环境框架进行补环境，吐出来的环境全补齐了，但就是过不了。所以放弃了补环境框架，直接挂上环境代理手动补。
- 如果只是要过瑞数六的话，环境也不用补很多。必要的location、navigator、document、window，还有其script和Meta等全部补上即可，也不用补太详细。所以这里就不给出补环境的代码了。
- 如果不是在vm2环境下一定要注意添加`delete __filename;`和`delete __dirname;`，这里会对这个进行检测。然后不管是不是在vm2环境下一定要添加`window.ActiveXObject = undefined;`。具体可看以下文章：https://mp.weixin.qq.com/s/3NeI6AendlTlNMIrKL0G8Q

### 2024-08-09

- 小红书新版逆向分析
- 小红书现在把代码上了混淆，这里我还是用补环境的方法去过。整体的环境检测没什么变化。所以这次更新对于纯算来说上了难度。

### 2024-08-13

- 顶象验证码滑块逆向分析
- 网站是顶象的官方网站，其他网站使用顶象都是定制版，所以用这一套不一定能过别的网站，还是需要修改一点东西。
- 需要请求的接口就只有两个，并且接口中的参数大部分可以固定，这里检验并不严格。
- 主要有以下几个难点：
  - 这里请求回来的图片是乱码图片，按照其网站js还原图片的代码逻辑用Python来复现。
  - 滑块到缺口的距离使用OpenCV来进行识别
  - 这里有检测滑块轨迹，但是检测并不严格，这里就直接伪造一个轨迹
  - ac参数需要补环境，并且其js代码一天会变两次。这里通过请求并覆写js代码的方式来实现实时动态变换

### 2024-08-17

- 数美验证码滑块逆向分析
- 网站是数美的官方网站
- 需要请求的接口就只有两个，并且接口中的参数大部分可以固定。
- 主要逆向的两个参数是第二个滑动后接口的mu和je参数。跟栈到加密位置就可以知道mu参数与轨迹有关，je参数与滑块移动的距离有关。整体的加密方式都是DES加密，可以扣js代码也可以直接调用包来进行加密。
- 这里使用ddddocr来进行识别滑块距离，识别出来还要除以2，因为请求得到的图片是600*300，而网站上图片是300\*150。
- 轨迹校验并不严格，随机生成即可。

### 2024-08-21

- 网易易盾验证码滑块逆向分析
- 网站是易盾的官方网站
- 需要请求的接口只有两个，大部分参数可以固定
- 这里所有参数都是在一个大的自执行函数中生成的，把全部代码扣下来到本地运行，这里需要补环境
  - 第一个请求接口中需要逆向的参数有cb、fp和acToken（这里acToken可以固定也可以自己逆向出来，这里我选择固定）。cb参数跟栈到加密点，把加密函数提取出来即可。fp参数跟栈就可以知道，他是取自window.gdxidpyhxde，并且放入了cookie中。在运行这个自执行函数时就会生成window.gdxidpyhxde，然后直接document.cookie获取即可
  - 第二个请求接口需要逆向的参数有cb和data。cb参数和第一个接口加密位置一样。data参数包含了滑块距离、轨迹参数和第一次请求返回的token。找到加密位置直接把代码扣下来，然后找到所需的加密函数进行提取即可。

### 2024-08-26

- 360天御滑块验证码逆向分析
- 网站是360天御的官方网站
- 需要请求的接口只有两个
- 这里js代码全部上了混淆，但是整体不难。可以选择解混淆也可以选择硬钢，需要耐心
  - 第一个接口需要逆向的参数有nonce和sign。nonce参数是一个生成的时间戳加上随机数。sign是一个md5加密，加密的字符串就自己跟栈看吧
  - 这里请求获取的图片是乱码，所以要进行还原。根据其js代码逻辑推理出图片还原步骤即可。
  - 第二个需要逆向的参数只有一个report。这个参数对第一个接口返回的两个参数进行md5加密，对轨迹参数进行RSA加密，然后合并起来就是report参数。这里轨迹可以看网站他生成的逻辑进行伪造

- 这里使用ddddocr识别成功率并不高，看了一下滑块图片发现是泛白的，一定程度影响了ocr识别的效果

### 2024-09-18

- 腾讯滑块验证码逆向分析（未完成）
- 网站是腾讯天御的官方网站
- 这里并没有完成这个逆向分析。主要在collect这里卡住了，使用补环境怎么补返回的都是一个错误堆栈信息，还需要再研究研究

### 2024-10-11

- 极验四代滑块逆向分析
- 网站是极验的官方网站
- 这里需要请求的接口就只有两个，相比与三代来说没那么繁琐
- 第一个接口的challenge是一个随机数，在gt4.js文件中可以找到生成的逻辑。
- 第二个接口只有w需要逆向。w参数对滑块距离、滑动时间等加密，具体细节跟栈到加密位置查看。这里用了RSA和AES加密，这里我是直接把整个代码文件复制到本地（这里需要补环境，环境检测也很简单，只需要补window、document、navigator、location以及里面相关参数。这里就不展示补环境的代码了），然后对加密的函数进行导入使用，也可以直接调用库去实现RSA和AES加密。
- 目前的极验四代并没有检测轨迹参数，后面应该会更新检测轨迹。
- 对比与三代来说，四代就简单很多了，逻辑很多都和三代差不多。三代需要逆向三个w参数，而四代只需要逆向一个w参数。





# 未解决

- [ ] **巨量算数中返回值加密data未进行解密**

- [x] **抖音的a_bogus参数逆向**

- [ ] **boss直聘的cookie中的__zp_token__**

- [ ] **腾讯验证码collect**

上述问题如有大佬能够解决，希望能够提交issue与我沟通，不胜感激!
