# Iris语言语法介绍

## 关于Iris
*Iris*是一门动态的解释性语言。它具有很多特性，你能够从它身上看到诸如*Java、C#、Ruby、Python、Perl、PHP*等语言的影子，同时它也具有一些自己的特性，关于这一点在您了解了它的具体使用方法后可能会更加清楚。*Iris*借鉴最多的是*Ruby*，同时又在*Ruby*的基础上进行了一些改变，但总体而言，*Iris*还是有*Ruby*有很多相似之处的。

开发*Iris*的初衷是想要打造一门易于扩展、能够在多个平台上使用的脚本语言，以支持跨平台游戏引擎*Iris 2D*的脚本化。这里谈到的跨平台主要是指*Windows/Windows Mobile、iOS、Android、Web Browser*等。

同时*Iris*的开发另一个目标就是能够方便地使用某一语言对*Iris*本身进行扩展，比如，在*Windows*上的*CppIris*能够和C\+\+进行非常方便的交互，进一步能够方便地用C\+\+扩展*Iris*，从而把现有的C\+\+库引入*Iris*，用*Iris*来进行驱动。因此*Iris*是粘性很大的胶水语言，可以说它是依赖于宿主语言而生的。

*Iris*是**完全面向对象**的，这一点和*Ruby*很像。在*Iris*中任何一个组件都是对象，因此*Iris*能够用优雅的面向对象方法解决问题（这甚至是强制性的）。虽然完全面向对象可能会造成一些问题，但是*Iris*的设计本身就不是以效率为重的，毕竟*Iris*的设计并不如*Lua*那般轻巧；而*Iris*的设计也是尽量以编码的优雅为主的。

*Iris*是完全动态的语言，对*元编程*的支持也和*Ruby*类似。您可以用流程控制语句来控制一个类是否被定义、在函数执行的时候动态修改某个类的方法，等等。基本上只要您能够想到的"动态"的特征，*Iris*都具备，当然，Iris的这点灵活性或许会给您带来麻烦——但您完全可以通过一些方法来避免这一点。

*Iris*是一门小巧精致的语言，它虽然并不具有C\+\+这样子重量级语言的特性，但是相信在一般的应用场合，*Iris*还是能够满足您的需求的。

## Iris 1.0

### Iris的表达式（Expression）

> 表达式是*Iris*的基本语法单位，*Iris*的表达式包括**字面量、赋值表达式、一元表达式、二元表达式、自运算赋值表达式、索引表达式、方法调用表达式、成员引用表达式、域表达式、数组表达式和Hash表达式**。下面进行逐一的介绍。

#### 1. 字面量

目前版本下的*Iris*拥有3种字面量，分别为整数字面量、浮点数字面量、字符串字面、true、false、nil，这些字面量分别是*Iris*内置的*Integer、Float、String、TrueClass、FlaseClass、NilClass*类的对象。

> **整数字面量**的表达形式为：[+/-]整数值，比如10、-23、+8等；  
> **浮点数字面量**的表达形式为：[+/-]小数值，比如1.37、-3.14、0.0等；  
> **字符串字面量**的表达形式为："字符串内容"，比如"Hello,World!"；  
> **true、false、nil**：用于表示真、假以及空的概念。

注意由于当前版本的Iris并没有实现大数，因此整数的范围是您所在环境下编译*Iris*的C\+\+编译器一个*int*类型值的范围、浮点数的范围是您所在环境下编译*Iris*的一个*double*类型值的范围。

字面量是写出来了就会被解释器自动解释为对应类的对象的值。

#### 2. 赋值表达式

赋值表达式是让指定变量有值的表达式。关于Iris的变量将会在后面介绍。

赋值表达式的形式为：

> 变量名 = 值。  

比如

```
a = 10
```

####  3. 一元表达式

一元表达式指的是对运算符所接受的运算对象只有一个的表达式。在Iris中共有四种一元表达式。

> 1. 逻辑非：表达形式为 !值，比如 !true;
> 2. 按位非：表达形式为 ~值，比如 ~1;
> 3. 取负：表达形式为 –值，比如 –a;
> 4. 取正：表达形式为 +值，比如 +10;

需要注意的是，在*Iris*中，一元表达式的执行都会被转换为运算符操作对象调用对应的运算符名的方法的形式。因此*Iris*的类是可以重载着一元运算符的。

#### 4. 二元表达式

二元表达式指的是运算符所接受的运算对象有两个的表达式。在*Iris*中有21种二元运算符，其中又分为算术运算符、逻辑运算符、关系运算符、位运算符四类。

> 1. 算术运算符：+（加）、-（减）、\*（乘）、/（除）、\*\*（乘方）、%（取模）；
> 2. 逻辑运算符：&&（逻辑与）、||（逻辑或）；
> 3. 关系运算符：>（大于）、>=（大于等于）、<（小于）、<=（小于等于）、==（等于）、!=（不等于）；
> 4. 位运算符：&（按位与）、|（按位或）、^（按位异或）、<<（算术左移）、>>（算术右移）、<<<（逻辑左移）、>>>（逻辑右移）；

二元表达式的形式是：

> 值 操作符 值。

需要注意的是，在*Iris*中，二元表达式的执行都会被转换为运算符操作的左对象调用对应的运算符名的方法的形式，它具有一个参数，这个参数是右对象。*因此Iris的类是可以重写二元运算符的*。

#### 5. 自运算赋值表达式

自运算赋值表达式是赋值表达式和二元表达式结合的糖衣语法，设op为某一二元运算符，则自运算赋值表达式等价于：

> ;temp = left op right  
> ;left = temp

目前*Iris*有12种自运算赋值表达式，分别为：

> +=、-=、*=、/=、%=、&=、|=、^=、<<=、>>=、<<<=、>>>=

#### 6. 索引表达式

索引表达式的形式为：

> 值[索引]。

任何一个重载了[]运算符类的对象都能够使用索引表达式，索引表达式常用于数组和Hash对象，用于依据索引来获取数组对应下标的值或者Hash中键对应的值。
比如：

```
;array = [1, 2, 3, 4] 
;print(array[2])	
```

需要注意的是，在*Iris*中，索引表达式的执行都会被转换为索引运算符操作的主对象调用"[]"方法的形式，它具有一个参数，这个参数是索引对象。**因此 *Iris*的类是可以重载"[]"运算符的**。同时如果是索引表达式的赋值的话，那么调用的就是"[]="方法了，同样的"[]="运算符也可以被重载。

#### 7. 方法调用表达式

方法调用表达式是指对某一个方法进行调用的表达式，方法包括**全局方法、类方法和实例方法**。

*Iris*中任何一个方法都是*Method*类的对象，使用方法调用表达式，事实上会被隐式转换为对这个*Method*类对象的*call*()方法的调用——而从理论上讲*call*()方法又是对 *call*()方法这个对象的*call*()方法的调用……这样子看下去似乎永无止境了，但请放心，*Iris*内部对此做了一些小的调整，因此虽然表面上看起来是这个样子，但实际上并不会无限调用下去。

方法调用表达式的形式为：

> 方法名([参数])。

#### 8. 成员引用表达式

成员引用表达式是指对某个对象的内部公开成员的引用，这包括对象的公开属性以及对象的公开方法。对于*Iris*类的成员访问权限将会在后面介绍。

成员引用表达式的形式为：

> 对象值.成员公开属性/对象值.成员公开方法()。

#### 9. 数组表达式

严格来说，*Iris*的数组表达式也属于字面量，但是这里特别地分出来进行说明。*Iris*的数组是**对普通意义上数组的扩充**，比如*Iris*的数组没有"界"的概念，您可以任意地访问Iris数组的任何一个位置 __（这个位置上如果不存在任何元素，则数组将返回nil，同时将数组长度扩展到您访问的这个位置为止，并全部插入nil）__。关于详细的*Iris*数组内容将在后面介绍。

数组表达式也就是字面定义数组的表达式，其形式为：

> [元素1,元素2,…元素n[,]]。

#### 10. Hash表达式

同样，严格来说，Iris的Hash表达式也属于字面量，这里同样特别地分出来进行说明。*Iris*的*Hash*由键值对构成，通过键能够获得相应的值，**如果不存在则返回nil**。

Hash表达式也就是字面定义Hash的表达式，其形式为：

> {键1=>值1, 键2=>值2,…, 键n=>值n[,]}。

### Iris语言的语句（Statement）

> Iris的语句是对表达式的组合，从而形成具有具体含义的语句。目前*Iris*拥有18种语句，下面将进行介绍。

