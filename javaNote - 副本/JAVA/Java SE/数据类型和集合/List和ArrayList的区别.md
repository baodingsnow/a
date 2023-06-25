List是一个接口，而ArrayList是List接口的一个实现类。 

       ArrayList类继承并实现了List接口。 

       因此，List接口不能被构造，也就是我们说的不能创建实例对象，但是我们可以像下面那样为List接口创建一个指向自己的对象引用，而ArrayList实现类的实例对象就在这充当了这个指向List接口的对象引用。

```java
List list = new List();//是错误的用法，因为list是一个接口
List list;//正确，list = null; 
/**这句创建了一个ArrayList实现类的对象后把它上溯到了List接口。此时它就是一个List对象
了，它有些ArrayList类具有的，但是List接口没有的属性和方法，它就不能再用了。 
*/
List list = new ArrayList();
ArrayList list=newArrayList();//创建一对象则保留了ArrayList的所有属性和方法
```

<mark>问题的关键: </mark>

        为什么要用 List list = new ArrayList() ,而不用 ArrayList alist = new ArrayList()呢？

问题就在于List接口有多个实现类，现在你用的是ArrayList，也许哪一天你需要换成其它的实现类，如 LinkedList或者Vector等等，这时你只要改变这一行就行了： List list = new LinkedList(); 其它使用了list地方的代码根本不需要改动。

![v2-76c3c04de2e8609c488fa0081fb99c26_1440w](E:\Sadnote\sad\picture\v2-76c3c04de2e8609c488fa0081fb99c26_1440w.png)

> 这张图里的内容对我们学习Java来说，非常的重要，白色的部分是需要去了解的，黄色部分是我们要去重点了解的，不但要知道怎么去用，至少还需要读一次源码。绿色部分内容已经很少用了，但在面试题中有可能会问到，我们来看一个经常出现的面试题：**Arraylist与Vector的区别是什么？**

**首先我们给出标准答案：  
1、Vector是线程安全的，ArrayList不是线程安全的。  
2、ArrayList在底层数组不够用时在原来的基础上扩展0.5倍，Vector是扩展1倍**

无一例外，**只要是关键性的操作，方法前面都加了synchronized关键字，来保证线程的安全性**。当执行synchronized修饰的方法前，系统会对该方法加一把锁，方法执行完成后释放锁，**加锁和释放锁的这个过程，在系统中是有开销的，因此，**在单线程的环境中，Vector效率要差很多。（多线程环境不允许用ArrayList，需要做处理）。

**和ArrayList和Vector一样，同样的类似关系的类还有HashMap和HashTable，StringBuilder和StringBuffer，后者是前者线程安全版本的实现。**

希望以后大家在面试过程中，能说出个因为所以，而不是一味的去背面试题，唯有理解，无需再背。

向上转型

对象引用

