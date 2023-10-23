C++入门
=======

由于本框架采用C++和基于C++的SFML图形库，所以如果您需要进一步进行修改和创作，就需要了解下述内容。

1. 数字变量的类型
~~~~~~~~~~~~~~~~

C++中数字常用变量类型分为 ``short`` , ``int`` , ``long`` , ``long long`` , ``float`` , ``double``。

``short`` 是短整型变量，范围为 ``-32768~+32767`` ， ``int`` 为整形变量，范围为 ``-2147483648~2147483647`` ， ``long`` 为32位长整型变量，实际上为 ``long int`` ，在C++中， ``long`` 和 ``int`` 范围相同， ``long long`` 为64位长整型数据，范围为 ``-9223372036854774808~9223372036854774807`` 。

``float`` 为浮点型变量，范围为 ``-3.4E38~3.4E38`` ， ``double`` 为双浮点类型变量，范围为 ``-1.7E308~1.7E308`` ，这两种类型支持小数点的计算。

如果在前面加上 ``unsigned`` ，范围则会从0开始，上限增加一倍。

2. 布尔类型
~~~~~~~~~~

在C++中，布尔类型变量（bool）的取值为 ``true`` 和 ``false`` ，但是， ``true`` 可以在计算中等价转换为 ``1`` ， ``false`` 在计算中可以等价转换为 ``0`` 。

所以可以延申处下面的小技巧，例如bool变量 ``a`` 为 ``true`` 时， ``b`` 会增加3，否则不增加，可以直接写为： ``b = b + a * 3;`` 

3. 字符串
~~~~~~~~~

字符串的类型为 ``string`` ，不同的字符串之间可以用 ``+`` 连接。

3.1. 宽字符串
-------------

需要注意，因为编码的缘故，中文/汉字/全角字符在直接使用 ``string`` 输出到屏幕上时会变成乱码，此时需要引进 **宽字符** 来表示中文字符，工程内提供了一个函数 ``str2wstr`` 来进行转换，不过一般不用担心，需要显示中文的地方（如 ``drawText`` 函数）内部都对输入的 ``string`` 类型变量进行了转换，您只需要放心用 ``string`` 即可。

3.2. std::to_string(value)
-------------------------

``to_string`` 是 **C++11** 引入的标准，可以将数字转换成字符串类型，需要使用 ``<string>`` 头文件。

3.3. stoi(str), stof(str)
--------------------------

``stoi`` 和 ``stof`` 可以将字符串转换成整型、浮点型变量，需要使用 ``<string>`` 头文件。

类似的函数还有如下几种：

``stoi`` ：将字符串转换为 ``int`` 类型。

``stol`` ：将字符串转换为 ``long`` 类型。

``stoll`` ：将字符串转换为 ``long long`` 类型。

``stoui`` ：将字符串转换为 ``unsigned int`` 类型。

``stoull`` ：将字符串转换为 ``unsigned long long`` 类型。

3.4 std::format(fmt, ...args)
------------------------------

``format`` 是 **C++20** 引入的标准，用以格式化字符串，需要使用 ``<format>`` 头文件和 ``using namespace std;`` 。

下面是 ``format`` 的一个示例：

.. code-block:: cpp
    :linenos:

    int a = 3, b = 4;
    std::cout << std::format("a={},b={}", a, b) << endl;

可以注意到， ``format`` 和 ``printf``有部分相似之处，即格式化输出，不过在 ``printf`` 中的例如 ``%d`` 等内容，都被换成了 ``{}`` ，然后在后面逐个表示变量即可，一般数字和字符串不需要再进行调整。

4. 条件分歧
~~~~~~~~~~~

4.1. 真伪值
----------

在C++中，因为历史原因（最初C语言标准并没有bool类型），在条件分歧等判断中， **真** 实际上是 **非零** ， **假** 实际上是 **零** 。

4.2. 三目运算符
-------------

使用符号 ``?`` 和 ``:`` 可以在同一行内进行条件分歧。

下面是一个示例：

.. code-block:: cpp
    :linenos:

    int a = 3;
    std::cout << (a > 10 ? "big" : "small") << endl;

这个示例的意思是， 输出时如果 ``a`` 大于10，则输出 ``big`` ，否则输出 ``small`` 。

5. Lambda表达式
~~~~~~~~~~~~~~~

Labmda表达式是C++11引入的标准，一般用于定义匿名函数，使得代码更加灵活简洁，最常见的Lambda表达式如下所示：

.. code-block:: cpp
    :linenos:

    auto plus = [] (int v1, int v2) -> int { return v1 + v2; }
    int sum = plus(1, 2);

在写比如自定义排序时，往常可能需要写一个 ``cmp`` 函数，但是这种只在特定范围调用的函数可以用Lambda表达式来写，比如如下示例：

.. code-block:: cpp
    :linenos:

    struct Item
    {
        Item(int aa, int bb) : a(aa), b(bb) {} 
        int a;
        int b;
    };
        
    int main()
    {
        std::vector<Item> vec;
        vec.push_back(Item(1, 19));
        vec.push_back(Item(10, 3));
        vec.push_back(Item(3, 7));
        vec.push_back(Item(8, 12));
        vec.push_back(Item(2, 1));

        // 根据Item中成员a升序排序
        std::sort(vec.begin(), vec.end(),
            [] (const Item& v1, const Item& v2) { return v1.a < v2.a; });

        // 打印vec中的item成员
        std::for_each(vec.begin(), vec.end(),
            [] (const Item& item) { std::cout << item.a << " " << item.b << std::endl; });
        return 0;
    }

5.1. Lambda表达式写法
--------------------

Lambda表达式有如下三种写法：

.. code-block:: cpp
    :linenos:

    [captures]<tparams>(params) lambda-specifiers {body};
    [captures](params) lambda-specifiers {body};
    [captures](params) {body};

5.2. captures
-------------

``captures`` 是捕获列表，可以把上下文变量以值或引用的方式捕获，在 ``body`` 中直接使用。

通过引用隐式捕获 ``[&]`` ：所有局部变量的名字都能使用，所有局部变量都通过引用访问。

通过值隐式捕获 ``[=]`` ：所有局部变量的名字都能使用，所有名字都指向局部变量的副本，这些副本是在lambda表达式的调用点获得。

5.3. tparams
------------

模板参数列表(C++20引入)，让Lambda可以像模板函数一样被调用。

5.4. params
-----------

参数列表，和正常函数类似。

5.5. lambda-specifiers
----------------------

Lambda说明符，包括specifiers，exception，attr，trailing-return-type和requires(C++20)，顺序不能改变，每一个组件都是可选的。
