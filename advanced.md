# Pokročilosti
## Změna historie
Do teď jsme si ukazovali, jak se dá jednoduše upravit přes `--amend` poslední commit v historii - tedy ten aktuální.

Jak ale upravíme i starší commity? No, nejdřív je třeba is uvědomit, že to s sebou nese několik problémů.

Na změnách (commitech), které jsou v historii, jsou totiž založené změny, které jsou novější. Jak jsem ukazoval dříve - nejdřív musí existovat commit, který vytvoří soubor a až po něm musí být commit, který upravuje tento soubor.

Takže pokud bychom chtěli v historii například smazat commit, který vytváří soubor, co je následně upravován, nebo bychom ho chtěli přesunou v historii dopředu, že by až přeskočil nějaký commit, který upravuje jím vytvořený soubor, tak dostanem **konflikt**. Zkrátka to není tak snadné. Proto je složitější manipulovat s commity v historii než upravovat commit, který je aktuální (HEAD commit).

Nicméně to je možné a existují minimálně dvě cesty, jak to udělat.

### Vytvoření nové branche z upraveného commitu
Krom toho, že se přes `git checkout` můžeme přepnout do jiné branche, tak se také můžeme prostě přepnout na commit.
```
git checkout < commit id >
```
To nám umožňuje následující postup:

1. najdu si v historii commit, který chci nějak změnit - například změni jeho commit message, upravit změny, které vytváří.
2. `checkoutnu` se na ten commit
3. vytvořím z něj opravnou pomocnou branch: `git checkout -b docasna-opravna-branch`
4. jsem přepnutý na `docasna-opravna-branch` a poslední commit je ten, na který jsme se checkoutovali ve druhém kroku
5. Nyní provedeme změnu - například změna commit message nebo změna obsahu commitu - vše přes `commit --amend`
6. nyní `cherry-pickenem` commity, které byly na původní branchi nad opravovaným commitem - případně řešíme konflikty
7. jakmile jsou commity `cherry-picknuty` tak smažeme původní branch `git branch -D puvodni-branch`
8. a opravnou si přejmenujeme tak, aby se jmenovala stejně jako původní `git branch -m puvodni-branch`

Tohle je trošku pracná metoda, ale je v rámci toho, co už umíme, druhá metoda je trošku víc fancy.

### Iteraktivní rebase
Rebase je mocná věc, ale jeho interaktivní podoba je ještě mocnější.

Ale taky se s tím dají udělat velké boty:

