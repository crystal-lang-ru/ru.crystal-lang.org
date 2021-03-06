# Array

[Array](http://crystal-lang.org/api/Array.html) (массив) это универсальная структура данных, содержащая элементы типа `T`. Обычно создается с помощью литералов массива:

```crystal
[1, 2, 3]         # Array(Int32)
[1, "hello", 'x'] # Array(Int32 | String | Char)
```

Массив может иметь смешанный тип, это значит, что `T` будет являться объединением типов, и они будут вычисляться при создании массива: либо при явном определении `T`, либо используя литерал массива. В последнем случае T будет определено с помощью дополнительного элемента литерала массива.

При создании пустого массива мы всегда должны определить T:

```crystal
[] of Int32 # создастся как Array(Int32).new
[]          # синтаксическая ошибка
```

## Массив строк

Массивы строк могут быть созданы с помощью специального синтаксиса:

```crystal
%w(one two three) # ["one", "two", "three"]
```

## Массив символов

Массивы символов также могут быть созданы с помощью специального синтаксиса:

```crystal
%i(one two three) # [:one, :two, :three]
```

## Array-подобные типы

Вы можете использовать синтаксис литералов массива вместе с другими типами, если у них определены методы `new` (без аргументов) и `<<`:

```crystal
MyType{1, 2, 3}
```

Если `MyType` не универсальный:

```crystal
tmp = MyType.new
tmp << 1
tmp << 2
tmp << 3
tmp
```

Если `MyType` универсальный:

```crystal
tmp = MyType(typeof(1, 2, 3)).new
tmp << 1
tmp << 2
tmp << 3
tmp
```

В случае использования универсального типа тип аргументов также может быть определен:

```crystal
MyType(Int32 | String) {1, 2, "foo"}
```
