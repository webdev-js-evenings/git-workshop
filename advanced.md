# Pokročilosti
## Iteraktivní rebase
Rebase je mocná věc, ale jeho interaktivní podoba je ještě mocnější.

Dovoluje vám měnit commity v historii.
Zkusme si:
```
git rebase -i head~5
```
Nám zobrazí commitovou historii až do pátého nejaktuálnějšího.

S tím že - nejaktuálnější commit je dole a nejstarší nahoře. Zpracování rebase se děje odzhora dolů a tak se cherry-pickuje.

### Co se s tím dá dělat
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
