# Git Flow
Je o tom, jak pracovat v týmu, kde všichni členové používají Git.

## Proč?
V první přednášce jsem mluvil o FTP. To nefunguje proto, že si všichni navzájem ničí změny a na jednou souboru může tedy pracovat jen jediný člověk v jednu chvíli. Plus je tady další tisíc problémů. Například ten, že se všechno v jednu chvíli smaže, pře spadne spojení, že se soubor poruší atd. atd...

Co kdyby ale každý měl u sebe kopii celého projektu a jen synchronizoval změny, které provedli jeho kolegové se změny, které provadí sám?

O tom je Git flow.

## Jak na to
Tohle je sranda obrázek organizačních struktur ve velkých IT firmách.
[![Forks](http://www.ericsson.com/uxblog/wp-content/uploads/2012/05/organizational_charts.jpg)](Forks)
Zajímavý je pro nás obrázek Applu. Protože takle nějak vypadá Git flow.

### Kdo je steve Jobs?
Ta červená tečka uprostřed je v našem případě náš společný repozitář, do kterého všichni přispíváme.
Minulou přednášku jsme si forknuli repozitář ten špatný, a dneska si forkneme už ten správný!

https://github.com/webdev-js-evenings/git-workshop

A naklonujte si ho, jako v poslední přednášce! (omlouvám se za komplikace)

```
git clone https://github.com/< github username >/git-workshop jmeno-slozky
```

Tohle je repozitář kolem kterého se bude všechno točit, ale není zas až tak důležitý jako Steve Jobs. Přemýšlejte o něm jako o repozitáři, kde prostě není zbytečný bordel, který si každý, kdo vyvíjí dělá na lokálním stroji, to je celé.

### Jak se přispívá?
Pod přispíváním **contributions** si zřejmě představíme přidává commitů a **pushování** do repozitáře. To je správně, ale v kontextu Git Flow nepřispíváme do vlastního repozitáře přímo. Ten slouží pouze jako naše vývojové prostředí. Hlavní cíl našich commitů je společný repozitář, který nevlastníme, takže ani většinou nemáme práva, abychom tam mohli přispívat.

Jak teda příspějeme?

Dělá se to tak, že řekneme správci společného repozitáře - hale, tady mám nějakej kód a byl bych rád, abyste ho přijali do vašeho repozitáře. A tomu se říká **pull request** nebo také **merge request**. Prostě někomu dáváme svoje commity k posouzení, aby rozhodl, jestli je začlení do svého repozitáře (**mergne**) nebo ne.

### Konec keců, vzhůru do praxe
Takže snad máme všichni forknuto a naklonováno. Ti, kteří minulou přednášku forkli špatný repozitář si musí špatný fork smazat a forknout si ten správný.

#### Milestone
Milestony jsou prostě milníky, které chceme dosáhnout. V našem případě zatím vytvoříme Milestone 0.1

#### Issues
V první řadě potřebujeme nějaké **issues**. Issue nebo taky task je prostě úkol, který máte zadaný. Issues jsou velice důležitá a jejich sledování je ještě důležitější!! Proto si každý vytvoříme issue zde: https://github.com/webdev-js-evenings/git-workshop/issues

A každý ho vyřešíme pull requestem.

Issue zní:
- Vytvořte soubor `ucastnici/6.8.2016/< vase username na githubu >.md`
- napište tam vaší recenzi tohodle kurzu - stačí na dva řádky

Prakticky ve všech aplikacích na sledování issues má každý takový úkol nějaké `ID`. Github ID používá samozřejmě taky a hojně. Snadno se přes `issue ID` páruje issue a pull request. Ukážeme si.

Tak, každý má issue, takže každý jde do konzole, `cd` do našeho naklonovaného forku a jdeme na to!

**Ještě tam nastavím milestony**

#### Feature branch
V Git Flow je zvykem vytvářet feature branche mimo hlavní **master** branch. Feature branch je větev, která řeší nějaké issue.

Podle mě dobrou praxí jí nějak rozumně pojmenovat, třeba `< Issue ID >-moje-feature`.

**!!POZOR!!** je důležité, aby tahle větev vycházela z aktuální vývojové větve!! To docílíme čím? Hm? `rebase`!

#### Dev Branch
Je větev v repozitáři, do které se posílají **pull requesty**. Je to větev, kde se může stát, že se tam mergne něco, co nefunguje. Narozdíl od branche **master** ta musí být ta krásná čistá, stabilní, že?

##### Vytvoření feature branch
Takže vytvoříme branch z branche `dev`. Nemáte ve svém repozitáři branch `dev`? Hmm...
```
// ale to už přece umíte
$ git checkout -b dev < Issue ID >-my-feature
```

#### Vytvoření commitu
Tak šup šup! Vytváře commity umíme, udělejte commit, který splní to, co je v Issue, které jste vytvořili!

Je také good practice psát do commit message issue ID a co s ní který commit dělá např:
```
Add file /ucastnici/6.8.2016/jenicek.md
closes #4234
```

OK, máme commit, jsme na feaure branchi, co teď?

#### Push
Pushnutí znamená, že svojí lokální branch na svém stroji pošlete do vzdáleného repozitáře.

Pokud tedy napíšete:
```
// radši píšu přesně to, co se děje
$ git push origin < vase branch >
```
Tak `git` pošle commity, které jste udělali do vašeho forku a vytvoří tam novou branch. Do toho.

A mrkneme se na internát!

#### Vytvoření pull requestu
Pokud si na Githubu klikneme na naší branch, ta se nám tam zobrazí možnost udělat pull request. Buďto klikneme sem nebo jde do záložky pull request a vytvoříme ho stejnou cestou.

Pokud jsme ve forku, tak se automaticky vyplní repozitář kam chceme poslat pull request a dokonce se vybere i nějaká branch.

My ale víme, že jsme vytvořili pull request z branch `dev` a tudíž chceme také poslat pull request do branch `next` v repozitáři `webdev-js-evenings/git-workshop`. To, jak budou vytvářet release a verzování hlavního repozitáře už je na jeho správcích. My prostě pošlete pull request do dev branche.

Vyplní se sám titulek podle posledního commitu. A zároveň je možné a důležité spojit pull request s issue ID. Proto je rozumné do popisku napsat:
```
closes #< Issue ID >
```
A Github vše spáruje, vidíte?

Existují klíčová slova - jako `address`, `closes` a další mě nenapadá. Které, pokud je napíšete do popisku pull request, dokáží například automaticky zavřít issue potom, co je přijat a mergnut pull request. Snad to bude fungovat...

Pull request hotov od všech? Paráda, jdu to kontrolovat...

#### Akceptování, komentování a mergnutí
Sobě jsem udělal komentář, abych něco změnil a aktualizoval změny, mrkneme se na to.

Ke změně pull requestu stačí jenom aktualizovat branch ve svém forku - to umíme, buďto tam pushneme nové commity nebo uděláme `ammend` udělám `ammend`, aby bylo vidět, co se stane.

**A všichni se mnou!!**

Jakmile jsou všechny komentáře vyřešeny, tak následuje merge!

Jdu na to.

Mělo by vidět, jak github spároval issues s pull requesty přes popisky v popisku pull requestu. Pokud jste dokonce napsali `closes #31323`, tak by se po mergnutí mělo zavřít i issue.

#### Tak to je celé...
Není :-D

#### Je krásné být aktuální
Tohle byla pohoda... Jak ale synchronizovat ty změny?

Tak, když jsem přijmul a mergnul všechny pull requesty, tak se automaticky mergnuly do větve `dev` v repozitáři `webdev-js-evenings/git-workshop`.
Nyní, pokud zadám nový úkol, například - podepište si svojí recenzi, tak pořád platí, že všehny pull requesty musíme vytváře z aktuální branche `dev`. Jak jí ale aktualizujeme?

No, musíme si přidat `webdev-js-evenings/git-workshop` jako **remote** repozitář a z něho si změny stahovat.

##### Přidání remote
Přidání remote je jednoduché a nejdřív si je hezky zobrazíme:
```
$ git remote -v
```
A vidíme jen origin, paráda. Teď zkusíme přidat.

```
$ git remote add origin https://github.com/webdev-js-evenings/git-workshop.git
```
Uh, to nejde, co? No to je proto, že jsem pojmenoval tenhle repozitář jako `origin` a nejde mít dva repozitáře pojmenované stejně.

**Origin** je výchozí název pro repozitář. V překladu to znamená `počátek` a to bychom měli ctít.
V případě, že si klonujeme repozitář, tak `origin` znamená url, ze které jsme klonovali. Protože Git si správně myslí, že místo, odkud klonujeme je to, které drží "pravdu", kde je ta aktuální větev, kterou vyvíjíme.

Ale v Git Flow je aktuální vývojová větev - ta, kde je pravda - v našem sdíleném repozitáři. Proto je zvykem pojmenovat remote, který ukazuje na sdílený repozitář (v našem případě `webdev-js-evenings/git-workshop`) jako **origin**.

Takže nejdřív si přejmenujeme remote našeho forku. Přejmenujeme si ho třeba na **public** protože většinou je náš fork veřejný, do kterého nám leze třeba projekťák nebo nám do něj leze senior vývojář, aby si stáhnul nedokončené změny a vyzkoušel nebo upravil to, co děláte.
```
$ git remote rename origin public
$ git remote -v
```
Tak vidíme zde nyní dva remoty, kam můžeme pushovat -
- public (url našeho forku)
- origin (sdílený adresář)

Jdeme aktualizovat

##### Actually buďme aktuální
Zatím to je snad všechno nějak jasný, asi ne, nevadí, cvik je cvik.

Teď si teda chceme konečně udělat naší `dev` větev aktuální tak, aby byla ve stejném stavu jako `dev` větev v `webdev-js-evenings/git-workshop`. Je to proto, abychom měli pořád aktuální změny a neměli v tom bordel a abychom mohli vyvíjet dál nad prací našich kolegů.

Jak jí na to příkaz? Je jich více...

###### Pull
Nejjednodušší z nich j **pull**. Pull prostě stáhne commity z nějaké remote branche.

Tedy:
```
// přepneme se na naší dev branch
$ git checkout dev
$ git pull origin dev
```

See the magic happen...

A je to tam? Všechno v pohodě? Pokud ne, tak jste dělali něco špatně :)). Pokud vám to nejde, zkuste si znovu forknout repo.

