C++入门
=======

由于本框架采用C++和基于C++的SFML图形库，所以如果您需要进一步进行修改和创作，就需要了解下述内容。

数字变量的类型
~~~~~~~~~~~~~

C++中数字常用变量类型分为 ``short`` , ``int`` , ``long`` , ``long long`` , ``float`` , ``double``。

``short`` 是短整型变量，范围为 ``-32768~+32767`` ， ``int`` 为整形变量，
范围为 ``-2147483648~2147483647`` ， ``long`` 为32位长整型变量，
实际上为 ``long int`` ，在C++中， ``long`` 和 ``int`` 范围相同，
``long long`` 为64位长整型数据，范围为 ``-9223372036854774808~9223372036854774807`` 。

``float`` 为浮点型变量，范围为 ``-3.4E38~3.4E38`` ， ``double`` 为双浮点类型变量，范围为 ``-1.7E308~1.7E308`` ，这两种类型支持小数点的计算。

如果在前面加上 ``unsigned`` ，范围则会从0开始，上限增加一倍。

布尔类型
~~~~~~~~

在C++中，布尔类型变量（bool）的取值为 ``true`` 和 ``false`` ，但是， ``true`` 可以在计算中等价转换为 ``1`` ， ``false`` 在计算中可以等价转换为 ``0`` 。

所以可以延申处下面的小技巧，例如bool变量 ``a`` 为 ``true`` 时， ``b`` 会增加3，否则不增加，可以直接写为： ``b = b + a * 3;`` 

字符串
~~~~~~

字符串的类型为 ``string`` ，不同的字符串之间可以用 ``+`` 连接。

宽字符串
-------

需要注意，因为编码的缘故，中文/汉字/全角字符在直接使用 ``string`` 输出到屏幕上时会变成乱码，
此时需要引进 **宽字符** 来表示中文字符，工程内提供了一个函数 ``str2wstr`` 来进行转换，不过一般不用担心，
需要显示中文的地方（如 ``drawText`` 函数）内部都对输入的 ``string`` 类型变量进行了转换，您只需要放心用 ``string`` 即可。

std::to_string(value)
---------------------

``to_string`` 是 **C++11** 引入的标准，可以将数字转换成字符串类型，需要使用 ``<string>`` 头文件。

stoi(str),stof(str)
--------------------

``stoi`` 和 ``stof`` 可以将字符串转换成整型、浮点型变量，需要使用 ``<string>`` 头文件。

类似的函数还有如下几种：

.. csv-table:: 
    :widths: 50, 100

    "stoi", "将字符串转换为int类型"
    "stol", "将字符串转换为long类型"
    "stoll", "将字符串转换为long long类型"
    "stoui", "将字符串转换为unsigned int类型"
    "stoull", "将字符串转换为unsigned long long类型"

std::format(fmt,...args)
--------------------------

``format`` 是 **C++20** 引入的标准，用以格式化字符串，需要使用 ``<format>`` 头文件和 ``using namespace std;`` 。

下面是 ``format`` 的一个示例：

.. code-block:: cpp
    :linenos:

    int a = 3,b = 4;
    std::cout << std::format("a={},b={}",a,b) << endl;

可以注意到， ``format`` 和 ``printf``有部分相似之处，即格式化输出，不过在 ``printf`` 中的例如 ``%d`` 等内容，
都被换成了 ``{}`` ，然后在后面逐个表示变量即可，一般数字和字符串不需要再进行调整。

条件分歧
~~~~~~~

真伪值
------

在C++中，因为历史原因（最初C语言标准并没有bool类型），在条件分歧等判断中， **真** 实际上是 **非零** ， **假** 实际上是 **零** 。

三目运算符
---------

使用符号 ``?`` 和 ``:`` 可以在同一行内进行条件分歧。

下面是一个示例：

.. code-block:: cpp
    :linenos:

    int a = 3;
    std::cout << (a > 10 ? "big" : "small") << endl;

