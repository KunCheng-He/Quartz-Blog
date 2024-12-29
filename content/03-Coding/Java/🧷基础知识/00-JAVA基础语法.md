> [!tip]
> 基础语法哪里都可以找到，熟悉之后其实大概率整体不会忘，最多是细节上的问题会变得模糊，本身写 `Python` 比较多，我希望在不熟悉的时候，回看一下整个文档，就可以再次快速上手，做到快速*热加载*。**参考书籍：《Java核心技术·卷Ⅰ：基础知识（原书第10版）》**

# 0 Java程序设计环境

![[Pasted image 20230717212007.png]]

![[Pasted image 20230717213115.png]]

+ 🍵 `Java` 有程序的执行入口是 `main` 方法，有固定的写法：`public static void main(String[] args) {...}` 
+ **一个 `*.java` 的源文件中仅能有一个 `public` 类，其它类的则不受限制，也可以不包含 `public` 类**
+ 如果源文件包含了 `public` 类，**那这个类的名称必须和文件名一致**
+ 可以将 `main` 方法写在 `非public` 类中，然后指定运行 `非public` 类
+ 规范上，**类、方法**的注释，需要以 `javadoc` 的方式来写

+ 🫦**命名规范**
	+ *包名*多单词：都小写（`aaa.bbb.ccc`）（一般结构：`com.公司名.项目名.业务模块名`）
	+ *类名、接口名*：大驼峰（`AaaBbbCcc`）
	+ *变量、方法名*：小驼峰（`aaaBbbCcc`）
	+ *常量名*：都大写（`XXX` 、`XXX_YYY`）

# 1 注释

`Java` 有三种注释，`//` 用于*单行注释*，`/*...*/` 用于*长注释*，`/**...*/` 可以用来自动生成文档。

```java
/**
 * ClassName: hello
 * Package: cn.hekuncheng.hello
 * Description: 输出 Hello Word
 *
 * @Author: KunCheng-He
 * @Create: 2024/3/27 - 10:49
 * @Version: v1.0
 */
public class hello {
    /* 这是主函数 */
    public static void main(String[] args) {
        // 输出 Hello Word
        System.out.println("Hello Word");
    }
}

class demo {  
    /**  
     * PI 是圆周率（域注释）  
     */  
    public static final double PI = 3.14;  
  
    /**  
     * 打招呼方法（方法注释）  
     * @param name 是打招呼的姓名  
     * @return 打招呼的内容  
     */  
    public static String hi(String name) {  
        return "hello, " + name;  
    }  
}
```

使用 `/**...*/` 编写的注释可以抽取出来形成文档，命令如下：

```shell
# 抽取一个包
javadoc -d docDirectory nameOfPackage

# 多个包生成文档
javadoc -d docDirectory nameOfPackage1 nameOfPackage2...

# 文件默认在包中
javadoc -d docDirectory *.java
```

# 2 数据类型

> [!tip] Java 数据类型
> Java是一种*强类型语言*，这就意味着必须为每一个变量声明一种类型。在Java中，一共有**8种基本类型**，其中有**4种整型**、**2种浮点类型**、1种用于表示*Unicode编码*的字符单元的**字符类型char**和1种用于表示真值的**boolean类型**。Java*大数值*不是新的数据类型，而是一个**对象**。

![[Pasted image 20230718205458.png]]

## 2.0 整形

|  类型  | 存储需求  |                   取值范围                   |
| :--: | :---: | :--------------------------------------: |
| int  | 4Byte |    -2147483648~2147483647(*正好超过20亿*)     |
| shot | 2Byte |               -32768~32767               |
| long | 8Byte | -9223372036854775808~9223372036854775807 |
| byte | 1Byte |                 -128~127                 |

+ 🌟 整形数据默认为 `int` 型，当希望默认为其它整形类型时，需要加上对应后缀，长整型的后缀为 `L` 或 `l`，如：`89L`。十六进制使用前缀 `0x` 或 `0X`，二进制前缀为 `0b` 或 `0B`，*Java7*开始二进制也可以用下划线的形式表示，如：`int a = 1_000_010;`
+ 📌 **低精度是可以自动向高精度转换的**，反之则不行，一个细节是**byte、short 和 char**之间是**不会相互自动转换**的，但只要参与计算，类型就会自动变为 `int` ，所以计算结果至少要用 `int` 接收
+ `高精度` -> `低精度` ，这个时候需要**强制类型转换**，如：`(int)3.7`
+ ⚠️ Java没有任何无符号（unsigned）形式的int、long、short或byte类型

