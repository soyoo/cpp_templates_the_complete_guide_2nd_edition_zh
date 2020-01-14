Chapter 3 Nontype Template Parameters



##### 3.1 Nontype Class Template Parameters

```c++
#include <array>
#include <cassert>

template<typename T, std::size_t Maxsize>
class Stack {
    private:
    std::array<T, Maxsize> elems; // elements
    std::size_t numElems;         // current number of elements
    public:
    Stack();                      // constructor
    void push(T const& elem);     // push element
    void pop();                   // pop element
    T const& top() const;         // return top element
    bool empty() const {          // return whether the stack is empty
        return numElems == 0;
    }
    std::size_t size() const {    // return current number of elements
        return numElems;
    }
};

template<typename T, std::size_t Maxsize>
Stack<T, Maxsize>::Stack ()
    : numElems(0)                 // start with no elements
    {
        // nothing else to do
    }

template<typename T, std::size_t Maxsize>
void Stack<T, Maxsize>::push (T const& elem)
{
    assert(numElems < Maxsize);
    elems[numElems] = elem;       // append element
    ++numElems;                   // increment number of elements
}

template<typename T, std::size_t Maxsize>
void Stack<T, Maxsize>::pop ()
{
    assert(!elems.empty());
    --numElems;                   // decrement number of elements
}

template<typename T, std::size_t Maxsize>
T const& Stack<T, Maxsize>::top () const
{
    assert(!elems.empty());
    return elems[numElems - 1];  // return last element
}
```



```c++
template<typename T, std::size_t Maxsize>
class Stack {
    private:
    std::array<T, Maxsize> elems; // elements
    ...
};
```



```c++
template<typename T, std::size_t Maxsize>
void Stack<T, Maxsize>::push (T const& elem)
{
    assert(numElems < Maxsize);
    elems[numElems] = elem;      // append element
    ++numElems;                  // increment number of elements
}
```



basics/stacknontype.cpp

```c++
#include "stacknontype.hpp"
#include <iostream>
#include <string>

int main()
{
    Stack<int, 20>         int20Stack;     // stack of up to 20 ints
    Stack<int, 40>         int40Stack;     // stack of up to 40 ints
    Stack<std::string, 40> stringStack;    // stack of up to 40 strings
    
    // manipulate stack of up to 20 ints
    int20Stack.push(7);
    std::cout << int20Stack.top() << '\n';
    int20Stack.pop();
    
    stringStack.push("hello");
    std::cout << stringStack.top() << '\n';
    stringStack.pop();
}
```



```c++
template<typename T = int, std::size_t Maxsize = 100>
class Stack {
    ...
};
```



3.2

```c++
template<int Val, typename T>
T addValue (T x)
{
    return x + Val;
}
```



```c++
std::transform (source.begin(), source.end(),   // start and end of source
               dest.begin(),                    // start of destination
               addValue<5, int>);               // operation
```



```c++
template<auto Val, typename T = decltype(Val)>
T foo();
```



```c++
template<typename T, T Val = T{}>
T bar();
```



3.3

```c++
template<double VAT>                           // ERROR: floating-point values are not
double process (double v)                      //        allowed as template parameters
{
    return v * VAT;
}

template<std::string name>
class MyClass {                               // ERROR: class-type objects are not
    ...                                       // allowed as template parameters
};
```



```c++
template<char const* name>
class MyClass {
    ...
};

MyClass<"hello"> x; // ERROR: string literal "hello" not allowed
```



```c++
extern char const s03[] = "hi";      // external linkage
char const s11[] = "hi";             // internal linkage

int main()
{
    Message<s03> m03;                // OK(all versions)
    Message<s11> m11;                // OK since C++11
    
    static char const s17[] = "hi";  // no linkage
    Message<s17> m17;                // OK since C++17
}
```



```c++
template<int I, bool B>
class C;
...
C<sizeof(int) + 4, sizeof(int)==4> c;
```



```c++
C<42, sizeof(int) > 4> c;    // ERROR: first > ends the template argument list
C<42, (sizeof(int) > 4)> c;  // OK
```



3.4

```c++
#include <array>
#include <cassert>

template<typename T, auto Maxsize>
class Stack {
    public:
    using size_type = decltype(Maxsize);
    private:
    std::array<T, Maxsize> elems; // elememts
    size_type numElems;           // current number of elements
    public:
    Stack();                      // constructor
    void push(T const& elem);     // push element
    void pop();                   // pop element
    T const& top() const;         // return top element
    bool empty() const {          // return whether the stack is empty
        return numElems == 0;
    }
    size_type size() const {      // return current number of elements
        return numElems;
    }
};

// constructor
template<typename T, auto Maxsize>
Stack<T, Maxsize>::Satck ()
    : numElems(0)                 // start with no elements
    {
        // nothing else to do
    }

template<typename T, auto Maxsize>
void Stack<T, Maxsize>::push (T const& elem)
{
    assert(numElems < Maxsize);
    elems[numElems] = elem;      // append element
    ++numElems;                  // increment number of elements
}

template<typename T, auto Maxsize>
void Stack<T, Maxsize>::pop ()
{
    assert(!elems.empty());
    --numElems;                 // decrement number of elements
}

template<typename T, auto Maxsize>
T const& Stack<T, Maxsize>::top () const
{
    assert(!elems.empty());
    return elems[numElems - 1]; // return last element
}
```



```c++
template<typename T, auto Maxsize>
class Stack {
    ...
};
```



```c++
std::array<T, Maxsize> elems; // elements
```



```c++
using size_type = decltype(Maxsize);
```



```c++
size_type size() const {    // return current number of elements
    return numElems;
}
```



```C++
auto size() const {        // return current number of elements
    return numElems;
}
```



```C++
#include <iostream>
#include <string>
#include "stackauto.hpp"

int main()
{
    Stack<int, 20u>       int20Stack;    // stack of up to 20 ints
    Stack<std::string, 40> stringStack;   // stack of up to 40 strings
    
    // manipulate stack of up to 20 ints
    int20Stack.push(7);
    std::cout << int20Stack.top() << '\n';
    auto size1 = int20Stack.size();
    
    // manipulate stack of up to 40 strings
    stringStack.push("hello");
    std::cout << stringStack.top() << '\n';
    auto size2 = stringStack.size();
    
    if (!std::is_same_v<decltype(size1), decltype(size2)>) {
        std::cout << "size types differ" << '\n';
    }
}
```



```c++
Stack<int, 20u> int20Stack;                 // stack of up to 20 ints
```



```c++
Stack<std::string, 40> stringStack;        // stack of up to 40 strings
```



```c++
auto size1 = int20Stack.size();
...
auto size2 - stringStack.size();
```



```C++
if (!std::is_same_v<decltype(size1), decltype(size2)>::value) {
    std::cout << "size types differ" << '\n';
}
```



```c++
if (!std::is_same_v<decltype(size1), decltype(size2)>) {
    std::cout << "size types differ" << '\n';
}
```



```c++
Stack<int, 3.14> sd; // ERROR: Floating-point nontype argument
```



```c++
#include <iostream>

template<auto T>   // take value of any possible nontype parameter(since C++17)
class Message {
    public:
    void print() {
        std::cout << T << '\n';
    }
};

int main()
{
    Message<42> msg1;
    msg1.print();        // initialize with int 42 and print that value
    
    static char const s[] = "hello";
    Message<s> msg2;     // initialize with char const[6] "hello"
    msg2.print();        // and print that value
}
```



```C++
template<decltype(auto) N>
class C {
    ...
};
int i;
C<(i)> x; // N is int&
```



