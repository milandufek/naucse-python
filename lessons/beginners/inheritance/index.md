# Dědičnost

Minule jsme probrali třídy.
Jako příklad jsme si ukázali třídu pro koťátka:

```python
class Kitten:
    def __init__(self, name):
        self.name = name

    def meow(self):
        print(f'{self.name}: Meow!')

    def eat(self, food):
        print(f'{self.name}: I like {food}!')
```

Zkus si udělat podobnou třídu pro štěňátka:

```python
class Puppy:
    def __init__(self, name):
        self.name = name

    def woof(self):
        print(f'{self.name}: Woof!')

    def eat(self, food):
        print(f'{self.name}: I like {food}!')
```

Většina kódu je stejná!
Kdybys měl{{a}} napsat i třídu pro kuřátka, kůzlátka,
slůňátka a háďátka, bez <kbd>Ctrl</kbd>+<kbd>C</kbd> by to bylo docela nudné.
A protože jsou programátoři líní psát stejný kód
několikrát (a hlavně ho potom udržovat), vymysleli
mechanismus, jak se toho vyvarovat.

Koťátka i štěňátka jsou zvířátka.
Můžeš si vytvořit třídu společnou pro všechna
zvířátka a do ní napsat všechno, co je společné.
Ve třídách pro jednotlivé druhy zvířat pak zbude jen to, co se liší.
V Pythonu se to dělá takto:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self, food):
        print(f'{self.name}: I like {food}!')


class Kitten(Animal):
    def meow(self):
        print(f'{self.name}: Meow!')


class Puppy(Animal):
    def woof(self):
        print(f'{self.name}: Woof!')


micka = Kitten('Micka')
azorek = Puppy('Azorek')
micka.meow()
azorek.woof()
micka.eat('mouse')
azorek.eat('bone')
```

Jak to funguje?
Příkazem `class Kitten(Animal):`
říkáš Pythonu, že třída `Kitten`
*dědí* ze třídy `Animal`
(angl. *inherits* from `Animal`).
Případně se můžeš setkat s jinými termíny:
„je odvozená” ze třídy `Animal`,
(angl. *derived from*),
nebo ji “rozšiřuje” (angl. *extends*).
A když už jsme u terminologie, odvozeným třídám se
říká taky *podtřídy* (angl. *subclasses*)
a `Animal` je tu *nadtřída*
(angl. *superclass*).

Když potom Python hledá nějakou metodu
(nebo jiný atribut), třeba `micka.eat`,
a nenajde ji přímo ve třídě daného objektu (u nás
`Kitten`), podívá se do nadtřídy.
Takže všechno, co je definované pro
`Animal`, platí i pro koťátka.
Pokud to tedy výslovně nezměníš.


## Přepisování metod a `super()`

Když se ti nebude líbit chování některé metody
v nadtřídě, stačí dát metodu stejného jména do
podtřídy:

```python
class Kitten(Animal):
    def eat(self, food):
        print(f'{self.name}: I really do not like {food}!')


micka = Kitten('Micka')
micka.eat('granule')
```

> [note]
> Je to podobné jako když jsme minule přepisoval{{gnd('i', 'y', both='i')}}
> atribut pomocí `micka.meow = 12345`.
> Python atributy hledá napřed na samotném objektu,
> potom na třídě toho objektu a pak na nadtřídě
> (a případně dalších nadtřídách té nadtřídy).

Občas se může stát, že v takovéto přepsané metodě budeš
potřebovat použít původní funkčnost, jen budeš chtít udělat ještě něco navíc.
To umí zařídit speciální funkce `super()`,
která umožňuje volat metody z nadtřídy.
Třeba takhle:

```python
class Kitten(Animal):
    def eat(self, food):
        print(f'({self.name} looks at {food}.)')
        super().eat(food)


micka = Kitten('Micka')
micka.eat('granule')
```

Pozor na to, že takhle volané metodě musíš dát všechny
argumenty, které potřebuje (kromě `self`,
který se jako obvykle doplní automaticky).
Toho se dá i využít – můžeš použít i jiné argumenty
než dostala původní funkce:

```python
class Snek(Animal):
    def __init__(self, name):
        name = name.replace('s', 'sss')
        name = name.replace('S', 'Sss')
        super().__init__(name)


