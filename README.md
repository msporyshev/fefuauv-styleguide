# **Соглашение о стиле написания кода** 

Здесь вы найдете рекомендации по написанию кода на языке C/C++, принятые для разработки программного обеспечения для автономных необитаемых водных/подводных аппаратов, разрабатываемых в лаборатории необитаемых подводных аппаратов и их систем Дальневосточного Федерального Университета (ДВФУ).

Для разработки ПО в рамках данного проекта необходимо придерживаться нижеописанных соглашений. 

Если у вас возник вопрос, ответ на который здесь не описан, руководствуйтесь здравым смыслом, своей фантазией и настроением людей, читающих ваш код.

## **Обозначения** 

1. CamelCase -- имя переменной начинается с заглавной буквы. Каждое последующее слово в имени переменной также начинается с заглавной буквы. Например: MyName.

2. camelCase -- имя переменной начинается с прописной буквы. Каждое последующее слово в имени переменной начинается с заглавной буквы. Например: myName.

3. under_score -- имя переменной начинается с прописной буквы. Каждое последующее слово отделено от предыдущего символом подчеркивания («_») и начинается с прописной буквы. Например: my_name.

4. ALL_CAPITALS -- все буквы в имени переменной являются заглавными. Каждое последующее слово отделено от предыдущего символом подчеркивания («_»). Например: MY_NAME.

## **Именования** 
### **Имена файлов** 

* Все конфигурационные файлы и файлы исходного кода именуются в стиле under_score.
* Файлы описания сообщений именуются в стиле CamelCase (так как генератор ROS в точности также будет именовать соответствующие им классы) и имеют расширение .msg. Например: `MyMessage.msg`
* Файлы с исходным кодом имеют расширение .cpp. Например: `my_source_file.cpp`
* Заголовочные файлы имеют расширение .h. Например: `my_header_file.h`

Давайте файлам информативные имена. Не стоит создавать файлы с названиями source.cpp, new_file.h или awesome_util.h. Исключение составляет файл с функцией main, который допускается именовать либо main.cpp, либо module_name.cpp.

### **Имена переменных**
Переменные именуются в стиле under_score.

Давайте переменным имена, отображающие сущность того, что в себе хранит эта переменная, исключением могут быть итераторы и счетчики в циклах.
```c++
int message_id;    
int object_count;            
int contour_num;   
```

#### **Имена констант**
* Константные указатели и ссылки именуем, как переменные. 
* Параметры, считанные из конфигурационного файла именуем, как переменные.

```c++
const MyClass* object_ptr;            
const MyClass& instance = other_instance;
int window_height = cfg.read("window_height");
```

Константы именуем в стиле ALL_CAPITALS. Допускается именовать константы, как переменные в случае, если их значение не известно на этапе компиляции.

```c++
const int FRAME_OFFSET = 100;    
const double size_ratio = cfg.read("size_ratio");
```

#### **Имена полей классов**
Паблик поля классов именуем как обычные переменные и константы. Прайвет или протектед поля именуем в стиле under_score_ -- с завершающим нижним подчерком.

```c++
 class TableInfo 
 {
 public:
     int selected_row;
     /*...*/
 private:
     string table_name_;  
     string tablename_;   
     static Pool<TableInfo>* pool_;  
 };
```

#### **Глобальные переменные**
Глобальные переменные именуем также, как обычные.

### **Имена пользовательских типов** ###

Все объявленные программистом типы данных (классы, структуры, тайпдефы, енумы и параметры шаблонов) именуются в стиле CamelCase. *Аббревиатуру предпочтительно считать одним словом и именовать маленьким буквами, кроме первой.* Пример из Google Style Guide:
```c++
// classes and structs
class UrlTable { /*...*/ };
class UrlTableTester { /*...*/ };
struct UrlTableProperties { /*...*/ };

// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// enums
enum UrlTableErrors { /*...*/ };

```
Давайте классам имена, отражающие сущность, которую они представляют. Имя класса -- имя существительное. 

### **Имена функций**
Функции именуем в стиле under_score. Предпочтительно использовать глаголы в качестве имен функций. Аргументы именуем как обычные переменные.
```c++
void get_name(int id) { /*...*/ }

class Runner 
{
    void run();
};
```

