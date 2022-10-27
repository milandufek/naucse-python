{% set coach_username = var('coach-username') or 'naucse' %}

# Spolupráce

„Opravdové” programy zřídka vznikají prací jednoho člověka.
Víc hlav víc ví, a tak je dobré si na projekt vytvořit tým.

Každý člen týmu potřebuje mít přístup k práci ostatních.
K tomu se dá použít Git: někde na internetu si zařídí *sdílený repozitář*,
se kterým se všichni budou synchronizovat.


## GitHub

Na Internetu existuje spousta stránek, kam se dají nahrávat gitové repozitáře
s kódem – např. [GitLab](https://gitlab.com/),
[BitBucket](https://bitbucket.org),
[Pagure](https://pagure.io/) nebo
[Launchpad](https://launchpad.net/).
Aktuálně nejpopulárnější je ale [GitHub](https://github.com), který si tady
ukážeme.

Jestli ještě nemáš uživatelský účet na [github.com](https://github.com), jdi
tam a založ si ho.

## Spojení tvého repozitáře s remote repozitářem
Na úvodní stránce Githubu po založení účtu uvidíš na úvodní stránce `Learn Git and GitHub without any code!` a pod ním najdeš tlačítko `Start a project`. Zvol si jméno pro svůj nový repozitář, třeba `pyladies` a zbytek nastavení nech tak, jak je a klikni na `Create repository`.

Uvidíš stránku s velkým množství různých příkazů. Nás zajímá druhá sekce `…or push an existing repository from the command line`.

Zkopíruj si příkaz začínající na `git remote add origin ...` a vlož ho do příkazové řádky tak, kde máš svůj gitový repozitář, který jsme si společně nastavily. Po stisknutí `enter` ti příkazová řádka nic nevypíše, což je signál, že všechno proběhlo v pořádku.

Stejným způsobem si zkopíruj a vlož k sobě do terminálu příkaz `git push -u origin main`. Github se tě zeptá na tvé přihlašovací údaje. Vlož je (pozor, když budeš psát heslo, tak to bude vypadat, že vůbec nepíšeš, to je však jen z důvodu bezpečnosti, klávesnice ti funguje v pořádku) a pak už by si měla vidět `Branch 'main' set up to track remote branch 'main' from 'origin'.`

To znamená, že tvůj lokální repozitář se právě nahrál na tzv. remote repozitář. Repozitář jsme označili jménem *origin*. Když se proklikneš do svého nového repozitáře na Githubu (`https://github.com/tvoje_jmeno/nazev_repozitare`), uvidíš tu přesnou kopii tvé složky s lekcemi.

Jak už napovídá název repozitáře, tvůj příspěvek do tohoto projektu bude
zápis do prezenčky: konkrétně přidání souboru s tvým jménem.
Jméno je to proto, aby nedocházelo ke kolizím: potřebujeme, aby příspěvky od
všech lidí, kteří prochází tenhle kurz, byly jiné.

Tvůj příspěvek bude ovšem veřejně vystaven na internetu.
Pokud nechceš vystavovat svoje občanské jméno, použij místo něj klidně
přezdívku, oblíbené jídlo nebo pár náhodných písmen. Ale:
* když budeš pojmenovávat soubor, buď originální, aby nedošlo ke konfliktům, a
* nesdílej nic, co nemáš právo sdílet (např. texty moderních písní).


## Vytvoření větve

Pomocí `git branch` zjisti, na jaké jsi aktuálně větvi.
Měla by to být větev `main`.

Tuhle „základní“ větev je dobré používat jen na revize, na kterých se už
shodl celý tým.
Proto když chceš do projektu přispět, jako první krok si pro svůj příspěvek
udělej novou větev a přepni se do ní.
Například pomocí:

```console
$ git branch pridani-jmena
$ git checkout pridani-jmena
```


## Posílání změn <small>(<code>git push</code>)</small>

Co když uděláš nějaké změny, například vypracuješ domácí úkol na svém druhém počítači a chtěla by, aby změny byly vidět v tvém repozitáři na Githubu? K tomu slouží příkaz `git push`.

Pamatuješ ještě na básničku, kterou jsme psali, když jsme se učili s Gitem? Napiš si jí ještě jednou, ať jí neztratíme. Přepni se do složky aktuální lekce, vytvoř soubor `basnicka.txt` a napiš do něj nějakou básničku. Přidej jí do Gitu (pomocí příkazů `git add` a `git commit`). Nezapomeň, že je třeba mít na obou počítačích nainstalovaný a nastavený Git. 

Pomocí příkazu `git push origin main` nahraješ nové změny na Github.


## Stažení změn <small>(<code>git pull</code>)</small>

Máš zase svůj oblíbený počítač, kde máš všechny materiály, ale chybí ti tam úkol, který si vypracovala na svém druhém počítači a nahrála na Github?

Přepni se do složky s materiály, kde máš svůj Gitový repozitář a napiš `git pull origin main`.
Všechny změny by se ti měly stáhnout do tvého lokálního repozitáře.


## Žádost o začlenění <small>(<em>pull request</em>)</small>

Pull requesty se používají, když chceš začlenit nějaké změny do projektu, na kterém pracuješ. Ve větvi `main` by měl být funkční a hotový kód, na různé pokusy slouží větve `branch`, o kterých jsme mluvili v minulé sekci.

My budeme využívat pull requesty k efektivní kontrole domácích projektů. Vytvoř si teď pro demonstraci novou větev, ve které upravíme naší básničku `git branch uprava_basnicky` a přepni se do ní `git checkout uprava_basnicky`. Teď udělej nějaké změny v básničce, přidej autora, další sloku, cokoliv tě napadne.

Přijde úpravy do Git jako novou revizi pomocí příkazů `git add` a `git commit`. Pak pomocí příkazu `git push origin uprava_basnicky` nahraj změny na Github. Vidíš, že v tomhle případě *nepíšeme* `git push origin main`, protože teď nechceme zveřenovat změny ve větvi main, ale ve větvi, kde jsme upravili naší básničku.

Jdi do svého repozitáře na Github a klikni na sekci `Pull requests`. Uvidíš velké zelené tlačítko `New pull request`. Po kliknutí uvidíš stránku nadepsanou `Compare changes`. Tady musíš nastavit, co kam chceš vlastně začlenit. Jako `base` nech větev main a do `compare` zvol větev `uprava_basnicky`.

Github ti ukáže všechny změny, které si na té větvi udělala, pak už stačí jen kliknout na `Create new pull request`. V sekci `Pull requests` najednou uvidíš v závorce (1). To znamená, že si vytvořila pull request.

Pošli odkaz na pull request svému kouči, který ti domácí úkol opraví. Díky Githubu ti může napsat komentáře přímo do kódu a nebude tak muset vypisovat čísla řádků a do je na nich špatně. Až ti kouč úkol schválí, můžeš změny sloučit do main větve pomocí tlačítka `Merge pull request`. Pak se ti správný a schválený úkol nahraje do tvé main větve. Aby si ho měla v main větvi i ve svém počítači, použij náš známý příkaz `git pull origin main`.

U přidání jména do prezenčky se to asi nestane, ale kdybys potřeboval{{a}}
na změně před začleněním ještě trochu zapracovat (třeba i po
pár dnech diskuse), nebyl by to problém.
Přepni se na svém počítači do větve `pridani-jmena`, udělej další revize,
a pomocí <code>git push <i>tvojejmeno</i> pridani-jmena</code>
*pull request* aktualizuj.


## Naklonování repozitáře <small>(<code>git clone</code>)</small>

Pokud budeš chtít mít přístup ke svým materiálům z lekcí z dalšího počítače, je nutné si tam repozitář tzv. naklonovat. Nezapomeň, že je třeba mít nainstalovaný a nastavený Git.
Na úvodní stránce tvého repozitáře uvidíš velké zelené tlačítko `Clone or download`. Klikni na něj a zvol možnost `Clone with HTTPS`. Ještě tu je druhá možnost (`Clone with SSH`), kterou nebudeme využívat. V příkazové řádce uvidíš `Cloning into 'nazev_repozitare'...`. Vytvořila se ti nová složka, kde je přesná kopie tvého repozitáře z githubu. Přepni se do ní, zkus příkaz `git status`. Funguje?


## Hlášení chyb <small>(<em>issues</em>)</small>

Občas nastane situace, kdy v nějakém projektu najdeš chybu, ale nemáš čas nebo
znalosti, abys ji opravil{{a}}. Pro tyto případy slouží na GitHubu záložka
 *Issues*, kde se nachází seznam nahlášených problémů.
Nenajdeš-li mezi nimi „svoji” chybu, můžeš ji
nahlásit – stačí kliknout na *New Issue*
a můžeš psát, kdy chyba nastává, co program dělá
špatně a co by měl dělat místo toho.


## Návod na odevzdání domácího úkolu

* Přepni se na větev main `git checkout main`
* Vytvoř si novou větev, kterou si pojmenuj nějak unikátně (např. piskvorky). Je lepší nepoužívat diakritiku a jako oddělovač používat podtržítko. `git branch piskvorky`
* Přepni se do nové větve. `git checkout piskvorky`
* Napiš úkol.
* Přidej změny do stage a pak udělej commit. `git add piskvorky`,  `git commit -m 'Home work - game piskvorky.'`
* Pushni větev do repozitáře na githubu. `git push origin piskvorky`
* Udělej nový pull request. Návod v sekci Žádost o začlenění (pull request)
* Pošli koučovi odkaz na pull request do slacku.
* Potom, co ti kouč úkol schválí, udělej merge do main větve. Na stránce pull requestu v dolní části je zelené tlačíko `Merge pull request`. 
* Stáhni si změny do lokálního repozitáře (ve tvém počítači) do větve main. `git checkout main`, `git pull origin main`
* Pokud budeš v průběhu práce na domácích úkolu dělat změny v jiných souborech, přepni se nejdřív na větev main, udělej změny a commitni je a pak se vrať do větve s domácím úkolem.
