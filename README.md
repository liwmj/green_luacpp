# green_luacpp

green_luacpp是C\++库，是嵌入Lua并注册C\++相关的对象到lua的封装库。


## 特性

* 只有头文件
* 轻量级
* 高效清洁
* 容易调用lua函数
* 容易设置或获取var在lua脚本
* 容易使用C\++静态函数扩展lua
* 容易使用C\++类扩展lua，lua C\++类一旦注册,可以使用它像装入的类型。
* 当lua引发异常，green_luacpp也将返回给C\++。


## 安装

只需要将头文件放置lua的include目录下即可使用。


## 支持
> 支持lua5.1 lua5.2


## 使用

##### 载入函数、设置路径
``` c++
green_luacpp_t green_luacpp;
try {
    /**< 注册C++ 对象到lua中 */
    green_luacpp.reg(lua_reg);

    /**< 载入lua文件 */
    green_luacpp.add_package_path("./");
    green_luacpp.load_file("test.lua");

} catch (exception& e) {
    printf("exception:%s\n", e.what());
}
```

##### 获取全局变量、设置全局变量
``` c++
int var = 0;
assert(0 == green_luacpp.get_global_variable("test_var", var));
/**< 设置全局变量 */
assert(0 == green_luacpp.set_global_variable("test_var", ++var));
```

执行lua语句
``` c++
green_luacpp.run_string("print("exe run_string!!")");
```

##### 调用lua函数
``` c++
/**< 调用lua函数, 基本类型作为参数 */
int32_t arg1 = 1;
float   arg2 = 2;
double  arg3 = 3;
string  arg4 = "4";
green_luacpp.call<bool>("test_func", arg1, arg2, arg3,  arg4);
```

##### lua table 和 C++ STL 转换
```
/**< 调用lua函数，stl类型作为参数， 自动转换为lua talbe */
vector<int> vec;        vec.push_back(100);
list<float> lt;         lt.push_back(99.99);
set<string> st;         st.insert("OhNIce");
map<string, int> mp;    mp["key"] = 200;
green_luacpp.call<string>("test_stl", vec, lt, st,  mp);
```

##### lua返回值转换 C++ STL
``` c++
/**< 调用lua 函数返回 talbe，自动转换为stl结构 */
vec = green_luacpp.call<vector<int> >("test_return_stl_vector");
lt  = green_luacpp.call<list<float> >("test_return_stl_list");
st  = green_luacpp.call<set<string> >("test_return_stl_set");
mp  = green_luacpp.call<map<string, int> >("test_return_stl_map");
```

##### C++ 作为参数
``` c++
/**< 调用lua函数，c++ 对象作为参数, foo_t 必须被注册过 */
foo_t* foo_ptr = new foo_t(456);
green_luacpp.call<bool>("test_object", foo_ptr);
```

##### C++ 对象作为返回值
``` c++
/**< 调用lua函数，c++ 对象作为返回值, foo_t 必须被注册过 */
assert(foo_ptr == green_luacpp.call<foo_t*>("test_ret_object", foo_ptr));

/**< 调用lua函数，c++ 对象作为返回值, 自动转换为基类 */
base_t* base_ptr = green_luacpp.call<base_t*>("test_ret_base_object", foo_ptr);
assert(base_ptr == foo_ptr);
```

##### 注册C++ 对象更容易
``` c++
/**< 注册C++ 对象到lua中 */
green_luacpp.reg(lua_reg);
```


## 谢谢

欢迎使用，祝使用愉快

* [https://liwmj.com](http://liwmj.com)
* [liwangmj@gmail.com](mailto:liwangmj@gmail.com)