standa = Snek('Stanislav')
standa.eat('mouse')
```

Jak je vidět, `super()` se dá bez problémů
kombinovat se speciálními metodami jako `__init__`.
Dokonce se to dělá poměrně často!


## Polymorfismus

Programátoři nezavedli dědičnost jen proto, že jsou
líní a nechtějí psát dvakrát stejný kód.
To je sice dobrý důvod, ale nadtřídy mají ještě jednu
důležitou vlastnost: když víme, že každé
`Kitten` nebo `Puppy`
nebo jakákoli jiná podtřída je zvířátko,
můžeš si udělat seznam zvířátek s tím,
že pak bude jedno, jaká přesně zvířátka to jsou:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self, food):
        print(f'{self.name}: I like {food}!')


class Kitten(Animal):
    def meow(self):
        print(f'{self.name}: Meow!')


class Puppy(Animal):
    def woof(self):
        print(f'{self.name}: Woof!')

animals = [Kitten('Micka'), Puppy('Azorek')]

for animal in animals:
    animal.eat('meat')
```

Tohle je docela důležitá vlastnost podtříd:
když máš nějaké `Kitten`, můžeš ho použít
kdekoliv kde program očekává `Animal`,
protože každé koťátko *je* zvířátko.

> [note]
> Tohle je docela dobrá pomůcka pro případy, kdy nebudeš vědět
> kterou třídu podědit z které.
> Každé *koťátko* nebo *štěňátko*
> je *zvířátko*, každá *chata*
> nebo *panelák* je *stavení*.
> V takových případech dává dědičnost smysl.
>
> Někdy se ale stane, že tuhle pomůcku zkusíš použít a vyjde ti
> nesmysl jako „každé auto je volant”.
> V takovém případě dědičnost nepoužívej.
> I když jak auto tak volant se dají „otočit doprava”,
> u každého to znamená něco jiného – a určitě nejde auto
> použít kdekoli, kde bys chtěl{{a}} použít volant.
> Takže v tomto případě je lepší si říct „každé auto
> *má* volant”, stejně jako „každé kotě
> *má* jméno”, udělat dvě nezávislé třídy a napsat něco jako:
>
> ```python
> class Car:
>     def __init__(self):
>         self.steering_wheel = SteeringWheel()
> ```
>
> (A až bude někdy nějaký vystudovaný informatik nespokojený
> s tím, že porušuješ
> [Liskovové substituční princip](https://en.wikipedia.org/wiki/Liskov_substitution_principle),
> jde o právě tento problém.)

## Generalizace

Když se teď podíváš na funkce `meow`
a `woof`, možná tě napadne, že by se
daly pojmenovat lépe, aby se daly použít pro všechna
zvířata, podobně jako `eat`.
Bude nejlepší je přejmenovat:


{# XXX: Every instance of "make_sound" should be highlighted #}
```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self, food):
        print(f'{self.name}: I like {food}!')


class Kitten(Animal):
    def make_sound(self):
        print(f'{self.name}: Meow!')


class Puppy(Animal):
    def make_sound(self):
        print(f'{self.name}: Woof!')


animals = [Kitten('Micka'), Puppy('Azorek')]

for animal in animals:
    animal.make_sound()
    animal.eat('meat')
```

Jak tenhle příklad naznačuje, psát nadtřídy ze kterých se dobře dědí
není jednoduché. Zvlášť to platí, kdyby se z nich mělo dědit v jiném
programu, než kde je nadtřída.
I z toho důvodu je dobré dědičnost používat hlavně v rámci svého kódu:
nedoporučuji dědit od tříd, které napsali ostatní (jako `bool` nebo
`pyglet.sprite.Sprite`), pokud autor nadtřídy výslovně nezmíní, že (a jak) se
z ní dědit má.

A to je zatím o třídách vše. Už toho víš dost na to,
aby sis napsal{{a}} vlastní zoo :)
