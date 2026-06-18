---
puppeteer:
  format: "a4"
  landscape: false
  scale: 1
  printBackground: true
---

<style>
@page {
  size: A4 portrait;
  margin: 2.54cm 3.18cm 2.54cm 3.18cm;
}

html,
body {
  margin: 0;
  padding: 0;
  font-family: SimSun, Songti SC, serif;
  font-size: 12pt;
  line-height: 1.5;
}

.markdown-body,
.vscode-body,
body > div {
  box-sizing: border-box;
  max-width: none !important;
  width: auto !important;
  margin: 0 !important;
  padding: 0;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  page-break-after: avoid;
}

p {
  text-indent: 2em;
}

p img {
  text-indent: 0;
}

ol,
ul {
  margin-left: 2em;
  padding-left: 1.2em;
}

li {
  text-indent: 0;
}

li > p {
  text-indent: 0;
  margin: 0;
}

pre,
code {
  white-space: pre-wrap;
  word-break: break-word;
}
</style>

# C++ 学习路线

> 适用基础：已有 C 语言、操作系统、计算机网络、数据结构等计算机专业基础。
> 目标：系统学习 C++，具备从事 C++ 相关研发工作的能力。

## 目录

- [1 总体路线](#1-总体路线)
- [2 第一阶段：C++ 基础与语言模型](#2-第一阶段c-基础与语言模型)
- [3 第二阶段：面向对象与资源管理](#3-第二阶段面向对象与资源管理)
- [4 第三阶段：STL 与泛型编程](#4-第三阶段stl-与泛型编程)
- [5 第四阶段：现代 C++](#5-第四阶段现代-c)
- [6 第五阶段：C++ 工程化](#6-第五阶段c-工程化)
- [7 第六阶段：并发与多线程](#7-第六阶段并发与多线程)
- [8 第七阶段：Linux C++ 系统编程](#8-第七阶段linux-c-系统编程)
- [9 第八阶段：网络编程与高性能服务](#9-第八阶段网络编程与高性能服务)
- [10 第九阶段：数据库、缓存与中间件](#10-第九阶段数据库缓存与中间件)
- [11 第十阶段：性能优化与底层能力](#11-第十阶段性能优化与底层能力)
- [12 第十一阶段：项目实战路线](#12-第十一阶段项目实战路线)
- [13 推荐书单顺序](#13-推荐书单顺序)
- [14 推荐时间安排](#14-推荐时间安排)
- [15 最终应形成的能力](#15-最终应形成的能力)

---

## 1 总体路线

这条路线按“能去做 C++ 研发工作”来设计，不是算法竞赛路线。你已经有 C、操作系统、网络、数据结构基础，所以重点应该放在现代 C++ 语言能力、工程能力、系统编程、性能与并发、项目经验上。

建议整体路线如下：

1. C++ 基础语法与 C 到 C++ 的思维转换
2. 面向对象与资源管理
3. STL 与泛型编程
4. 现代 C++：C++11 到 C++20
5. 工程化：CMake、调试、测试、代码规范
6. 并发、多线程、内存模型
7. Linux C++ 系统编程
8. 网络编程与高性能服务
9. 性能优化与底层能力
10. 项目实战与求职准备

---

## 2 第一阶段：C++ 基础与语言模型

### 2.1 阶段目标

本阶段目标是从“会写 C”转到“用 C++ 的方式写程序”。C++ 不是“带 class 的 C”，它更强调对象生命周期、类型系统、资源管理和抽象能力。

### 2.2 重点内容

需要重点学习：

- 引用
- 函数重载
- 默认参数
- `const`
- `constexpr`
- 命名空间
- 类与对象
- 构造函数
- 析构函数
- 拷贝构造
- 拷贝赋值
- 静态成员
- `friend`
- 运算符重载
- `new/delete`
- 对象生命周期

### 2.3 核心思想

C 里经常手动管理资源，而 C++ 更强调“对象生命周期管理资源”，也就是 RAII：资源在构造函数中获取，在析构函数中释放。

```cpp
#include <cstdio>
#include <stdexcept>

class File {
public:
    explicit File(const char* path) {
        fp_ = std::fopen(path, "r");
        if (!fp_) {
            throw std::runtime_error("open failed");
        }
    }

    ~File() {
        if (fp_) {
            std::fclose(fp_);
        }
    }

    File(const File&) = delete;
    File& operator=(const File&) = delete;

private:
    FILE* fp_ = nullptr;
};
```

这个例子体现了 C++ 的一个核心思想：资源由对象负责，函数异常退出时析构函数仍然会执行。

### 2.4 推荐资料

- 《C++ Primer》第五版：主教材
- 《Effective C++》：第二遍强化类设计和资源管理
- cppreference：查标准库和语法细节

### 2.5 阶段产出

建议完成：

- 简单学生管理系统
- 日志类
- 配置文件解析器
- 简单文件封装类

要求：用类封装资源，不要满屏裸指针和手写释放逻辑。

---

## 3 第二阶段：面向对象与资源管理

### 3.1 阶段目标

本阶段目标是理解 C++ 类设计，而不是只会写 `class`。要能说清楚对象之间的关系、资源所有权、复制语义和析构责任。

### 3.2 重点内容

需要重点学习：

- 封装
- 继承
- 多态
- 虚函数
- 虚析构函数
- 抽象类
- 接口类
- public/private/protected 继承
- 对象切片
- 组合优于继承
- RAII
- Rule of Three
- Rule of Five
- Rule of Zero
- 智能指针

必须掌握：

```cpp
std::unique_ptr<T>
std::shared_ptr<T>
std::weak_ptr<T>
std::make_unique
std::make_shared
```

### 3.3 智能指针示例

```cpp
#include <memory>

class Engine {
public:
    void start() {}
};

class Car {
public:
    Car() : engine_(std::make_unique<Engine>()) {}

    void run() {
        engine_->start();
    }

private:
    std::unique_ptr<Engine> engine_;
};
```

这里 `Car` 独占 `Engine`，所以用 `unique_ptr`。如果对象所有权说不清楚，C++ 代码通常就会变乱。

### 3.4 阶段产出

建议完成：

- 实现一个简单对象池
- 实现一个不可复制但可移动的资源类
- 写一个小型图形类层次，例如 `Shape / Circle / Rectangle`
- 用智能指针重构一段裸指针代码

---

## 4 第三阶段：STL 与泛型编程

### 4.1 阶段目标

本阶段目标是熟练使用标准库，减少手写低质量轮子。C++ 研发中，STL 是日常生产力的核心。

### 4.2 容器

需要掌握：

```cpp
vector
array
deque
list
map
set
unordered_map
unordered_set
string
```

重点理解：

- `vector` 适合连续存储和随机访问
- `list` 适合频繁中间插入删除，但缓存局部性差
- `map/set` 基于有序结构，支持有序遍历
- `unordered_map/unordered_set` 基于哈希，平均查找更快
- `string` 是字符序列容器，不要当 C 字符数组使用

### 4.3 算法

需要掌握：

```cpp
sort
find
lower_bound
upper_bound
accumulate
transform
for_each
remove_if
unique
```

### 4.4 迭代器

需要理解：

- 输入迭代器
- 输出迭代器
- 前向迭代器
- 双向迭代器
- 随机访问迭代器

不同算法对迭代器能力有不同要求。例如 `sort` 需要随机访问迭代器，所以可以用于 `vector`，不能直接用于 `list`。

### 4.5 函数对象与 lambda

需要掌握：

```cpp
std::function
lambda
bind
```

示例：

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums{1, 2, 3, 4, 5, 6};

    nums.erase(
        std::remove_if(nums.begin(), nums.end(), [](int x) {
            return x % 2 == 0;
        }),
        nums.end()
    );
}
```

这个例子是经典的 erase-remove idiom，必须熟练掌握。

### 4.6 阶段产出

建议完成：

- 用 STL 写一个词频统计工具
- 写一个 LRU Cache
- 写一个简易排行榜系统
- 熟悉 `unordered_map` 和 `map` 的区别
- 用 `vector`、`list` 分别实现同一功能并比较性能

---

## 5 第四阶段：现代 C++

### 5.1 阶段目标

本阶段目标是掌握企业开发中常用的 C++11/14/17/20 特性。现代 C++ 的核心方向是：更安全的资源管理、更强的类型表达能力、更好的泛型编程、更少的手写样板代码。

### 5.2 C++11/14

C++11/14 是现代 C++ 的分水岭。学习这一部分时，不要只记语法，要理解它们解决了什么工程问题：减少冗余、增强类型安全、管理资源、支持并发、提高泛型代码表达力。

#### 5.2.1 `auto`

`auto` 让编译器根据初始化表达式推导变量类型，适合类型很长、类型明显、迭代器场景。不要滥用到让代码读者看不出变量含义。

```cpp
#include <map>
#include <string>

int main() {
    std::map<std::string, int> scores{{"Alice", 90}, {"Bob", 85}};

    // 迭代器类型很长，使用 auto 更清晰
    auto it = scores.find("Alice");
    if (it != scores.end()) {
        it->second += 5;
    }

    // 类型从右侧一眼能看出，适合 auto
    auto count = scores.size();
}
```

注意：`auto` 会丢掉顶层 `const`，引用也需要显式写出来。

```cpp
const int x = 10;
auto a = x;        // int
const auto b = x;  // const int
auto& c = x;       // const int&
```

#### 5.2.2 范围 for

范围 for 适合遍历容器，比手写迭代器更简洁。遍历大对象时用引用，仅读取时用 `const auto&`。

```cpp
#include <iostream>
#include <string>
#include <vector>

int main() {
    std::vector<std::string> names{"Alice", "Bob", "Cindy"};

    for (const auto& name : names) {
        std::cout << name << '\n';
    }

    for (auto& name : names) {
        name += "_user";
    }
}
```

#### 5.2.3 lambda

lambda 用于定义临时函数对象，常配合 STL 算法、回调、异步任务使用。需要重点理解捕获列表。

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> nums{1, 5, 3, 9, 2};
    int threshold = 4;

    auto cnt = std::count_if(nums.begin(), nums.end(), [threshold](int x) {
        return x > threshold;
    });

    std::cout << cnt << '\n';
}
```

常见捕获方式：

- `[x]`：按值捕获 `x`
- `[&x]`：按引用捕获 `x`
- `[=]`：默认按值捕获用到的外部变量
- `[&]`：默认按引用捕获用到的外部变量

工程中要谨慎使用 `[&]`，异步任务或线程中引用捕获局部变量很容易悬空。

#### 5.2.4 右值引用与移动语义

右值引用 `T&&` 让 C++ 可以区分“还要继续使用的对象”和“即将被销毁、资源可以被拿走的对象”。移动语义的目标是避免昂贵拷贝。

```cpp
#include <cstring>
#include <utility>

class Buffer {
public:
    explicit Buffer(std::size_t n)
        : size_(n), data_(new char[n]) {}

    ~Buffer() {
        delete[] data_;
    }

    Buffer(const Buffer& rhs)
        : size_(rhs.size_), data_(new char[rhs.size_]) {
        std::memcpy(data_, rhs.data_, size_);
    }

    Buffer& operator=(const Buffer& rhs) {
        if (this == &rhs) {
            return *this;
        }
        Buffer temp(rhs);
        swap(temp);
        return *this;
    }

    Buffer(Buffer&& rhs) noexcept
        : size_(rhs.size_), data_(rhs.data_) {
        rhs.size_ = 0;
        rhs.data_ = nullptr;
    }

    Buffer& operator=(Buffer&& rhs) noexcept {
        if (this == &rhs) {
            return *this;
        }
        delete[] data_;
        size_ = rhs.size_;
        data_ = rhs.data_;
        rhs.size_ = 0;
        rhs.data_ = nullptr;
        return *this;
    }

    void swap(Buffer& other) noexcept {
        std::swap(size_, other.size_);
        std::swap(data_, other.data_);
    }

private:
    std::size_t size_ = 0;
    char* data_ = nullptr;
};
```

`std::move` 本身不移动资源，它只是把对象转换成右值，真正移动发生在移动构造或移动赋值里。

#### 5.2.5 完美转发

完美转发用于写泛型包装函数，把参数的左值/右值属性原样传给下层函数。核心是转发引用 `T&&` 和 `std::forward<T>`。

```cpp
#include <iostream>
#include <utility>

void process(int& x) {
    std::cout << "left value\n";
}

void process(int&& x) {
    std::cout << "right value\n";
}

template <typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}

int main() {
    int a = 10;
    wrapper(a);   // left value
    wrapper(20);  // right value
}
```

记忆方式：想无条件转成右值，用 `std::move`；想在模板中保留实参原始属性，用 `std::forward`。

#### 5.2.6 `nullptr`

`nullptr` 是空指针常量，类型是 `std::nullptr_t`，比 `NULL` 或 `0` 更安全。

```cpp
void f(int) {}
void f(char*) {}

int main() {
    // f(NULL);     // 可能调用 f(int)，产生歧义或误用
    f(nullptr);     // 明确调用 f(char*)
}
```

#### 5.2.7 `enum class`

`enum class` 是强类型枚举，不会随意隐式转换成整数，也不会把枚举名污染到外层作用域。

```cpp
enum class Color {
    Red,
    Green,
    Blue
};

void paint(Color color) {}

int main() {
    paint(Color::Red);
    // paint(0); // 编译错误，类型更安全
}
```

#### 5.2.8 `override` 和 `final`

`override` 用来明确表示重写虚函数。如果函数签名写错，编译器会报错。`final` 用来禁止继续重写或继承。

```cpp
class Base {
public:
    virtual ~Base() = default;
    virtual void run() const {}
};

class Derived : public Base {
public:
    void run() const override {}
};

class FinalClass final {};
```

工程中建议：所有重写虚函数都写 `override`。

#### 5.2.9 `default` 和 `delete`

`default` 表示显式要求编译器生成默认函数，`delete` 表示明确禁止某个函数。

```cpp
class Connection {
public:
    Connection() = default;

    Connection(const Connection&) = delete;
    Connection& operator=(const Connection&) = delete;
};
```

这比旧式把拷贝构造函数声明为 `private` 更直观。

#### 5.2.10 `constexpr`

`constexpr` 表示函数或变量可以在编译期求值，适合常量计算、数组大小、模板参数等场景。

```cpp
constexpr int square(int x) {
    return x * x;
}

int arr[square(4)];
```

C++14 放宽了 `constexpr` 函数限制，可以写更复杂的局部变量和控制流。

#### 5.2.11 智能指针

智能指针是现代 C++ 管理堆资源的核心工具。

```cpp
#include <memory>

class Task {
public:
    void run() {}
};

int main() {
    auto task = std::make_unique<Task>();
    task->run();

    auto sharedTask = std::make_shared<Task>();
    auto anotherRef = sharedTask;
}
```

使用原则：

- 独占所有权用 `std::unique_ptr`
- 共享所有权用 `std::shared_ptr`
- 观察共享对象但不延长生命周期用 `std::weak_ptr`
- 优先使用 `std::make_unique` 和 `std::make_shared`

#### 5.2.12 线程库

C++11 开始标准库提供跨平台线程能力。基础组件包括 `std::thread`、互斥锁、条件变量、future、atomic。

```cpp
#include <iostream>
#include <thread>

void work() {
    std::cout << "working\n";
}

int main() {
    std::thread t(work);
    t.join();
}
```

线程对象析构前必须 `join()` 或 `detach()`，否则程序会调用 `std::terminate()`。

### 5.3 C++17

C++17 的特点是让现代 C++ 更适合工程开发：返回多个值更方便，编译期分支更清晰，标准库提供了更强的类型表达能力。

#### 5.3.1 结构化绑定

结构化绑定可以把 `pair`、`tuple`、结构体中的成员直接拆出来。

```cpp
#include <map>
#include <string>

int main() {
    std::map<std::string, int> scores{{"Alice", 90}, {"Bob", 80}};

    for (const auto& [name, score] : scores) {
        // name 是 key，score 是 value
    }
}
```

它常用于遍历 `map`、函数返回 `pair`、解析多个返回值。

#### 5.3.2 `if constexpr`

`if constexpr` 是编译期分支，不满足的分支不会被实例化，常用于模板代码。

```cpp
#include <iostream>
#include <type_traits>

template <typename T>
void printValue(const T& value) {
    if constexpr (std::is_integral_v<T>) {
        std::cout << "integer: " << value << '\n';
    } else {
        std::cout << "other: " << value << '\n';
    }
}
```

普通 `if` 是运行期判断，两边代码都要能通过编译；`if constexpr` 适合根据类型选择实现。

#### 5.3.3 `std::optional`

`optional<T>` 表示“可能有一个 T，也可能没有”。它比用特殊值、空指针或异常表达“无结果”更清楚。

```cpp
#include <optional>
#include <string>

std::optional<int> parseInt(const std::string& s) {
    try {
        return std::stoi(s);
    } catch (...) {
        return std::nullopt;
    }
}

int main() {
    auto value = parseInt("123");
    if (value) {
        int x = *value;
    }
}
```

#### 5.3.4 `std::variant`

`variant` 表示类型安全的联合体：一个变量可以是几种类型之一，但同一时刻只能持有一种。

```cpp
#include <iostream>
#include <string>
#include <variant>

using ConfigValue = std::variant<int, double, std::string>;

void print(const ConfigValue& value) {
    std::visit([](const auto& v) {
        std::cout << v << '\n';
    }, value);
}
```

适合配置值、消息类型、表达式节点等场景。

#### 5.3.5 `std::any`

`any` 可以保存任意可拷贝类型，但取值时需要知道真实类型。它灵活，但类型约束弱，滥用会让代码难维护。

```cpp
#include <any>
#include <iostream>
#include <string>

int main() {
    std::any value = std::string("hello");

    if (value.type() == typeid(std::string)) {
        std::cout << std::any_cast<std::string>(value);
    }
}
```

优先级通常是：能用明确类型就不用 `any`，能用 `variant` 就不用 `any`。

#### 5.3.6 `std::string_view`

`string_view` 是非拥有型字符串视图，不拷贝字符串，只保存指针和长度。它适合函数参数、解析器、日志格式化等只读场景。

```cpp
#include <iostream>
#include <string_view>

void printName(std::string_view name) {
    std::cout << name << '\n';
}

int main() {
    printName("Alice");
    std::string s = "Bob";
    printName(s);
}
```

注意：`string_view` 不拥有数据，不能指向已经销毁的临时字符串。

```cpp
std::string_view bad() {
    std::string local = "danger";
    return local; // 错误：返回后 string_view 悬空
}
```

#### 5.3.7 文件系统库

`std::filesystem` 用于跨平台处理路径、目录、文件大小、遍历文件树。

```cpp
#include <filesystem>
#include <iostream>

namespace fs = std::filesystem;

int main() {
    fs::path dir = ".";

    for (const auto& entry : fs::directory_iterator(dir)) {
        std::cout << entry.path().string() << '\n';
    }
}
```

文件系统库适合写构建工具、日志清理工具、资源扫描器、批处理程序。

### 5.4 C++20

C++20 让 C++ 的泛型约束、范围式数据处理、协程和模块化能力更进一步。作为求职准备，C++20 不一定要求全部精通，但要理解它们解决的问题。

#### 5.4.1 concepts

concepts 用来约束模板参数，让模板错误更早、更清晰。

```cpp
#include <concepts>
#include <iostream>

template <typename T>
concept Number = std::integral<T> || std::floating_point<T>;

template <Number T>
T add(T a, T b) {
    return a + b;
}

int main() {
    std::cout << add(1, 2) << '\n';
    std::cout << add(1.5, 2.5) << '\n';
}
```

没有 concepts 时，模板错误常常很长；有 concepts 后，可以直接表达“这个模板只接受满足某种能力的类型”。

#### 5.4.2 ranges

ranges 让算法能直接作用于范围对象，并支持管道式组合。它适合表达筛选、变换、截取等数据处理流程。

```cpp
#include <iostream>
#include <ranges>
#include <vector>

int main() {
    std::vector<int> nums{1, 2, 3, 4, 5, 6};

    auto evenSquares = nums
        | std::views::filter([](int x) { return x % 2 == 0; })
        | std::views::transform([](int x) { return x * x; });

    for (int x : evenSquares) {
        std::cout << x << ' ';
    }
}
```

ranges 的很多 view 是惰性求值，只有遍历时才真正计算。

#### 5.4.3 coroutine 基础

协程允许函数挂起和恢复，适合异步 IO、生成器、任务调度。C++20 提供协程语言机制，但标准库没有直接给出完整网络协程框架，所以实战中常结合 asio、cppcoro 或项目自研框架。

示意代码如下：

```cpp
// 伪代码：真实协程需要 promise_type 等框架支持
Task<std::string> fetchData() {
    std::string data = co_await asyncRead();
    co_return data;
}
```

学习重点不是马上手写协程框架，而是理解：

- `co_await`：等待异步结果
- `co_yield`：产生一个值后挂起
- `co_return`：从协程返回结果
- 协程本质是编译器把函数改写成状态机

#### 5.4.4 modules

modules 用来替代传统头文件包含模型，减少重复解析头文件带来的编译成本，也能减少宏污染。

示意：

```cpp
// math.ixx
export module math;

export int add(int a, int b) {
    return a + b;
}
```

```cpp
// main.cpp
import math;

int main() {
    return add(1, 2);
}
```

目前很多项目仍以头文件和源文件为主，modules 可以先了解思想和基本写法。

### 5.5 最重要的现代 C++ 能力

这一节是现代 C++ 学习的优先级排序。如果时间有限，先把下面这些能力练熟。

#### 5.5.1 移动语义

移动语义解决“大对象复制成本高”的问题。你需要能判断一个类是否应该支持移动、移动后对象应处于什么状态、移动操作是否应该 `noexcept`。

```cpp
#include <string>
#include <vector>

std::vector<std::string> buildNames() {
    std::vector<std::string> names;
    names.push_back("Alice");
    names.push_back("Bob");
    return names; // 返回值优化或移动，避免昂贵拷贝
}
```

#### 5.5.2 完美转发

完美转发主要用于泛型工厂、容器封装、回调包装。

```cpp
#include <memory>
#include <utility>

template <typename T, typename... Args>
std::unique_ptr<T> makeObject(Args&&... args) {
    return std::make_unique<T>(std::forward<Args>(args)...);
}
```

这里 `Args&&...` 保留实参属性，`std::forward` 把参数原样交给 `T` 的构造函数。

#### 5.5.3 智能指针

智能指针要和所有权一起理解。

```cpp
class Session {};

std::unique_ptr<Session> createSession() {
    return std::make_unique<Session>();
}
```

函数返回 `unique_ptr` 表示“把所有权交给调用者”；参数使用 `Session*` 或 `Session&` 通常表示“只使用，不拥有”。

#### 5.5.4 lambda

lambda 是现代 C++ 中表达局部逻辑的主要工具，尤其适合 STL 算法和异步回调。

```cpp
#include <algorithm>
#include <vector>

void removeNegative(std::vector<int>& nums) {
    nums.erase(
        std::remove_if(nums.begin(), nums.end(), [](int x) {
            return x < 0;
        }),
        nums.end()
    );
}
```

#### 5.5.5 `optional / variant / string_view`

这三个类型分别解决三类常见表达问题：

- `optional<T>`：可能没有值
- `variant<A, B, C>`：多个类型选一个
- `string_view`：只读、不拥有字符串视图

综合示例：

```cpp
#include <optional>
#include <string>
#include <string_view>
#include <variant>

using ConfigValue = std::variant<int, bool, std::string>;

std::optional<int> parseInt(std::string_view s) {
    try {
        return std::stoi(std::string(s));
    } catch (...) {
        return std::nullopt;
    }
}

ConfigValue parseValue(std::string_view s) {
    if (s == "true") {
        return true;
    }
    if (s == "false") {
        return false;
    }
    if (auto number = parseInt(s)) {
        return *number;
    }
    return std::string(s);
}
```

#### 5.5.6 `constexpr` 和 `if constexpr`

`constexpr` 把能在编译期确定的计算提前完成，`if constexpr` 让模板代码根据类型选择分支。

```cpp
#include <type_traits>

constexpr int maxSize() {
    return 1024;
}

template <typename T>
auto normalize(T value) {
    if constexpr (std::is_integral_v<T>) {
        return value;
    } else {
        return static_cast<int>(value);
    }
}
```

学习建议：现代 C++ 不要按语法点孤立学习，最好围绕“资源管理、泛型封装、接口表达、错误处理”来练习。

### 5.6 阶段产出

建议完成：

- 写一个支持移动语义的动态数组 `Vector`
- 写一个简单的 `optional`
- 写一个配置加载模块，使用 `optional`、`variant`、`string_view`
- 用 C++17 重构一段传统 C++ 代码

---

## 6 第五阶段：C++ 工程化

### 6.1 阶段目标

本阶段目标是能进入真实项目，而不是只会写单文件代码。C++ 工程开发非常重视构建、调试、测试、规范和依赖管理。

### 6.2 必须掌握

需要掌握：

- CMake
- Git
- Linux 编译链
- 静态库
- 动态库
- 头文件与源文件组织
- include 依赖管理
- 单元测试
- 日志系统
- 调试
- Sanitizer
- 代码格式化

### 6.3 常用工具

```bash
g++
clang++
cmake
make
ninja
gdb
valgrind
perf
clang-format
clang-tidy
GoogleTest
AddressSanitizer
ThreadSanitizer
```

### 6.4 CMake 示例

```cmake
cmake_minimum_required(VERSION 3.20)
project(MyProject)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(my_app main.cpp)
```

### 6.5 阶段产出

建议完成：

- 用 CMake 管理一个多目录 C++ 项目
- 接入 GoogleTest
- 使用 ASan 查内存错误
- 使用 gdb 调试崩溃
- 使用 clang-format 统一代码风格
- 写 README 说明项目构建和运行方式

---

## 7 第六阶段：并发与多线程

### 7.1 阶段目标

本阶段目标是能写正确的多线程 C++ 程序。多线程难点不在于创建线程，而在于共享状态、同步、生命周期、异常、死锁和性能。

### 7.2 重点内容

并发学习要按“线程生命周期、共享数据保护、线程间通信、异步结果、无锁基础、工程模型”的顺序推进。

#### 7.2.1 `std::thread`

`std::thread` 用来创建线程。线程对象析构前必须 `join()` 或 `detach()`，否则程序会终止。工程中更推荐用 RAII 类或 C++20 的 `std::jthread` 管理线程生命周期。

```cpp
#include <iostream>
#include <thread>

void worker(int id) {
    std::cout << "worker " << id << '\n';
}

int main() {
    std::thread t(worker, 1);
    t.join();
}
```

错误点：不要让局部变量被线程引用后，主线程提前退出作用域。

```cpp
void bad() {
    int x = 10;
    std::thread t([&] {
        // 如果 detach 后 bad 返回，x 会悬空
        ++x;
    });
    t.detach();
}
```

#### 7.2.2 `std::mutex`

`std::mutex` 用来保护共享数据，避免多个线程同时读写造成数据竞争。

```cpp
#include <mutex>

std::mutex mtx;
int counter = 0;

void add() {
    mtx.lock();
    ++counter;
    mtx.unlock();
}
```

上面写法不推荐，因为中间如果抛异常，`unlock()` 不会执行。应使用 RAII 锁。

#### 7.2.3 `std::lock_guard`

`lock_guard` 在构造时加锁，析构时解锁，适合简单临界区。

```cpp
#include <mutex>

std::mutex mtx;
int counter = 0;

void addSafe() {
    std::lock_guard<std::mutex> lock(mtx);
    ++counter;
}
```

原则：能用 `lock_guard` 就不要手写 `lock()` 和 `unlock()`。

#### 7.2.4 `std::unique_lock`

`unique_lock` 比 `lock_guard` 灵活，可以延迟加锁、提前解锁、配合条件变量使用。

```cpp
#include <mutex>

std::mutex mtx;

void work() {
    std::unique_lock<std::mutex> lock(mtx);
    // 访问共享数据
    lock.unlock();
    // 执行不需要锁的耗时操作
}
```

条件变量的 `wait()` 必须配合 `unique_lock` 使用。

#### 7.2.5 `std::condition_variable`

条件变量用于线程等待某个条件成立，典型场景是生产者消费者队列。

```cpp
#include <condition_variable>
#include <mutex>
#include <queue>

std::mutex mtx;
std::condition_variable cv;
std::queue<int> q;

void producer() {
    {
        std::lock_guard<std::mutex> lock(mtx);
        q.push(42);
    }
    cv.notify_one();
}

void consumer() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, [] {
        return !q.empty();
    });

    int value = q.front();
    q.pop();
}
```

`wait(lock, predicate)` 会自动处理虚假唤醒。不要写成只 `wait(lock)` 后直接认为条件满足。

#### 7.2.6 `std::future`

`future` 表示未来某个异步结果。它常和 `promise` 或 `async` 搭配。

```cpp
#include <future>
#include <iostream>

int calc() {
    return 40 + 2;
}

int main() {
    std::future<int> result = std::async(std::launch::async, calc);
    std::cout << result.get() << '\n';
}
```

`get()` 会等待结果，只能调用一次。

#### 7.2.7 `std::async`

`std::async` 用来启动异步任务。建议显式指定 `std::launch::async`，避免实现选择延迟执行导致行为不符合预期。

```cpp
#include <future>
#include <vector>

long long sumRange(int begin, int end) {
    long long sum = 0;
    for (int i = begin; i < end; ++i) {
        sum += i;
    }
    return sum;
}

int main() {
    auto f1 = std::async(std::launch::async, sumRange, 0, 500000);
    auto f2 = std::async(std::launch::async, sumRange, 500000, 1000000);

    long long total = f1.get() + f2.get();
}
```

#### 7.2.8 `std::atomic`

`atomic` 用于无数据竞争地读写简单共享变量。它适合计数器、状态标志等场景，但不能替代所有锁。

```cpp
#include <atomic>

std::atomic<int> counter{0};

void add() {
    counter.fetch_add(1, std::memory_order_relaxed);
}
```

如果只是统计计数，`memory_order_relaxed` 通常足够；如果涉及线程间发布数据，就要更谨慎地选择内存序。

#### 7.2.9 内存序

内存序决定原子操作周围的读写在多线程中的可见性和重排约束。初学阶段可以先掌握三个层次：

- `memory_order_relaxed`：只保证原子变量本身操作不撕裂，不保证同步其他数据
- `memory_order_acquire/release`：用于发布和获取数据
- `memory_order_seq_cst`：最强顺序，默认选项，理解简单但可能成本更高

示例：

```cpp
#include <atomic>
#include <string>

std::string data;
std::atomic<bool> ready{false};

void producer() {
    data = "hello";
    ready.store(true, std::memory_order_release);
}

void consumer() {
    while (!ready.load(std::memory_order_acquire)) {}
    // 看到 ready 为 true 后，也能看到 producer 写入的 data
}
```

内存序很容易写错。业务代码优先用锁和条件变量，只有性能瓶颈明确时再考虑复杂原子设计。

#### 7.2.10 死锁

死锁通常满足四个条件：互斥、持有并等待、不可抢占、循环等待。工程上常用固定加锁顺序、减少锁粒度、使用 `std::lock` 同时加多个锁来避免。

```cpp
#include <mutex>

std::mutex a;
std::mutex b;

void safe() {
    std::scoped_lock lock(a, b); // C++17，同时加锁，避免顺序死锁
}
```

不要在持锁时调用未知外部函数，因为外部函数可能再尝试获取其他锁。

#### 7.2.11 线程池

线程池用于复用固定数量的线程，避免频繁创建销毁线程。核心组件是任务队列、工作线程、条件变量、停止标志。

简化示意：

```cpp
#include <condition_variable>
#include <functional>
#include <mutex>
#include <queue>
#include <thread>
#include <vector>

class ThreadPool {
public:
    explicit ThreadPool(std::size_t n) {
        for (std::size_t i = 0; i < n; ++i) {
            workers_.emplace_back([this] {
                while (true) {
                    std::function<void()> task;
                    {
                        std::unique_lock<std::mutex> lock(mtx_);
                        cv_.wait(lock, [this] {
                            return stop_ || !tasks_.empty();
                        });
                        if (stop_ && tasks_.empty()) {
                            return;
                        }
                        task = std::move(tasks_.front());
                        tasks_.pop();
                    }
                    task();
                }
            });
        }
    }

    ~ThreadPool() {
        {
            std::lock_guard<std::mutex> lock(mtx_);
            stop_ = true;
        }
        cv_.notify_all();
        for (auto& worker : workers_) {
            worker.join();
        }
    }

    void submit(std::function<void()> task) {
        {
            std::lock_guard<std::mutex> lock(mtx_);
            tasks_.push(std::move(task));
        }
        cv_.notify_one();
    }

private:
    std::mutex mtx_;
    std::condition_variable cv_;
    std::queue<std::function<void()>> tasks_;
    std::vector<std::thread> workers_;
    bool stop_ = false;
};
```

#### 7.2.12 生产者消费者模型

生产者消费者模型用于解耦任务提交和任务处理。网络服务器、日志系统、线程池都常用这个模型。

核心规则：

- 队列是共享资源，必须加锁
- 消费者队列为空时等待
- 生产者放入任务后通知
- 退出时要通知所有等待线程

这个模型要重点练，因为它是很多 C++ 服务端组件的基础。

### 7.3 示例

```cpp
#include <condition_variable>
#include <mutex>
#include <queue>

std::mutex mtx;
std::queue<int> q;
std::condition_variable cv;

void producer() {
    {
        std::lock_guard<std::mutex> lock(mtx);
        q.push(1);
    }
    cv.notify_one();
}

void consumer() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, [] {
        return !q.empty();
    });

    int value = q.front();
    q.pop();
}
```

### 7.4 阶段产出

建议完成：

- 实现一个线程池
- 实现生产者消费者队列
- 实现定时任务调度器
- 用 ThreadSanitizer 检查数据竞争
- 总结死锁产生条件和避免方法

---

## 8 第七阶段：Linux C++ 系统编程

### 8.1 阶段目标

本阶段目标是具备后端、基础架构、服务端开发能力。你已有操作系统基础，这一阶段要从“知道概念”变成“能写代码”。

### 8.2 重点内容

Linux 系统编程要把操作系统知识落到系统调用和工程实践上。重点不是背 API，而是理解文件描述符、进程、内存、信号、IO 多路复用这些机制如何组合成服务端程序。

#### 8.2.1 Linux 文件 IO

Linux 中“文件”是统一抽象，普通文件、socket、管道、设备都可以用文件描述符表示。常用系统调用包括 `open/read/write/close/lseek`。

```cpp
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("data.txt", O_RDONLY);
    if (fd < 0) {
        return 1;
    }

    char buf[1024];
    ssize_t n = read(fd, buf, sizeof(buf));
    close(fd);
}
```

实际项目中要处理短读、短写、错误码 `errno`，并用 RAII 封装文件描述符。

#### 8.2.2 进程

进程是资源分配单位。常用调用有 `fork`、`exec`、`waitpid`。`fork` 会复制当前进程，父子进程从 `fork` 返回处继续执行。

```cpp
#include <sys/wait.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();
    if (pid == 0) {
        execlp("ls", "ls", "-l", nullptr);
        _exit(1);
    } else if (pid > 0) {
        waitpid(pid, nullptr, 0);
    }
}
```

写多进程程序时要注意：子进程会继承父进程打开的文件描述符。

#### 8.2.3 线程

Linux 线程本质上是共享地址空间的轻量执行流。C++ 中优先使用 `std::thread`，需要和系统接口交互时再了解 pthread。

```cpp
#include <pthread.h>
#include <iostream>

void* run(void*) {
    std::cout << "pthread\n";
    return nullptr;
}

int main() {
    pthread_t tid;
    pthread_create(&tid, nullptr, run, nullptr);
    pthread_join(tid, nullptr);
}
```

服务端中，线程常和线程池、任务队列、异步 IO 结合使用。

#### 8.2.4 信号

信号是 Linux 的异步通知机制，例如 `SIGINT`、`SIGTERM`、`SIGPIPE`。服务程序通常需要处理退出信号，实现优雅关闭。

```cpp
#include <atomic>
#include <csignal>

std::atomic<bool> stop{false};

void onSignal(int) {
    stop.store(true);
}

int main() {
    std::signal(SIGINT, onSignal);
    while (!stop.load()) {
        // main loop
    }
}
```

信号处理函数里能安全调用的函数很少，复杂逻辑应放回主循环处理。

#### 8.2.5 管道

管道用于有亲缘关系进程间通信。匿名管道用 `pipe` 创建，返回两个文件描述符：读端和写端。

```cpp
#include <unistd.h>

int main() {
    int fds[2];
    pipe(fds);

    if (fork() == 0) {
        close(fds[0]);
        write(fds[1], "hi", 2);
        close(fds[1]);
        _exit(0);
    } else {
        close(fds[1]);
        char buf[16]{};
        read(fds[0], buf, sizeof(buf));
        close(fds[0]);
    }
}
```

#### 8.2.6 共享内存

共享内存让多个进程访问同一块内存，速度快，但需要同步机制配合，例如信号量、互斥锁、文件锁。

常见方式：

- POSIX shared memory：`shm_open` + `mmap`
- System V shared memory：`shmget` + `shmat`
- 文件映射：`mmap`

共享内存适合高频、大数据量进程间通信，但复杂度比管道和 socket 高。

#### 8.2.7 `mmap`

`mmap` 可以把文件或匿名内存映射到进程地址空间，常用于文件读写优化、共享内存、内存池。

```cpp
#include <fcntl.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <unistd.h>

int main() {
    int fd = open("data.txt", O_RDONLY);
    struct stat st {};
    fstat(fd, &st);

    void* addr = mmap(nullptr, st.st_size, PROT_READ, MAP_PRIVATE, fd, 0);
    if (addr != MAP_FAILED) {
        // 使用 addr 读取文件内容
        munmap(addr, st.st_size);
    }
    close(fd);
}
```

注意映射长度、权限、文件生命周期和缺页带来的性能影响。

#### 8.2.8 `socket`

socket 是网络编程核心抽象。TCP 服务端基本流程是 `socket`、`bind`、`listen`、`accept`、`read/write`、`close`。

```cpp
#include <arpa/inet.h>
#include <sys/socket.h>
#include <unistd.h>

int main() {
    int listenfd = socket(AF_INET, SOCK_STREAM, 0);

    sockaddr_in addr {};
    addr.sin_family = AF_INET;
    addr.sin_addr.s_addr = htonl(INADDR_ANY);
    addr.sin_port = htons(8080);

    bind(listenfd, reinterpret_cast<sockaddr*>(&addr), sizeof(addr));
    listen(listenfd, SOMAXCONN);

    int connfd = accept(listenfd, nullptr, nullptr);
    close(connfd);
    close(listenfd);
}
```

实际服务端还要设置非阻塞、端口复用、错误处理和连接管理。

#### 8.2.9 `epoll`

`epoll` 是 Linux 高性能 IO 多路复用机制，适合管理大量连接。核心 API 是 `epoll_create1`、`epoll_ctl`、`epoll_wait`。

```cpp
#include <sys/epoll.h>
#include <unistd.h>

int main() {
    int epfd = epoll_create1(0);

    epoll_event ev {};
    ev.events = EPOLLIN;
    ev.data.fd = 0; // 监听标准输入作为示例
    epoll_ctl(epfd, EPOLL_CTL_ADD, 0, &ev);

    epoll_event events[16];
    int n = epoll_wait(epfd, events, 16, 1000);

    close(epfd);
}
```

需要理解 LT 和 ET 模式：LT 更容易写正确，ET 性能潜力更高但必须配合非阻塞 IO 并读到 `EAGAIN`。

#### 8.2.10 定时器

服务端需要定时器处理连接超时、心跳、周期任务。Linux 可用 `timerfd` 把定时器变成文件描述符，方便接入 `epoll`。

```cpp
#include <sys/timerfd.h>
#include <unistd.h>

int main() {
    int tfd = timerfd_create(CLOCK_MONOTONIC, TFD_NONBLOCK);

    itimerspec ts {};
    ts.it_value.tv_sec = 1;
    ts.it_interval.tv_sec = 1;
    timerfd_settime(tfd, 0, &ts, nullptr);

    close(tfd);
}
```

项目中也常用最小堆、时间轮实现大量连接超时管理。

#### 8.2.11 daemon

daemon 是后台服务进程。传统做法包括 `fork`、`setsid`、切换工作目录、重定向标准输入输出、设置 umask 等。现代 Linux 项目更多交给 systemd 管理，但仍要理解 daemon 基本原理。

#### 8.2.12 日志

日志是服务端排查问题的生命线。一个可用日志系统通常包含：

- 日志级别：DEBUG、INFO、WARN、ERROR
- 时间戳
- 线程 id
- 文件名和行号
- 异步落盘
- 日志滚动

简化宏示例：

```cpp
#include <iostream>

#define LOG_INFO(msg) \
    std::cout << "[INFO] " << __FILE__ << ":" << __LINE__ << " " << msg << '\n'

int main() {
    LOG_INFO("server started");
}
```

#### 8.2.13 崩溃分析

服务崩溃后要能定位问题。基本流程是：开启 core dump，复现崩溃，用 gdb 查看调用栈、线程、局部变量。

常用命令：

```bash
ulimit -c unlimited
gdb ./server core
bt
info threads
thread apply all bt
```

#### 8.2.14 core dump

core dump 是进程崩溃时的内存快照。调试时要确保二进制带调试符号，编译加 `-g`，生产环境可保留符号文件。

```bash
g++ -g -O0 main.cpp -o app
```

学习系统编程时，建议每写一个系统调用都检查返回值和 `errno`，这是 Linux C++ 工程代码的基本素养。

### 8.3 推荐资料

- 《Unix 环境高级编程》
- 《Linux 高性能服务器编程》
- man 手册

### 8.4 阶段产出

建议完成：

- 写一个 mini shell
- 写一个多进程任务执行器
- 写一个基于 `epoll` 的 echo server
- 写一个日志库
- 写一个简易定时器模块

---

## 9 第八阶段：网络编程与高性能服务

### 9.1 阶段目标

本阶段目标是面向 C++ 后端研发、基础架构、游戏服务器、交易系统等方向。

### 9.2 重点内容

网络编程学习要从 TCP/UDP 基础开始，然后逐步进入非阻塞 IO、事件循环、协议设计和服务治理。

#### 9.2.1 TCP/UDP 编程

TCP 是面向连接、可靠、有序的字节流协议；UDP 是无连接、不保证可靠和顺序的数据报协议。服务端研发最常用 TCP，实时音视频、游戏部分场景会使用 UDP。

TCP 服务端流程：

1. 创建 socket
2. 绑定 IP 和端口
3. 监听
4. 接受连接
5. 收发数据
6. 关闭连接

UDP 服务端没有连接建立过程，主要用 `recvfrom/sendto`。

#### 9.2.2 阻塞 IO

阻塞 IO 是最简单的模型：没有连接时 `accept` 阻塞，没有数据时 `read` 阻塞。它适合学习和低并发场景。

```cpp
char buf[1024];
ssize_t n = read(connfd, buf, sizeof(buf)); // 没有数据时可能阻塞
```

缺点是一个线程很容易卡在某个连接上，无法高效处理大量连接。

#### 9.2.3 非阻塞 IO

非阻塞 IO 中，如果操作暂时无法完成，系统调用立即返回，并设置 `errno` 为 `EAGAIN` 或 `EWOULDBLOCK`。

```cpp
#include <fcntl.h>

void setNonBlock(int fd) {
    int flags = fcntl(fd, F_GETFL, 0);
    fcntl(fd, F_SETFL, flags | O_NONBLOCK);
}
```

非阻塞 IO 通常必须配合 `epoll`，否则会变成低效轮询。

#### 9.2.4 IO 多路复用

IO 多路复用让一个线程同时监听多个文件描述符。常见机制：

- `select`：跨平台但 fd 数量受限
- `poll`：无固定 fd 上限，但每次线性扫描
- `epoll`：Linux 高性能方案，适合大量连接

服务端常见结构是：一个或多个 IO 线程运行事件循环，业务任务交给工作线程池。

#### 9.2.5 Reactor 模型

Reactor 是“事件到来后再处理”的模型。核心组件包括事件循环、事件分发器、连接对象、回调函数。

简化流程：

1. `epoll_wait` 等待事件
2. 事件到达后分发给对应连接
3. 可读事件触发读取
4. 可写事件触发发送缓冲区数据
5. 定时器处理超时连接

伪代码：

```cpp
while (running) {
    int n = epoll_wait(epfd, events, maxEvents, timeout);
    for (int i = 0; i < n; ++i) {
        if (events[i].events & EPOLLIN) {
            handleRead(events[i].data.fd);
        }
        if (events[i].events & EPOLLOUT) {
            handleWrite(events[i].data.fd);
        }
    }
}
```

muduo 就是典型 Reactor 风格网络库。

#### 9.2.6 Proactor 模型

Proactor 是“异步操作完成后通知应用”的模型。应用提交异步读写请求，内核或运行时完成后通知回调。Windows IOCP 更接近 Proactor；Linux 下常用 Reactor，也可以通过 io_uring 接近 Proactor 风格。

理解区别：

- Reactor：通知“可以读/可以写”，应用自己执行读写
- Proactor：通知“读写已完成”，应用处理结果

#### 9.2.7 `epoll`

`epoll` 在网络服务中通常搭配非阻塞 socket。需要掌握：

- `EPOLLIN`：可读
- `EPOLLOUT`：可写
- `EPOLLERR`：错误
- `EPOLLHUP`：挂断
- LT：水平触发
- ET：边缘触发

ET 模式读数据时要循环读到 `EAGAIN`：

```cpp
while (true) {
    ssize_t n = read(fd, buf, sizeof(buf));
    if (n > 0) {
        // append to input buffer
    } else if (n == 0) {
        // peer closed
        break;
    } else {
        if (errno == EAGAIN || errno == EWOULDBLOCK) {
            break;
        }
        // error
        break;
    }
}
```

#### 9.2.8 粘包/拆包

TCP 是字节流，没有消息边界。一次 `read` 可能读到半个包，也可能读到多个包，这就是粘包/拆包问题。解决方法是应用层协议自己定义边界。

常见方案：

- 固定长度消息
- 分隔符，例如 `\r\n`
- 长度字段 + 消息体

长度字段示例：

```cpp
struct PacketHeader {
    uint32_t bodyLength;
};

// 接收方先读 4 字节长度，再读 bodyLength 字节正文
```

#### 9.2.9 心跳机制

心跳用于检测连接是否存活。常见做法是客户端定期发送 ping，服务端回复 pong，并记录最后活跃时间。

设计要点：

- 心跳间隔不能太短，否则浪费网络和 CPU
- 超时时间通常是心跳间隔的数倍
- 业务数据也可以刷新活跃时间
- 服务端需要定时扫描超时连接

#### 9.2.10 连接管理

连接管理包括连接创建、销毁、读写缓冲区、状态、超时、错误处理。一个连接对象通常包含：

- fd
- 输入缓冲区
- 输出缓冲区
- 对端地址
- 最后活跃时间
- 当前状态
- 读写回调

注意：非阻塞写可能一次写不完，所以需要输出缓冲区和 `EPOLLOUT` 事件。

#### 9.2.11 序列化协议

序列化是把内存对象转换成字节流，反序列化是从字节流恢复对象。常见选择：

- JSON：可读性好，性能一般
- protobuf：二进制、高性能、跨语言
- flatbuffers：适合高性能场景
- 自定义二进制协议：性能高，但维护成本高

protobuf 示例：

```proto
syntax = "proto3";

message LoginRequest {
  string username = 1;
  string password = 2;
}
```

#### 9.2.12 RPC 基础

RPC 的目标是像调用本地函数一样调用远程服务。一个基本 RPC 框架通常包含：

- 服务接口定义
- 序列化
- 网络传输
- 请求 id
- 超时控制
- 错误码
- 服务注册与发现

调用流程：

1. 客户端把函数名和参数序列化
2. 通过网络发送请求
3. 服务端反序列化并调用本地函数
4. 服务端序列化返回值
5. 客户端拿到结果或超时错误

#### 9.2.13 HTTP 基础

HTTP 是文本协议，常见组成包括请求行、请求头、空行、请求体。

示例：

```http
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive

```

需要掌握：

- GET/POST
- 状态码
- Header
- Body
- keep-alive
- chunked
- HTTP/1.1 连接复用

写 HTTP server 时，重点是正确解析请求、处理半包、支持静态文件和合理错误响应。

#### 9.2.14 WebSocket 基础

WebSocket 在 HTTP 握手后升级为全双工长连接，适合聊天、实时推送、在线协作等场景。

特点：

- 先通过 HTTP Upgrade 建立连接
- 建立后是帧协议
- 支持服务端主动推送
- 需要心跳保活

WebSocket 服务端要处理握手、帧解析、掩码、ping/pong、连接关闭。

### 9.3 建议学习库

这些库建议按“先会用，再读设计，再模仿实现关键组件”的顺序学习。

#### 9.3.1 muduo

muduo 是陈硕编写的 Linux C++ 网络库，典型 Reactor 模型，适合学习高质量服务端代码设计。

重点看：

- `EventLoop`
- `Channel`
- `Poller/EPollPoller`
- `TcpConnection`
- `TcpServer`
- `Buffer`
- 线程模型

学习方式：

1. 先跑 echo server
2. 画出 Reactor 类关系图
3. 理解 one loop per thread
4. 仿写一个迷你 Reactor

muduo 的价值不只是网络 API，而是它展示了 C++ 服务端如何组织对象生命周期和回调。

#### 9.3.2 asio / boost.asio

asio 是跨平台异步 IO 库，boost.asio 使用广泛，现代 C++ 中也有 standalone asio。它支持 TCP、UDP、定时器、异步回调、协程等。

异步读的风格大致如下：

```cpp
// 示意代码，省略完整错误处理和对象生命周期管理
socket.async_read_some(buffer(data), [](const error_code& ec, std::size_t n) {
    if (!ec) {
        // process data
    }
});
```

学习重点：

- `io_context`
- `ip::tcp::socket`
- `async_read/async_write`
- strand
- timer
- buffer 生命周期

asio 的难点在对象生命周期，异步回调执行时对象必须仍然活着，常用 `shared_from_this()` 管理连接对象。

#### 9.3.3 protobuf

protobuf 是 Google 的跨语言序列化协议，适合 RPC、服务通信、数据存储。它通过 `.proto` 文件定义数据结构，然后生成 C++ 代码。

示例：

```proto
syntax = "proto3";

message User {
  int64 id = 1;
  string name = 2;
  repeated string tags = 3;
}
```

C++ 使用方式大致是：

```cpp
User user;
user.set_id(1);
user.set_name("Alice");

std::string bytes;
user.SerializeToString(&bytes);
```

重点理解字段编号、向前兼容、必填字段变化风险、二进制协议不可直接人工阅读。

#### 9.3.4 grpc

grpc 是基于 HTTP/2 和 protobuf 的 RPC 框架，适合微服务之间通信。

`.proto` 中可以定义服务：

```proto
syntax = "proto3";

service UserService {
  rpc GetUser(GetUserRequest) returns (GetUserResponse);
}

message GetUserRequest {
  int64 id = 1;
}

message GetUserResponse {
  string name = 1;
}
```

学习重点：

- service 定义
- client stub
- server handler
- deadline 超时
- status/error
- streaming

grpc 适合工程落地，但如果目标是理解网络库底层，还是要先学 socket、epoll、Reactor。

#### 9.3.5 brpc

brpc 是百度开源的工业级 RPC 框架，国内 C++ 后端和基础架构方向可以了解。它支持多协议、高并发、服务治理相关能力。

建议了解：

- RPC 调用模型
- bthread
- naming service
- load balance
- timeout/retry
- builtin service

brpc 不建议作为网络编程入门第一站，更适合在掌握基本网络模型和 RPC 概念后，用来学习工业级框架设计。

### 9.4 阶段产出

建议完成：

- 基于 `epoll` 写一个 Reactor 网络库
- 实现简单 HTTP server
- 实现聊天室
- 使用 protobuf 定义协议
- 做一个 RPC demo

---

## 10 第九阶段：数据库、缓存与中间件

### 10.1 阶段目标

本阶段目标是让你的 C++ 服务能接真实业务。很多 C++ 岗位不只写算法或底层模块，也要和数据库、缓存、消息队列、RPC 协议打交道。

### 10.2 重点内容

需要学习：

- MySQL 基础
- Redis 基础
- 连接池
- 线程池
- 缓存一致性
- 消息队列基础
- 序列化和反序列化
- 配置中心思想
- 服务发现了解即可

### 10.3 阶段产出

建议完成：

- C++ 封装 MySQL 连接池
- C++ 封装 Redis 客户端调用
- 写一个短链接服务
- 写一个在线聊天室，用户状态放 Redis

---

## 11 第十阶段：性能优化与底层能力

### 11.1 阶段目标

本阶段目标是成为更有竞争力的 C++ 工程师。C++ 的优势之一就是可以控制性能，但性能优化必须基于测量，而不是凭感觉。

### 11.2 重点内容

需要学习：

- cache locality
- false sharing
- 内存对齐
- 对象布局
- 虚函数开销
- 小对象优化
- 内存池
- lock-free 基础
- profiling
- benchmark
- CPU cache
- 分支预测
- SIMD 了解

### 11.3 常用工具

```bash
perf
valgrind
gprof
Google Benchmark
heaptrack
flamegraph
```

### 11.4 阶段产出

建议完成：

- 写一个内存池
- 对比 `vector/list/deque` 遍历性能
- 用 benchmark 测试不同实现
- 用 perf 找性能热点
- 优化一个慢查询或慢计算模块

---

## 12 第十一阶段：项目实战路线

建议至少做 3 个项目，分别覆盖语言、系统、网络。

### 12.1 项目一：现代 C++ 工具库

内容：

- 日志模块
- 配置解析
- 线程池
- 定时器
- 阻塞队列
- 对象池

要求：

- CMake 管理
- GoogleTest
- clang-format
- ASan 检查
- README 写清楚设计

### 12.2 项目二：高性能网络服务器

内容：

- `epoll`
- Reactor
- 连接管理
- 线程池
- 定时器
- HTTP 解析
- 静态文件服务

进阶：

- 加 keep-alive
- 加日志
- 加压力测试
- 使用 webbench 或 wrk 测试

### 12.3 项目三：分布式或业务型项目

任选一个：

- RPC 框架 demo
- 即时聊天服务器
- 短链接系统
- 文件上传下载服务器
- 游戏房间服务器
- C++ 后端 + MySQL + Redis 项目

要求：

- 有协议设计
- 有数据库设计
- 有异常处理
- 有日志
- 有压测结果
- 有部署说明

---

## 13 推荐书单顺序

按顺序读，不要一上来读最难的。

1. 《C++ Primer》第五版
2. 《Effective C++》
3. 《STL 源码剖析》选读
4. 《深度探索 C++ 对象模型》选读
5. 《Effective Modern C++》
6. 《C++ Concurrency in Action》
7. 《Unix 环境高级编程》
8. 《Linux 高性能服务器编程》
9. 《深入理解计算机系统》选读重点章节

---

## 14 推荐时间安排

如果每天学习 3 到 4 小时，可以按下面节奏推进。

### 14.1 第 1-2 个月

学习内容：

- C++ Primer
- STL
- 类、对象、RAII
- 小项目练习

建议目标：

- 能写中等规模单机 C++ 程序
- 熟悉类、对象、构造析构、STL 容器和算法

### 14.2 第 3-4 个月

学习内容：

- Effective C++
- 现代 C++
- CMake
- GDB
- GoogleTest
- 工具库项目

建议目标：

- 能写较规范的现代 C++ 代码
- 能组织多文件项目
- 能调试和测试代码

### 14.3 第 5-6 个月

学习内容：

- 多线程
- Linux 系统编程
- socket
- epoll
- 网络服务器项目

建议目标：

- 能写多线程程序
- 能写基本 Linux 网络服务
- 能理解 Reactor、连接管理、定时器

### 14.4 第 7-8 个月

学习内容：

- MySQL
- Redis
- protobuf
- 综合项目
- 性能优化
- 面试题

建议目标：

- 有一个能写进简历并讲清楚的综合项目
- 能说明项目中的设计取舍和性能数据

### 14.5 第 9 个月以后

学习内容：

- 深入源码
- muduo
- leveldb
- brpc
- 继续优化项目
- 准备简历和面试

建议目标：

- 从“会写项目”提升到“理解优秀项目怎么设计”
- 能应对 C++ 后端、基础架构、系统开发等方向面试

---

## 15 最终应形成的能力

找 C++ 研发工作时，比较核心的是这些能力：

- 能正确管理对象生命周期
- 能写现代 C++，不用满屏裸指针
- 熟悉 STL，并知道容器适用场景
- 熟悉 CMake、gdb、Linux 编译调试
- 能写多线程代码，并知道数据竞争、死锁问题
- 能写 socket/epoll 网络程序
- 能解释 RAII、虚函数、智能指针、移动语义
- 能做性能分析，而不是凭感觉优化
- 有 1 到 2 个能讲清楚设计取舍的项目

建议从《C++ Primer》快速过一遍语言主体，同时把 `Effective C++` 的 55 条和代码练习结合起来。路线不要只读书，最关键的是每个阶段都产出一个小项目。
