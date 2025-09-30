# Тест разработчика C# №2 (шаблон)

<aside>
⚠️ Ответы опубликуйте в собственном Gist (с расширением MD чтобы сохранить форматирование и читаемость).
  
⚠️ Отвечайте самостоятельно, не стремитесь подсмотреть в справочниках, гугле и тому подобных. Если не знаете ответ на вопрос - совершенно нормально его пропустить. Качество и самостоятельность ответов будем обсуждать на очных интервью, поэтому предлагаем не читерить и экономить Ваше и наше время.
</aside>

# 1. Общие вопросы

### 1.1 String и DateTime

Что будет выведено в консоль и почему?

```csharp
class Program {
  static String location;
  static DateTime time;
 
  static void Main() {
    Console.WriteLine(location == null ? "location is null" : location);
    Console.WriteLine(time == null ? "time is null" : time.ToString());
  }
}
```

---

> Ваш ответ
location is null - т.к. строка по умолчанию null
переменная time выведет дату по умолчанию - т.к. это структура

### 1.2 Enumerators

Что будет выведено в консоль?

```csharp
IEnumerable<int> EnumerateInts(int length) {
  for (var i = 0; i < length; i++) {
    Console.WriteLine(i);
    yield return i;
  }
}

var items = EnumerateInts(3);
```

---

> Ваш ответ
0
1
2

### 1.3 Базовые классы List

Перечислите приблизительно на память от каких базовых классов наследуется `List<>` и какие интерфейсы он реализует?

---

> Ваш ответ
Object, IEnumerable, IList

# 2. Entity Framework

### 2.1 Запросы

Какие запросы (приблизительно) будут направлены в SQL-сервер во время исполнения следующего кода:

```csharp
class Student {
  public int Id {get;set;}
  public string Name {get;set;}
  public int Course {get;set;}
  public DateTime BirthDate {get;set;}
}

var student = context.Students.FirstOrDefault(s => s.Id == 1);
student.Name = "Oleg";
context.SaveChanges();
```

---

> Ваш ответ
Select TOP 1 * from Students Where Id = 1
update Students set Name = "Oleg" Where Id = 1

### 2.2 Трекинг изменений

Для чего предназначен метод `IQueryable.AsNoTracking()`? Когда его следует использовать?

---

> Ваш ответ
Чтобы не отслеживать изменения обновлений данных в БД. Используют когда не критично получать последние обновления в БД для выборки

### 2.3 Запросы

Что произойдёт при выполнении первой строки и второй? Какого типа будут переменные `items1` и `items2`?

```csharp
var items1 = context.Students.Take(5).ToList();
var items2 = context.Students.ToList().Take(5);
```

---

> Ваш ответ
items1 - к БД сразу будет отправлен запрос с изъятием только 5 элементов. IEnumerable
items2 - Сначала забираем из БД весь список элементов, затем в памяти берем 5 элементов. IQueryable 

# 3. Dependency Injection

### 3.1. Жизненный цикл

Опишите чем отличается регистрация типов или объектов методами `.AddTransient()`, `.AddScoped()` и `.AddSingleton()`?

---

> Ваш ответ
AddTransient - Один экземпляр на каждое обращение
AddScoped - Один экземпляр на запрос 
AddSingleton - Один экземпляр на все приложение

# 4. Разное

### 4.1. Паттерн MVVM

Что такое MVVM? Почему он обрёл популярность именно в WPF?

> Ваш ответ
Паттерн разработки Model-View-ViewModel. Легкая тестируемость, т.к. ViewModel независима. Есть автоматические привязки к данным, что пользоваляет автоматическое обновление UI

### 4.2. INotifyPropertyChanged

Опишите состав интерфейса INotifyPropertyChanged. В чём разница?

```csharp
PropertyChanged("City");
PropertyChanged(null);
```

> Ваш ответ
PropertyChanged("City"); - изменении свойства City
PropertyChanged(null); - принудительное обновление всех свойств

### 4.3 Методы HTTP-протокола

Объясните разницу между PUT, POST, PATCH.

> Ваш ответ
PUT - обновление записи
POST - добавление записи
PATCH - Частичное обновление записи

# 5. Задачи

### 5.1 Проектирование БД

Спроектируйте таблицы в реляционной БД для хранения двумерных матриц произвольного размера (M**×**N). Укажите в свободной форме состав полей таблицы/таблиц.

---

> Ваш ответ
1 таблица: matrices (Название матрицы (varchar), Количество строк N (INT), Количество столбцов M (INT))
2 таблица: matrixElements (Индекс строки (INT), Индекс столбца (INT), Значение элемента (DOUBLE))
### 5.2 Уникальный символ

Напишите реализацию функции, которая принимает на вход строку из символов `a-z`, а на выходе выдаёт первый уникальный символ (который встречается в строке ровно 1 раз), либо `null`, если такого символа нет.

---

```jsx
public static char? FirstUnique(string str) {
    if (string.IsNullOrEmpty(str))
        return null;
    
    var charDict = new Dictionary<char, int>();
    
    foreach (char c in str) {
        if (charDict.ContainsKey(c)) {
            charDict[c]++;
        } else {
            charDict[c] = 1;
        }
    }
    
    foreach (char c in str) {
        if (charDict[c] == 1) {
            return c;
        }
    }
	return null;
}
```