[![Rebase](http://65.media.tumblr.com/4c5d2f68624a568985c59423a27fcb3c/tumblr_o5fadyZqP61ugyavxo1_1280.jpg)](Rebase)<br>
“Junior programmer's first `git rebase –interactive`“ - Salvador Dalí, 1936, Oil on Canvas

Tedy, interaktivní rebase nám dovoluje měnit commity v historii.
Zkusme si:
```
git rebase -i head~5
```
Nám zobrazí commitovou historii až do pátého nejaktuálnějšího.

S tím že - nejaktuálnější commit je dole a nejstarší nahoře. Zpracování rebase se děje odzhora dolů a tak se cherry-pickuje.

#### Co se s tím dá dělat
- měnit pořadí commitů: to se hodí, když máte ošklivou historii
- přidávat commity: můžete prostě dopsat kamkoli `pick < commit id >` a commity se přidá
- `drop` odebírat commity: buďto `pick` přepíšete na `drop` nebo jenom na `d` a commit zmizí z historie.
- `edit`: přepište tam, kde chcete `pick` na `edit` nebo `e` a rebasování se zastaví na commitu a vy budete moc provést `ammend`
- `reword`: přepište `pick` na `reword` nebo `r` a rebase zastaví a zobrazí vám editor pro změnu commit message commitu
- `squash`: sloučit dva commity do jednoho - přesuňte si bordel commit tam, kam patří a pak ho přepište `pick` na `squash` nebo jenom `s` a commit se `ammendne` do commitu v seznamu nad ním tj. do staršího v historii a k tomu vám to nabídne změnu `commit message`
- `fixup`: fixup je to samé, co squash, ale nevyzívá pro změnu commit message

Tohle je asi to nejmocnější na správu commitů, co Git nabízí, kdo s tím umí je za vodou... Jdeme zkoušet!!

Rebasování merge commitů je trošku složitější, neboť za sebou schovávají více commitů, aby rebase pochopil, že má brát merge commit jako jeden commit a né jako všechny, které jsou za ním - což dělá defaultně, tak musíme napsat přepínač `-p`.
```
git rebase -i -p head~10
```

## Deployment přes git repozitář
**!!WARN!!** tohle je jenom pro testovací účely!! Takovéhle nastavení se **nehodí** pro produkci. Protože vám na server kopíruje celou gitovou historii + soubory, které mohou být jen pro vývoj - a to je špatně!! Tohle nastavení se hodí jenom pro testování a debuggování!!

Git funguje na ssh. Proto vám nebrání si udělat z jakéhokoli počítače vzdálený gitový repozitář. A jakou to má souvislost s deploymentem?

Git má zabudované `hooks` - hooky, které se spustí při nějaké akci. Například při po provedení commitu, přes pushováním apod. Nás zajímá hook, který se zavolá ve chvíli, kdy byl repozitář aktualizován - když je do něj pushnuto. Jak na to?

Můžete si to vyzkoušet na třeba na Macu tak, že si aktivujete `Vzdálené přihlášení` v nastavení sdílení. Pak se budete moc přihlásit přes ssh do svého počítače např. takto: `ssh < vaseUzivatelskeJmeno >@< ip adresa >`- ip adresu zjistíte přes příkaz `ifconfig` u měj je celý příkaz: `ssh vojtatranta@192.168.0.101`.

Pokud se přihlásíte na váš počítač (nebo jakýkoli jiný sever, ke kterému máte ssh přístup) tak pak už jenom zbývá například v home složce uživatele, kde jste přihlášeni vytvořit složku, kde bude ležet váš projekt.
```
mkdir deployment
```
A pak složku initnout jako git repozitář:
```
cd deployment
// vytvoříme prázdný gitový repozitář, což bude náš testovací remote
git init
```
A vytvořit složku, kde budou vašeho projektu, přístupné webserveru:
```
mkdir www
```
Nyní je třeba nastavit tzv. `worktree` gitu. Worktree je složka, ve kterém je trackovaný projekt. Ve výchozím nastavení je to složka ve kterém je git initnut (kde je složka `.git`). V našem případě to tak nechceme - pokud chceme document root nastavit na složku, která je trackovaná gitem, tak není dobrý nápad nechat přístupnou celou historii gitu ve složce, která je přístupná webseverem. Historie gitu je totiž samozřejmě uchována ve složce `.git`.

Takže jdeme přenastavit worktree tak, aby ukazoval na složku `~/deployment/www`
```
cd .git
nano config
```
A tady přidáme klíč `worktree` s správnou cestou pod `[core]`:
```
[core]
  filemode = true
  worktree = /Users/vojtatranta/deployment/www
```
Nyní bude git sledovat složku `/Users/vojtatranta/deployment/www` jako by byla tady: `/Users/vojtatranta/deployment` Takže můžeme bezpečně namířit document root na složku `/Users/vojtatranta/deployment/www` a přes web se nikdo nedostane ke gitové historii.

### Nastavení hooku
Ještě je třeba nastavit `post-receive` hook tak, aby přepnul na aktuální verzi jakmile se pushne do repozitáře.

Měli bychom být ve složce `~/deployment/.git` a příhlášení na ssh (pokračujeme dál ve výše zmíněném) a zadáme:
```
// vytovříme hook soubor
touch hooks/post-receive
```
Pak ho upravíme tak, aby pokaždé checkoutnul na novou verzi branche master, kam budeme pushovat deploynutý kód (můžete mít jakoukoli jinou branch).
```
git checkout -f master
// a pod to si můžeme dát nějaký instalační skript, který spustíme ve worktree třeba:
// cd ../www/
// npm install
// make install
```
A ještě změníme práva tak, aby byl soubor spustitelný:
```
chmod +x hook/post-receive
```

### Nastavení deployment remote
No a teď jenom stačí přidat deployovací remote v lokálním repozitáři (už ne na ssh, tam je hotovo).

Takže v lokálním repozitáři přidáme další remote a nazveme ho třeba `test` aby bylo jasné, že tam pushujeme nějakou funkční verzi.

A pak zadáme ssh adresu a oddělíme ji `:` pak zadáme cestu k .git složce, která kontroluje náš worktree na vzdáleném serveru.
```
git remote add test vojtatranta@192.168.0.101:deployment/.git
```
Jen vyměňte za vaše přístupové údaje.

No a to je všechno. Pokud teď zadáte: `git push test master` mělo by proběhnout pushnutí a pokud jste si nastavili document root, tak by se měl váš projekt ukázat přes nějakou webovou URL v prohlížeči. Nebo minimálně se můžete přihlásit na ssh a zkontrolova, jestli jsou soubory ve worktree, tedy tady: `~/deployment/www`.

Jestli vám to nefunguje, tak napište issue, pokusí se poradit :)).

