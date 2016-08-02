# Webdev / JS Evenings - Git workshop

## O kurzu
Naučíme vás používat Git, abyste byli schopni všech základních operacích a budete rozumět všem základním pojmům.
Kromě toho se dostaneme i k těm složitějším věcem, jako je například rebasování, mergování, řešení konfliktů, úprava commitů
a to i těch hluboko v historii.

## O nás
Webdev / JS evenings klub byl založený Nikitou Mironovem. Nikita začínal učit Javascript a pak na něj navázal Honza Václavík a Dan Rys s jejich
kurzem vývojem mobilních aplikací v Ionicu.

Teď jsem na řadě já Vojta Tranta(@iVojta), JS vývojář v Avocode.

Po mně následuje Petr a jeho kurz o automatizaci vývoje.

Za to, že umím git vděčím hlavně chlapcům @lukas_rychtecky a @jankuca

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



## Co je to Commit
Commit je základní stavební kámen Gitu, kolem commitu se točí naprosto všechno. Commit zabalí změnu provedenou v textu tak, aby na ní bylo možné navazovat, aby jí bylo možné libovolně přesouvat nebo jí měnit.

Commit je jedna kostička z lega. Git repozitář je pak Legoland.

[![LEGO](http://www.kmart.com.au/wcsstore/Kmart/images/espots/lego-151119-hero-banner-yellowfeature-mobile.jpg)](Lego)

Commit uchová jakoukoli změnu v textu. Je jedno, jestli se změní název souboru, jestli se někam přesune, jestli se změní jeden řádek nebo tisíc, každá uložená změna se zanese.

Commit je neměnitelný (immutable) - nedá se změnit, pouze se dá nahradit jiným.

Commit je jeden z lístků stromu.

Commit se skládá z:
- jména autora
- časového údaje o vytvoření
- commit ID
- commit message
- popis změny (diff)
- ukazetele na předka

Nejdůležitější jsou:
- commit ID
- commit message
- popis změny
- ukazatel na předka


Commit ID je string, který unikátní identifikuje commit. Všechna porovnání se dějou jen a pouze přes Commit ID (hash).

Commit message je zpráva, kterou píše autor, má lidsou řečí popisovat, co daný commit dělá.

Commit si pamatuje jenom jednoho svého předka (stromová struktura s orientovanými hranami).

Popise změny diff řiká, že se například smazal tenhle řádek, že nakonec souboru byl přidán tenhle řádek, že řádek byl nahrazen tímhle, že byl soubor smazán, že byl soubor přesunut atd.

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

Tato změna bude **unstaged**. Unstaged změny jsou takové, které nejsou připravené ke commitnutí.

V tomhle případě je změnou vytvoření nového souboru. Git dokáže jednoduše rušit unstaged změny přes příkaz `git checkout`, ale to nefunguje na přidání složek nebo souborů, ty se normálně mažou pomocí `rm`.
```
// smažeme prázdný soubor
$ rm notes.md

// mrkneme se na změny
$ git status
```

A změny jsou fuč!

`git status` budete psát neustále, proto je dobrý si pro něj vytvořit alias

```
// vytovření aliasu pro git status = git s
$ git config --global alias.s 'status'

// výpis aliasů v ~/.gitconfig
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

Hotovo, commit udělán, co teď? Jdeme dál commitovat!

Prve se ale mrkneme, co jsme udělali. K zobrazení commitů slouží příkaz **show**:

**GOTCHA:*** Proč nepoužíjeme příkaz `git diff`, když už ho umíme? Protože tady se jedná už o změny v commitu. Diff slouží pro zobrazení změn, které nejsou commitnuté.

```
$ git show <? commit id > // bez poslední parametru ukazuje vždy poslední commit
```

Vidíme změnu v původním commitu, čas, autora, commit message - to je náš první commit, jdeme ho teda smazat!

No, smazat. Ono smazat commit je poměrně dost těžká práce. Ukážeme si.

nejdřív se ukážeme seznam všech commitů - příkaz **log**, které jsme doteď vytvořili - nelekejte se, jsou tam i moje původní a to je dobře.

```
// zobrazit seznam commitů na aktuální branch
$ git log
// hezčí zobrazí seznamu
$ git log --pretty=oneline -n 50 --graph --abbrev-commit
// uděláme na něj alias
$ git config --globa alias.l 'log --pretty=oneline -n 50 --graph --abbrev-commit'
//vyzkoušíme
$ git l

```
Git log ukazuje nahoře nejaktuálnější commit - poslední vytvořený a dole pak jsou ty nejstarší. Měl by být vidět nejnovější ten commit, který jsme právě vytvořili.

Jdeme ho smazat.

Nejjednoduší "smazání" je přes příkaz **reset**:
```
// $ git reset < odkud kam >
// smazání posledního commitu (nejaktuálnějšího)
$ git reset --hard head~1 // odeber jeden nejnovější commit (na dané větvi)
// případně
// $ git rest --hard head~2 // odeber dva nejnovější commity (na dané větvi)
```
Nyní se podíváme na stav commitů, jak jdou za sebou `git l`. Vidíme, že náš commit je pryč, smazaný konec světa, jdeme to dělat znova!

**GOTCHA:** Smazat něco v Gitu je těžká práce, fakt hodně těžká. Smazat commit je z toho asi nejtěžší. Jak smazat ho úplně. Git to dělá po nějaké době sám, maže ty commity, které nejsou u žádné branche (vysvětlíme). Chová se jako garbage collector a prostě maže to, k čemu není přístup.

Takže Vojto, ten commit je smazaný? Ne, není. Jenom jsme ho odebrali z větve, snadno ho dáme zpátky. Je spousta cest, jak to udělat.

V našem případě si můžeme krásně zkusit příkaz **cherry-pick** (cherry-pick prostě vezme odněkud commit a přidá vám ho pod ruku, jak kdybyste ho právě udělali ručně):

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





