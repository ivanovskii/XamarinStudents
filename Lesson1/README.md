
[Содержание](#)
- [Lesson 1](#lesson-1)
  - [App.xaml](#appxaml)
  - [App.xaml.cs](#appxamlcs)
  - [MainPage.xaml](#mainpagexaml)
  - [Margin и Padding](#margin-%d0%b8-padding)
  - [Frame и BoxView](#frame-%d0%b8-boxview)
  - [Форматирование и интерполяция строк](#%d0%a4%d0%be%d1%80%d0%bc%d0%b0%d1%82%d0%b8%d1%80%d0%be%d0%b2%d0%b0%d0%bd%d0%b8%d0%b5-%d0%b8-%d0%b8%d0%bd%d1%82%d0%b5%d1%80%d0%bf%d0%be%d0%bb%d1%8f%d1%86%d0%b8%d1%8f-%d1%81%d1%82%d1%80%d0%be%d0%ba)
  - [Преобразование типов](#%d0%9f%d1%80%d0%b5%d0%be%d0%b1%d1%80%d0%b0%d0%b7%d0%be%d0%b2%d0%b0%d0%bd%d0%b8%d0%b5-%d1%82%d0%b8%d0%bf%d0%be%d0%b2)
  - [Stepper и Slider](#stepper-%d0%b8-slider)
  - [Необходимые ссылки](#%d0%9d%d0%b5%d0%be%d0%b1%d1%85%d0%be%d0%b4%d0%b8%d0%bc%d1%8b%d0%b5-%d1%81%d1%81%d1%8b%d0%bb%d0%ba%d0%b8)
- [Homework](#homework)


# Lesson 1

## App.xaml

С помощью атрибута ```x:Class``` элемент ```Application``` задает полное название производного класса приложения. По умолчанию класс называется ```App```, с указанием названия проекта, то есть в данном случае ```Example.App```

Как правило, основная задача данного файла состоит в определении ресурсов, общих для приложения. Поэтому тут по умолчанию определен пустой элемент ```Application.Resources```, в который, собственно, и помещаются ресурсы.

## App.xaml.cs

Он также содержит определение класса ```App```.

В конструкторе ```App()``` после инициализации компонентов(```InitializeComponent()```) задается начальная страница. По умолчанию это MainPage. В данном случаем мы модифицировали эту строчку кода и положили объект ```MainPage``` в ```NavigationPage```. Данное действие открывает нам возможность использовать стек навигации. Иначе нам доступен только ```PushModalAsync```, который в свою очередь всего лишь открывает страницу поверх других открытых страниц как панель или баннер.
```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

Определение класса содержит ключевое слово ```partial```, что говорит о том, что это частичный класс. То есть определение этого класса может лежать в нескольких файлах, что и позволяет собирать ```App.xaml``` и ```App.xaml.cs``` в один файл приложения App.g.cs, который вы можете найти после компиляции приложения в каталоге проекта ```obj/Debug```.

## MainPage.xaml

Это самый первый Page, который создается по умолчанию вместе с шаблоном Blank. Изначально там лежит ```Label``` в ```StackLayout``` и делается вызов ```InitializeComponent()``` в конструкторе класса.

Мы поместили ```StackLayout``` в ```ScrollView``` чтобы иметь возможность прокатывать большое количество элементов как ленту.

```DisplayAlert``` - это всплывающее окно, думаю его представлять не стоит

## StackLayout в ScrollView

```StackLayout``` определяет размещение элементов в виде горизонтального или вертикального стека. Для позиционирования элементо он определяет два свойства:
* ```Orientation```: определяет ориентацию стека - вертикальный или горизонтальный
* ```Spacing```: устанавливает пространство между элементами в стеке, по молчанию равно 6 единицам

При создании стека или любого другого элемента компоновки может сложиться ситуация, когда не все элементы будут помещаться на экране. В этом случае необходимо создать прокрутку с помощью элемента ```ScrollView```

## Margin и Padding

Для установки отступов у элементов применяются свойства ```Margin``` и ```Padding```. Свойство ```Margin``` определяет внешний отступ элемента от других элементов или контейнера. А свойство Padding устанавливает внутрение отступы - от внутреннего содержимого элемента до его границ. Оба этих свойства представляют структуру ```Thickness```.

## Frame и BoxView

Контейнер ```Frame``` создает вокруг вложенного содержимого прямоугольную границу, за цвет которой отвечает свойство ```BorderColor```. Также отличительной особенностью фрейм является то, что по умолчанию он уже имеет внутренние отступы в 20 единиц

```BoxView``` представляет прямоугольник. Чаще всего ```BoxView``` используется для создания окрашенных областей, либо в качестве контейнера для изображений

## Навигация между страницами

Для поддержки навигации на каждой странице Page в Xamarin Forms определено свойство Navigation. Это свойство предтавляет интерфейс ```INavigation```, в котором есть следующие методы:
```csharp
1. Task PushAsync(Page page)
2. Task PushModalAsync(Page page)
```
В качестве параметра здесь передается объект Page - страница, на которую надо осуществить переход.

Второй метод имеет в своем названии слово "Modal" и осуществляет переход на модальную страницу.Обычно модальные страницы используются, когда приложению нужно получить некоторую информацию от пользователя. При этом до получения информации нельзя возвращаться на предыдущую страницу.

Также имеются методы для возврата на предыдущую страницу:
```csharp
1. Task<Page> PopAsync()
2. Task<Page> PopModalAsync()
```

## Динамическая загрузка XAML

Мы сами можем загружать код XAML, причем динамически в процессе выполнения программы, используя метод ```LoadFromXaml()```. Подобный способ может применяться, например, при получении кода XAML от веб-сервиса или, когда в зависимости от хода программы необходимо предоставить альтернативные версии интерфейса. Но вместе с тем стоит отметить, что динамическая загрузка XAML снижает производительность приложения, поэтому по возможности ее следует избегать.

## Picker

Визуально ```Picker``` представляет собой обычное текстовое поле, по нажатию на которое открывается список для выбора, что-то наподобие выпадающего списка.

```Picker``` содержит список для выбора, а отследить выбранный элемент мы можем с помощью обработчик события ```SelectedIndexChanged```

## Форматирование и интерполяция строк

Также в коде используется некоторое выражение следующего вида:

```csharp
$"Selected {(sender as Picker)?.SelectedItem.ToString()}"
```

Это так называемая интерполяция строк. Знак доллара перед строкой указывает, что будет осуществляться интерполяция строк. Внутри строки опять же используются плейсхолдеры ```{...}```, только внутри фигурных скобок уже можно напрямую писать те выражения, которые мы хотим вывести.
Интерполяция по сути представляет более лаконичное форматирование. При этом внутри фигурных скобок мы можем указывать не только свойства, но и различные выражения языка C#

Если бы этой возможности не было, то строка выглядела бы следующим образом:


```csharp
"Selected " + (sender as Picker)?.SelectedItem.ToString()
```

или


```csharp
"Selected {0}", (sender as Picker)?.SelectedItem.ToString()
```

где в ```{0}``` подставился бы ```(sender as Picker)?.SelectedItem.ToString()```

## Преобразование типов

Остается только разобраться в выражении

```csharp
(sender as Picker)?.SelectedItem.ToString()
```
* ```sender as Picker```
Это банальное приведение ```sender``` к типу ```Picker```
* ```?.```
Это null-условный оператор доступа к членам, который дает доступ к операнду только в том случае, если он имеет значение, отличное от NULL. Если операнд имеет значение null, результатом применения оператора является null. Null-условный оператор доступа к элементу ```?.``` также называется элвис-оператором.
То есть при неудачном касте ```sender``` в ```Picker``` мы получим null и соответственно ```SelectedItem``` уже не выполнится

```SelectedItem``` и ```ToString()``` выполняют интуитивно понятные действия

## Stepper и Slider

Stepper позволяет устанавливать числовое значение

Свойства, которые настраивают его поведение:
* ```Minimum```: устанавливает минимальное значение
* ```Maximum```: устанавливает максимальное значение
* ```Increment```: устанавливает шаг изменения значения

Для отслеживания изменения значения можно обработать событие ```ValueChanged```

Slider представляет собой горизонтальный ползунок и во многих аспектах он похож на Stepper. Свойства аналогичны

## Необходимые ссылки

1. [Приложение и класс Application](https://metanit.com/sharp/wpf/3.php)
2. [Частичные классы и методы](https://metanit.com/sharp/tutorial/3.21.php)
3. [StackLayout и ScrollView](https://metanit.com/sharp/xamarin/3.2.php)
4. [Позиционирование элементов на странице](https://metanit.com/sharp/xamarin/3.17.php)
5. [Margin and Padding](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/margin-and-padding)
6. [Кнопки](https://metanit.com/sharp/xamarin/3.7.php)
7. [Frame и BoxView](https://metanit.com/sharp/xamarin/3.3.php)
8. [Всплывающие окна](https://docs.microsoft.com/ru-ru/xamarin/xamarin-forms/user-interface/pop-ups)
9.  [PushAsync](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.inavigation.pushasync?view=xamarin-forms)
10. [Метод LoadFromXaml и загрузка XAML](https://metanit.com/sharp/xamarin/2.12.php)
11. [Выпадающий список Picker](https://metanit.com/sharp/xamarin/3.11.php)
12. [Форматирование и интерполяция строк](https://metanit.com/sharp/tutorial/7.5.php)
13. [Преобразование типов](https://metanit.com/sharp/tutorial/3.11.php)
14. [Операторы доступа к членам](https://docs.microsoft.com/ru-ru/dotnet/csharp/language-reference/operators/member-access-operators#null-conditional-operators--and-)
15. [Stepper и Slider](https://metanit.com/sharp/xamarin/3.12.php)

# Homework

Реализовать приложение, функционалом схожеес тем, что продемонстрировано в HW1.xd.
Открыть этот файл можно с помощью AdobeXD.

Самостоятельно разобраться с работой [Image](https://metanit.com/sharp/xamarin/3.9.php)

Реализовать сохранение корзины. Для примера можно использовать следующий [пример](https://metanit.com/sharp/xamarin/6.2.php)