### **Пространства имен** ###

Пространства имен именуются в стиле under_score.

```c++
using namespace my_namespace;
```

### **Перечислимые типы** ###

Перечисления именуются в стиле CamelCase. 

Поля перечисления именуются в стиле ALL_CAPITALS во избежания коллизий имен (констант в нашем коде не так много). Допускается использование перечислимых типов в качестве констант.

Например:

```c++
enum Color 
{
    GREEN,
    RED,
    YELLOW,
};

enum VisionConstants {
    FRAMES = 100,
    POOL_SIZE = 2,
};
```

По-тихоньку переходим на использование `enum class` в качестве перечислымых типов, это максимально уменьшает вероятность коллизий имен. Также отпадает необходимость именования в стиле ALL_CAPITALS. Поэтому при использовании `enum class` и его полей в качестве перечислимого типа (не в качестве констант)допускается писать его поля в стиле CamelCase.
```c++
enum class Color 
{
    Red,
    Green,
    Blue,
};

Color line_color = Color::Red;
```

### **Макросы** 

Макросы именуются в стиле ALL_CAPITALS.

Используйте макросы только в том случае, если действительно не можете без них обойтись.

Например:

```c++
#ifdef SURFACE_VEHICLE
/* лучше так не делать */ 
#endif
```

## **Форматирование** ##

### **Общие правила**
* Длина строки составляет не более 120 символов. (Проверять никто не будет, сдать вас может только человек с самым маленьким экраном ноутбука в команде.)
* Вместо табуляций для отступа следует использовать пробелы. Отступ составляет 4 пробела. 
* Все файлы в кодировке utf-8.
* Никогда не ставим отступ или пробел после открывающейся круглой скобки и перед закрывающейся


### **Определения, объявления и вызовы функций**
* Допускается писать "однострочные" функции, если они содержат одну инструкцию и укладываются в оговоренный размер строки.
* После списка аргументов функции идет перенос строки (а не фигурная скобка)
```c++
int short_func() { return something; }

ReturnType ClassName::function_name(Type par_name1, Type par_name2) 
{
    DoSomething();
    /*...*/
}
```
* Если список аргументов не влезает в оговоренный размер строки, допускается выполнять перенос необходимое количество раз, начиная с любого аргумента.
* Перенесенный список аргументов отделяется как минимум **двумя отступами**, чтобы отделить его визуально от кода функции.
* Аналогичные правила форматирования для вызова функции с несколькими аргументами.
```c++
ReturnType ClassName::function_name(Type par_name1, Type par_name2,
        Type par_name3, Type par_name4)  
{ 
    DoSomething();
    /*...*/
}

ReturnType ClassName::function_name(
        Type par_name1, 
        Type par_name2,
        Type par_name3,
        Type par_name4)  
{ 
    DoSomething();
    /*...*/
}


DoSomething(
        arg1, arg2, arg3);
DoSomething(arg1, 
        arg2, arg3);
DoSomething(arg1, 
        arg2, 
        arg3);

```

Прочие правила:
* Никогда не ставим отступ между названием функциии и открывающейся круглой скобкой
* Никогда не переносим открывающуюся круглую скобку после названия функции

### **Операторы**
#### **Операторы ветвлений и циклов**
+ В операторах ветвлений и циклов открывающуюся фигурную скобку оставляем да одной строке с оператором.
+ Фигурные скобки ставим в любом случае, даже если внутри находится только одна инструкция.
+ Допускается писать конструкции else if на одной строке без фигурных скобок после else.
```c++
if (condition) {  
  ...  
} else if (...) {  
  ...
} else {
  ...
}

for (int i = 0; i < 10000; ++i) {
    printf("I love you\n");
}
```
#### **switch**
+ case внутри оператора switch не отделяются отступом
+ Допускается не ставить скобки внутри case если это необходимо
+ Форматирование подрядыдущих кейзов без брейков -- произвольным образом
+ default обязателен

```c++
switch(var) {
case 0: case 1: case 2: {
    do();
    break;
}
case 4: {
    do_something_else();
    break;
}
default: {
    /*...*/
}
}

```