## 2.1 浮点类型

|  类型  | 存储需求 |                   取值范围                    |
|:------:|:--------:|:---------------------------------------------:|
| float  |  4Byte   |     大约±3.40282347E+38F(有效位数为6~7位)     |
| double |  8Byte   | 大约±1.79769313486231570E+308(有效位数为15位) |

+ 🌟 浮点类型默认类型为 `double`，也可以使用后缀 `D` 或 `d`，`float` 类型的后缀为 `f` 或 `F`，
+ 🚩 三个特殊的浮点数：*正无穷*、*负无穷*、*NaN*（不是一个数字）。（当进行分母为0的除法运算时，得到的结果就是 `NaN`）
+ 🚩 Java 中表示以上三个特殊浮点数的方法：`Double.POSITIVE_INFINITY`、`Double.NEGATIVE_INFINITY`、`Double.NaN`。判断某个数与之是否相等应该使用具体的**类方法**，如：`Double.isNaN(a)`。
+ **如果在数值计算中不允许有任何舍入误差，就应该使用** `BigDecimal类`

## 2.2 char类型

> char类型的值用**单引号**申明，可以表示为十六进制值，其范围从`\u0000` 到 `\Uffff`

部分*特殊字符的转义序列*如下表所示

| 转义序列 | 名称  | Unicode值 |
| :--: | :-: | :------: |
| `\b` | 退格  | `\u0008` |
| `\t` | 制表  | `\u0009` |
| `\n` | 换行  | `\u000a` |
| `\r` | 回车  | `\u000d` |
| `\'` | 单引号 | `\u0027` |
| `\"` | 双引号 | `\u0022` |
| `\\` | 反斜杠 | `\u005c` |

+ 📌 *Unicode转义序列*会在解析代码之前得到处理。例如，"`\u0022+\u0022`"并不是一个由引号(U+0022)包围加号构成的字符串。实际上，`\u0022`会在解析之前转换为`"`，这会得到`""+""`，也就是一个**空串**

> [!important] char类型的使用建议
> 在Java中，char类型描述了UTF-16编码中的一个代码单元。强烈建议不要在程序中使用char类型，除非确实需要处理UTF-16代码单元。**最好将字符串作为抽象数据类型处理**。

## 2.3 boolean类型

> [!tip] boolean类型
> boolean（布尔）类型有两个值：`false`和`true`，用来判定逻辑条件。*整型值*和*布尔值*之间**不能**进行相互转换。

# 3 变量

🌟 Java中，每个变量都有一个类型（type），一般采用**小驼峰**的方式命名，变量名必须是一个以**字母开头并由字母或数字构成**的序列，除了一般的字母外，还包括 `_`、`$`（不推荐使用）

> 🌟 如果想要知道哪些Unicode字符属于Java中的“字母”，可以使用`Character`类的`isJavaIdentifierStart`和`isJavaIdentifierPart`方法来检查。

```java
public class hello {
    public static void main(String[] args) {
        // @ 在Java中不属于“字母”，所以输出结果为：false
        System.out.println(Character.isJavaIdentifierPart('@'));
    }
}
```

## 3.0 变量的初始化

🚫 声明一个变量之后，必须用赋值语句对变量进行显式初始化，千万不要使用未初始化的变量。

```java
int a = 1;
int b;
b = 2;
```

## 3.1 常量

> [!important] 常量
> Java中，利用关键字`final`指示常量。关键字`final`表示这个变量只能被赋值一次，一旦被赋值之后，就**不能够再更改**了。习惯上，**常量名使用全大写**。

在`Java`中，经常希望某个常量可以在一个类中的多个方法中使用，通常将这些常量称为*类常量*。可以使用关键字`static final`设置一个类常量。如果一个常量被声明为`public`，那么其他类的方法也可以使用这个常量。

```java
public class hello {
    public static final double PI = 3.14;

    public static void main(String[] args) {
        ...;
    }
}
```

# 4 运算符

> [!important] 运算符
> Java中，使用算术运算符`+`、`-`、`*`、`/`表示*加、减、乘、除*运算。当参与`/`运算的两个操作数**都是整数**时，表示**整数除法**；*否则*，表示*浮点除法*。整数的求余操作（有时称为取模）用`%`表示。

