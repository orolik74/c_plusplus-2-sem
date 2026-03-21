```c++
std::cout << std::hex << std::setw(8) << std::setfilll('0') << x;
```

```c++
#include <iostream>
#include <fstream>
#include <stringstream>
#include <format>

fmt::format("{}", %z/%ull)
```


```c++
std::fstream f("input.txt", std::ios::in);
uint32 u32;
std::string s;

f >> u32 >> s;

std::ostream& operator << (std::ostream& s, T x);
(f << x) << y;
```

```c++
std::cout << "Hi" << std::endl; // "\n" или "\r\n"
s >> x;
std::getline();
```

```c++
in - чтение
out - запись
app - добавить в конец
binary - двоичный режим
trunc - очистить файл при открытии
ate - встать в конец при открытии
```