Paráda, umíme si držet naší `dev` větev aktuální a z ní můžeme dál pracovat.

##### Fetch
**Fetch** je ale podle mě trošku lepší přístup k celé záležitosti. Příkaz dokáže stáhnout všechny změny na nějakém remotu, ale neaplikuje je. Místo toho vytvoří lokálně **remote branch**, která sama sleduje změny. Fetch je fajn, protože je neškodný, on jenom všechny změny stáhne a uloží je lokálně na disk a to je všechno, nikam žádný commity nepřehodí, nic nezmění.

Pojďme to zkusit.
```
$ git fetch origin
```
Mělo by se něco vypsat - například to, že se vytvořila branch `origin/dev`.

No a nyní nám stačí naší `dev` branch jenom rebasnout, nemusíme dělat přímo **pull** který je tak nějak mimo naší kontrolu. Fetch je méně destruktivní a navíc prostě stáhne všechno. Tedy pokud bychom se chtěli podívat na to, jak pracuje náš kolega, tak si ho přidáme jako remote, fetchneme si jeho repozitář a vidíme všechny jeho branche a dokonce se na ně můžeme jednoduše checkout. V našem případě prostě:
```
git checkout origin/dev
```
A jsme na remote tracking branch, ze které můžeme dělat normální feature branche stejně jako bychom je dělali z  vlastní lokální branch `dev`. Takže si neprasíme workspace, což je fajn.

Pořád tedy funguje:
```
$ git checkout -b feature-branch origin/dev
```
A teď je to to samé jako:
```
$ git checkout dev
$ git checkout -b feature-branch
```
Je to stejné proto, že jsme přes **pull** aktualizovat lokální branch `dev`.

Pokud ale pořád chceme používat normálně lokální branch `dev` a nechceme používat destruktivní **pull** tak prostě rebasneme:
```
$ git checkout dev
$ git rebase origin/dev
```
Můžete si to vyzkoušet tak, že si resetnete commity z branche `dev`.
A je to!



