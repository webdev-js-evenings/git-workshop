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
- co je to **branch**
- co je **remote**
- co je **push**
- základní operace
  - clone
  - fork
  - git init
  - git remote add


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

[ Initial Commit 4fs98hgd ]----[ Commit sdv42jhj5 ]----[ Commit 4gt65kj ]---[ Commit f87bvmwo5 ]

### Základní operace c Commity
#### Vytvoření commitu

Forkneme si repozitář (repozitář je složka na serveru nebo lokálně, ve které je Git inicializovaný - prostě tam, kde se daj dělat Git příkazy).
```

```



