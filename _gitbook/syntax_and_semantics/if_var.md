# if var

Если переменная является условием `if`, то внутри ветки `then` она будет рассматриваться как переменная типа, отличного от `Nil`:

```crystal
a = some_condition ? nil : 3
# a может быть Int32 или Nil

if a
  # Поскольку попасть сюда возможно только если a истинно (truthy),
  # то a не может быть nil. А значит она Int32.
  a.abs
end
```

То же происходит, когда переменная a задаётся в условии `if`:

```crystal
if a = some_expression
  # здесь a не nil
end
```

Эта же логика действует, если в условии если логическое И (`&&`):

```crystal
if a && b
  # здесь обе переменные a и b точно не будут Nil
end
```

Здесь правая часть выражения `&&` также гарантирует, что `a` не будет `Nil`.

Само собой, в случае переопределения переменной внутри ветки `then` тип переменной будет изменён в зависимости от переопределяющего выражения.

Приведённая выше логика **не действует** с переменными объекта, класса и глобальными переменными:

```crystal
if @a
  # здесь @a может быть nil
end
```

Так происходит потому, что любой вызов метода в теории может привести переменную объекта к `nil`. Другая причина заключается в том, что другой поток может изменить значение этой переменной объекта после проверки условия.

Есть два варианта сделать что-нибудь с `@a` если она не `nil`:

```crystal
# Первый вариант: назначить переменную a
if a = @a
  # здесь a не будет nil
end

# Второй вариант: использовать `Object#try` из стандартной библиотеки
@a.try do |a|
  # здесь a не будет nil
end
```

Эта логика также не работает с вызовами процедур и методов, включающих геттеры и свойства, поскольку процедуры и методы, которые могут быть nil (или, если брать шире, относятся к объединённому типу), не могут гарантировать возвращение одного и того же типа при двух успешных вызовах.

```crystal
if method # первый вызов метода может вернуть Int32 или Nil
          # здесь мы узнали, что первый вызов вернул не Nil
  method  # второй вызов всё ещё может вернуть Int32 или Nil
end
```

Приёмы описания переменных объекта выше можно применить также и для процедур с методами.