这个示例的意思是， 输出时如果 ``a`` 大于10，则输出 ``big`` ，否则输出 ``small`` 。

Lambda表达式
~~~~~~~~~~~~

Labmda表达式是C++11引入的标准，一般用于定义匿名函数，使得代码更加灵活简洁，最常见的Lambda表达式如下所示：

.. code-block:: cpp
    :linenos:

    auto plus = [] (int v1,int v2) -> int { return v1 + v2; }
    int sum = plus(1,2);

在写比如自定义排序时，往常可能需要写一个 ``cmp`` 函数，但是这种只在特定范围调用的函数可以用Lambda表达式来写，比如如下示例：

.. code-block:: cpp
    :linenos:

    struct Item
    {
        Item(int aa,int bb) : a(aa),b(bb) {} 
        int a;
        int b;
    };
        
    int main()
    {
        std::vector<Item> vec;
        vec.push_back(Item(1,19));
        vec.push_back(Item(10,3));
        vec.push_back(Item(3,7));
        vec.push_back(Item(8,12));
        vec.push_back(Item(2,1));

        // 根据Item中成员a升序排序
        std::sort(vec.begin(),vec.end(),
            [] (const Item& v1,const Item& v2) { return v1.a < v2.a; });

        // 打印vec中的item成员
        std::for_each(vec.begin(),vec.end(),
            [] (const Item& item) { std::cout << item.a << " " << item.b << std::endl; });
        return 0;
    }

Lambda表达式写法
----------------

Lambda表达式有如下三种写法：

.. code-block:: cpp
    :linenos:

    [captures]<tparams>(params) lambda-specifiers {body};
    [captures](params) lambda-specifiers {body};
    [captures](params) {body};

captures
--------

``captures`` 是捕获列表，可以把上下文变量以值或引用的方式捕获，在 ``body`` 中直接使用。

通过引用隐式捕获 ``[&]`` ：所有局部变量的名字都能使用，所有局部变量都通过引用访问。

通过值隐式捕获 ``[=]`` ：所有局部变量的名字都能使用，所有名字都指向局部变量的副本，这些副本是在lambda表达式的调用点获得。

tparams
-------

模板参数列表(C++20引入)，让Lambda可以像模板函数一样被调用。

params
------

参数列表，和正常函数类似。

lambda-specifiers
------------------

Lambda说明符，包括specifiers，exception，attr，trailing-return-type和requires(C++20)，顺序不能改变，每一个组件都是可选的。

std::ranges
~~~~~~~~~~~

``std::ranges`` 是C++20的新特性，以下是几个常用的算法。

std::ranges::any_of(container,condition)
-----------------------------------------

用于判断一个容器中是否有任意一个符合条件，条件可用 ``lambda`` 表达式来确定，下面举个简单的例子。

按照传统的方法，在地图中查找一个符合要求xy坐标的事件的函数haveAnEvent(x,y)写法如下：

.. code-block:: cpp
    :linenos:

    for (auto ev : mapEvents)
        if (ev.x == x && ev.y == y)
            return true;
    return false;

但是有了 ``any_of`` 之后，可以写成：

.. code-block:: cpp
    :linenos:

    return ranges::any_of(mapEvents,[&](auto ev){
        return (ev.x == x && ev.y == y);
    });

std::ranges::count(container,compare) & std::ranges::count_if(container,condition)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

第一个 ``std::ranges::count`` 返回的是容器中和给定值相等的元素的数量，
第二个 ``std::ranges::count_if`` 返回的是 **满足指定条件** 的元素数量，可以是大于或者小于，下面是一个简单的例子：

.. code-block:: cpp
    :linenos:

    std::vector<int> numbers = {1,2,3,2,4,2,5};
    int valueToCount = 2;
    int count = std::ranges::count(numbers,valueToCount);
    std::cout << "Count of " << valueToCount << " is: " << count << std::endl;

