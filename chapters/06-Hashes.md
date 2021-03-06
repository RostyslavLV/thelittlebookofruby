# Глава шоста

> Хеші…

Масиви надають хороший спосіб індексації елементів колекції за числом, однак можуть бути випадки, коли набагато зручніше індексувати їх по–іншому. Якщо, наприклад, ви створювали колекцію рецептів, було б набагато зрозуміліше індексувати рецепти за їхніми назвами, як от “Великий шоколадний торт” та “Coq au Vin”, а не 23, 87 і так далі.

Ruby має клас, який дозволяє вам зробити це. Він називається `Hash` (хеш). Він еквівалентний до того, що у інших мовах називають _словником_. Як і справжній словник, елементи індексуються за деяким унікальним ключем (у словнику — це може бути словом) та значенням (у словнику — це визначення для певного слова).

## Створення хешів

[**`hash1.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/6/hash1.rb):

Ви можете створити хеш шляхом створення нового екземпляра класу `Hash`:

```ruby
h1 = Hash.new
h2 = Hash.new("Якесь кільце")
hash1.rb
```

Обидва приклади створюють порожній хеш. Об’єкт `Hash` завжди має значення за замовчуванням – значення, яке буде повертатись тоді, коли не знайдеться відповідне значення для певного індексу. У цих прикладах, `h2` ініціалізований зі значенням за замовчуванням: `"Some kind of ring"`; `h1` ініціалізується без такого значення, тому його значенням за замовчуванням буде `nil`.

Маючи створений `Hash`–об’єкт, ви можете додавати у нього елементи, використовуючи синтаксис схожий на масиви: вказуючи індекс між квадратними дужками та використовуючи `=` для присвоєння значення.

Очевидна відмінність тут у тому, що для масивів індексом (_ключем_) має бути ціле число; для хешу це може бути будь-який унікальний елемент даних:

```ruby
h2['treasure1'] = 'Срібне кільце'
h2['treasure2'] = 'Золоте кільце'
h2['treasure3'] = 'Рубінове кільце'
h2['treasure4'] = 'Сапфірове кільце'
```

Часто ключем може бути число або, як у прикладі вище, рядок. В принципі, ключем може бути об’єкт будь–якого типу. Маючи деякий клас, скажімо `X`, таке присвоєння є цілком можливим:

```ruby
x1 = X.new('мій Xobject')
h2[x1] = 'Діамантове кільце'
```

Є коротший спосіб створення хешів та ініціалізації їх з парами ключ–значення. Просто додайте ключ, після якого буде йти `=>` та асоційоване з ним значення. Кожна пара ключ–значення повинна бути відділена комою, а всі пари ключів та значень мають бути всередині фігурних дужок:

```ruby
h1 = {
  'room1' => 'Скарбниця',
  'room2' => 'Тронний зал',
  'loc1' => 'Лісова поляна',
  'loc2' => 'Гірський потік'
}
```

> ### Унікальні ключі?
>
> Потурбуйтесь про присвоєння ключів в хеші. Якщо ви використовуєте один і той самий ключ двічі, ви перезаписуєте початкове значення. Це те саме, що і двічі присвоїти значення одному й тому ж індексові масиву. Розглянемо такий приклад:
>
> ```ruby
> h2['treasure1'] = 'Срібне кільце'
> h2['treasure2'] = 'Золоте кільце'
> h2['treasure3'] = 'Рубінове кільце'
> h2['treasure4'] = 'Сапфірове кільце'
> ```
>
> Тут ключ `'treasure1'` було використано двічі. Як результат, `'Silver ring'` буде замінено `'Sapphire ring'`, тому ми отримаємо такий хеш:
>
> ```ruby
> h3 = {
>   'treasure3' => 'Рубінове кільце',
>   'treasure1' => 'Срібне кільце',
>   'treasure4' => 'Сапфірове кільце',
>   'treasure2' => 'Золоте кільце'
> }
> ```

## Індексування в хеші

Щоб звернутись до значення в хеші, вкажіть ключ між двома квадратними дужками:

```ruby
puts(h1['room2']) #=> "Тронний зал"
```

Якщо ви передасте ключ, якого не існує, повернеться значення за замовчуванням. Пригадайте, що ми не передавали значення за замовчуванням для `h1`, проте робили це для `h2`:

```ruby
p(h1['unknown_room'])       #=> nil
p(h2['unknown_treasure'])   #=> 'Якесь кільце'
```

Використовуйте метод `default`, щоб отримати значення за замовчуванням та метод `default=`, щоб встановити його (дивіться Главу 4 для детальнішої інформації про _get_ та _set_ методи):

```ruby
p(h1.default)
h1.default = 'Дивовижне місце'
```

## Операції над хешами

[**`hash2.rb`**](https://github.com/LambdaBooks/thelittlebookofruby/blob/master/examples/6/hash2.rb):

Методи `keys` (ключі) та `values` (значення) для `Hash` повертають масиви, тож ви можете використовувати різні методи масивів, щоб маніпулювати ними. Ось кілька простих прикладів (зауважте, що значення, які отримаються в результаті запуску рядка коду, показані у коментарях, що починаються з `#=>`):

```ruby
h1 = { 'key1'=>'val1', 'key2'=>'val2', 'key3'=>'val3', 'key4'=>'val4' }
h2 = { 'key1'=>'val1', 'KEY_TWO'=>'val2', 'key3'=>'VALUE_3', 'key4'=>'val4' }

p(h1.keys & h2.keys)          # перетин множин (ключів)
#=> ["key1", "key3", "key4"]

p(h1.values & h2.values)      # перетин ключів (значень)
#=> ["val1", "val2", "val4"]

p(h1.keys + h2.keys)          # конкатенація
#=> ["key1", "key2", "key3", "key4", "key1", "key3", "key4", "KEY_TWO"]

p(h1.values - h2.values)      # відмінність
#=> ["val3"]

p((h1.keys << h2.keys))       # додавання в кінець
#=> ["key1", "key2", "key3", "key4", ["key1", "key3", "key4", "KEY_TWO"]]

p((h1.keys << h2.keys).flatten.reverse) # розгортання масивів та реверсування
#=> ["KEY_TWO", "key4", "key3", "key1", "key4", "key3", "key2", "key1"]
```

Зверніть особливу увагу на відмінність між конкатенацією з допомогою `+`, щоб додавати значення другого масиву до першого, та додаванням в кінець з допомогою `<<`, щоб додавати цілий другий масив, як останній елемент першого масиву:

```ruby
a =[1,2,3]
b =[4,5,6]
c = a + b #=> c=[1, 2, 3, 4, 5, 6] a=[1, 2, 3]
a << b    #=> a=[1, 2, 3, [4, 5, 6]]
```

При додаванні через `<<`, перший масив (_приймач_ або _receiver_) модифікується, тоді як `+` повертає новий масив, а масив–приймач залишає незмінним. Якщо після додавання в кінець масиву через `<<` ви вирішите, що ви хочете додати елементи масиву, замість того, щоб _вкладати_ цілий масив масив–приймач, ви можете зробити це з допомогою методу `flatten`:

```ruby
a=[1, 2, 3, [4, 5, 6]]
a.flatten #=> [1, 2, 3, 4, 5, 6]
```
