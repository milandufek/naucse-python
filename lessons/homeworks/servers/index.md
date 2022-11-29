# Domácí projekty

Tento domácí úkol ti pomůže procvičit všechny znalosti z posledních lekcí:
- opakování objektového programování
- využívání otevřeného API třetích stran
- práci se soubory
- práci s HTTP dotazy pomocí knihovny `requests`
- práci s datovým formátem [JSON](https://en.wikipedia.org/wiki/JSON)
- práce s dalšími balíčky jako `os`, `datatime` a `qrcode`

Vytvoř jednoduchý účetní program, který bude pracovat s kurzovním lístkem a
generovat faktury s QR platbou pro uživatele.

Program se zeptá na položku, její cenu a měnu. Pokud měna nejsou koruny,
přepočítá částku na koruny podle platného kurzovního lístku ze zdrojové měny.
Když měna neexistuje/není zadána, vyhodí vlastní výjimku `CurrencyError`.
Variabilní symbol (VS) můžeš vygenerovat náhodný -
ten pak použij i jako unikátní identifikátor faktury a obrázku s QR kódem.
Pokus se zaručit, že bude vždy unikátní.

Program piš objektově. Měl by obsahovat část pro konvertor měn,
pro samotnou položku k fakturaci, vygenerování QR platby,
řídící část a možná nějaké pomocné funkce pro obecnou práci.
Kromě řídící části tzv. `main` by program neměl obsahovat žádné `inputy` ani `printy`.
Chceš aby byl i strojově použitelný, např. mohl přečíst 1000 záznamů
z jiného seznamu nebo API a pro každý vygeneroval fakturu, kterou třeba někam pošle,
to dělat nemusíš ale mysli na to při návrhu :).

## Očekávaný výstup programu

V základní variantě, se program na toto zeptá, následně vypíše výsledek a ukončí se.

```bash
$ python qr_payment_gen.py
Generátor faktur.
Částka v jiných měnách než Kč, bude převedena na koruny podle platného kurzu ČNB.

Zadej položku platby: Platba za kurz Pyladies
Částka: 10000
Měna: Kč

Generuji fakturu...
Faktura je hotová na odkaze <cesta_k_vygenerovanému_souboru/qr_platba_{unikatni_ID}.html>
```

S čím si program neporadí je jednotka nebo měna, která není na kurzovním lístku nebo není zadána vůbec.
Nulová nebo záporná částka. Program by neměl pokračovat.

## Zdroje

### Kurzy
Jako kurzovní lístek použij [veřejné API z Kurzy.cz](https://www.kurzy.cz/html-kody/json/kurzy-bank.htm).
Aby se nemusely kurzy při každém spouštění aplikace stahovat,
ukládej kopii lokálně ve formátu JSON do souboru a aktualizuj ji,
jen pokud jsou data v souboru ze staršího kalendářního dne nebo neexistují vůbec.
V opačném případě je pouze načti ze souboru.

### QR platba
Nastuduj si [formát QR platby](https://qr-platba.cz/pro-vyvojare/specifikace-formatu/) v jakém ho banky očekávají,
stačí základní varianta s účtem, částkou, VS a zprávou pro příjemce.

Pro samotné vygenerování obrázku použij Python knihovnu `qrcode` (vyžaduje nainstalovat i `pillow`).

Účet banky použij nějaký vymyšlený (ale validní), co bude nadefinovaný jako výchozí (počítej, že ho bude možné někdy měnit).

QR kód by ale měl být validní, aplikace mobilního bankovnictví ho musí umět načíst!

### HTML template pro fakturu
Pro finální fakturu s QR platbou **použij tuto HTML šablonu**, kterou si ulož jako soubor.
Ten pak budeš pouze `formátovat` a generovat z něho konkrétní platby, jak Baťa cvičky.

```html
<html>
  <head>
    <title>Faktura číslo {}</title>
  </head>
  <body>
    <h1>Platba za {}</h1>
      <p>Částka: {}Kč</p>
      <p>Na účet: {}</p>
      <p>VS: {}</p>
    <h2>QR platba</h2>
      <p><img src="{}"></p>
  </body>
</html>
```

## Bonus

### 1. Více položek
Pokud chceš svůj program vylepšit, napiš ho tak, aby šlo zadat více položek
a vygenerovat pro ně jednu společnou fakturu, kde bude celková částka v Kč
ale bude obsahovat seznam všech dílčích položek a jejich ceny v původní i české měně.

Pro hezké zobrazení na faktuře už to bude vyžadovat i práci se změnou HTML šablony ;-)

### 2. Parser čísla a měny
Program si poradí se zápisem částky a měny v jednom řádku
- 10Kč
- 10 Eur
- €10
- USD 10
- 10USD
- apod...