.. code-block:: cpp
    :linenos:

    std::vector<int> numbers = {1,2,3,4,5,6,7};
    int threshold = 3;
    auto condition = [threshold](int x) {
        return x > threshold;
    };
    int count = std::ranges::count_if(numbers,condition);
    std::cout << "Count of elements greater than " << threshold << " is: " << count << std::endl;


std::ranges::find(container,compare) & std::ranges::find_if(container,condition)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

第一个 ``std::ranges::find`` 返回的是在范围内查找的与给定值相等的元素的 **迭代器** ，
第二个 ``std::ranges::find_if`` 返回的是 **满足指定条件** 的元素的迭代器。

RM中有一个函数叫做 ``check_event(x,y)`` ，返回的是在(x,y)坐标上的事件ID，按理来说，本应该这么写：

.. code-block:: cpp
    :linenos:

    for (auto ev : mapEvents)
        if (ev.x == x && ev.y == y)
            return ev.ID;
    return -1;

但是现在可以写成这样：

.. code-block:: cpp
    :linenos:

    auto ev = ranges::find_if(mapEvents,[&](auto ev) {
        return ev.x == x && ev.y ==y;
    });
    return ev == mapEvents.end() ? -1 : ev->ID;

std::ranges::transform(container,start,function)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

这个函数可以给容器内从 ``start`` 开始的所有元素进行操作 ``function`` ，例如：

.. code-block:: cpp
    :linenos:

    std::vector<int> numbers = {1,2,3,4,5};
    std::vector<int> squaredNumbers(numbers.size());
    // 使用 std::ranges::transform 对每个元素进行平方操作
    std::ranges::transform(numbers,squaredNumbers.begin(),[](int x) {
        return x * x;
    });
    // 打印转换后的结果
    for (int square : squaredNumbers) {
        std::cout << square << " ";
    }

std::ranges::min(container) & std::ranges::max(container)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

可以求容器内的最小/最大值，但是前提要是可对比的对象，或者自行写好重载小于号或者compare函数。

std::ranges::all_of(container,condition) & std::ranges::none_of(container,condition)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

用于检查容器内元素是否 **全部** 都 **满足** 或 **不满足** 条件。

std::filesystem
~~~~~~~~~~~~~~~~

``std::filesystem`` 是C++17中引入的标准库，它提供了一组用于操作文件系统的函数和类。
它的目标是为了简化对文件和目录的操作，使文件系统的操作更加便捷和安全。

std::filesystem::path
-----------------------

``std::filesystem::path()`` ：默认构造函数，创建一个空的路径对象。

``std::filesystem::path(const std::string&)`` ：使用给定的字符串构造路径对象。

``std::filesystem::path::string()`` ：将路径对象转换为字符串表示。

路径操作
--------

``std::filesystem::current_path()`` ：返回当前工作目录的路径对象。

``std::filesystem::exists(const std::filesystem::path&)`` ：检查路径是否存在。

``std::filesystem::create_directory(const std::filesystem::path&)`` ：创建一个新的目录。

``std::filesystem::remove(const std::filesystem::path&)`` ：删除文件或目录。

``std::filesystem::rename(const std::filesystem::path&,const std::filesystem::path&)`` ：重命名文件或目录。

文件和目录属性
--------------

``std::filesystem::file_size(const std::filesystem::path&)`` ：返回文件的大小。

``std::filesystem::last_write_time(const std::filesystem::path&)`` ：返回最后修改时间。

``std::filesystem::is_directory(const std::filesystem::path&)`` ：检查路径是否为目录。

``std::filesystem::is_regular_file(const std::filesystem::path&)`` ：检查路径是否为普通文件。

文件遍历
---------

``std::filesystem::directory_iterator`` ：遍历指定目录中的文件和目录条目。

``std::filesystem::recursive_directory_iterator`` ：递归遍历指定目录及其子目录中的文件和目录条目。
