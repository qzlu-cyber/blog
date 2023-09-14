---
id: cpp-bind
slug: /cpp-bind
title: std::bind
authors: Kaesar
---
**`bind`** 用于将函数与其参数绑定在一起，生成一个可调用的对象（通常是函数对象或者函数指针）**`std::bind`** 的使用主要是为了实现函数对象的 **参数绑定** 、**部分参数传递**或者 **参数重排序** 。

**`std::bind`** 需要传递一个函数指针或者函数对象来进行绑定。

用法：

```cpp
template< class F, class... Args >
std::bind(F&& f, Args&&... args);

#include <iostream>
#include <functional>

int add(int a, int b) {
    return a + b;
}

int main() {
    // 使用 std::bind 绑定 add 函数的第一个参数为 10
    auto addTen = std::bind(add, 10, std::placeholders::_1);

    // 调用绑定后的函数对象
    int result = addTen(5); // 相当于调用 add(10, 5)
    std::cout << "Result: " << result << std::endl; // 输出 "Result: 15"

    return 0;
}
```

**`std::bind`** 在 Kaesar 中的应用：

```cpp
Application.cpp

#define BIND_EVENT_FN(x) std::bind(&Application::x, this, std::placeholders::_1)

void Application::OnEvent(Event& e)
{
    EventDispatcher dispatcher(e);
    dispatcher.Dispatch<WindowCloseEvent>(BIND_EVENT_FN(OnWindowClose)); // 检查事件 e 的 EventType 是否是 WindowClose，如果是就执行 OnWindowClose 函数
		...
}

bool Application::OnWindowClose(WindowCloseEvent& e)
{
    m_Running = false;
    return true;
}
```

> Q：为什么 **`std::bind`** 里 **`Application::x`** 前要加 & 符号？

> A：****由于 **`Application::x`** 是一个成员函数，如果不加 **`&`** 符号，它将被解释为成员函数的名字而不是指向该成员函数的指针。而 **`std::bind`** 需要一个指向成员函数的指针来正确地绑定成员函数。所以，** `&Application::x`** 会生成一个指向 **`Application`** 类中成员函数 **`x`** 的指针，然后 **`std::bind`** 将这个指针与后面的参数一起使用，创建一个可调用对象。

> Q：为什么要和 **`this`** 绑定？

> A：因为 **`Application::x`** 是一个成员函数，它需要通过一个类的实例（即对象）来调用。当使用 **`std::bind`** 绑定成员函数时，需要提供一个合适的对象来调用该函数。通过将 **`this`** 作为绑定的参数，将 **`Application::x`** 与当前对象绑定在一起，使得在调用生成的可调用对象时，成员函数 **`Application::x`** 能够正确地操作当前对象的成员变量和方法。

> Q：为什么第一个例子中 **`std::bind`** 里 `add` 前没有加 & 呢？

> A：因为 **`std::bind`** 在处理普通函数（ **非成员函数** 、**全局函数**或 **静态成员函数** ）时，会自动进行函数指针的转换，不需要手动添加  **`&`** 。

所以，简言之：

* 对于普通函数（全局函数或静态成员函数），不需要加  **`&`** ，直接使用函数名即可。
* 对于成员函数，需要加 **`&`** 来取得成员函数的指针，并通过 **`std::bind`** 来绑定该成员函数和对应的对象（通常是  **`this`** ）。
