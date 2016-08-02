# Webdev / JS Evenings - Git workshop

## O kurzu
Naučíme vás používat Git, abyste byli schopni všech základních operacích a budete rozumět všem základním pojmům.
Kromě toho se dostaneme i k těm složitějším věcem, jako je například rebasování, mergování, řešení konfliktů, úprava commitů
a to i těch hluboko v historii.

## O nás
Webdev / JS evenings klub byl založený Nikitou Mironovem. Nikita začínal učit Javascript a pak na něj navázal Honza Václavík a Dan Rys s jejich
kurzem vývojem mobilních aplikací v Ionicu.

Teď jsem na řadě já Vojta Tranta(@iVojta), JS vývojář v Avocode.

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

Git je jedna kostička z lega.

[![LEGO](http://www.kmart.com.au/wcsstore/Kmart/images/espots/lego-151119-hero-banner-yellowfeature-mobile.jpg)](Lego)

Commit uchová jakoukoli změnu v textu. Je jedno, jestli se změní název souboru, jestli se někam přesune, jestli se změní jeden řádek nebo tisíc, každá uložená změna se zanese.

Commit je neměnitelný (neměnitelný) - nedá se změnit, pouze se dá nahradit jiným.

Commit je jeden z lístků stromu.

Commit se skládá z:
- jména autora
- časového údaje o vytvoření
- Commit ID
- popis změny (diff)
- ukazetele na předka

Nejdůležitější jsou:
- Commit ID
- popis změny
- ukazatel na předka


Commit ID je string, který unikátní identifikuje commit. Všechna porovnání se dějou jen a pouze přes Commit ID (hash).

Commit si pamatuje jenom jednoho svého předka (stromová struktura s orientovanými hranami).

Popise změny diff řiká, že se například smazal tenhle řádek, že nakonec souboru byl přidán tenhle řádek, že řádek byl nahrazen tímhle, že byl soubor smazán, že byl soubor přesunut atd.

```
[ Initial Commit 4fs98hgd ]----[ Commit sdv42jhj5 ]----[ Commit 4gt65kj ]---[ Commit f87bvmwo5 ]
```

### Základní operace s Commity
#### Vytvoření commitu

**Forkneme** si **repozitář** (repozitář je složka na serveru nebo lokálně, ve které je Git inicializovaný - prostě tam, kde se daj dělat Git příkazy).

https://github.com/js-evenings/git-workshop

**Forknutí** znamená, že si repozitář zkopírujeme ke svému účtu na Githubu (GitLabu, Bitbucketu...), přičemž si naše kopie pamatuje svého původního bratra, ale chová se jako vlastní adresář.

Otevřeme si Git Bash nebo Terminal a **naklonujeme** si repozitář (to znamená, že si ho zkopírujeme z internetu do počítače) - URL je forku.

Zkusíme si základní příkazy:
```
// zobrazení stavu adresáře, ze začátku neukáže nic, pře jsme ještě nic neudělali
```
$ git status
```
// vytvoříme si textový soubor pro poznámky v markdownu (.md)
$ touch notes.md
```

Pokud znova spustíme `git status` měla by se zobrazit změny `new file: notes.md`

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

Prve je potřeba si změny, se kterýma počítáme do commitu - to jsou ty hezké učesané, odložit do **stage** fáze před commit. Staged změny jsou ty, které budou commitnuty, jakmile napíšeme příkaz `git commit`. Přidání do stage se dělá pomocí příkazu:

```
// přidání do stage pro commit - příprava pro commit, uschování změn
$ git add notes.md // přidávat jdou soubory, složky, nebo všechno pomocí přepínače --all
```

Nyní `git s` ukáže, že `notes.md` změní barvu tudíž, jsou ve staged změnách pod napisem `Changes to be committed`. Super!

Zajímavá věc je to, že stagednutí změny opravdu tuhle změnu "zamrazí". Takže pokud ten soubor upravíme, tak další změna už nebude staged - na to bacha!

Vyzkoušet - zapsat něco do souboru `notes.md` a uložit ho a pak dát `git s`. Objeví se kategorie `modified` v unstage části popisu.

Tuhle změnu ale nechceme tudíž se jí zbavíme - buďto to amatérsky vymažeme v tom daném souboru nebo přes gitovej příkaz **checkout**.

```
// zrušíme změny v souboru (!pozor, checkout toho umí mraky)
$ git checkout notes.md
```
**GOTCHA:** jaktože se nesmazala změna "přidání `notes.md`"? Protože tahle změna je už staged - na přidaný soubor jsme zavolali `git add notes.md`.

Pokud chceme vytáhnout změnu, kterou jsme přidali do stage pro commit přes `git add`, tak musíme zavolat **reset**:

```
$ git reset notes.md // vrátí stage změnu zpět do unstaged - pokud uděláme git commit, tak se změna necommitne.
```