> *默认*情况下，虚拟机设计者允许对**中间计算结果采用扩展的精度**。但是，对于使用`strictfp`关键字标记的方法**必须使用严格的浮点计算**来生成可再生的结果。可以用该关键字标记`main`方法

```java
public static strictfp void main(String[] args)
```

## 4.0 自增运算

**自增运算细节**，平时用不到，可能面试会问，也蛮有意思的

```java
int i = 1;
i = i++; // 细节 1. temp=i; 2. i=i+1; 3. i=temp;
// 所以最后 i = 1

int i = 1;
i = ++i; // 细节：1. i=i+1; 2. temp=i; 3. i=temp;
// 所以最后 i = 2
```


## 4.1 数学函数与常量

> [!info] 数学函数
> `Math类`中，包含了各种各样的数学函数。在*Math类*中，为了达到*最快的性能*，所有的方法都*使用计算机浮点*单元中的例程。**如果得到一个完全可预测的结果比运行速度更重要的话**，那么就**应该使用**`StrictMath类`。它使用“自由发布的Math库”（fdlibm）实现算法，以**确保在所有平台上得到相同的结果**。

📌 在Java中，*没有幂运算*，因此需要借助于Math类的`pow方法`。其它常用方法如下表：

|  说明  |                            方法                             |
| :--: | :-------------------------------------------------------: |
| 三角函数 | `Math.sin` `Math.cos` `Math.tan` `Math.atan` `Math.atan2` |
| 反函数  |            `Math.exp` `Math.log` `Math.log10`             |
|  常量  |                    `Math.PI` `Math.E`                     |
| 四舍五入 |           `Math.roud`（需要使用强制类型转换成(int)，默认为long）           |

## 4.2 数值类型转换

![[Pasted image 20240327144319.png]]

> 实线箭头代表转换没有信息丢失，虚箭头表示可能有精度损失的转换。当两个数进行二元计算时，如果数据类型不一样，则*自动向高精度转换*。

> [!tip] Tips
> 不要在boolean类型与任何数值类型之间进行强制类型转换，这样可以防止发生错误。只有极少数的情况才需要将布尔类型转换为数值类型，这时可以使用条件表达式b?1:0。

## 4.3 结合赋值与运算符

> 🌟 如果运算符得到一个值，其类型与左侧操作数的**类型不同**，就会发生**强制类型转换**

```java
int x = 10;
x += 3.5; // 等同于 (int)(x + 3.5)
```

## 4.4 关系与boolean运算

> 🌟 相等性`==`、不等性`!=`、常用比较 `>` `<` `<=` `>=`

逻辑运算
> 🚨 `&&` 这是 **短路与**，`&` 这是 **逻辑与** （`||` 短路或 `|` 逻辑或 **同理**）
> 如：`a>1 && b<5` 与 `a>1 & b<5`
> **区别在于：** `&&` 做完第一个判断后，如不成立，后续不再判断，`&` 中所有判断会全部做完

三元运算
> 三元运算：`条件 ? 表达式1 : 表达式2;` （三元运算是一个整体，会**优先统一到高精度**，如：true ? new Integer(1) : new Double(5.7)；以上代码*返回的值*为`1.0`，而*不是*`1`）

## 4.5 位运算

> 🌟 与 `&` 、或 `|` 、异或 `^` 、非 `~` 、左移 `<<`、右移 `>>`、

> [!important] Tips
> `>>>`运算符会**用0填充高位**，这与`>>`不同，它会用*符号位填充高位*。**不存在`<<<`运算符**。

## 4.6 枚举类型

> [!tip] 使用场景
> 有时候，变量的取值只在一个有限的集合内。例如：销售的服装或比萨饼只有小、中、大和超大这四种尺寸。当然，可以将这些尺寸分别编码为1、2、3、4或S、M、L、X。但这样存在着一定的隐患。在变量中很可能保存的是一个错误的值（如0或m）。针对这种情况，可以自定义枚举类型。

```java
enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE};
Size size = Size.SMALL;
```

# 5 字符串

> [!tip] String
> Java没有内置的字符串类型，而是在标准Java类库中提供了一个**预定义类**，很自然地叫做`String`。每个用**双引号**括起来的字符串都是String类的一个实例。

