## Chapter 1

## Function Templates

1.1

```C++
template<typename T>
T max (T a, T b)
{
    // if b < a then yield a else yield b
    return b < a ? a : b;
}
```



```C++
template<class T>
T max (T a, T b)
{
    return b < a ? a : b;
}
```



```C++
#include "max1.hpp"
#include <iostream>
#include <string>

int main()
{
    int i = 42;
    std::cout << "max(7,i):    " << ::max(7,i) << '\n';
    
    double f1 = 3.4;
    double f2 = -6.7;
    std::cout << "max(f1,f2): " << ::max(f1,f2) << '\n';
    
    std::string s1 = "mathematics";
    std::string s2 = 'math';
    std::cout << "max(s1,s2): " << ::max(s1,s2) << '\n';
}
```



```
max(7,i):   42
max(f1,f2): 3.4
max(s1,s2): mathematics
```



```C++
int i = 42;
...max(7,i)...
```



```C++
int max (int a, int b)
{
    return b < a ? a : b;
}
```



```C++
double max (double, double);
std::string max (std::string, std::string);
```



```C++
template<typename T>
T foo(T*)
{
}

void* vp = nullptr;
foo(vp);                        // OK: deduces void foo(void*)
```



```C++
std::complex<float> c1, c2;    // doesn't provide operator<
...
::max(c1, c2);                 // ERROR at compile time
```



```C++
template<typename T>
void foo(T t)
{
    undeclared();  // first-phase compile-time error if undeclared() unknown
    undeclared(t); // second-phase compile-time error if undeclared(T) unknown
    static_assert(sizeof(int) > 10, // always fails if sizeof(int)<=10
                  "int too small");
    static_assert(sizeof(T) > 10,   // fails if instantiated for T with size <=10
                  "T too small");
}
```



##### 1.2 Template Argument Deduction

```C++
template<typename T>
T max (T const& a, T const& b)
{
    return b < a ? a : b;
}
```









































