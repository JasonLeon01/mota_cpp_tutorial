# nlohmann/json入门

*nlohmann/json* 是一个开源的C++库，用于处理JSON数据。它提供了简单、直观的API，使得在C++中解析、创建和操作JSON数据变得非常方便。

本部分将会介绍 *nlohmann/json* 的基本用法，包括如下内容：
- 如何解析JSON数据
- 如何创建JSON数据
- 如何访问JSON数据
- 如何修改JSON数据
- 如何序列化JSON数据

## 如何从文件中读取json数据

```cpp
    ifstream f("example.json");
    nlohmann::json data =nlohmann::json::parse(f);
    /* nlohmann::json data;
    data >> f;*/
```

## json作为第一类数据类型

以下是一些示例，可帮助您了解如何使用该类。

假设您要创建 JSON 对象:

```cpp
    nlohmann::json data = {
        {"pi", 3.141},
        {"happy", true},
        {"name", "Niels"},
        {"nothing", nullptr},
        {"answer", {
            {"everything", 42}
        }},
        {"list", {1, 0, 2}},
        {"object", {
            {"currency", "USD"},
            {"value", 42.99}
        }}
    };
```

有了这个库，你可以写：

```cpp
    // create an empty structure (null)
    json j;
    // add a number that is stored as double (note the implicit conversion of j to an object)
    j["pi"] = 3.141;
    // add a Boolean that is stored as bool
    j["happy"] = true;
    // add a string that is stored as std::string
    j["name"] = "Niels";
    // add another null object by passing nullptr
    j["nothing"] = nullptr;
    // add an object inside the object
    j["answer"]["everything"] = 42;
    // add an array that is stored as std::vector (using an initializer list)
    j["list"] = { 1, 0, 2 };
    // add another object (using an initializer list of pairs)
    j["object"] = { {"currency", "USD"}, {"value", 42.99} };
    // instead, you could also write (which looks very similar to the JSON above)
    json j2 = {
        {"pi", 3.141},
        {"happy", true},
        {"name", "Niels"},
        {"nothing", nullptr},
        {"answer", {
            {"everything", 42}
        }},
        {"list", {1, 0, 2}},
        {"object", {
            {"currency", "USD"},
            {"value", 42.99}
        }}
    };
```

请注意，在所有这些情况下，您永远不需要“告诉”编译器您要使用哪种 JSON 值类型。
如果您想明确或表达一些边缘情况，函数 `json::array()` 和 `json::object()` 将有所帮助：

```cpp
    // a way to express the empty array []
    json empty_array_explicit = json::array();
    // ways to express the empty object {}
    json empty_object_implicit = json({});
    json empty_object_explicit = json::object();
    // a way to express an _array_ of key/value pairs [["currency", "USD"], ["value", 42.99]]
    json array_not_object = json::array({ {"currency", "USD"}, {"value", 42.99} });
```

## 序列化json对象

您可以将json对象序列化为字符串，一般的常见std类型均可直接使用括号赋值，例如：

```cpp
    std::vector<int> c_vector {1, 2, 3, 4};
    json j_vec(c_vector);
    // [1, 2, 3, 4]
    std::deque<double> c_deque {1.2, 2.3, 3.4, 5.6};
    json j_deque(c_deque);
    // [1.2, 2.3, 3.4, 5.6]
    std::list<bool> c_list {true, true, false, true};
    json j_list(c_list);
    // [true, true, false, true]
    std::forward_list<int64_t> c_flist {12345678909876, 23456789098765, 34567890987654, 45678909876543};
    json j_flist(c_flist);
    // [12345678909876, 23456789098765, 34567890987654, 45678909876543]
    std::array<unsigned long, 4> c_array {{1, 2, 3, 4}};
    json j_array(c_array);
    // [1, 2, 3, 4]
    std::set<std::string> c_set {"one", "two", "three", "four", "one"};
    json j_set(c_set); // only one entry for "one" is used
    // ["four", "one", "three", "two"]
    std::unordered_set<std::string> c_uset {"one", "two", "three", "four", "one"};
    json j_uset(c_uset); // only one entry for "one" is used
    // maybe ["two", "three", "four", "one"]
    std::multiset<std::string> c_mset {"one", "two", "one", "four"};
    json j_mset(c_mset); // both entries for "one" are used
    // maybe ["one", "two", "one", "four"]
    std::unordered_multiset<std::string> c_umset {"one", "two", "one", "four"};
    json j_umset(c_umset); // both entries for "one" are used
    // maybe ["one", "two", "one", "four"]
```

但是显然，我们还会需要自定义一些类型，比如：

```cpp
    struct MyStruct {
        int i;
        std::string str;
        bool b;
    };
```

这时候，你觉得应该怎么做？如果你觉得要这样：

```cpp
    json j;
    MyStruct my_struct;
    j["i"] = my_struct.i;
    j["str"] = my_struct.str;
    j["b"] = my_struct.b;
    ofstream outputFile("data.json");
    outputFile << j.dump(4);
    outputFile.close();
```

那就有点脱裤子放屁了，失去了“序列化”的最重要意义——简洁表述。

官方给我们定义了一个宏 `NLOHMANN_DEFINE_TYPE_INTRUSIVE(name, member1, member2, ...)` ，用于自定义类型的序列化，我们可以这样使用：

```cpp
    struct MyStruct {
        int i;
        std::string str;
        bool b;
        NLOHMANN_DEFINE_TYPE_INTRUSIVE(MyStruct, i, str, b)
    };
    json j;
    MyStruct my_struct;
    j = my_struct;
    ofstream outputFile("data.json");
    outputFile << j.dump(4);
    outputFile.close();
```

这样就可以自动序列化了，是不是很简洁？

## 反序列化json对象

反序列化也是一样的，我们可以这样使用：

```cpp
    json j = R"(
    {
        "happy": true,
        "pi": 3.141
    }
    )"_json;
    bool b = j.at("happy");
    double pi = j.at("pi");
```

对于自定义的类型，我们可以这样使用：

```cpp
    struct MyStruct {
        int i;
        std::string str;
        bool b;
        NLOHMANN_DEFINE_TYPE_INTRUSIVE(MyStruct, i, str, b)
    };
    json j = R"(
    {
        "i": 1,
        "str": "hello",
        "b": true
    }
    )"_json;
    MyStruct my_struct = j;
```