Если `A::operator B()` и `B::B(A)`, то это CE при `(B)a`

Лучше не делать c-style cast, так как он много чего умеет

`static_cast<T>(x)`
* Все, что умеют неявные приведения типов
* Приведение указателя на базовый класс к указателю на наследника (downcasting)
* Приведение указателя на void к другому типу указателя
* Легче увидеть в коде, понятнее, что хотели сделать

`reinterpret_cast<T>(x)`
* Приведение между указателями любых типов
* Приведение между ссылками любых типов
* Приведение между указателями и числами
* обычно нужен в низкоуровневом коде

`const_cast<T>(x)`

* Позволяет убрать/добавить константность у объекта
```c++
    const Value& ay(const Key& key) {
        //...
    }

    Value& at(const Key& key) {
        return const_cast<Value&> (const_cast<const map&>(*this).at(key));
    }
```

`dynamic_cast<T>(x)`
* Безопасное приведение указателя/ссылки на базовый класс к указателю на дочерний класс
* Если приведение некорректно, возвращается нулевой указатель, либо (в случае приведения типов ссылок)
бросается исключение `std::bad_cast`

#### Как работает dynamic_cast?
* Переходит по указателю на таблицу виртуальных функций, рядом с ней компилятор кладет 
мета инфу о типе
* dynamic_cast работает только для классов с виртуальными методами (хотя бы с 1)
* RTTI Run-Time Type Information

`std::type_info` в `<typeinfo>`
Позволяет узнать немного информации о типе объекта - вывести имя после менглинга
или посчитать хеш

```c++
#include <typeinfo>
#include <iostream>

int main() {
    int i = 10;
    std::cout << typeid(i).name() << std::endl;
    std::cout << typeid(std::ostream&).name() << std::endl;
}
```

typeid + RTTI (если включен RTTI, то вернет тип настоящего объекта)

Есть класс-обертка `type_index`, фактически хранит указатель на `typeid`

Имя до менглинга можно получить, в библиотеке Boost
```c++
#include <boost/core/demangle.hpp>
#include <iostream>

int main() {
    using namespace boost::core;
    std::cout << demangle(typeid(int).name()) << std::endl; 
}
```

