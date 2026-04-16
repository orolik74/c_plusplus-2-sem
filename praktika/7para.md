30.03.26

```c++
#include <type_traits>
static_assert(std::is_default_constructible_v<s1> == true, "message: broo, use norm types");
_v, _t (::value, ::type)
struct s1 {
    std::string str;
};
```

```c++
typename T::x * y;
```

Должны оставаться верными инварианты класса

* Базовая гарантия: состояние может измениться, но оно корректное (в том числе про утечки памяти)
* Сильная (строгая) гарантия: если что-то идет не так, то оно остается тем же
* Гарантия отсутствия исключений: вообще нет исключений

>copy-and-swap гарантирует строгую гарантию

namespaces:
```c++
namespace n{
    inline namespace v1{}
    namespace v2{}
    namespace v3{}
}
n::foo
n::v1::foo
n::v2::foo
```

```c++
//До 11 стандарта
void print() {
    std::cout << std::endl;
}

template <typename T, typename ... Args> // typename(class)... - любое количество любого типа?
void print(T first, Args... args) {
    std::cout << first << " ";
    print(args...); // раскрытие с запятыми
}

//main.cpp
print(1, "Hi", true, 1.23);

```

Каждый раз создает новые функции для каждого этапа рекурсии

```c++
//C++14/11
template<class Args>
void printAllLegacy(Args... args) {
    int dummy[] = {0, (std::cout << args << " ", 0)...};
    // 0 чтобы было))
    // (std::cout << args << " ", 0) -- первое вычисляется что-то, 
    // но подставляется 0 через оператор запятая
    // ... показывает в какую сторону раскрывать код
    (void) dummy; // подавление warning
}
```

Унарная свертка справа
```c++
//C++17 +
template<class... Args>
auto sumRight(Args... args) {
    return (args + ...); // (arg1 + (arg2 + (arg3 + ...)))
}

template <class... Args>
auto sumLeft(Args... args) {
    return (... + args); // (((arg1 + arg2) + arg3) + ...)
}

template <class... Args>
auto sumWithInit(Args... args) {
    return (0 + ... + args); // (((0 + arg1) + arg2) + ...)   
}
```

```c++
template<typename... Args>
bool allTrue(Args... args) {
    return (true && ... && args); // (true && arg1 && arg2 && ...)
}
```

CRTP - Curiously Recurring Template Pattern

```c++
template<class Derived>
class Shape {
public:
    void draw() const {
        static_cast<const Derived *> (this) -> drawImppl();
    }
};

class Circle: public Shape<Circle> {
public:
    void drawImpl() const {std::cout << "Circle" << std::endl;}
};

class Square: public Shape <Square> {
public:
    void drawImpl() const {std::cout << "Square" << std::endl;}
};

template<class T>
void draw Shape(const Shape<T>& s) {
    s.draw();
}
```

```c++
template <cladd T>
auto toString(const T& value) {
    if constexpr (std::is_arithmetic_v<T>) {
        return std::to_string(value);
    }
    else if constexpr (std::is_same_v <T, std::string>) {
        return value;
    }
    else {
        return std::string("Not supported");
    }
}
```