#### 1. 常语句（Normal Statement）

常语句就是对一个表达式的执行，它的格式为：

> ;表达式。

请注意，*Iris*一个相当大的特点就是每一条常语句都是分号在前。
例：

```
;a = 10
;b = a + 20
;print(a, ",",b)
```

#### 2. if语句（*if Statement*）

*Iris*有**两种if语句**，一种是__用于控制条件执行的，另一种适用于循环的__，分别称为**条件if**和**循环if**。下面分别进行介绍。

##### 2.1. 条件if

条件if有四种表达形式：

> 1. if(条件) { … }
> 2. if(条件) { … } else {…}
> 3. if(条件) { … } elseif(条件1) {…} elseif(条件2) {…} … elseif(条件n)
> 4. if(条件) { … } elseif(条件1) {…} elseif(条件2) {…} … elseif(条件n) { … } else { … }

条件判断由上至下，当满足其中一个条件的时候（if/elseif），就会执行其后紧随的大括号内的一系列语句——两个大括号及其内部的语句被称为块（*Block*）。而如果条件都没有满足，同时存在else语句的话，那么将会执行else后的块。

请注意，Iris中除了false和nil以外都被视作真。

例：

```
;a = 1
if(a == 0) {
    ;print("a = 0")
}
elseif(a == 1) {
    ;print("a = 1")
}
else {
    ;print("a = other")
}
```

##### 2.2 循环if

循环if是*Iris*特有的循环语法。您可能会发现，*Iris*没有while语句，是的，*Iris*移除了while语句，同时加入了可能更加方便使用的便的**循环if**。

Iris的if循环的基本语法如下：

> if(循环基础条件, 循环次数 [, 计次变量]){  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 循环体  
> }

首先，循环基础条件是循环得以进行的基础，*Iris*第一次遇到到循环体的时候，将会检测循环基础条件，如果条件为真，那么就开始执行循环体。

而循环次数可以指定一个具体的数字来控制这个循环的执行次数，但是，请注意，__如果循环次数没有达到但是循环基础条件已经为假了的话，那么并不会继续执行循环__。

计次变量是一个可选的参数，它可以方便地记录下当前循环进行的次数，需要注意的是，计次变量必须是一个局部变量（不能为类变量、实例变量、全局变量等）。

循环的停止条件有如下几个：

> 1. 遇到return语句；
> 2. 遇到break语句；
> 3. 基础循环条件为假；
> 4. 循环次数达到指定值；

以上四点任何一点达到，循环都会停止。

例：

```
if(i <= 3, 4, i) {
    ;print(i)
}
```

上面这个例子将会顺序打印1、2、3，但并不会打印4，因为当i=4的时候，循环基础条件为假，结束循环。

另外，如果您设定的循环次数为小于等于零的数的话，那么if循环将会完全视基础循环条件是否成立而进行，因此您可以这样子实现一个无限循环：

```
if(true, 0) {
    // 循环体
}
```

而以下表达和直接使用if语句效果相同：

```
if(true, 1) {
    // 条件体
}
```

#### 3. switch语句（*switch Statement*）

*Iris*的switch语句和C\+\+的switch语句非常类似，但是更加易用，并且在特性上也有所不同。

switch语句是多路条件选择语句，它可以和条件if语句等价互换，但如果选择项较多的话switch语句还是比较方便的。

switch语句的基本语法如下：

> switch(待比较的表达式) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;when(表达式1, 表达式2,…, &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;表达式n) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;when(表达式n+1, 表达式n+2,…, 表达式m) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//…  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[else {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}]  
> }  

switch语句的执行首先计算待比较的表达式，保存其值，然后对每一个when子语句的表达式列表中对每一个表达式进行计算，如果遇到有一个when子语句的表达式列表中的某个表达式计算结果和待比较的表达式的值相同的话，那么就执行这条when子语句的块。如果全部不满足，且存在else子语句的话，那么将执行else语句中的内容。

需要注意的是Iris的switch语句执行完一个when或者else子语句之后，将会直接结束，而__不会像C\+\+的switch语句一样顺序往下继续执行__。

例：

```
;c = "Hello"
switch (c) {
    when(1, 2) {
        ;print("Number")
    }
    when("Hello") {
        ;print("String")
    }
    else {
        ;print("Other")
    }
}
```

#### 4. for语句（*for Statement*）

for语句是对*Iris*中有范围概念的对象的遍历的一种糖衣循环，它可以用if循环替代。
基本语法为：

1、对数组：

> for(迭代变量in 数组) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> }

例：

```
;array = [1, 2, "array"]
for(I in array){
    ;print(i, "\n")
}
```

上面这个例子将会顺序打印出数组中的所有元素。

2、对*Hash*：

> for((键,值) in Hash) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> }  

例：

```
;hash = {1 => "Kuroneko", 2=> "Kurumi", 3=>10}
for((key, value) in hash) {
    ;print(key, "=>", value)
}
```

上面这个例子将会打印出该Hash的所有键值对。

3、对*Range*对象

> for(值 in Range对象) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 语句  
> }

例：

```
for(i in (0 -> 10]) {
    ;print(I, "\n")
}
```

上面这个例子将会顺序打印出1-10的所有整数。

这里提一下*Range*对象，Range对象是Iris中对范围的抽象，比如"a"-"z",0-9等都可以被称为范围。Iris内置了一些范围，当然，您也可以继承*Range*类自定义一些自己的*Range*。

Iris的*Range*是基于数学的区间概念（离散的区间），比如您可以方便地字面指定一个拥有开闭的区间：

> **(0 -> 100)**：1-99的整数区间；**（左开右开区间）**  
> **("a" -> "z"]**："b"-"z"的字符串区间；**（左开右闭区间）**  
> **["Z" -> "A")**："Z"-"B"的字符串区间；**（左闭右开区间）**  
> **[-100 -> 20]**：-100-20的整数区间；**（左闭右闭区间）**。  

#### 5. break语句（*break Statement*）

break语句用于从循环中跳出。

注意，break语句仅能跳出包含该语句的所有循环的最内层的循环。而如果break语句出现在非循环语句的块中的话，Iris将会**报错**。

例：

```
for(i in [0 -> 10]) {
    if(i == 8) {
        ;break
    } else {
        ;print(i, "\n")
    }
}
```

上面这个例子将会顺序打印0-7的所有整数。

#### 6. continue语句（*continue Statement*）

continue语句用于立即结束本次循环进入下一次循环。

同样的，continue语句仅能结束包含该语句的所有循环的最内层的循环。而如果continue语句出现在非循环语句的块中的话，Iris将会**报错**。

例：

```
for(i in [0->10]) {
    if(i % 2 == 0) {
        ;continue
    }
    ;print(i)
}
```

上面这个例子将会顺序打印0-10中所有的奇数。

#### 7. 方法定义语句（*Method-Define Statement*）

方法定义语句用于在*Iris*中定义方法，而定义的同时将会产生相应的方法（*Method*）对象。至于方法定义在何处需要看定义该方法处于的上下文，在*Iris*中定义方法有三种上下文：**全局上下文、类上下文、模块上下文**。

__全局上下文表示该方法定义在全局，可以直接全局调用__。全局定义的方法无论是实例方法还是类方法，如果名称相同那么会视为同一方法，后定义的将会覆盖前定义的。
类上下文表示该方法定义在某个类中，此时实例方法和类方法将会区分对待。

__模块上下文和类上下文类似，表示该方法定义在某个模块中，此时实例方法和类方法也会区分对待__。

定义实例方法的语句为：

> fun 方法名([参数1, 参数2, … 参数n][, *可变参数]) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 方法体  
> }

定义类方法的语句为：

> fun self.方法名([参数1, 参数2, … 参数n][, *可变参数]) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 方法体  
> }

例：

```
fun fib(a, b, from, to) {
    ;print(b)
    if(from == to) {
        ;return
    }
    ;return fib(b, a + b, from + 1, to)
}
;print(1)
;fib(1, 1, 1, 10)
```

上面这个例子将会递归地顺序打印斐波那契数列的前10项。

类方法的定义将会在介绍类的时候说明。

注意如果定义了可变参数，那么调用方法的时候，属于可变参数的那些实参__将会被打包成为一个数组传给定义的那个可变参数形式参数变量__。

#### 8. return语句（*return Statement*）

return语句用于从块中返回值。在*Iris*中return除了可以在方法中返回值以外，还可以从块中返回值，关于块是*Iris*中比较高级的概念，它与*Iris*的**闭包**息息相关，将会放到后面进行说明。

