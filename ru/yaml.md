---
contributors:
- [Leigh Brenecki, 'https://github.com/adambrenecki']
- [Suhas SG, 'https://github.com/jargnar']
translators:
- [Sergei Babin, 'https://github.com/serzn1']
---

YAML как язык сериализации данных предназначен прежде всего для использования людьми.

Это строгое надмножество JSON с добавлением синтаксически значимых переносов строк и 
отступов как в Python. Тем не менее в отличие от Python, YAML запрещает 
использование табов для отступов.

```yaml
---  # начало документа

# Комментарий в YAML выглядит как-то так.

######################
# Скалярные величины #
######################

# Наш корневой объект (который продолжается до конца документа) будет соответствовать
# типу map, который в свою очередь соответствует словарю, хешу или объекту в других языках.
key: value
another_key: Другое значение ключа.
a_number_value: 100
scientific_notation: 1e+12
# Число 1 будет интерпретировано как число, а не как логический тип. Если необходимо чтобы
# значение было интерпретировано как логическое, необходимо использовать true
boolean: true
null_value: null
key with spaces: value

# Обратите внимание что строки используются без кавычек, но могут и с кавычками.
however: 'Строка заключенная в кавычки.'
'Ключ заключенный в кавычки.': "Полезно если нужно использовать ':' в вашем ключе."
single quotes: 'Содержит ''одну'' экранированную строку'
double quotes: "Содержит несколько: \", \0, \t, \u263A, \x0d\x0a == \r\n, экранированных строк."

# Многострочные строковые значения могут быть записаны как 'строковый блок' (используя |),
# или как 'сложенный блок' (используя '>').
literal_block: |
    Значение всего текста в этом блоке будет присвоено ключу 'literal_block',
    с сохранением переноса строк.

    Объявление продолжается до удаления отступа и выравнивания с ведущим отступом.

        Любые строки с большим отступом сохраняют остатки своего отступа -  
        эта строка будет содержать дополнительно 4 пробела.
folded_style: >
    Весь блок этого текста будет значением 'folded_style', но в данном случае
    все символы новой строки будут заменены пробелами.

    Пустые строки будут преобразованы в перенос строки.

        Строки с дополнительными отступами сохраняют их переносы строк -
        этот текст появится через 2 строки.

##################
# Типы коллекций #
##################

# Вложения используют отступы. Отступ в 2 пробела предпочтителен (но не обязателен).
a_nested_map:
  key: value
  another_key: Another Value
  another_nested_map:
    hello: hello

# В словарях (maps) используются не только строковые значения ключей.
0.25: a float key

# Ключи также могут быть сложными, например многострочными.
# Мы используем ? с последующим пробелом, чтобы обозначить начало сложного ключа.
? |
  Этот ключ
  который содержит несколько строк
: и это его значение

# YAML также разрешает соответствия между последовательностями со сложными ключами
# Некоторые парсеры могут выдать предупреждения или ошибку
# Пример
? - Manchester United
  - Real Madrid
: [2001-01-01, 2002-02-02]

# Последовательности (эквивалент списка или массива) выглядят как-то так
# (обратите внимание что знак '-' считается отступом):
a_sequence:
  - Item 1
  - Item 2
  - 0.5  # последовательности могут содержать различные типы.
  - Item 4
  - key: value
    another_key: another_value
  -
    - Это последовательность
    - внутри другой последовательности
  - - - Объявления вложенных последовательностей
      - могут быть сжаты

# Поскольку YAML это надмножество JSON, вы можете использовать JSON-подобный 
# синтаксис для словарей и последовательностей:
json_map: {"key": "value"}
json_seq: [3, 2, 1, "takeoff"]
в данном случае кавычки не обязательны: {key: [3, 2, 1, takeoff]}

##########################
# Дополнительные функции #
##########################

# В YAML есть удобная система так называемых 'якорей' (anchors), которые позволяют легко
# дублировать содержимое внутри документа. Оба ключа в примере будут иметь одинаковые значения:
anchored_content: &anchor_name Эта строка будет являться значением обоих ключей.
other_anchor: *anchor_name

# Якоря могут использоваться для дублирования/наследования свойств
base: &base
  name: Каждый будет иметь одинаковое имя

# Регулярное выражение << называется ключом объединения независимо от типа языка.
# Он используется, чтобы показать что все ключи одного или более словарей должны быть
# добавлены в текущий словарь.

foo: &foo
  <<: *base
  age: 10

bar: &bar
  <<: *base
  age: 20

# foo и bar могли бы иметь имена: Каждый из них имеет аналогичное имя

# В YAML есть теги (tags), которые используются для явного объявления типов.
explicit_string: !!str 0.5
# В некоторых парсерах реализованы теги для конкретного языка, пример для Python
# пример сложного числового типа.
python_complex_number: !!python/complex 1+2j

# Мы можем использовать сложные ключи с включенными в них тегами из определенного языка
? !!python/tuple [5, 7]
: Fifty Seven
# Могло бы быть {(5, 7): 'Fifty Seven'} в Python

#######################
# Дополнительные типы #
#######################

# Строки и числа не единственные величины которые может понять YAML.
# YAML также поддерживает даты и время в формате ISO.
datetime: 2001-12-15T02:59:43.1Z
datetime_with_spaces: 2001-12-14 21:59:43.10 -5
date: 2002-12-14

# Тег !!binary показывает что эта строка является base64-закодированным
# представлением двоичного объекта.
gif_file: !!binary |
  R0lGODlhDAAMAIQAAP//9/X17unp5WZmZgAAAOfn515eXvPz7Y6OjuDg4J+fn5
  OTk6enp56enmlpaWNjY6Ojo4SEhP/++f/++f/++f/++f/++f/++f/++f/++f/+
  +f/++f/++f/++f/++f/++SH+Dk1hZGUgd2l0aCBHSU1QACwAAAAADAAMAAAFLC
  AgjoEwnuNAFOhpEMTRiggcz4BNJHrv/zCFcLiwMWYNG84BwwEeECcgggoBADs=

# YAML может использовать объекты типа ассоциативных массивов (set), как представлено ниже:
set:
  ? item1
  ? item2
  ? item3
or: {item1, item2, item3}

# Сеты (set) являются простыми эквивалентами словарей со значениями 
# типа null; запись выше эквивалентна следующей:
set2:
  item1: null
  item2: null
  item3: null

...  # конец документа
```

### Больше информации

+ [YAML официальный вебсайт](http://yaml.org/)
+ [YAML онлайн валидатор](http://www.yamllint.com/)
