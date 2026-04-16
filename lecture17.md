класс, у которого перегружен оператор вызова `operator()`
```c++
struct add {
    int operator()(int x, int y) const { // const, очев, не обязательно
        return x + y;
    }
};
add func;
int x = func(3, 4); // x = 7
```
функциональный объект

Похожи на функции, но могут хранить дополнительные данные.  

Функциональные объекты, которые возвращают `bool`, называется предикатами  
1 аргумент -- унарный  
2 аргумента -- бинарный

```c++
swap(T& x, T& y);
iter_swap(It p, It q); //своп того, на что указывают итераторы
clamp(const T& x, const T& min, const T& max) // ограничит значение диапазоном [min, max]
//у минимума и максимума есть версия с компаратором
```
```c++
count(It begin, It end, const T& x); // сколько раз в посл. встречается x
count_if(It begin, It end, UnaryPred); //сколько раз в посл., удовлетв. усл
all_of(It begin, It end, UnaryPred pred); // правда ли, что все элементы удовл
any_of(It begin, It end, UnaryPred pred); // правда ли, что хотя бы 1 элемент удовл
none_of(It begin, It end, UnaryPred pred); // правда ли, что все элементы не удовл

equal(It1 begin1, It1 end1, It2 begin2); // совпадают ли последовательности 
//(длина должна быть равной)
equal(It1 begin1, It1 end1, It2 begin2, It2 end2);// то же самое, 
//но разной длины не совпадают
pair<It1, It2> mismatch(It1 begin1, It1 end1, It2 begin2);//вернуть первое несовп
//в том, где есть begin и end при разной длине {end, it}, где it это место, где ост.
lexicographial_compare(It1, It1, It2, It2)//сравнить две последовательности лексикографически
It find(It begin, It end, const T& x)// найти первое вхождение x
It find_if(...)//очев
It min_element(It begin, It end);//найти минимальный
It max_element(...)
pair<It, It> minmax_element(..)//очев, что делает
```

```c++
fill(It begin, It end, const T& x)// заполнить x
generate(It begin, It end, Generator gen) // заполнить последовательность 
//результатами вызова get
copy(ItIn src_begin, ItIn src_end, ItOut dst_begin)
//copy знает про то, что ты делаешь, memcpy не знает
//copy 
copy_if(..)//очев
reverse(all(a)) // очев
rotate(It begin, It middle, It end)// циклический сдинуть элементы последовательности
//middle окажется в начале
transform() //как copy, только с унарной операцией

sort(all(a));
stable_sort(all(a));//стабильная сортировка
partial_sort(begin, middle, end)// сдвинуть младшие middle - begin элементов 
//последовательности в начало

lower_bound(all(a), x);//>=
upper_bound(all(a), x);//>
```
`iterator traits` -- стандартный шаблонный класс, параметризующийся типом итератора,
и хранящий некую информацию о нем

Есть частичная специализация для указателей

```c++
template<typename It>
void quick_sort(It begin, It end) {
    using traits = std::iterator_traits<It>;
    const typename traits::value_type& pivot = ...;
}
```
проще использовать `auto`

```c++
next(it)
next(it, n)
advance(it, n) // сдвинуть it на n
distance(it1, it2) // на сколько нужно сдвинуть it1, чтобы получить it2
```

Категории итераторов
1. Forward iterator: *it ++it
2. Bidirectional iterator: --it
3. Random access iterator: it += n, it -= n

Есть классы-теги для определения категории итераторов
`iterator_category`

```c++
template <typename It>
It next(It it, int n, std::bidirectional_iterator_tag) {
    ...
}
```
потом просто перегружаем в функцию из 2 аргументов

```c++
while (n --> 0) {}
```