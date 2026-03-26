## Статические методы, паттерны проектирования, синглтоны, зависимости, автотестирование

### inline
Метод, определенный прямо в определении класса, по умолчанию inline
Метод, определенный снаружи тоже можно сделать inline

static поле в классе - на все экземпляры класса  
Методв класса тоже могут быть статическими, нет this, как обычная функция, только с static полям доступ  

static меод можно вызвать через класс или через объект
```c++
o1.count();
Object::count();
```

### singleton
Хотим только один объект класса  
Конструктор приватный, копирование надо запретить  
Сам экземпляр надо где-то хранить и тд

Классический соинглтон Майерса
```c++
class Settings{
private:
    Settings(); //конструктор
public:
    
    Settings(const Settings&) = delete;
    Settings& operator =(const Settings&) = delete;
    
    static Settings& instance(){
        static Settings settings;
        return settings;
    }
}
```

шаблонный singleton

```c++
template<typename T>
class Singleton{
private:
    Singleton(); //конструктор
public:
    
    Singleton(const Settings&) = delete;
    Singleton& operator =(const Settings&) = delete;
    
    static T& instance(){
        static T singleton;
        return singleton;
    }
}
```

НО!! ограничений на T не наложили

```c++
class Settings: public Singleton<Settings>{
private:
    friend class Singleton<Settings>;
    Settings() = default;
public:
    void loadFromFile(...){...}
};
```

Прием наследования класса `class A: public Base<A>` CRTP - Curiously Recurring Template Pattern
___
Если f() внутри вызывает g(), то f зависит от g, аналогично про классы  
Хотим, чтобы зависимостей было меньше
Но, хотим, чтобы зависимостей было больше

Если 2 части программы зависят друг от друга, говорят о циклической зависимости

Это почти всегда плохо, так как нельзя использовать по отдельности

Можно из объединить или выделить общую часть

### Model View паттерны
MVC, MVP, MVVM

### Тестирование
