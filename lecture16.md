В конструкторах по умолчанию есть дефолтный конструктор,
конструктор копирования и оператор присваивания, деструктор и некоторые функции(типа размер)

`data()` -- даеет указатель на данный массив  
`.at()` -- по индексу, если дальше, чем есть элементы, то исключение

```c++
std::vector<int>().swap(v); // очистить всю память вектора до 0
v.shrink_to_fit(); // capacity -> size(капасити к сайз приводится)
```

splice() за $O(n)$

```c++
std::u8string = std::basic_string<...>
```

адаптеры контейнеров

```c++
std::stack<int, std::vector<int>>
std::queue<intm std::list<int>>
std::priority_queue<int, std::deque<int>>


std::make_heap/push_heap/pop_heap
```

`erase(it)` - удаляет элемент и возвращает итератор на следующий
`insert(it, value)` - вставляет элемент и возвращает новый инератор на него

Ассоциативные контейнеры
* Упорядоченные: set, multiset, map, multimap  
  Обычно на красно-черном дереве, отношения порядка, $O(logN)$
* Неупорядоченные  
  unordered_set, unordered_multiset, unordered_map, unordered_multimap  
  Хеш-таблицы с закрытой адресацией, в среднем $O(1)$, но $O(N)$ в худшем случае

