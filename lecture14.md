При template подстановку делает компилятор, а не процессор

Код шаблонного класса должен быть в заголовочном файле, потому что мы заранее не знаем,
какие типы под T можем подставить

* Методы шаблонного класса всегда inline
* `template <typename T>` и `template<class T>` ничем не отличается
* Класс с подставленным типом - специализация шаблона
* Можно делать using
* Инстанцирование

```c++
template <typename T>
void swap(T& a, T& b){
    T temp = a;
    a = b;
    b = temp;
}

int main(){
    int i = 10;
    int j = 20;
    swap<int> (i, j);
}
```

Шаблонные функции тоже должны быть в заголовочных файлах

Типы аргументов могут зависеть от шаблонных параметров.
Можно написать просто swap(i, j), если типы i и j понятны

```c++
template <size_t S>
class Bitset{
    uint8_t data[(S + 7) / 8];
};
```

```c++
template<typename T, template <typename> class Container> // class == typename
struct Stack{
    Container<T> data;
    public:
    ...
};

Stack<int, Array> stack1;
Stack<double, List> stack2;
```

Можно сделать значение параметра по умолчанию:
```c++
template <typename T, template <typename> class Container = Deque>
```

```c++
template <typename T>
class Array{...};
template<>
class Array<bool>{
//    если T == bool, то можем как-то по-другому что-то делаетс
}
```

`std::expected`

```c++
class DivByZeroException{
public:
    const char* what(){
        return "division by zero"; 
    }    
};

...

try {
    c = divide(a, b);
}
catch (DivByZeroException& e){

}
```
Исключения летят по стеку вызова функции

`thow;` - кидает исключение дальше

Когда мимо локальной переменной пролетает исключение, то оно вызывает деструктор

```c++
AudioManager() try: input(fopen("meow.wav", "rb")), effect("wrum-wrum"){}
catch(...){
    fclose(input);
    throw;
}
```

RAII - топ, если можно, то желательно так

Исключение в деструкторе - это аварийное завершение программы

По умолчанию нет стектрейса

