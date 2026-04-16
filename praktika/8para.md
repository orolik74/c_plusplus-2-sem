SFINAE (Substitution Failure Is Not An Error)

```c++
#include <type_traits>
template <class T>
typename std::enable_if<std::is_integral<T>::value, T>::type
//template<class T> = std::enable_if<std::is_integral<T>::value, T>::type
process(T value) {
    std::cout << "Integral: " << value << std::endl;
    return value;
}
```

`is_integral` $-$ это надо смотреть по ситуации менять

```c++
template<int N>
struct Factorial {
    static constexpr int value = N * Factorial<N - 1>::value;
};
template<>
struct Factorial <0> {
    static constexpr int value = 1;
};

Factorial<5>::value // происходит на этапе компиляции
```

move-сумантика (C++11)

```c++
std::vector<int> foo{
    std::vector <int> v(10000000);
    return v;
}
```

Copy Elision, RVO/NRVO позволяли не копировать то, что можно не копировать

lvalue -- int x = a; (условно то, что слева от равно) есть имя, можно взять указатель  
rvalue -- то, что условно справа  
xvalue -- то, что готово умереть(то, что после какого-то места не используется больше)

```c++
std::move() //static_cast с lvalue на rvalue
static_cast<T&&>(T&x);
```

```c++
class point {
    int* data;
    int data_size;
    point(const point& oter);
    point(point&& other);
    point& operator = (const point& 0);
    point& operator = (point&& 0);
};

point::point(point&& o) {
    data = o.data;
    data_size = o.data_size;
    o.data = nullptr;
    o.data_size = 0;
}
```

```c++
std::vector<int> foo() {
    std::vector<int> v;
    return std::move(v); // не надо, если можно, то сделается автоматически
}


std::string s = "hello";
std::string t = std::move(s); // больше нельзя использовать s
std::cout << s; // все, что угодно, но вщ может было warning или error


const std::string s = "const";
std::string t = std::move(s); // вызов копирующего конструктора

std::string s = std::move(std::string{"H"}); // честное перемещение

for (auto&& x: v) {
    std::string&& s = x; // ERROR
    int && x = 5; // OK
}
std::string x = std::move(v[1]);
```