return将会立即结束当前方法的执行并返回值，如果return是在循环中，那么无论循环深度有多大，所有的循环都会被跳出然后返回值。

例：

```
;array = [0, 1, 2, 3, nil, 4, 5, 6]
fun elem_count_before_nil(array) {
    ;count = 0
    for(i in [0, array.size())) {
        if(array[i] != nil){
            ;count += 1
        }
        else {
            ;return count
        }
    }
}
;print(elem_count_before_nil(array))
```

上面这个例子将会计算数组中nil元素之前的所有元素的个数并打印出来。

请注意，return仅能出现在方法或者块中，如果在其他情况下使用return语句那么Iris将会报错。

#### 9. 类定义语句（*Class-Define Statement*）

类是*Iris*这种面向对象语言中非常重要的组件，*Iris*的一切设计都是基于类的，因此可以说没有类，*Iris*不可能正常运作。

一个类就类似于一件产品的设计图纸，有了这张图纸就可以批量生产出无穷多个实实在在的产品，这一般被称为类的实例或者对象，在*Iris*中一般习惯称其为对象（*Object*）。

定义一个类非常简单：

> class 类名 [extends 父类]  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[involves 模块名1, 模块名2, …, 模块名n]  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[joints 接口名1, 接口名2, …, 接口名n]  
> {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 类体  
> }

*Iris*的继承是单继承，也就是说一个*Iris*类只允许有一个父类——*Iris*的设计屏蔽掉了多继承可能造成的继承混乱问题，如果您没有指定具体的父类，那么默认任何您创建的类的父类都是*Object*类，*Object*类是所有类的父类。

这里简单介绍一下*Iris*的继承体系，*Iris*底层的几个类分别为*Object、Class、Module、Method、Interface*类，它们的继承关系为*Object*类为其他类的父类，__而包括*Object*类在内的所有类的类对象本身又是*Class*类的对象__。——这里可能有些不好理解，简单说明一下。在*Iris*中，类本身也是一个对象，称为类类的对象（*Object of class Class*），下文如果没有做特殊说明，类对象，都是指某个类的类对象。而且类对象本身是可以被引用的，不过类对象永远被保存在常量中（*Iris*中要求类名必须大写字母开头，而以大写字母开头的标识符都是常量），因此保存类对象的容器是不可修改的。

类中允许定义实例方法、类方法、类变量，以及设定访问器等。但是请注意，*Iris*类不允许定义类中类，类中类这种类似命名空间的效用在*Iris*中可以用模块来代替。

类中定义的实例方法，可以为类的对象所调用；而类中定义的类方法，则仅能够由类本身调用。请注意，*Iris*在这里和C\+\+的特性是不一样的，在C\+\+中，类的对象可以调用类的静态成员函数，但是*Iris*中对象是不允许调用类的静态成员函数的。

下面的例子定义了一个分数类用于展示*Iris*的类定义：

```
class Fraction {
    ;gset [@numerator]
    ;gset [@denominator]

    // 构造方法
    fun __format(numerator, denominator) {
        // 约分处理
        ;gcd = Fraction.gcd(numerator, denominator)
        ;@numerator = numerator / gcd
        ;@denominator = denominator / gcd
    }
    
    // 显示
    fun display() {
        ;print(numerator, "/", denominator)
    }
    
    // 重写+方法
    fun +(obj) {
        if(obj.instance_of(Fraction) {
            ;gcd = Fraction.gcd(@denominator, obj.denominator)
            ;lcm = Fraction.lcm(@denominator, obj.denominator)
            ;return Fraction.new(
                @numerator * lcm / @denominator
                + obj.numerator * lcm / @denominator, lcm)
        }
    }
    
    // 最大公约数
    fun self.gcd(a, b) {
        if(a < b) {
            ;t = a
            ;a = b
            ;b = t
        }
        if(t = a % b != 0, 0) {
            ;a = b
            ;b = t
        }
        ;return b
    }
    
    // 最小公倍数
    fun self.lcm(a, b) {
        ;return a * b / gcd(a, b)
    }
}
;fra1 = Fraction.new(1, 2)
;fra2 = Fraction.new(6, 8)
;(fra1 + fra2).show()
```

这个例子将会输出5/4。

*Iris*一个类的对象通过调用类方法*new*来通过类对象生成。

上面的例子中涉及到了访问器，关于访问器将会在后面介绍。另外，在上面的例子中，还出现了构造方法*__format*，构造方法是在生成对象之后对象首先会隐式调用的方法，它是*Iris*完成的调用。这里需要说明的是，*Iris*只有构造方法而没有析构方法，因为*Iris*拥有垃圾回收机制，因此会在恰当的时刻回收所有托管到*Iris*上的垃圾内存，因此不需要析构方法。

#### 10. 模块定义语句（*Module-Define Statement*）

*Iris*的模块在*Iris*中起到了命名空间的作用，一个模块能够定义模块中的模块，以及模块中的类，但模块的功能不仅仅是如此，*Iris*学习了*Ruby*的特征，模块实现了*mix-in*，弥补了因为*Iris*没有采用多继承而造成的一些缺点。

模块中可以定义实例方法、类方法和类变量、常量。一旦一个类包括了一个模块，那么当一个类的对象调用实例方法的时候，那么*Iris*还会在这个模块中搜索是否有这个方法——类变量和类方法同理。

模块需要被包括才能生效，而类和模块都能包括模块，包括的关键字是involve。定义一个模块非常简单：

> module 模块名 [involves 模块名1, 模块名2, …, 模块名n] {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 模块体  
> }

下面定义了一个简单的Math模块。

```
module Math {
    fun self.sqrt(n) {
        // 平方根倒数速算法
        ;threehalfs = 1.5
        ;x2 = n * 0.5
        ;y = n
        ;i = y
        ;i = 0x5f3759df – (i >> 1);
        ;y = i.to_float()
        ;y = y * (threehalfs – x2 * y * y)
        ;return 1.0 / y
    }
}

;print(Math.sqrt(2))
```

这个例子将会返回2的平方根并打印。

#### 11. 接口定义语句（*Interface-Define Statement*）

*Iris*的接口和静态预言的接口不同，*Iris*的接口仅仅作为一种规范的工具存在的。事实上您完全可以不使用接口，但如果您需要一种类扩展时候的规范的话，那么接口也是一种比较好的选择。

*Iris*的接口的功能非常单纯：一个类如果接入了一个接口的话，那么它必须实现接口中声明了的实例方法——并且实现的实例方法不仅要同名，参数个数和类型也要一样（这里的类型一样指的是如果有可变参数那么在实现里面也必须要有可变参数）。

接口中不定义具体方法，它只是一种对类定义的约束手段，以满足"__某一大类必须要实现某某方法以保证统一调用__"这一要求。另外接口是可以继承的，而且接口是多继承。

定义一个接口：

> interface 接口名 [joints 接口名1, 接口名2, …, 接口名n] {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 接口体  
> }

关于接口中的方法的声明在下一节中介绍。

#### 12. 接口方法声明语句（*Interface Function Declare Statement*）

接口方法声明语句用于在接口中声明形式上的方法，类似于C语言中的函数原型。但接口中是不允许定义具体的方法的，前面已经说过了，接口就是一种约束规范。

接口方法声明语句的格式为：

> ;fun 方法名([参数1, 参数2, …, 参数n][, *可变参数])

下面定义一个简单的接口来展示简单的插件系统的一个模拟：

```
interface Plugin {
    ;fun install(env)
    ;fun run(env)
    ;fun uninstall(env)
}

class MyPlugin joints Plugin {
    fun install(env) {
        ;print(env, ", ", "installed!")
    }

    fun run(env) {
        ;print(env, ", ", "run!")
    }

    fun uninstall(env) {
        ;print(env, ", ", "uninstalled!")
    }
}
```

#### 13. Groan语句（*Groan Statement*）

*Iris*具有异常处理机制，异常处理是替代传统错误处理的比较先进的方式。*Iris*的异常处理机制是这样的：

> *Iris*在执行的时候抛出一个异常，那么解释器将会带着这个异常往上回溯，一直到顶层为止，如果遇到异常处理语句，那么*Iris*将这个异常交付给异常处理语句进行相应的处理，否则如果到顶层环境*Iris*将会报错。

Groan语句用于抛出一个异常对象，这个异常对象可以是任何*Iris*的对象。调用方法如下：

> ;groan(表达式)

