---
layout: page
title: Сопоставление с образцом
category: basics
order: 4
lang: ru
---

Сопоставление с образцом (pattern matching) - важная часть языка Elixir. Она позволяет сопоставлять простые значения, структуры и даже функции. В этом уроке мы начнем изучать как использовать эту возможность.

{% include toc.html %}

## Оператор сопоставления

В языке Elixir оператор `=` называется оператором сопоставления. С его помощью можно как назначать, так и сравнивать значения:

```elixir
iex> x = 1
1
```

Простейшее сопоставление:

```elixir
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

Сопоставление с коллекциями:

```elixir
# Lists
iex> list = [1, 2, 3]
iex> [1, 2, 3] = list
[1, 2, 3]
iex> [] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

iex> [1|tail] = list
[1, 2, 3]
iex> tail
[2, 3]
iex> [2|_] = list
** (MatchError) no match of right hand side value: [1, 2, 3]

# Кортежи

iex> {:ok, value} = {:ok, "Successful!"}
{:ok, "Successful!"}
iex> value
"Successful!"
iex> {:ok, value} = {:error}
** (MatchError) no match of right hand side value: {:error}
```

## Фиксирующий оператор

Мы уже разобрались, что оператор сопоставления делает присвоение в тех случаях, когда левая сторона сопоставляемого включает переменную. В некоторых случаях переприсвоение переменной является нежелательным. В таких случаях используется "фиксирующий оператор" `^`.

Когда мы закрепляем переменную с его помощью - сопоставление происходит с имеющимся значением переменной вместо присвоения нового значения:

```elixir
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
iex> {x, ^x} = {2, 1}
{2, 1}
iex> x
2
```

Версия Elixir 1.2 добавила поддержку этого оператора в ключи ассоциативных массивов и функциональные ветвления:

```elixir
iex> key = "hello"
"hello"
iex> %{^key => value} = %{"hello" => "world"}
%{"hello" => "world"}
iex> value
"world"
iex> %{^key => value} = %{:hello => "world"}
** (MatchError) no match of right hand side value: %{hello: "world"}
```

Пример использования в функциональном ветвлении:

```elixir
iex> greeting = "Hello"
"Hello"
iex> greet = fn
...>   (^greeting, name) -> "Hi #{name}"
...>   (greeting, name) -> "#{greeting}, #{name}"
...> end
#Function<12.54118792/2 in :erl_eval.expr/5>
iex> greet.("Hello", "Sean")
"Hi Sean"
iex> greet.("Mornin'", "Sean")
"Mornin', Sean"
```
