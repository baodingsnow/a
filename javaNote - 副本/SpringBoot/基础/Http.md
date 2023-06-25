

# $\textcolor{LightSteelBlue}{TCP三次握手四次挥手详解}$

**TCP三次握手**

所谓三次握手(Three-way Handshake)，是指建立一个TCP连接时，需要客户端和服务器总共发送3个包。

三次握手的目的是连接服务器指定端口，建立TCP连接,并同步连接双方的序列号和确认号并交换 TCP 窗口大小信息.在socket编程中，客户端执行connect()时。将触发三次握手。



> 握手过程
>    1、第一次握手：客户端给服务器发送一个 SYN 报文。
>
>    2、第二次握手：服务器收到 SYN 报文之后，会应答一个 SYN+ACK 报文。
>
>    3、第三次握手：客户端收到 SYN+ACK 报文之后，会回应一个 ACK 报文。
>
>    4、服务器收到 ACK 报文之后，三次握手建立完成。



# URL和URI

uri和url的关系
uri是url的父级，url是uri的子级。
可能有人就奇怪了，咦？明明是url包含了uri为啥uri反而是父级？
请注意，我这里用的是级别来描述，而不是包含。
我没有说url是uri的一部分，而是说是他的子级。
想要理解这个概念，最好的说明就是java的继承关系。url继承了uri。这样来看是不是瞬间就明白了。
因为url继承了所有uri的内容，所以它比uri更加详细，但是uri是它的父级。

有什么作用？
url的作用
url一般是一个完整的链接，我们可以直接通过这个链接（url）访问到一个网站，或者把这个url复制到浏览器访问网站。
使用URL时我们就是一个直接用户的角色，直接访问就完事了。

uri的作用
uri并不是一个直接访问的链接，而是相对地址（当然如果相对于浏览器那么uri等同于url了）。这种概念更多的是用于编程中，因为我们没必要每次编程都用绝对url来获取一些页面，这样还需要进行分割“http://xx/xxx”前面那一串，所以编程的时候直接request.getRequestURI就行了，当然如果是重定向的话，就用URL。



# http请求方法



**1、GET方法：**对这个资源的查操作。

**2、DELETE方法：**对这个资源的删操作。但要注意：客户端无法保证删除操作一定会被执行，因为HTTP规范允许服务器在不通知客

户端的情况下撤销请求。



**3、PUT和POST**

PUT和POS都有更改指定URI的语义.但PUT被定义为idempotent的方法，POST则不是.idempotent的方法:如果一个方法重复执行

多次，产生的效果是一样的，那就是idempotent的。也就是说：

PUT请求：如果两个请求相同，后一个请求会把第一个请求覆盖掉。（所以PUT用来改资源）

Post请求：后一个请求不会把第一个请求覆盖掉。（所以Post用来增资源）


**4、get和post**

1、GET参数通过URL传递，POST放在Request body中。

2、GET请求会被浏览器主动cache，而POST不会，除非手动设置。

3、GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

4、Get 请求中有非 ASCII 字符，会在请求之前进行转码，POST不用，因为POST在Request body中，通过 MIME，也就可以传输非 ASCII 字符。

5、 一般我们在浏览器输入一个网址访问网站都是GET请求

6、HTTP的底层是TCP/IP。HTTP只是个行为准则，而TCP才是GET和POST怎么实现的基本。GET/POST都是TCP链接。GET和POST能做的事情是一样一样的。但是请求的数据量太大对浏览器和服务器都是很大负担。所以业界有了不成文规定，（大多数）浏览器通常都会限制url长度在2K个字节，而（大多数）服务器最多处理64K大小的url。

7、GET产生一个TCP数据包；POST产生两个TCP数据包。对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

8、在网络环境好的情况下，发一次包的时间和发两次包的时间差别基本可以无视。而在网络环境差的情况下，两次包的TCP在验证数据包完整性上，有非常大的优点。但并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。

# **Springboot中RestController中接收参数的注解：**

@RequestParam 用于获取请求参数

@PathVariable 映射 URL 绑定的[占位符](https://so.csdn.net/so/search?q=占位符&spm=1001.2101.3001.7020)
通过 @PathVariable 可以将 URL 中占位符参数绑定到控制器处理方法的入参中:URL 中的 {xxx} 占位符可以通过

@PathVariable(“xxx”) 绑定到操作方法的入参中。





# header、query、body参数











## @RequestBody

进入方法前的权限验证

@RequestBody
主要用来接收前端传递给后端的json字符串中的数据的(请求体中的数据的)，所以一般应用到POST/PUT提交方法。也就是前端提交form表单数据的json格式。

前端需要设置header头为的content-type设置为application/json;charset=UTF-8，并且需要将数据转成json对象，例如：

header: {
    contentType: 'application/json;charset=UTF-8'
}


一般会这样用， 加上javax.validation.Valid包中的 @Valid 注解，接收参数的同时验证传递参数是否完整

@RequestBody @Valid Form form


## RequestParam

这个使用来获取key-value格式传递的参数，例如GET方式中http://xxxx?token=12321或者是http://ssss?page=1，中可以用RequestParam获取到对应key的value。

@RequestParam(value="keyname", request=false, defaultValue="zhangsan")



## @PathVariable

这个是获取url中参数对应的值，必须与RequestMapping中的占位符保持一致，不然会报错。

@GetMapping("/detail/{id}")
public void getData(@PathVariable("id") Long id) {}
