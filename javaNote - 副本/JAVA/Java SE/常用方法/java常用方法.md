# $\textcolor{SkyBlue}{Java的Arrays.toString()方法介绍} $

```Java
int[] array= {3,8,5,65,34,27};
System.out.println(array.toString());
System.out.println(Arrays.toString(arry));

结果
[I@1b6d3586
[3, 8, 5, 65, 34, 27]

```

如果想要把[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)中的内容打印出来，直接使用toString方法只会打印出数组的地址，因此需要使用Arrays的toString方法，可以从其内部实现中看出来，该方法支持入参可以是long，float，double，int，boolean，byte，object型的数组。

```ruby


很有说法
```

# $\textcolor{SkyBlue}{BeanUtils.copyProperties的用法} $

>  我们如果有两个具有很多相同属性的[JavaBean](https://so.csdn.net/so/search?q=JavaBean&spm=1001.2101.3001.7020)，一个很常见的情况就是Struts里的PO对象（持久对象）和对应的ActionForm，传统的方式对属性逐个赋值：
>
> ```java
> casesUserIntegralEntity.setPluginId(casesUserIntegralEntityList.get(i).getPluginId());  			casesUserIntegralEntity.setTypeKey(casesUserIntegralEntityList.get(i).getTypeKey()); 	casesUserIntegralEntity.setPrimaryId(casesUserIntegralEntityList.get(i).getPrimaryId());
>  casesUserIntegralEntity.setName(casesUserIntegralEntityList.get(i).getName());
>   casesUserIntegralEntity.setIntegralUser(casesUserIntegralEntityList.get(i).getIntegralUser());                     casesUserIntegralEntity.setIntegralStanderd(casesUserIntegralEntityList.get(i).getIntegralStanderd());
> 
> ```
>
> 

 这时候我们如果用copyProperties，直接一行代码，然后就搞定了。

```java
BeanUtils.copyProperties("转换前的类", "转换后的类");  
```

但是有几点我们需要注意：

BeanUtils.copyProperties(a, b);

b中的存在的属性，a中一定要有，但是a中可以有多余的属性；
a中与b中相同的属性都会被替换，不管是否有值；
a、 b中的属性要名字相同，才能被赋值，不然的话需要手动赋值；
Spring的BeanUtils的CopyProperties方法需要对应的属性有getter和setter方法；
如果存在属性完全相同的内部类，但是不是同一个内部类，即分别属于各自的内部类，则spring会认为属性不同，不会copy；
spring和apache的copy属性的方法源和目的参数的位置正好相反，所以导包和调用的时候都要注意一下

# $\textcolor{SkyBlue}{函数方法调用顺序} $

  父类静态代码块 ->子类静态代码块 ->父类非静态代码块 -> 父类构造函数 -> 子类非静态代码块 -> 子类构造函数。

# $\textcolor{SkyBlue}{类加载器} $

# $\textcolor{SkyBlue}{类型转换} $

Integer对象的方法

Integer.parseInt("");是将字符串类型转换为int的基础数据类型

Integer.valueOf("")是将字符串类型数据转换为Integer对象

Integer.intValue();是将Integer对象中的数据取出，返回一个基础数据类型int         