表达式产生一个对象，Iris解释器将会停止接下来的语句的运行，而带着这个对象沿着调用链往上走，直至遇到一个异常处理语句为止，并将这个对象交给异常处理语句的serve子语句；如果一直到调用链顶层（即*Main*环境）都没有遇到异常处理语句，那么*Iris*将会报错。

#### 14. 异常处理语句（*Exception Handling Statement*）

*Iris*的异常处理语句用于处理由*Groan*语句产生的异常对象，并截获这个对象进行指定的处理。异常处理语句的结构如下：

> order {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 可能出现异常的语句  
> }  
> serve(接受异常对象的变量) {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 异常处理  
> }  
> [ignore {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// 无论是否有异常都必须做的处理  
> }]  

下面结合Groan语句和异常处理语句处理一个只能够接受*Ingeter*参数的方法。

```
fun print_integer(i) {
    if(!i.instance_of(Integer)) {
        ;groan("Error parameter : it must be an integer !\n")
    }
    ;print("Integer is ", I, "\n")
}

order {
    ;print_integer("Hello, Iris!")
}
serve(e) {
    ;print(e)
}
ignore {
    ;print("must do.")
}
```

上述语句将会打印出：

> Error parameter : it must be an integer !  
> must do.

#### 15. 访问器定义语句（*Accessor-Define Statement*）

访问器定义语句用于公开化*Iris*类的实例变量，使其能够通过对象在外部被访问、设置，因为*Iris*的实例变量是强制私有的。这是类似C#的get/set语句的语法。

*Iris*的访问器定义语句有三种：get、set、gset，其中get/set语句又有默认和自定义两种。默认的访问器语句，是*Iris*将会自动为您定义一般行为的访问器，而自定义的访问器将会运行您所自定义的访问器逻辑。gset语句将会为您设定默认的get和set访问器。

