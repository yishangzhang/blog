使用 GCC 和 -fdump-tree-all 选项
GCC 提供了一些选项来生成中间代码和调试信息。
可以使用 -fdump-tree-all 选项来生成详细的中间代码文件：

`g++ -std=c++11 -fdump-tree-all your_code.cpp -o your_program`
这会生成多个 .txx 文件，其中包含了编译过程中的各种中间表示。

# 网址 更便捷
[https://cppinsights.io/](https://cppinsights.io/) 

## before
```cpp
#include <iostream>

// 定义一个函数模板
template <typename T>
T add(T a, T b) {
    return a + b;
}

int main() {
    // 使用函数模板进行整数加法
    int intResult = add(3, 4);
    std::cout << "3 + 4 = " << intResult << std::endl;  // 输出 7

    // 使用函数模板进行浮点数加法
    double doubleResult = add(3.5, 4.5);
    std::cout << "3.5 + 4.5 = " << doubleResult << std::endl;  // 输出 8.0

    return 0;
}

```

## after
```cpp
#include <iostream>

template<typename T>
T add(T a, T b)
{
  return a + b;
}

/* First instantiated from: insights.cpp:11 */
#ifdef INSIGHTS_USE_TEMPLATE
template<>
int add<int>(int a, int b)
{
  return a + b;
}
#endif


/* First instantiated from: insights.cpp:15 */
#ifdef INSIGHTS_USE_TEMPLATE
template<>
double add<double>(double a, double b)
{
  return a + b;
}
#endif


int main()
{
  int intResult = add(3, 4);
  std::operator<<(std::cout, "3 + 4 = ").operator<<(intResult).operator<<(std::endl);
  double doubleResult = add(3.5, 4.5);
  std::operator<<(std::cout, "3.5 + 4.5 = ").operator<<(doubleResult).operator<<(std::endl);
  return 0;
}

```

