# 1. Git, Commit a Branch
## Co se naučíte
- vysvětlíme si k čemu je GIT
- co je to **commit**
- co je to **branch** (`checkout, branch, checkout -b`)
- co je **remote**
- co je **push**
- základní operace
  - clone
  - fork
  - git init
  - git remote add
  - checkout
- pokročilé operace
  - rebase
  - merge
  - interactive rebase
  - fixup
  - squash
  - reword
  - reflog


## K čemu to je dobrý?
Představte si, že na projektu pracuje tisíc lidí. Co když mají všichni najednou otevřený jeden soubor? Nebo jsou všichni na jednom síťovém uložišti...?

## Co je to Commit
[![Commits](http://nvie.com/img/merge-without-ff@2x.png)](Commits)

Commit je základní stavební kámen Gitu, kolem commitu se točí naprosto všechno. Commit zabalí změnu provedenou v textu tak, aby na ní bylo možné navazovat, aby jí bylo možné libovolně přesouvat nebo jí měnit.

Commit je jedna kostička z lega. Git repozitář je pak Legoland.

[![LEGO](https://upload.wikimedia.org/wikipedia/commons/2/20/Lego_dublo_arto_alanenpaa_5.JPG)](Lego)

Commit uchová jakoukoli změnu v textu. Je jedno, jestli se změní název souboru, jestli se někam přesune, jestli se změní jeden řádek nebo tisíc, každá uložená změna se zanese.

Commit by měl být co možné nejlepší a měl by dávat sémanticky smysl. To znamená, že každý commit by měl mít nějaký jeden cíl a ten by měl plnit. Ne že tam budou změny které dohromady nedávají smysl.

Commit je neměnitelný (immutable) - nedá se změnit, pouze se dá nahradit jiným.

Commit je jeden z lístků stromu.

Commit se skládá z:
- jména autora
- časového údaje o vytvoření
- commit ID (ad3a5b4)
- commit message
- popis změny (diff)
- ukazetele na předka

Nejdůležitější jsou:
- commit ID
- commit message - píšou se většinou ručně
- popis změny
- ukazatel na předka


**Commit ID** je string - strašně dlouhá unikátní změť znaků, pro operaci s commity se používá prvních 7 znaků. Všechna porovnání se dějou jen a pouze přes Commit ID (hash), neřeší se žádnej čas, jméno autora, prostě dva commity jsou stejné tehdy, když mají stejné commit id.

**Commit message** je zpráva, kterou píše autor, má lidskou řečí popisovat, co daný commit dělá - tu si musíte napsat sami.

**ukazatel na předka** - commit si pamatuje jenom jednoho svého předka (stromová struktura s orientovanými hranami).

**Popis změny** diff řiká, že se například smazal tenhle řádek, že nakonec souboru byl přidán tenhle řádek, že řádek byl nahrazen tímhle, že byl soubor smazán, že byl soubor přesunut atd.


Tady máte schémátko, jak vypadá commity, když jdou za sebou...
```
[ Initial Commit 4fs98hgd ]----[ Commit sdv42jhj5 ]----[ Commit 4gt65kj ]---[ Commit f87bvmwo5 ]
```

### Základní operace s Commity
#### Vytvoření commitu

**Forkneme** si **repozitář** (repozitář je složka na serveru nebo lokálně, ve které je Git inicializovaný - prostě tam, kde se daj dělat Git příkazy).

https://github.com/js-evenings/git-workshop

**Forknutí** znamená, že si repozitář zkopírujeme ke svému účtu na Githubu (GitLabu, Bitbucketu...), přičemž si naše kopie pamatuje svého původního bratra, ale chová se jako samostatný adresář, který bychom si sami vytvořili.

Otevřeme si Git Bash nebo Terminal a **naklonujeme** si repozitář (to znamená, že si ho zkopírujeme z internetu do počítače) - URL je u forku.

Hurá do terminálu:
```
$ cd /tam/kam/potřebujeme
$ git clone https://github.com/< tvoje username >/git-workshop.git < případně název složky, kam to chceš >
```

Zkusíme si základní příkazy:
```
// zobrazení stavu adresáře, ze začátku neukáže nic, pře jsme ještě nic neudělali
$ git status
```
```
// vytvoříme si textový soubor pro poznámky v markdownu (.md)
$ touch notes.md
```

Pokud znova spustíme `git status` měla by se zobrazit změny `new file: notes.md`. Tyhle změny se zobrazují vždy oproti současnému stavu.

Tato změna bude **unstaged** . Unstaged změny jsou takové, které nejsou připravené ke commitnutí.

Krom toho bude soubor `notes.md` v terminologii Gitu brán jako **untracked**. Untracked soubor je takový soubor, který ještě nikdy nebyl přidán do Gitu. Tudíž Git sice vidí, že tam takový soubor je, ale nesleduje zatím jeho změny.

V tomhle případě je změnou vytvoření nového souboru. Git dokáže jednoduše rušit unstaged změny přes příkaz `git checkout`, ale to nefunguje na přidání složek nebo souborů, ty se normálně mažou pomocí `rm`.
```
// smažeme prázdný soubor
$ rm notes.md

// mrkneme se na změny
$ git status
```

A změny jsou fuč!

##### Alias
`git status` budete psát neustále, proto je dobrý si pro něj vytvořit **alias**. Alias je prostě zkratka pro příkaz, aby člověk nemusel psát jak kkt `git status` tak si jde nastavit místo `status` jenom `s` tj.: `git status === git s`:

```
// vytovření aliasu pro git status = git s
$ git config --global alias.s 'status'
// důležité je "alias.s" to, co je za tečkou bude zkratka

// výpis aliasů v ~/.gitconfig
// nebo příkazem
$ git config --get-regexp alias
```

Vytvoříme znovu soubor `notes.md` a pak přes `git s` uvidíme, že soubor byl znovu objeven Gitem!

Unstaged změny nejdou commitnout:

```
$ git commit -m "tohle je přidání souboru" // nic se nestane
```

Prve je potřeba si změny, se kterýma počítáme do commitu, - to jsou ty hezké učesané, odložit do **stage** fáze před commit, prostě si je tam hezky připravujeme. Staged změny jsou ty, které budou commitnuty, jakmile napíšeme příkaz `git commit`. Přidání do stage se dělá pomocí příkazu:

```
// přidání do stage pro commit - příprava pro commit, uschování změn
$ git add notes.md // přidávat jdou soubory, složky, nebo všechno pomocí přepínače --all
```

Nyní `git s` ukáže, že `notes.md` změní barvu tudíž, jsou ve staged změnách pod napisem `Changes to be committed`. Super!

Zajímavá věc je to, že stagednutí změny opravdu tuhle změnu "zamrazí". Takže pokud ten soubor upravíme, tak další změna už nebude staged - na to bacha!

Vyzkoušet - zapsat něco do souboru `notes.md` a uložit ho a pak dát `git s`. Objeví se kategorie `modified` v unstage části popisu.

Tuhle změnu ale nechceme tudíž se jí zbavíme - buďto to amatérsky vymažeme v tom daném souboru nebo přes gitovej příkaz **checkout**.

Nejprve si ale zobrazíme změny tak, jak je zachytil git. K tomu je příkaz **diff**:
```
$ git diff // ukáže, že jsme něco napsali do souboru - zelené řádky s plusy
// pokud chceme zobrazit stagnuté změny, musíme přidat přepínač --staged
//odejdeme z VIMu shift + Z Z
```

```
// zrušíme změny v souboru (!pozor, checkout toho umí mraky)
$ git checkout notes.md
```
**GOTCHA:** jaktože se nesmazala změna "přidání `notes.md`"? Protože tahle změna je už staged - na přidaný soubor jsme zavolali `git add notes.md`.

Pokud chceme vytáhnout změnu, kterou jsme přidali do stage pro commit přes `git add`, tak musíme zavolat **reset**:

```
$ git reset notes.md // vrátí stage změnu zpět do unstaged - pokud uděláme git commit, tak se změna necommitne.

// nyní bych mohl udělat
$ rm notes.md
// a všechny změny by byly zase v čoudu
```
No tak to už umíme, tak si pojďme ten soubor commitnout příkazem:

```
$ git add --all

$ git commit
```
**GOTCHA:** defaultní editor pro úpravu commit messages a vůbec všeho textového v Gitu je VIM.
A nikdo neví, jak do něj něco napsat nebo hůř, jak se z něj dostat proto:
```
// zapisovací mód
$ i
```
Nyní napíšeme nějakou commit message - to, co jsme udělali. V našem případě asi něco jako:
```
přidání souboru pro poznámky notes.md
```
A pak pryč z VIMu

```
// ofiko quit je myslim :q!
//jinak
$ ESC
$ shift + Z Z
```
Dá se to nějak přenastavit, já tam mám Sublime, ale už nevim, jak jsem to udělal cc @jankuca

Jasně, nikoho nebaví se furt přepínat do VIMu. Proto existuje zkratka, kterou používám:
```
$ git commit -m "commit messsage"
```
A rovnou si na ní uděláme aliasík, ne? Co takle, kdyby `git cm "commit message"` bylo to samé?
```
$ git confit --globa alias.cm 'git commit -m'
```
Vyzkoušíme si:
```
// udělat změnu
$ git add notes.md
$ git cm 'moje message'
```

Hotovo, commit udělán, co teď? Jdeme dál commitovat!

Prve se ale mrkneme, co jsme udělali. K zobrazení commitů slouží příkaz **show**:

**GOTCHA:*** Proč nepoužíjeme příkaz `git diff`, když už ho umíme? Protože tady se jedná už o změny v commitu. Diff slouží pro zobrazení změn, které nejsou commitnuté.

```
$ git show <? commit id > // bez poslední parametru ukazuje vždy poslední commit
```

Vidíme změnu v původním commitu, čas, autora, commit message - to je náš první commit, jdeme ho teda smazat!

#### Smazání commitu
No, smazat. Ono smazat commit je poměrně dost těžká práce. Ukážeme si.

nejdřív se ukážeme seznam všech commitů - příkaz **log**, které jsme doteď vytvořili - nelekejte se, jsou tam i moje původní a to je dobře.

```
// zobrazit seznam commitů na aktuální branch
$ git log
// hezčí zobrazí seznamu
$ git log --pretty=oneline -n 50 --graph --abbrev-commit
// uděláme na něj alias
$ git config --globa alias.l 'log --pretty=oneline --graph --abbrev-commit --branches --decorate -n 100'
//vyzkoušíme
$ git l

```
Git log ukazuje nahoře nejaktuálnější commit - poslední vytvořený a dole pak jsou ty nejstarší. Měl by být vidět nejnovější ten commit, který jsme právě vytvořili.

Jdeme ho smazat.

Nejjednoduší "smazání" je přes příkaz **reset**:
```
// $ git reset < odkud kam >
// smazání posledního commitu (nejaktuálnějšího)
$ git reset --hard HEAD~1 // odeber jeden nejnovější commit (na dané větvi)
// případně
// $ git rest --hard HEAD~2 // odeber dva nejnovější commity (na dané větvi)
```
**HEAD**? Řikáte si, co je to HEAD? Head je označení pro poslední commit větve nebo prostě commit, na kterém jste nastaveni. Proto mám poznamenáno:

`Odeber jeden nejnovější commit tj.: HEAD~1 - od indexu 0 - 1` Pokud bych napsal `git reset --hard HEAD`, tak se nic nestane, protože říkám: `odeber commit od 0 do 0`

Nyní se podíváme na stav commitů, jak jdou za sebou `git l`. Vidíme, že náš commit je pryč, smazaný konec světa, jdeme to dělat znova!

**GOTCHA:** Smazat něco v Gitu je těžká práce, fakt hodně těžká. Smazat commit je z toho asi nejtěžší. Git po nějaké době maže sám ty commity, které nejsou u žádné branche (vysvětlíme). Chová se jako garbage collector a prostě maže to, k čemu není přístup.

Takže Vojto, ten commit je smazaný? Ne, není. Jenom jsme ho odebrali z větve, snadno ho dáme zpátky. Je spousta cest, jak to udělat.

#### Cherry-pick
V našem případě si můžeme krásně zkusit příkaz **cherry-pick** (cherry-pick prostě vezme odněkud commit a přidá vám ho pod ruku (na vrchol branch a posune tak ukazatel HEAD), jak kdybyste ho právě udělali ručně):

```
// cherry-pick je opravdu jednoduchý, prostě vezme commit (jedno odkud) a nastaví ho jako poslední na aktuální branch
$ git cherry-pick < commit id >
```

Zkontrolujeme, že je vše zpátky, jak má být aliasovaným příkazem `git l`. Juuhůů!

Takže umíme:
- udělat změnu (prostě napsat něco nového do souboru, vytvořit soubor...)
- smazat unstaged změnu
- přidat změnu do stage pro commit
- smazat změnu ze stage pro commmit
- vytvořit commit
- jít do insert modu ve VIMu
- odejít z VIMu
- smazat poslední commity z branche
- cherry-picknout commit zpátky

Procvičit! Celé znovu!!
**Úkoly**: Vše kontrolovat přes `git s` nebo `git l`!!!
- vytvořit soubor `cvicime.md`
- přidat soubor do stage pro commit
- odebrat ze stage pro commit
- smazat soubor
- přidat soubor
- přidat do stage pro commit
- commit s message `pridali jsme soubor cvicime.md pro cviceni`
- udělat změny v souboru
- smazat změny přes git
- udělat znovu změny
- přidat změny do stage pro commit
- commitnout změny s message `cvicebni zmeny v souboru cvicime.md`
- smazat oba dva commity najednou
- dostat přes Git je zpátky

```
$ cd ..
$ rm -rf git-workshop
// znova!
```

### Pokročilé operace

#### Úprava commitu
Často se stává něco jako `Dopiči, tohle jsem tam commitnul blbě, to musim opravit.` anebo `Tohle by asi mělo být ve zvláštním commitu, ne?`.

##### Ammend - Dopiči, tohle jsem tam commitnul blbě, to mělo být takle...`
Tak co vás napadá jako první možnost opravy commitu? Udělat novej?

To je taky možnost, ale, nejjednoduší způsob, jak opravit nejaktuálnější commit je přes **amend**.

Uděláme pár změn a pak vytvoříme soubor `ten-se-ma-jmenovat-jinak.js`. A všechno commitneme (to už umíme)!

Teď si každej řekne "Do piči, ten soubor `ten-se-ma-jmenovat-jinak.js` se měl přeci jmenovat `takle-se-ma-jmenovat.js`!!!"

Jak to smáznout z commitu? Mno.

Jak jsem pravil, commit je neměnitelný - (immutable), tudíž commit jako takový nejde změnit, jde pouze nahradit jiným - snad nekecám. To je nám ale celkem šumák, protože výsledek je stejný. Kolikrát to ani člověk nepozná, neboť se u commitu třeba jenom změnit `commit id`.

Takže **amend**:
```
// smažeme soubor, co nechceme - normální změna
$ mv ten-se-ma-jmenovat-jinak.js ten-se-ma-jmenovat-jinak.js

// přidáme normálně do stage pro commit
$ git add ten-se-ma-jmenovat-jinak.js

// teď už neuděláme klasický commit, nýbrž
$ git commit --am
```

`--am` nebo též `--amend` říká "tyhle změny přidej do předchozího commitu" tj. nevytváří se nový commit, změny jsou jen přilepeny k aktálnímu.

`--no-edit` říká, že nechceme měnit původní commit message, ale chceme ponechat tu z předchozího commitu. Pokud bychom toto vynechali, tak by vyskočil VIM a to je zbytečná práce.

Pokud nepoužijeme `--no-edit` tak pomocí `git commit --am` můžeme jednoduše upravovat **commit message** posledního commitu.

Šup s tím do aliasu:
```
$ git config --global alias.cam 'commit --am --no-edit'
```
Takže pro `amend` teď stačí napsat jenom `git cam` a všechno funguje, bombička.

##### Reset --soft rozdělení commitu - Tohle by asi mělo být ve zvláštním commitu, ne?
(http://stackoverflow.com/questions/927358/how-to-undo-last-commits-in-git)

Na tohle je `amend` krátký. Tady je totiž potřeba vytáhnout soubor ven z commitu a to přes ammend neuděláme.

Ale je to možná snažší než ammendovat. Takže.
```
// uděláme nějaké změny třeba v notes.md
// vytvoříme soubor, který má být zvlášť v commitu
$ touch pro-zvlastni-commit.js
$ git add --all
$ git cm "tenhle commit rozdelime"
```
Klasika, to umíme. Můžeme se mrknout přes `git s` na to, jak vypadá workspace, měl by být čistý a přes `git show` na to, jak vypadá commit, který jsme právě udělali.

Je tam naše změna, plus je dole napsáno, že přibyl nový soubor. To jsme chtěli. Tak teď jak ten commit rozbijem?

Vlastně nám stačí se dostat do fáze před tím, než jsme commit vytvořili - tam kde změny jsou provedené, ale nejsou commitnuté a přes to dělá **git reset <--soft>**

Soft reset smaže commit, ale ponechá změny ve workspace.
```
//1 psát nemusíme, už známe, že tohle znamená - resetni jeden commit do minulosti, můžeme jich resetnout i více
$ git reset --soft HEAD~
```

Nyní `git s` ukáže, že máme necomitnuté změny ve workspace a `git l` ukáže, že commit s message `tenhle commit rozdelime` už není v historii.

No a nyní můžeme přidat zvlášť soubor `notes.md` a commitnout ho do commitu s nějakou zvláštní message a stejně tak pak přidáme soubor `pro-zvlastni-commit.js` a k němu také napsat jinou message a je to! Pro kontrolu použíjeme `git l`. A vidíme dva commity, které jsme právě udělali.

No a teď si ty dva bordel commity resetneme a jedeme dál. A podíváme se na `git l` - dva commity, které jsme udělali by tam neměli být.


### Gitignore
Btw. stává se, že se do Gitu přimotaj soubory, které tam nechcete. Typick na Macu je to soubor `.DS_Store` (na Windows zase `Thumbs.db`), do kterého OS X přidává nějaké kokotiny o aktuální složce. Tyhle soubory jde jednoduše ignorovat pomocí souboru `.gitignore`.

Takže si ho vytvoříme a dáme si do něj třeba ten `.DS_Store`:
```
$ touch .gitignore
$ vim .gitignore
// napsat .DS_Store
// dají se přidávat i složky nebo cesty:
// moje-slozka/
// cesta/k/moji/slozce
```

Pokud jste už přidali soubor, který nechcete mít v Gitu, nezoufejte. Gitu se dá snando říct, aby ho přestal sledovat (**trackovat**) a pak ho začal ignorovat.

Avšak logicky soubory v `.gitignore`, které jsou už commitnuté jsou nadále sledované. `.gitignore` odignoruje pouze soubory, které jsou untracked (nebyly zatím nikdy přidány přes `git add ...`)

Smazat již trackované soubory je snadné:
```
$ git rm --cached < název souboru >
```

Blbý je, pokud je tenhle soubor už commitnutej, to se pak musí z vyzobnout z commitu, který ho přidal - to umí `git reset --soft`, že?

## Branching
Větvení je další esenciální featura Gitu.

Každý Commit má jenom jednoho předka, ale nikde není psáno, že jeden commit nemůže mít několik potomků. Commity se sdružují do stromové struktury:

[![GitBranch](https://www.drupal.org/files/repositorydiagram.png)](Branch)

Pokud vytvoříme v Gitu branch (větev) tak umožňujeme, aby jeden Commit měl více potomků a tím se nám vývoj větví.

### K čemu to je dobrý?
Představme si, že chceme mít naše poznámky v angličtině. Do teď jsme si je psali česky. Jak to udělat, abychom to měli i v angličtině? No, pokud máme jenom jednu větev, tak jediný způsob je udělat commity, které češtinu přeloží do angličtiny, ale tím pádem ztratíme čestinu, která bude utopená někde v historii.

Nejlepší by bylo zároveň dál psát česky a zároveň překládat češtinu do angličtiny. Prostě vytvořit si anglickou větev.

### Jak na branchování
Jednoduše se dá zjistit na jaké branch jsme teď + zobrazení všech větví:
```
$ git branch

// rychle udělat alias
$ git config --global alias.b branch
```
A zobrazí se nám **master**.

**Master** je další pojem z Gitu. Master je ustálený název pro hlavní větev vývoje, do které jsou spojeny (mergnuty) vývojové větve. Tahle větev by měla být stabilní a dokonce by se do ní přímo nemělo commitovat - k tomu se dostaneme až budeme dělat `git flow`.

Pro naše účely ale nebudeme tak striktní a řekneme si, že naše hlavní vývojová větev **master** bude větev v češtině a větev **english** jí bude následovat.

#### Vytvoření vývojové větve
Vytvořit **branch** - větev se dá mnoha způsoby.
```
// klasicky
$ git branch english
```
Stalo se to, že jsme z rodičovské verze `master` vytvořili novou větev, která se jmenuje `english`.

`git checkout` se používá nejen pro smazání unstaged změn, ale taky k přepnutí branchí:
```
$ git checkout english
$ git b
```
Ukáže se nám seznam branchí a naše aktuální, na kterou jsme se přepnul přes `git checkout`.

Tohle je ale zdlouhavý způsob. Sám `git branch` používám jenom k zobrazení větví. K vytváření větví používám:
```
// zpět na master
$ git checkout master
$ git b // jsme v masteru

// smazání větve
$ git branch -D english

// vytvoření větve english a přepnutí do ní rovnou
$ git checkout -b english
$ git b // jsme v english větvi
```
No a co teď?

Když nyní commitneme, tak commit bude stále pouze na této větvi. Tak do toho, uděláme si soubor, který bude pouze pro tuhle větev.
```
$ touch english.md
$ git add english.md
$ git cm "add file in english on english branch"
```
Nyní, pokud se přepneme do masteru soubor `english.md` zmizí, protože je vedený pouze na větvi `english`.

A pozor, pokud si zobrazíme `git l`. Tak vidíme commit, ze kterého vychází branch `english` a pokud uděláme nyní další commit tak se stane, že tenhle commit, ze kterého vychází větev `english` bude mít dva potomky.

To je v pohodě. Ale horší budou jiné věci...

Nyní, pokud chceme ukázat větvení, tak se přepneme zpátky do masteru a uděláme commit:
```
$ git checkout master
$ touch czech.md
$ git add --all
$ git cm "added file in czech on master branch"
```
Zkusíme příkaz `git l`, pokud nemáte nastavený stejně alias, napište:

```
git log --graph --all --decorate --pretty=oneline --abbrev-commit
```
Voila - už nám rostou větvičky ze stromu a bude hůř!

Resetneme si bordel, co jsme udělali na obou brančích a přepneme se do `english` a přeložíme si kus `notes.md` a změnu comitneme.

Teď to začne bejt hustý. Máme tedy kousek textu přeloženej a teď si představíme, že normálně píšeme poznámky dál v češtině na větvi `master`.

Takže se přeneme do `masteru` zase zapíšeme několik poznámek do `notes.md` a změnu commitneme.

No a co se teď stalo? My jsme aktulizovali větev `master`, ale pokud se přepneme do `english`, tak zde ta změna není vidět.

Proč?

odpověď se nachází v `git log` nebo v `git l`.

Jde o to, že když jsme vytvářeli branch `english` tak ona se vytvoří z body, který byl tehdy aktuální. Commity, které nyní vytvoříme na rodiči se nepromítnou, do `english` - logicky.

Proto je třeba nějak říct větvi `english`, aby se aktualizovala a my mohli překládat dál, jak to uděláme?

### Rebase
Rebase je asi nejmocnější nástroj v Gitu, co existuje. Jdou s ním dělat neskutečný věci, ale prozatím stačí, když ho použijeme na aktualizaci větve `english` tak, aby její první commit měl za předka poslední, aktuální commit na větvi `master`. Prostě jí `pře-bázujeme`.

Nyní je čas si prohlédnout `git l`. A vidíme, že `english` vychází z neaktuálního commitu v `master`. Nyní zavoláme:
```
$ git rebase master
```
`gite přebázuj aktuální větev na větev master = posuň větev english tak, aby vycházela z nejaktuálnější commitu větve master`

no a teď:
```
$ git l
```

A strom je pryč, neboť není potřeba, vše je aktuální. Ukazatelé na předky byly posunuty, tudíž nejaktuálněší commit z celého repozitáře je poslední commit na větvi `english`.

UF!

## Co se nestihlo
- spojování větví `rebase` a `merge` jaký je rozdíl?

### Rebase
jenom vezme commity a vloží je do historie vaší branche. Pokud se stane konflikt, tak vás rebase mód vyzve k úpravě commitu.

### Merge
Merge vezme branch, kterou mergujete do aktulní branche. Pak ji "schová" do `merge commitu` a pokud se vyskytnout konflikty, tak vám poručí je vyřešit a pak změny commitnout a tím se vytvoří merge commmit.

#### Merge Commit
Je zvlášní druh commitu, který drží ukazele na commity z branche, kterou mergujete. Takže je to commit, který v sobě schovává víc commitů. Výsledek merge je stejný jako výsledek rebasu - máte spojené dvě větve v jednu - s tim rozdílem, že merge vytvoří explicitní merge commit, kde jsou vyřešené konflitky, zatímco pomocí rebasu jen měníte stávající commity tak, aby nekonfliktovali.

`Merge commit` se dá snadno smazat normálně pomocí `git reset --hard`.

V Master a Dev branchích jsou vidět merge commity, aby bylo jasné, jaké branch se kdy udělala a kdo jí udělal.