> get访问器：  
> 默认：;get \[实例变量\] ()  
> 自定义：get \[实例变量\] () { // 您自己的逻辑 }

> set访问器：  
> 默认：;set \[实例变量\](临时变量)  
> 自定义：set \[实例变量\](临时变量) { // 您自己的逻辑 } 

> gset访问器只有默认形式：  
> ;gset \[实例变量\](临时变量)

访问器定义语句的实质，__是*Iris*自动地在类内部定义"\_\_get_+实例变量名"和"\_\_set_+实例变量名"的方法__，而用户在调用成员访问表达式的时候，*Iris*将会调用这两个方法。

下面是一个例子，类A的对象可以通过成员访问表达式来访问到它的实例变量 *@interger*，并且只能够为 *@interger*赋 *Interger*类型的值，否则会抛出异常。

```
class A {
    ;get [@integer] ()
    set [@interger] (value) {
        if(!value.instance_of(Integer) {
            ;groan("Error parameter : it must be an Integer !")
        }
        ;@integer = value
    }
    
    fun __format() {
        ;@interger = 0
    }
}

;obj = A.new()
;print(obj.integer, "\n")
;obj.interger = 10
;print(obj.interger, "\n")
order {
    ;obj.integer = "Hello, Iris!"
}
serve(e) {
    ;print(e)
}
```

上面的例子会打印

> 0  
> 10  
> Error parameter : it must be an Integer !

#### 16. 方法权限定义语句（*Method Authority Statement*）

方法权限定义语句用于定义类方法、实例方法的访问权限，请注意，在*Iris*中权限定义仅仅是对于方法而言的，因为*Iris*不同于C\+\+没有成员变量的说法。

方法权限定义的关键字有三个：everyone/native/personal，分别对应公开/继承/私有方法，与C\+\+中的public/protected/private类似。

定义权限的语句格式为：

> ;everyone/native/personal \[方法名\]

其中everyone为方法定义了这样的访问行为：

> 对于实例方法，类内部实例方法、对象、子类实例方法内部都能够调用该方法；对于类方法，类内部类方法、类对象都能进行调用。

native为方法定义了这样的访问行为：

> 对于实例方法，类内部实例方法、子类实例方法内部都能够调用该方法；对于类方法，仅类内部类方法能进行调用。

personal为方法定义了这样的访问行为：

> 对于实例方法，仅类内部实例方法可以调用；对于类方法，仅类内部类方法可以调用。

由上面可以看到，native和personal对于类方法而言作用一致。

请注意，必须在定义了方法之后才能定义其权限，否则Iris将因为找不到指定名称的方法而报错。

下面是一个例子，展示了方法的权限定义：

```
class A {
    fun __personal_method() {
        ;print("Personal Method!", "\n")
    }

    fun _native_method() {
        ;print("Native Method!" , "\n")
    }

    fun everyone_method() {
	    ;print("Everyone Method!" , "\n")
    }

    ;personal [__personal_method]
    ;native [_native_method]
    ;everyone [everyone_method]
}

class B extends A {
    fun call_method() {
        ;_native_method()
        ;everyone_method()
    }
}

;obj= B.new()
;obj.call_method()
```

这个例子的结果将会打印出

> Native Method!  
> Everyone Method!

而如果您使用obj对象调用 *\_\_personal_method*()或者 *_native_method*()的时候，*Iris*将会报错。

#### 17. block语句和cast语句（*block Statement and cast Statement*）

block语句和cast语句是配合使用的语句，其中block用于判断调用方法的时候是否存在隐式传入的*Block*，如果有进入with语句块否则进入without语句块；而cast则代表了那个传进来的*Block对象*。

以下例子根据是否有*Block*产生两种行为：

```
fun call_fun() {
    ;print("Before", "\n")
    ;block
    ;print("After", "\n")
}
with {
    ;cast.call("With Block")
}
without {
    ;print("Without Block", "\n")
}

;call_fun() {iterator => [msg] : ;print(msg, "\n")}
;call_fun()
```

上面的例子将会分别打印出：

> Before  
> With Block  
> After  
> Before  
> Without Block  
> After  

block/cast语句事实上是一个语法糖，上述代码可以按照下面这样子实现：

```
fun call_fun(blk) {
    ;print("Before", "\n")
    if(blk) {
        ;blk.call("With Block")
    } else {
        ;print("Without Block", "\n")
    }
    ;print("After", "\n")
}

;call_fun(nil)
;call_fun(Block.new() {iterator => [msg] : ;print(msg, "\n"})
// 或者 ;call_fun(lambda() { iterator => [msg] : ;print(msg, "\n"})
```

顺便一提，上面提到的*lambda*()函数的实现非常简单：

```
module Kernel {
    fun self.lambda() {
        ;return block
    }
    with {
        ;return cast
    }
    without {
        ;groan("No block taken!")
    }
}
```

关于*Block*的用法将会在后面进行介绍。

#### 18. super语句（*super Statement*）

super语句用于子类的实例方法调用同名的父类实例方法，如果父类不存在这个方法那么将会报错。值得一提的是，由于*Iris*的语言性质，所以子类在覆盖父类方法的时候，只需要子类方法的名字和父类方法相同即可，并不要求参数个数要相同。

super语句的格式为：
	

> ;super(参数列表)

注意，这里的参数是传给父类的同名方法的。同时*Iris*中super关键字只能用于实例方法的调用中。
以下例子展示了super语句的用法。

```
class A {
    fun hello(msg) {
        ;print(msg, "from class A!", "\n")
    }
}

class B {
    fun hello(msg) {
        ;super(msg)
        ;print(msg, "from class B!" , "\n")
    }
}

;obj = B.new()
;obj.hello("Hello")
```

以上例子将会打印如下信息：

> Hello from class A!  
> Hello from class B!

### Iris语言的高级用法

#### 1. Iris的上下文环境

在进一步阐述*Iris*的闭包之前，首先需要介绍一下Iris的上下文环境的内容。

上下文环境（Context Environment）指的是任何一段*Iris*代码在执行时所处的环境，该环境包括了该环境下的局部变量、调用者等信息，它类似于*Python*中的栈帧（Frame）的概念。以下简称“上下文”。

在*Iris*中，上下文环境的存在保证了：

> 1. 方法的调用

1. 类、模块、接口的定义
2. 迭代器块的传递
3. 闭包

当*Iris*初始化执行状态的时候，将自动生成一个上下文，此上下文环境称为*Main*上下文，*Main*上下文有时也被称为全局上下文，它代表了*Iris*执行时的最基本的上下文。

换言之，在*Iris*中，所有**不在方法体、类体、模块体、接口体以及闭包中执行的环境统称Main上下文**。

*Iris*的任何局部变量都会被注册到当前的上下文中**（如果在*Main*环境下，实例变量、类变量也会被注册到当前上下文中；而在类、模块的上下文中，实例变量会定义到当前上下文中，类变量会直接定义到类、模块中，而接口中禁止定义任何变量）**。除此之外，上下文还会携带一些其他的解释器执行过程中需要的信息。

*Iris*的上下文切换保证了方法调用与闭包等功能的实现，简单来讲，以下几种情形会存在上下文切换：

> 1. 调用方法的时候；

1. 创建或修改类、模块以及接口的时候；
2. 创建闭包的时候。

分别以以上三种情况进行说明：

##### 1. 调用方法的时候

当方法被调用的时候，**如果调用方处于*Main*环境下、且没有显式的调用者，那么*Iris*认为方法的调用者为一个特殊的*Main*对象**，该对象不对外公开（在实现上事实上仅仅是将调用方设置为空指针）。

考虑以下代码：

```
fun foo(param) {
    // do something
}

;foo(bar)
```

上述代码在*Main*环境注册了一个*foo*方法，之后同样在*Main*环境下调用了*foo*方法，此时调用方为*Main*对象。

在执行这个调用的时候，会从*Main*环境“进入”到*foo*方法内部，此时会出现上下文环境的切换，*Iris*解释器会做这样子的工作：

> 1. **创建一个新的上下文环境，并将该上下文环境的上级环境指定为*Main*环境，当前上下文环境压栈**；

1. 将*bar*作为参数进行压栈；
2. 将新建的上下文环境作为当前的上下文环境，代码执行流转到*foo*方法内部；
3. 在当前上下文环境中注册局部变量*param*，同时将栈顶的*bar*赋值给*param*（如果有多个参数，那么在调用方法时最后压栈的值对应最后一个参数）；
4. 执行*foo*中的代码； 
5. 执行完毕解释器携带最后执行语句所返回的值退出方法，执行流重新转回之前调用方法的地方；
6. 从上下文栈中弹出顶部的上下文环境作为当前的上下文环境。

再考虑以下代码：

```
class A {
    fun foo() {
        // do something
    }
    
    fun foo2() {
        ;foo()  // 2
    }
}

class B {
    fun __format(obj) {
        ;@obj = obj;
    }
    
    fun bar() {
        ;obj.foo2()
        // do something
    }
}

;obj = B.new(A.new())
;obj.bar() // 1
```

此处执行调用的流程和前述代码大同小异，需要注意的是代码中标记的的两个地方：

> 1: 此处存在显式的调用方*obj*，因此切换上下文环境的时候，新的上下文环境将会指定*obj*作为调用方；
> 2: 此处虽然没有显式的调用方，但是由于是在*A*类对象的实例方法内调用另外一个实例方法，因此此处会默认将调用方*@obj*作为调用方。

##### 2. 创建或修改类、模块以及接口的时候

*Iris*在创建类、模块或者接口的时候，也会发生上下文环境的切换，下面以一个*Class*的创建为例。

```
;$flag = false

// 1
class A {
    // 2
    fun a() {
        // do something
    }
    
    // 3
    fun self.b() {
        // do something
    }
    
    if($flag) {
        fun c() {
        }
    }
}
// 4
```

在上面的代码中，一共标注了4个关键地方，下面对这4个地方进行简单说明。

> 1: 在这里*Iris*解释器将会发生上下文切换，此处将会新建一个上下文环境，整个过程和方法调用相同，不同的是这里的上下文环境会被标识为特殊的上下文环境，并且它保存了一个调用方，调用方是*A*这个类对象；
> 2: 在2这个地方，*Iris*解释器通过当前上下文获取到了调用方（*A*类对象），然后向其注册了*a()*这个**实例方法**；
> 3: 在3这个地方，*Iris*解释器通过当前上下文获取到了调用方（*A*类对象），然后向其注册了*b()*这个**类方法**；
> 4: 在4这个地方，类定义结束，上下文环境切换回去。

模块、接口定义时的上下文切换和类定义时的上下文切换类似，此处不再赘述。

##### 3. 创建闭包的时候

创建闭包时的上下文切换的内容放在下一节**Block闭包**中说明，此处从略。

#### 2. Block闭包

闭包是携带变量穿越作用域或者延迟执行一段代码的一种实现手段。在*Iris*中，闭包通过*Block*对象来实现，***Iris*中的任何闭包都是*Block*类的一个对象**。

在*Iris*中一个*Block*对象代表了一组代码，它从表面上看起来和*Method*对象并没有什么区别，*Block*对象比起*Method*对象有一个特殊的功能，那就是**它能够记录创建它时候的上下文环境**，除此之外，*Block*对象还有一个自己的上下文环境。

考虑以下代码：

```
fun get_block() {
    return Block.new() {
        iterator => [msg] :
        ;print(msg)
    }
}

;blk = get_block()
;blk.call("Hello, World!")
```

上面的代码会打印

> Hello, World!

在了解*Block*的时候，需要先了解*Iris*中闭包的写法。

当没有参数要传递给闭包的时候，这个闭包写成：

```
;Block.new() {
    // do something
 }
```

当有参数要传递给闭包的时候，这个闭包写成：

```
;Block.new() {
    iterator => [param1, [param2, ..., *variableParam]]:
    // do something
 }
```

其中*variableParam*表示可变参数，意义与方法中的可变参数相同。

在示例代码中，当我们调用*get_block*方法的时候，将会返回一个*Block*对象，然后我们对这个对象调用*call*方法并传递了一个参数，此时就完成了这个*Block*对象的调用。

在创建*Block*对象（调用```Block.new()```）的时候，会完成这样一些工作：

> 1. 和调用普通方法一样，创建一个新的上下文环境并进行其他相同的操作，然后根据后面的*Block*块创建一个临时的*InnerBlock*（不对外公开）对象，这个对象保存了新的上下文环境的上级上下文环境的引用（即创建*Block*对象时的上下文环境），并将其保存在新的上下文中，然后调用```Block.new()```方法；

1. ```Block.new()```方法中通过当前上下文环境获取到*InnerBlock*对象，并通过这个*InnerBlock*对象获取到创建当前*Block*对象时的上下文环境以及块中的代码；
2. **当前的*Block*对象新建一个自己的上下文环境，并将创建当前*Block*对象时上下文环境作为自己的上下文环境的上级上下文环境**。
3. 返回对象。

当*Block*对象调用自己的*call*实例方法的时候，将会把传递给*call*的参数原封不动地传给定义块的时候定义的参数，这个过程和调用方法并没有什么区别。

下面来解释*Block*实现闭包的原理。

首先，需要注意到上面关于创建*Block*对象时提到的**第3点**，一个*Block*对象内部事实上是保存了这样一个**上下文引用链**：

> *Block*对象自己的上下文环境  
>  <- *Block*对象定义时的上下文环境
>  <- *Block*对象定义时的上下文环境的上级上下文环境
>  <- ...
>  <- *Main*环境

而由于*Block*对象有了这么一条**上下文引用链**，因此它能够**逐层地去搜索变量**，从而达到携带变量穿透定义域的功能。

考虑以下代码：

```
;a = 1
;blk = Block.new() {
    ;b = 2
    ;c = 3
    ;return Block.new() {
        ;c = 4
        ;print(String.format("a={0},b={1},c={2}", a, b, c))
    }
 }
;blk2 = blk.call()
;blk2.call()
```

以上代码将会打印：

> a=1,b=2,c=4

可见，最里层的*Block*（即*blk2*）通过**上下文引用链**查找到了整条链上的所有变量，并且注意到由于在*blk*自身的块中定义了变量```;c=4```，因此外层的```;c=3```并没有被引用到。

进一步地，这里说明一下如果在*Block*中引用了一个变量，那么这个变量会视情况进行定义：

> 1. 变量为局部变量的情形
>    *Iris*解释器将会顺着*上下文引用链进行查找*，如果找到该局部变量，那么直接引用之；**否则在*Block*对象自己的上下文环境中定义之**；

1. 变量为实例变量的情形
   同样的，*Iris*解释器将会顺着*上下文引用链进行查找*，此时查找的方法是如果某个上下文环境中拥有调用者（该情况的出现一般是由于该*Block*是由某个对象的实例方法生成并返回的），那么将会在这个调用者对象中去查找实例变量，而如果该上下文环境中没有调用者那么按照一般情况查找，如果查找到，则引用之；**否则在*Block*对象自己的上下文环境中定义之**；
2. 变量为类变量的情形
   同样的，*Iris*解释器将会顺着*上下文引用链进行查找*，此时查找的方法是如果某个上下文环境中拥有调用者（该情况的出现一般是由于该*Block*是由某个对象的实例方法生成并返回的），那么将会在这个调用者对象所属的类/模块中去查找类变量，而如果该上下文环境中没有调用者那么按照一般情况查找，如果查找到，则引用之；**否则在*Block*对象自己的上下文环境中定义之**；
3. 变量为全局变量的情形
   按照一般情况处理。

此外还有**常量**的查询，常量的查询和**类变量的查询基本一致，唯一的区别在于如果查找不到常量则*Iris*解释器直接报错，*Iris*禁止在闭包中定义常量**。

了解完*Block*对象，现在来看看*Iris*的**迭代器块**的写法。

*Iris*的迭代器块指的是在调用某个方法的时候，在圆括号后再跟一个块，里面编写代码；这个块将会作为一个*Block*对象传进这个方法中，而在方法中可以通过*cast*关键字引用到该*Block*对象。

考虑以下代码：

```
fun call_fun() {
    ;print("Before", "\n")
    if(cast) {
        ;cast.call("With Block")
    } else {
        ;print("Without Block", "\n")
    }
    ;print("After", "\n")
}

;call_fun()
;call_fun() {
    ;print(msg, "\n"}
 }
```

以上代码将会打印：

> Before
> Without Block
> After
> Before
> With Block
> After

可见，如果不给该方法编写块的话，那么*cast*将为*nil*。同时，如果需要根据有没有传入块分别进行处理，那么可以使用*with/without*语法：

```
fun call_fun() {
    ;print("Before", "\n")
    ;block
    ;print("After", "\n")
}
with {
    ;cast.call("With Block")
}
without {
    ;print(msg, "\n"}
}
```

其中*block*是一个占位符，调用时根据是否传入*Block*而分别执行*with*和*without*块中的代码。

由于*Iris*的*Block*对象类似函数式语言中的*lamba*表达式，因此*Iris*中提供了一个*lambda*方法，如果用*Iris*语言实现的话大概是下面这样（事实上是在*Kernel*中实现的内置方法）：

```
fun lambda() {
   if(cast) {
        ;return cast
   }
   ;groan(Error.new("No block taken"))
}
```

使用的方法如下：

```
;printer = lambda() { [msg]: ;print(msg) }
;adder = lambda() { [a, b]: ;return a + b }

;printer.call(adder.call(1, 2))
```

以上代码将会打印：

> 3
> 

### 关于Iris的细节说明

#### 1. 常量

_Iris_中的常量约定以大写字母开头进行表示，亦即：**凡是以大写英文字母开头的标识符都代表一个常量**。此外，常量一旦定义则不能被改变，**重新为已经定义的常量赋值会导致Iris解释器抛出运行时异常**。

只能在三种情况下定义常量：

> 1. 全局环境下；
> 2. 类体中；
> 3. 模块体中。

除此之外在任何地方定义常量都会被视为非法行为，**Iris解释器将会抛出运行时异常**。

定义常量的位置决定了常量的所属，以下是一个例子：

```
;TOP_CONST = "Top Const"
module TestModule {
    ;MODULE_CONST = "Module Const"
    class TestClass {
        ;CLASS_CONST = "Class Const"
        
        fun self.print_const() {
            ;print(CLASS_CONST, "\n")
            ;print(MODULE_CONST, "\n")
            ;print(TOP_CONST, "\n")
        }
    }
    
    fun self.print_const() {
        ;print(MODULE_CONST, "\n")
        ;print(TOP_CONST, "\n")
    }
}

fun main_print() {
    ;print(TOP_CONST, "\n")
}

;print(TOP_CONST, "\n")
;print("\n")
;main_print()
;print("\n")
;TestModule.print_const()
;print("\n")
;TestModule::TestClass.print_const()
```

以上代码将会打印出：

> Top Const
>
> Top Const
> Module Const
>
> Top Const
> Module Const
> Class Const

##### 补充1：Iris中常量的查找顺序

*Iris*中的常量按照如下顺序查找：

> 1. 如果当前上下文为**运行时上下文**，那么查找顺序为当前对象的类、当前对象的类所包含的模块、当前类的父类、当前类的父类所包含的模块……一直上升到全局环境下查找，如果中途查找到，则返回之，否则报错；（如果当前环境已经是全局环境，那么该常量将被添加到全局环境中）
> 2. 如果当前上下文为**模块定义上下文**，那么查找顺序为当前模块、当前模块的上层模块……一直上升到全局环境下查找，如果中途查找到，则返回之，否则往该模块添加此常量，常量值为*nil*；
> 3. 如果当前上下文为**类定义上下文**，那么查找顺序为当前类、当前对象的类所包含的模块、当前类的父类、当前类的父类所包含的模块……一直上升到全局环境下查找，如果中途查找到，则返回之，否则往该类添加此常量，常量值为*nil*；

##### 补充2：Iris中类、模块以及接口的定义与常量的关系

*Iris*中规定类、模块以及接口名必须以大写字母开头，类似以下的定义将会导致*Iris*解释器抛出运行时错误：

```
class test_class {}
module $test_module {}
interface @interface {}
```

之所以这样子规定，是因为***Iris*中所有的类、模块以及接口都是以常量的形式存在于各自的环境中**，换言之，这些类、模块以及接口对象都能够按照常量的方式来引用。

> 注：*Iris*秉承“Everything is Object”的理念实现了面向对象模型，这也直接导致了*Iris*中的常量有这样子的特性，具体请移步至《Iris面向对象模型介绍》进行深入了解。

考虑如下代码：

```
module A {
    module B {
        class C {
        }
        
        interface D {
        }
    }
    
    class C {
    }
}

;print(A.__constance.keys.include?("B"), "\n")
;print(A.__constance.keys.include?("C"), "\n")
;print(A::B.__constance.keys.include?("C"), "\n")
;print(A::B.__constance.keys.include?("D"), "\n")
```

以上代码会打印出：

> true
> true
> true
> true

可见，定义模块、类以及接口事实上就是在往相应的环境里添加常量对象（__Class类对象、Module类对象、Interface类对象__）的过程，以上代码完全可以用以下元编程的方法来实现：

```
;A = Module.new() // 全局环境中
;A.add_constance("B", Module.new()).add_constance("C", Class.new())
;A.constance("B").add_constance("C", Class.new()).add_constance("D", Interface.new())

;print(A.__constance.keys.include?("B"), "\n")
;print(A.__constance.keys.include?("C"), "\n")
;print(A::B.__constance.keys.include?("C"), "\n")
;print(A::B.__constance.keys.include?("D"), "\n")
```

以上代码会打印出：

> true
> true
> true
> true

#### 2. 变量

*Iris*有以下几种变量：

> 1. 局部变量；
> 2. 实例变量；
> 3. 类变量；
> 4. 全局变量。

关于**实例变量**以及**类变量**的说明在《Iris面向对象模型》第5节中有所提及，这里结合局部变量进行更进一步的说明。

##### 2.1. 局部变量

*Iris*中的局部变量是指**以小写字母开头的合法标识符命名的变量**。局部变量的作用域为：**全局环境、方法内部、闭包**。

> 注意：*Iris*禁止在类体、模块体以及接口体内定义局部变量。

在全局环境下定义的局部变量将会被注册到全局环境中，在方法内部定义的局部变量将只会在方法执行的过程中被定义到当前方法的执行上下文中使用，而闭包下的局部变量的行为就比较特殊，这里放到4.3节来进行深入介绍。

**除闭包情况以外，在任何环境下定义的局部变量能且仅能在同一环境下被访问到**，考虑如下代码：

```
;a = 10
fun my_print() {
    ;print(a)
}

;print(a, "\n")
;my_print()
```

以上代码将会打印出：

> 10
> nil

注意到*my_print*()方法内部是无法访问到定义在全局环境中的局部变量*a*的，当*my_print*()方法调用的时候，它的执行上下文被切换到了一个新的上下文中，而在这个新的上下文中并没有*a*存在，因此*Iris*自动定义*a*为*nil*并注册到该环境中。

##### 2.2. 实例变量

*Iris*中的实例变量是指**以@开头的合法标识符命名的变量**。实例变量的作用域为：**全局环境、对象内部、闭包**。

实例变量在全局环境中的行为与局部变量一致，而关于闭包中的实例变量同样放到4.3节中来进行详细阐述。

实例变量是**一个实例专有的变量**，同一个类的不同的实例即便变量名同名，值也可能是不同的。在该实例的所有实例方法中，都可以引用到该对象的实例变量。

下面是一个例子：

```
class A {
    fun __format(param) {
        ;@param = param
    }
    
    fun my_print() {
        ;print("@a", "\n")
    }
}

;obj1 = A.new("Hahaha")
;obj2 = A.new("Nonono")
;obj1.my_print()
;obj2.my_print()
```

以上代码将会打印：

> Hahaha
> Nonono

实例变量的定义发生在**在对象第一次引用它的时候**。

那么，如果在类方法中定义实例变量是怎么样的行为呢？由于*Iris*规定**在类或者模块中定义的类方法事实上就是该类对象或者该模块对象的实例方法**，因此，类方法中定义的实例变量将会成为**该类对象或者该模块对象的实例变量**。

以下是一个例子，由于*getter/setter*语句仅用于类对象的实例变量，因此这里采用元编程的方式取出模块对象的实例变量。

```
module TestModule {
    fun self.my_define_instance_variable() {
        ;@value = "Hello"
    }
}

;print(TestModule.__instance_variables.include?("@value"), "\n")
;TestModule.my_define_instance_variable()
;print(TestModule.instance_variable("@value"))
```

以上代码将会打印：

> false
> Hello

##### 2.3. 类变量

*Iris*中的类变量是指**以@@开头的合法标识符命名的变量**。实例变量的作用域为：**全局环境、对象内部、闭包**。

同样，全局环境下类变量的行为与局部变量相同，而关于闭包中的类变量同样放到4.3节中来进行详细阐述。

类变量是一个类的实例共享的变量，严格来说类变量不符合*Iris*面向对象模型的要求，类变量完全可以用其他方式模拟出来，类变量仅仅是语法糖一般的存在。

以一段代码来解释类变量的行为：

```
class TestClass {
    ;@@value = 0
    
    fun increase() {
        ;@@value += 1
    }
    
    fun print_class_variable() {
        ;print(@@value, "\n")
    }
}

;obj1 = TestClass.new()
;obj2 = TestClass.new()
;obj1.print_class_variable()
;obj2.increase()
;obj2.print_class_variable()
```

以上代码会打印出：

> 0
> 1

类变量的查找顺序类似常量，在此不再赘述。

##### 2.4. 全局变量

*Iris*中的类变量是指**以$开头的合法标识符命名的变量**。实例变量的作用域为：**任何作用域**。

*Iris*的全局变量无论在何处被定义，都会自动注册到全局环境中，而在任何地方引用，也都会在全局环境中查找。

#### 3. 类、模块与接口

这部分内容请参考《Iris面向对象模型》。

## Iris 1.1

### Iris的Lambda表达式和注解

> 在*Iris* 1.1中主要为*Iris*语言增加了两项语言特性：
>
> 1. Lambda表达式：便于函数式风格的程序设计；
> 2. 注解：提供安全的动态类型检查以及可定制的语言扩展；

#### 1、Lambda表达式

##### 1.1 Lambda表达式的语法

*Iris*的*Lambda*表达式其实是生成*Block*对象的语法糖，提供类似于函数语言中的*Lambda*表达式一样的语法来简化*Block*对象的生成。

*Iris*中*Lambda*表达式的语法如下：

> ([参数列表]) => {[*Lambda*表达式要执行的代码块]}

以下的代码示例都是合法的*Iris**Lambda*表达式：

```clike
() => { ;print("Hello, World") }
(param) => { ;print(param) }
(arg, *params) => { 
	;print(arg, '\n')
	;params.each() { iterator => [e] : ;print(e, '\n') }
}
```

*Lambda*表达式的参数列表和*Iris*中方法、*Block*对象的参数列表意义完全相同。

同时，由于***Lambda*表达式是对生成*Block*对象的简化**，因此上述示例代码事实上等同于下述代码：

```clike
Block.new() { ;print("Hello, World") }
Block.new() { iterator => [param] : ;print(param)}
Block.new() {
    iterator => [arg, *params] :
	;print(arg, '\n')
    ;params.each() {iterator => [e] : ;print(e, '\n') }
}
```

因此，当我们要调用*Lambda*表达式的时候，我们要按照*Block*对象的方式来进行调用，下面是一个完整的示例：

```clike
;max = (a, b) => { ;return a > b ? a : b }
;i = 100
;j = 200
;print(max.call(a, b))
```

上述代码执行后将会打印

> 200

##### 1.2 Iris Kernel模块中的lambda方法

在*Iris* 1.1中，*Kernel*模块也提供了一个名字叫做*lambda*的方法用于创建*Block*对象，它的实现代码很简单：

```clike
fun lambda() {
    ;return block
}
with {
	;return cast
}
without {
	;groan("No block taken!")
}
```

使用的时候如下：

```
;max = lambda() { iterator => [a, b] : ;return a > b ? a : b }
```

*Iris*建议使用*Lambda*表达式来创建闭包对象。

##### 1.3 Iris中的self关键字的行为

在*Iris* 1.1之前对于各个环境下*self*关键字的行为一直是未定义的，为了避免*Iris*中的*self*关键字出现*Javascript*中*this*关键字那样令人迷惑的效果，在这里对*self*关键字的行为作出唯一的规定：

> 1. *self*关键字**只允许出现在方法体或者*Block*中**；
> 2. 当*self*关键字出现在方法体中时，如果不存在调用该方法的对象，那么*self*的使用将会被视为非法调用，解释器将会抛出错误；如果存在调用该方法的对象，那么*self*就是这个对象；
> 3. 当*self*关键字出现在*Block*中时，*self*将会沿着闭包链一直往上查找，直至查到第一个**不是闭包环境的环境**，此时这个环境中如果存在调用对象，那么*self*便是这个环境中的调用对象，否则，*self*的使用将会被视为非法调用，解释器将会抛出错误。

针对以上几个情况给出几个例子：

```clike
;obj = self // 1

fun foo() {
  	;return self  // 2
}

;f = () => { ;print(self.name) }

class Test {
  	;gset [@name]
    
    fun __format(f) {
      	;@f = f
        ;@name = "Hello"
    }
  
  	fun call_block() {
      	;@f.call()
  	}
}

;obj = Test.new(f)
;obj.call_block()  // 3
```

在上述代码中：

> 1 处将会在编译期就报错，因为*self*必须存在于方法体或者*Block*中；  
>
> 2 处如果直接调用**foo()**的话，也会报错，因为没有具体的调用对象；  
>
> 3 处将的*obj*对象的实例变量*@f*是一个内部定义了*self*关键字的*Block*对象，当这里调用该*Block*对象的时候，*self*将会向上回溯查询，最终查询到第一个调用者*obj*，于是*self*指向*obj*，此处打印出**Hello**。

#### 2、注解

##### 2.1 注解的作用

在*Java*、*C#*这样子的语言中都存在**注解**这样子的元数据，它提供了代码级别的说明，能够对代码中的类、方法等元素进行说明、注释，提供辅助文档编写、代码分析以及提供更多编译信息的功能，并且这些注解都是可以由用户自定义的。

 在*Iris* 1.1中，也增加了名为**注解（*Annotation*）**的语法，*Iris*中的注解主要是用来为**任何一个*Iris*对象做额外的预先标注、处理工作，以在动态执行的过程中完成代码层面以外的额外工作**。

*Iris*中的注解的应用场景有且不限于以下几点：

> 1. 参数类型检查
> 2. 入口标注
> 3. 预处理

此外，*Iris*的注解语法可能会在未来的版本中发生新增、变化，且尽量保证向下兼容。

##### 2.2 注解的语法

*Iris*中的注解需要放在一个**表达式、类、模块、接口、方法、块**的前方，且中间不能有别的其他元素。

*Iris*中一个合法的注解形式为：

```
#注解名([参数列表])
[表达式|类定义语句|模块定义语句|接口定义语句|方法定义语句|块语句]
```

***Iris*中任何一个注解都是*Annotation*类及其子类的一个对象**，它将会作用于被它所注解的那个对象身上，作用时机为**被注解的那个表达式、语句执行完毕**的时候。

下面以*Iris*内置的名为***TypeCheck***的注解为例，来直观地说明一下*Iris*注解的作用。

```clike
fun print_if_string(obj) {
	;print(
		#TypeCheck(String)
		obj
	 )
}

;print_if_string("Hello, World!") // Correct
;print_if_string(1) // Wrong type!
```

上面的例子等价于：

```clike
fun check_type(obj, tar_class) {
	if(obj.__class == tar_class) {
		;return obj
	}
	;groan("Wrong type!")
}

fun print_if_string(obj)}
	;print(check_type(obj, String))
}
```

可以看到，虽然可以定义方法来进行相同的类型检查工作，但是**注解使得这一工作更加一目了然**。

需要注意的是，在上述例子中*TypeCheck*看起来似乎是作用到了*obj*这个对象身上了，但实际并不然，这其中有这样一个过程：

> 1. ```obj```是一个表达式，它**返回*obj*这个实例变量中的对象**；
> 2. 表达式执行完毕，注解*TypeCheck*生成一个实例并作用在这个返回的对象身上。

##### 2.3 注解的实质

注解的实质是*Iris*编译器在进行编译的过程中，编译器扫描到注解的时候，会在当前的**表达式、类、模块、接口、方法、块**后方自动插入一些语句，以上面的**TypeCheck**的例子来说，编译器事实上是插入了这样一段代码：

```clike
fun print_if_string(obj) {
	;print(
		;TypeCheck.new().apply(obj, String)
	 )
}
```

根据注解的对象的不同，注解的作用时间也不一样：

> 1. 如果是表达式，那么会在表达式返回值的时候作用；
> 2. 如果是类、模块、接口，那么会在该类、模块、接口定义完毕的时候作用；
> 3. 如果是方法，那么会在方法被定义后、添加到其对应的环境中之前作用；
> 4. 如果是块，那么会在块被定义之后作用；

其次，每一个注解事实上都是***Annotation*类及其子类的一个对象**，上述**TypeCheck**这个注解事实上也是如此，它的定义类似下面这样：

```clike
class TypeCheck extends Annotation {
	fun __format() {
      	;super()
	}
  	
  	fun parameter_define() {
      	;return [Class]
  	}
  
  	fun apply(obj, *params) {
      	if(obj.__class != params[0]) {
          	;groan("Error Type!")
      	}
      	;return obj
  	}
}
```

一般来说，一个可用的注解类应该重写*parameter_define*以及*apply*方法，这两个方法的签名如下：

```clike
fun parameter_define();
fun apply(obj, *params);
```

其中*parameter_define*用于定义*apply*方法*params*这个可变参数所能接受的参数类型，这个方法需要返回一个数组，其中按顺序存放要传入注解*params*中的各个参数的类型（比如上面```#TypeCheck(String)```这行注解传入到*apply*的*params*中的就是*[String]*）；如果参数可变或者需要用户自行控制，那么直接返回*nil*即可。

而*apply*是**将注解应用到目标对象身上的关键方法**，它的第一个参数就是被注释的对象，第二个参数就是在注解中传入的参数。

##### 2.4 注解作用示例

> 下面以不同对象为例，说明注解是如何作用在不同对象身上的

###### 2.4.1 注解作用在表达式上

当注解用于表达式上的时候，它只会**对最接近它的那个表达式的返回值作用**。

考虑以下代码：

```clike
;#TypeCheck(<ClassName>)
 xxx().yyy().zzz()

;(#TypeCheck(<ClassName>)
  xxx()).yyy().zzz()
/*
也可以写成
;(#TypeCheck(<ClassName> xxx()).yyy().zzz()
*/

;(#TypeCheck(<ClassName>)
  xxx().yyy()).zzz()
/*
也可以写成
;(#TypeCheck(<ClassName>) xxx().yyy()).zzz()
*/
```

上面三个注解的作用对象并不相同：

1. 第一个注解的作用对象是表达式```xxx().yyy().zzz()```的整体返回值；
2. 第二个注解的作用对象是表达式```xxx()```的返回值；
3. 第三个注解的作用对象是表达式```xxx().yyy()```的整体返回值；

再次强调，**注解只能作用于表达式（Expression）上，而不能作用于语句（Statement）**，所以如果上述代码写成：

```clike
#TypeCheck(<ClassName>)
;xxx().yyy().zzz()
```

那么编译器将会直接报错，因为```;xxx().yyy().zzz()```是一个常语句。

###### 2.4.2 注解作用在类、模块、接口上

由于注解作用在类、模块、接口上的逻辑是一致的，所以这里放在一起说明。

作用在类、模块、接口上的注解将会在类、模块、接口定义完毕之后作用。下面我们自定义一个注解，用来检查该类的所有实例方法的名字是否都以小写字母开头。

```
class CheckInstanceMethodName extends Annotation {
	fun __format() {
      	;super()
	}
  	
  	fun parameter_define() {
      	;return nil
  	}
  
  	fun apply(obj, *params) {
      	;regex = Regex.new(@"^[a-z].*")
      	;(#TypeCheck(Class) obj).__own_instance_methods.each() {
			iterator => [m] :
			if(!regex.match(m.name)) {
				;print('Wrong method name : ', m.name, '\n')
			}
      	}
      	
      	return obj
  	}
}
```

然后我们定义类：

```
#CheckInstanceMethodName()
class TestClass {
	fun Test() {}
	fun test() {}
}
```

这个示例将会打印：

> Wrong method name : Test

###### 2.4.3 注解作用在方法、块上

当注解作用在方法或块上时，可以控制方法或块的作用时机。

> 1. 调用方法或块前
> 2. 调用方法或块后
> 3. 定义方法或块时

除3以外，作用时机的控制的关键在于*Method*（*Block*）对象的两个属性：\_\_pre_call/\_\_post_call。

其中：

> *__pre_call*接受签名为 (obj, \*params) => {} 的*Block*对象，其中*obj*表示调用该方法的对象，*params*表示调用该方法时传入的参数；
>
> *__post_call*接受签名为(result) => {} 的*Block*对象，其中*result*表示该方法执行完毕的返回值。

下面以定义方法的注解为例，举一个具体的例子来说明这三种作用时机。在下面的例子中，我们定义一个*StartWithLowerCase*注解来检查被注解的方法是否是以小写字母开头，如果是，则打印出该方法的名字；然后我们再定义一个*ParameterCheck*注解来在方法被调用前检查传入的参数是否为指定类型（实现类似静态语言类型检查的功能）；最后，我们定义一个*ConvertResultToString*注解来把被注解的方法的返回值都转换成*String*。

```clike
class StartWithLowerCase extends Annotation {
	fun __format() {
      	;super()
	}
  
    fun parameter_define() {
      	;return nil
  	}
  
  	fun apply(obj, *params) {
      	if(['a'=>'z'].include(obj.name[0])) {
          	;print(obj.name, "\n")
      	}
      	;return obj
  	}
}

class ParameterCheck extends Annotation {
  	fun __format() {
      	;super()
  	}
  
    fun parameter_define() {
      	;return nil
  	}
  	
  	fun apply(obj, *params) {
		;obj.__pre_call = (obj, *rparams) => {
          	if(!all_extends(rparams, params)) {
              	;groan("Invaild parameter list.\n")
          	}
		 }
      	;return obj
    }
  
  	fun all_extends(objs, classes) {
      	if(objs.size != classes.size) {
          	;return false
      	}
      	;i = 0
        if(true, i < objs.size) {
          	if(!objs[i].__instance_of(classes[i])) {
              	;return false
          	}
        }
      	;return true
  	}
}

class ConvertResultToString extends Annotation {
  	fun __format() {
      	;super()
  	}
  
    fun parameter_define() {
      	;return []
  	}
  
  	fun apply(obj, *params) {
		;obj.__post_call = (result) => {
          	;return result.to_string()
		 }
    }
}
```

然后编写我们的测试类：

```clike
class TestClass {
    #StartWithLowerCase()
  	fun foo1() {}
  	
  	#StartWithLowerCase()
  	fun Foo2() {}
  
  	#ParameterCheck(String, String)
  	fun foo3(p1, p2) {
      	;print("Hello, ", p1, " ", p2, '\n')
  	}
  
  	#ConvertResultToString()
	fun foo4() {
      	;return true
	}
}

;obj = TestClass.new()
;obj.foo3("One", "Two")
;print(obj.foo4())
;obj.foo3(1, 2)
```

执行代码后，将会打印：

> foo1
>
> Hello, One Two
>
> true
>
> Invaild parameter list

##### 2.5 注解作用的顺序

同一个对象可以作用多个注解，注解的作用顺序为：**离作用对象最近的那个注解先作用，然后再以此往前作用**。

比如，如果有3个注解*A、B、C*，作用在一个表达式上：

```clike
#C()
#B()
#A()
expression
```

那么执行顺序将会是：

```clike
C.new().apply(B.new().apply(A.new().apply(expression)))
```

事实上上述代码与下面的代码等价：

```clike
#C() (#B() (#A() expression))
```