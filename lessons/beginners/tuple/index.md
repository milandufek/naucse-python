## <var>N</var>-tice

Když už známe seznam, podívejme se na jeho sestřičku: takzvanou
<var>n</var>-tici (angl. *tuple*).

<var>N</var>-tice může, podobně jako seznam, obsahovat <var>n</var> prvků. 
<var>N</var>-tice se dvěma prvky je *dvojice*
neboli *pár* (angl. *pair*); se třemi
prvky *trojice* (angl. *3-tuple*),
se čtyřmi *čtveřice* (angl. *4-tuple*), atd.

> [note]
> Pro úplnost: existují i <var>n</var>-tice s jedním prvkem (hmm… „jednice”?)
> a s nula prvky (prázdné <var>n</var>-tice, angl. *empty tuple*),
> ale ty se v praxi tolik nepoužívají.

<var>N</var>-tice se tvoří jako seznamy, jen kolem sebe nemají hranaté závorky.
Stačí čárky mezi prvky:

```python
dvojice = 'Pat', 'Mat'
print(dvojice)
```

Chovají se skoro stejně jako seznamy, jen nejdou měnit.
Nemají tedy metody jako `append` a `pop` a nedá se jim přiřazovat do prvků
(např. `ntice[1] = 2`).
Dají se ale použít v cyklu `for` a dají se z nich číst jednotlivé prvky
(např. `print(ntice[1])`).

```python
osoby = 'máma', 'teta', 'babička'
for osoba in osoby:
    print(osoba)

prvni = osoby[0]
print(f'První je {prvni}')
```

> [note]
> Vypadá to povědomě? Aby ne!
> <var>N</var>-tice jsme už použil{{gnd('i', 'y', both='i')}} dříve:
> `for pozdrav in 'Ahoj', 'Hello', 'Ciao':`
> ve skutečnosti používá <var>n</var>-tici.

Python umí ještě jeden trik: pokud chceš přiřadit
do několika proměnných najednou, stačí je na levé
straně rovnítka oddělit čárkou a na pravou stranu
dát nějakou „složenou” hodnotu – třeba právě <var>n</var>-tici.

```python
x, o = 'xo'
jedna, dva, tri = [1, 2, 3]
```

## Funkce, které vracejí <var>n</var>-tice

`zip` je zajímavá funkce.
Používá se ve `for` cyklech, podobně jako funkce `range`, která „dává” čísla.

Když funkce `zip` dostane dva seznamy
(či jiné věci použitelné ve `for`),
„dává” dvojice, a to tak, že nejdřív spáruje
první prvek jednoho seznamu s prvním prvkem
druhého seznamu,
pak druhý s druhým, třetí s třetím a tak dál.

Hodí se to, když máš dva seznamy se stejnou
strukturou – příslušné prvky k sobě „patří”
a chceš je zpracovávat společně:

```python
osoby = 'máma', 'teta', 'babička', 'vrah'
vlastnosti = 'hodná', 'milá', 'laskavá', 'zákeřný'
for osoba, vlastnost in zip(osoby, vlastnosti):
    print('{} je {}'.format(osoba, vlastnost))
```

Když `zip` dostane tři seznamy,
bude tvořit trojice, ze čtyř seznamů nadělá čtveřice a tak dále.

Další funkce, která vrací dvojice, je `enumerate`.
Jako argument bere seznam (či jinou věc použitelnou
ve `for`) a vždy spáruje index (pořadí v seznamu) s příslušným prvkem.
Jako první tedy dá
(0, *první prvek seznamu*), potom
(1, *druhý prvek seznamu*),
(2, *třetí prvek seznamu*)
a tak dále.

```python
prvocisla = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]

for i, prvocislo in enumerate(prvocisla):
    print('Prvočíslo č.{} je {}'.format(i, prvocislo))
```

## Malé <var>n</var>-tice

Jak vytvořit <var>n</var>-tici s žádným nebo jedním prvkem? Takhle:

```python
prazdna_ntice = ()
jednoprvkova_ntice = ('a', )
```

Druhý příklad jde i bez závorek –
`jednoprvkova_ntice = 'a',` –
ale to vypadá jako zapomenutá čárka.
Když budeš *opravdu* potřebovat jednoprvkovou
<var>n</var>-tici, radši ji pro přehlednost ozávorkuj.


## Kdy použít seznam a kdy <var>n</var>-tici?

Seznamy se používají, když předem nevíš,
kolik v nich přesně bude hodnot,
nebo když je hodnot mnoho.
Například seznam slov ve větě,
seznam účastníků soutěže, seznam tahů ve hře
nebo seznam karet v balíčku.
Oproti tomu `for pozdrav in 'Ahoj', 'Hello', 'Hola', 'Hei', 'SYN':`
používá <var>n</var>-tici.

Když se počet prvků může v průběhu programu měnit, určitě sáhni po seznamu.
Příklad budiž seznam přihlášených uživatelů, nevyřešených požadavků,
karet v ruce nebo položek v inventáři.

<var>N</var>-tice se často používají na hodnoty
různých typů, kdy má každá „pozice”
v <var>n</var>-tici úplně jiný význam.
Například seznam můžeš použít na písmena abecedy,
ale dvojice „podíl a zbytek“ je <var>n</var>-tice.

Prázdné <var>n</var>-tice a <var>n</var>-tice s jedním
prvkem se zapisují trochu divně a má to své důvody:
může-li nastat situace, kdy takovou sekvenci budeš
potřebovat, většinou je lepší sáhnout po seznamu.
Například seznam hracích karet v ruce nebo
seznam lidí aktuálně sledujících video může být občas prázdný.

Seznamy i n-tice mají i technické limity:
<var>n</var>-tice nejdou měnit a až se naučíš pracovat se slovníky,
zjistíš že seznamy tam nepůjdou použít jako klíče.
V takových případech je potřeba použít ten druhý typ sekvence.

Často není úplně jasné, který typ použít.
V takovém případě je to pravděpodobně jedno.