> [!important] **String不可更改！！！**
> `Java`文档中将`String类`对象称为**不可变字符串**，**不能修改Java字符串中的字符**。如果想要修改某个String实例的字符串值，只能*生成新的字符串*后，*将该实例的索引指到新的字符串即可*。（不可修改的其中一个优点在于原始字符串与复制的字符串可以**共享**公共存储池中相同的字符）

**String类**常用的一些`API`如下表，静态方法需要通过`String`关键字调用，普通方法则通过*实例*调用

|                                      方法                                       |          说明           |
| :---------------------------------------------------------------------------: | :-------------------: |
|               `String substring(int beginIndex, int endIndex)`                |      返回区间范围内的子串       |
| `public static String join(CharSequence delimiter, CharSequence... elements)` |  多个字符串拼接，简单的可以使用`+`   |
|                       `boolean equals(Object anObject)`                       | 比较字符串相等（`==`比较的是内存地址） |
|               `boolean equalsIgnoreCase(String anotherString)`                |     比较字符串相等，忽略大小写     |
|                                `int length()`                                 |         字符串长度         |
|                           `char charAt(int index)`                            |      返回给定位置的代码单元      |
|                     `int compareTo(String anotherString)`                     |     按字典顺序比较两个字符串      |
|                      `boolean startsWith(String prefix)`                      |   测试此字符串是否以指定的前缀开头    |
|                       `boolean endsWith(String suffix)`                       |   测试此字符串是否以指定的后缀结尾    |
|                              `int indexOf(...)`                               |     返回查找字符串的开始位置      |
|                            `int lastIndexOf(...)`                             | 返回查找字符串最后一个匹配字串的开始位置  |
|        `String replace(CharSequence target, CharSequence replacement)`        |         替换字符串         |
|                                `String trim()`                                |     返回去掉前尾空格的新字符串     |

+ **基本数据类型与 String 之间的转换**
	+ base -> String：`int a; String b = a + "";` 加一个双引号就转过去了
	+ String -> base：`String b = "100"; Integer.parseInt(b);` 需**使用对应基础数据类型的包装类**，再调用 `parsexxx` 方法即可

# 6 输入输出

**输入相关**
```java
Scanner in = new Scanner(System.in);
// 回车结束
String a = in.nextLine();
// 空格结束
String b = in.next();
// 读整数
int c = in.nextInt();

// 密码的输入
Console cons = System.console();
char[] password = cons.readPassword("Password:");
```

**输出相关**
```java
// 正常输出变量，结尾没有换行
System.out.print(a);

// 格式化输出
System.out.printf("%.2f", 5.986);

// 输出自带换行
System.out.println(b);
```

# 7 控制流程

**条件语句**
```java
if(condition) {
    ;
}else if(condition) {
    ;
} else {
    ;
}

// 简化版本
if(condition) ... else ...;
```

**循环**
```java
// while 先判断，再执行
while(condition) {
    ;
}

// do-while 先执行一次，再判断
do {
    ;
} while(condition);

// 通用 for 循环
for (int i = 0; i < xx; i++) {
    ;
}
```

**多重选择语句**（📌 如果在case分支语句的末尾**没有break语句**，那么就会*接着执行*下一个case分支语句）
```java
switch (choice) {
  case 1:
     ...;
     break;
  case 2:
     ...
  default:
      ...;
}
```

# 8 大数值

> [!info] 大数值
> 如果基本的整数和浮点数精度不能够满足需求，那么可以使用`java.math`包中的两个很有用的类：`BigInteger`和`BigDecimal`。这两个类可以处理包含任意长度数字序列的数值。`BigInteger`类实现了**任意精度的整数运算**，`BigDecimal`实现了**任意精度的浮点数运算**。对应的*计算*也需要使用提供的*类方法*。普通数值通过`valueOf`方法转为大数值实例。

> 具体的计算方法为：和`add`、差`subtract`、积`multiply`、商`divide`

```java
public static void main(String[] args) {
    BigInteger a = new BigInteger("7482646927839246235435");
    // 输出：7482646927839246235500
    System.out.println(a.add(BigInteger.valueOf(65)));
}
```

# 9 数组

数组是一种数据结构，用来存储**同一类型值**的集合。通过一个整型下标可以访问数组中的每一个值。📌创建一个数字数组时，所有元素都初始化为`0`。`boolean`数组的元素会初始化为`false`。对象数组的元素则初始化为一个特殊值`null`，这表示这些元素（还）未存放任何对象。基本的语法如下：