#### **Операторы выражений**
+ Ставим пробелы вокруг бинарных операторов, кроме `.` и  `->`. В случае оператора `,` пробел ставится только после него.
+ Ставим пробелы в тернарном операторе ` ? : `.
+ не ставим пробелов между унарным оператором и его операндом.
```c++
x = *p;
p = &x;
x = r.y;
x = r->y
y = x + z--;
y = x == 0 ? 1 : 0;
```

### **Пространства имен**
+ Открывающаяся фигурная скобка находится в одной строке с именем неймспейса
+ Отступ внутри неймспейса не делается
+ Рекомендуется добавлять коментарий с именем неймспейса, который закрываем в строке с закрывающейся фигурной скобкой
+ Неймспейсы не отделяются друг отдруга пустой строкой, зато отделяются ей от всего остального.

```c++
namespace foo {
namespace bar {
namespace {

void foo() {  
  ...
}

} // namespace

/*...code...*/

} // namespace bar
} // namespace foo
```

### **Классы**
+ Перенос строки перед открывающейся фигурной скобкой обязателен
+ Необходимо начинать описание класса с паблик полей
+ Для спецификаторов области видимости полей класса отступ не делается.
```c++
class MyClass 
{
public:
    MyClass(): var_(0) 
    {
        ...
    }

    void foo() { ... }

protected:
    void protected_method() 
    {
        ...
    }
private:
    int var_;
}
```

## **Общие указания по написанию кода**
+ Пишем слово `const` везде, где его только можно написать. (Речь об указателях и ссылках)

### **Инклуд гарды в хедерах**

Вместо инклуд гардов в хедерах следует всегда использовать директиву `#pragma once.`

### **Исключения**
Механизм исключений предпочтителен над использованием кодов возврата.
Соответственно исключение может быть брошено везде, кроме кода низкоуровневых драйверов устройств.

Поэтому необходимо писать безопасный относительно исключений код. А именно:
+ Никаких "голых" указателей. Используем shared_ptr или weak_ptr
+ Любой объявленный деструктор должен быть виртуальным
+ Код, который необходимо выполнить в конце функций (в духе `finalize_smth()` ) нобходимо заворачивать либо в SCOPE_EXIT, либо в деструктор любого объекта, созданного на стеке.
+ Не следует бросать исключений в конструкторах и деструкторах

### **Структуры или классы?**
Используем структуры в том и только том случае, если все их поля будут паблик и все функции будут "однострочными". Иначе используем класс.

### **Модули**
+ Хедер файл начинается с `#pragma once`
+ Далее идут инклуды модулей внешних библиотек или других рос пакетов, обернутые в треугольные скобки. 
+ Далее идут инклуды локальных модулей, обернутые в двойные кавычки 
+ Далее идут неймспейсы и классы

Запрещено использовать ключевое слово `using` в хедерах!, а также заводить в них алиасы для неймспейсов.
```
// myclass.h
#pragma once

#include <iostream>
#include <vector>
#include <opencv/core.h>
#include <opencv/highgui.h>
#include <config_reader/yaml_reader.h>

#include "my_other_class.h"
#include "my_local_utils.h"

/*...class declaration... */
```

+ В .cpp файле разрешается использовать как `using`, так и алиасы неймспейсов.
```c++
// myclass.cpp
#include <vector>

#include "my_class.h"

using std::vector;
using namespace cv;

namespace msg = ros::geom_msgs;

```
+ Вместо статических переменных и функций в модуле рекомендуется исользовать анонимные неймспейсы. Таким образом вместо этого:
```c++
static void foo() { ...}
static int global_var;
```
Пишем это:
```c++
namespace {
    void foo() { ...}
    int global_var;
} // namespace
```


### **Определения классов**

+ Реализацию функций класса выносим из хедера. Оставляем только inline функции.
+ inline функции реализуем сразу внутри класса.
+ Если метод объекта не изменяет его полей, используем `const` в его определении
+ Не используем слово `mutable`
+ Используем explicit перед всеми конструкторами классов, имеющими ровно 1 аргумент без значения по умолчанию.

```c++
// myclass.h
...

class MyClass 
{
public:
    MyClass();
    explicit MyClass(int a, int b = 0);
    explicit MyClass(string name);

    const Data& get_data() const;

    inline void func() 
    { 
        /*...*/ 
    }
}
```