```java
// 申明一个数组
int[] a;

// 初始化创建数组
a = new int[100];
```

## 9.0 for each循环

`for each` 为增强 `for` 循环，可以很方便的实现数组遍历，基本格式如下：

```java
for (variable : collection) statement
```

定义一个变量用于暂存集合中的每一个元素，并执行相应的语句。**collection 这一集合表达式必须是一个数组或者是一个实现了 Iterable接口 的类对象**。

## 9.1 数组初始化与匿名数组

`Java` 可以不使用 `new` 创建数组并同时完成初始化，如：

```java
int[] smallPrimes = {2, 3, 5, 7, 11, 13};
```

还可以创建**匿名数组**，**使用匿名数组这种语法形式可以在不创建新变量的情况下重新初始化一个数组**

```java
smallPrimes = new int[] {17, 19, 23, 29, 31, 37};
```

## 9.2 数组的拷贝

> [!tip] 浅拷贝与深拷贝
> `Python` 中可以称为 **浅拷贝与深拷贝**，`Java` 中我不知道是否可以这样称呼，但是本质上是一样的。**浅拷贝中，两个变量引用同一个数组，指向的地址是一样的**。**深拷贝中，两个变量指向的地址不同，底层重新开辟了一个新的内存空间对值进行复制**。

两种拷贝的例子如下：

```java
// 浅拷贝
int[] a = {1, 2, 3};
int[] b = a;

// 深拷贝
int[] b = Arrays.copyOf(a, a.length)
// 这个方法在拷贝的同时通常用来增加数组的大小
int[] b = Arrays.copyOf(a, 2 * a.length)
```

深拷贝需要使用 `Arrays` 类中的 `copyOf` 方法。如果进行了*数组的扩容*，数组元素是数值型，那么多余的元素将被赋值为 `0`；如果数组元素是布尔型，则将赋值为 `false`。相反，如果长度*小于*原始数组的长度，则*只拷贝最前面的数据元素*。

## 9.3 命令行参数

🌟 每一个 `Java` 应用程序都有一个带 `String arg[]` 参数的 `main` 方法。这个参数表明 `main` 方法将接收一个*字符串数组*，也就是*命令行参数*。例子如下：

```java
public class Hello {
    public static void main(String[] args) {
        if (args.length == 0 || args[0].equals("-h"))
            System.out.print("Hello,");
        else if (args[0].equals("-g"))
            System.out.print("Goodbye,");
        // 打印命令行参数
        for (int i = 0; i < args.length; i++)
	        System.out.print(" " + args[i])
    }
}
```

## 9.4 Arrays常用API

|                             API名称                              |                              描述                              |
| :------------------------------------------------------------: | :----------------------------------------------------------: |
|               `static String toString(type[] a)`               |             返回包含a中数据元素的字符串，这些数据元素被放在括号内，并用逗号分隔。              |
|           `static type copyOf(type[] a, int length)`           |                            数组深拷贝                             |
|    `static type copyOfRange(type[] a, int start, int end)`     |         返回与a类型相同的一个数组，其长度为length或者end-start，数组元素为a的值         |
|                  `static void sort(type[] a)`                  |                      采用优化的快速排序算法对数组进行排序                      |
|          `static int binarySearch(type[] a, type v)`           | 采用二分搜索算法查找值v。如果查找成功，则返回相应的下标值；否则，返回一个负数值r。-r-1是为保持a有序v应插入的位置 |
| `static int binarySearch(type[] a, int start, int end,type v)` |                        限定了二分搜索算法的查找范围                        |
|              `static void fill(type[] a, type v)`              |                       将数组的所有数据元素值设置为v                        |
|          `static boolean equals(type[] a, type[] b)`           |               如果两个数组大小相同，并且下标相同的元素都对应相等，返回true               |

## 9.5 多维数组

多维数组将使用多个下标访问数组元素，它适用于表示表格或更加复杂的排列形式。典型的*二维数组*与一维数组的申明方法类似，初始化的方法也基本相同：

```java
int[][] a;
a = {
    {1, 2, 3},
    {4, 5, 6},
};

for (int[] a : b) {
    for (int b : c)
        System.out.print(c)
}
```
