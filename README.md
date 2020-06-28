# AutoTL

###### _Python 3.8, IDE: PyCharm 2020.1.2 (Community Edition)_

Program má za úkol vyplnit technický list přijatého, nebo vyrobeného, pořadu v dabingovém studiu. 
Data pro vyplnění přebírá z markerů Avid Media Composeru vyexportovaných do textového souboru.

Soubory přebírá ze složky Input_TXT a vyplněné listy uloží do složky Output.
Šablony pro vyplnění jsou uloženy ve složce Data/Templates. Množství nastavení (zkratky poznámek,
adresy buněk pro vyplnění, výchozí hodnoty atd.) lze upravit v Data/Config.ini.

## Funkce:

### Soubor markerů:

Kódování textu souboru určuje prefix ve jméně souboru oddělený podtržítkem (např. `UTF_{jméno souboru}`).
Pokud jméno souboru prefix neobsahuje,
otevře se v kódování MacCentralEurope. Prefixy jde změnit/přidat v Config.ini (sekce `[encoding]`).

#### Výchozí nastavení je:<br/>
default - MacCentralEurope<br/>
`PC` - windows-1250<br/>
`MAC` - MacCentralEurope<br/>
`UTF` - UTF-8

Zápis markerů v souboru je následující:

(název pc)/t(timecode)/t(stopa markeru)/t(barva markeru)/t(komentář markeru)/t1/n

`INGEST	00:05:24:10	V1	green	Dialogový titulek	1`

Zpracovává se pouze timecode(TC), barva a komentář.
___
### Filtrování a úprava metadat:

Nejdříve se oddělí markery obsahující metadata pořadu
od markerů s poznámkami. Hlavní marker s metadaty musí mít barvu určenou v Config.ini (sekce `[markery]`,
výchozí je fialová - magenta) a dále se zpracovává samostatně.
Zbytek markerů metadat se oddělí, pokud komentář odpovídá zkratce určené v Config.ini (sekce `[zkratky]`).
Kde jdou též změnit nebo přidat alternativní zkratky.

#### Výchozí zkratky jsou:<br/>
`in` - začátek pořadu<br/>
`zt` - stopáž pro zkrácené titulky<br/>
`out` - konec pořadu<br/>
`tl` - začátek textless<br/>
`end` - konec textless

Metadata v hlavním markeru musí být zapsána každé na novém řádku. Marker musí vždy obsahovat platný druh
technického listu. Zkratku pro druh listu jde změnit/přidat v Config.ini (sekce `[zkratky]`).

#### Výchozí zkratky jsou:<br/>
`TVB_Vstup` - Vstupní materiál pro TV Barrandov<br/>
`TVB_Vystup` - Výstupní materiál pro TV Barrandov<br/>
`PRIMA_Vstup` - Vstupní materiál pro TV Prima<br/>
`PRIMA_Vystup` - Výstupní materiál pro TV Prima

Zbytek metadat musí být zapsáno ve tvaru {klíč}:{hodnota}. Pokud řádek obsahuje pouze klíč (bez dvojtečky),
přidělí se výchozí hodnota určená v Config.ini (sekce `[metadata]`). Prázdné řádky a řádky začínající '#' se ignorují.
Datum se přidá automaticky. Šablony metadat jsou uloženy ve složce `Meta_marker`.

#### Příklad markerů metadat:<br/>
`INGEST	00:02:00:00	V1	green	in	1`<br/>
`INGEST	00:02:00:01	V1	magenta	TVB_Vstup`<br/>
`Nazev_ORIG: Law and order`<br/>
`Nazev_EP_ORIG: Loco parentis`<br/>
`Serie: S10`<br/>
`EP: 10`<br/>
`HD`<br/>
`16x9`<br/>
`Stereo`<br/>
`A1: CZmix`<br/>
`A2: CZmix`<br/>
`Technik_zapis: Tschernoster`<br/>

Následuje příprava na zápis. Technické listy se navzájem liší, proto úpravy určuje druh listu
(TVB_Vstup, TVB_Vystup,...). Většina úprav probíhá automaticky bez možnosti nastavení.
Například spojení jména pořadu a čísla série, spojení čísla a jména epizody atd..
Metadata která se mají zapsat velkými písmeny jde nastavit v Config.ini (sekce `[caps_meta]`).

#### Výchozí metadata vypsaná velkými písmeny:<br/>
Nazev_CZ<br/>
Nazev_EP_CZ<br/>
Nazev_ORIG<br/>
Nazev_EP_ORIG

Pokud je druh listu TVB_Vystup, ověřuje se formát obrazu. Techniský list pro 16:9 Pillarbox se liší od standartního.
___
### Úprava poznámek:

Pokud komentář markeru odpovídá zkratce určené v Config.ini (sekce `[zkratky]`), celý komentář se vymění.
Zkratky jde změnit/přidat v Config.ini.

#### Výchozí zkratky poznámek:<br/>
`dt` - Dialogový titulek, je v textless<br/>
`dt_ne` - Dialogový titulek, NENÍ v textless<br/>
`as` - Asynchron<br/>
`mt` - Místopisný titulek, je v textless<br/>
`mt_ne` - Místopisný titulek, NENÍ v textless<br/>
`ct` - Časový titulek, je v textless<br/>
`ct_ne` - Časový titulek, NENÍ v textless<br/>
`tit` - Titulek, je v textless<br/>
`tit_ne` - Titulek, NENÍ v textless<br/>
`lup` - Lupanec

Barva markeru určuje způsob zapsání. Barvu lze nastavit v Config.ini (sekce `[markery]`).
Font a velikost písma jde také nastavit Config.ini (sekce `[pismo]`).

#### Výchozí nastavení:<br/>
Font - Arial<br/>
Velikost - 11

fialový marker (magenta) - označuje marker s metadaty (viz. Filtrování a úprava metadat)

bílý marker (white) - zapíše se do technického listu jako poznámka bez timecodu<br/>

černý marker (black) - marker označující černou v pořadu, přidá před komentář 'Černá '<br/>
Např. `INGEST	00:34:59:17	V1	black	2s	1` - se zapíše jako: 'Černá 2s'

červený marker (red) - zapíše se červeným písmem

modrý marker (blue) - zapíše se tučným písmem

ostatní barvy (green, cyan, yellow) - zapíše se normálním písmem

Markery je možné spojit a zapsat do technického listu jako poznámku s rozsahem timecodu.
Komentář obou markerů musí začínat znakem pro spojení následovaný libovolným číslem (výchozí zápis je `*{číslo}`).

Timecode druhého markeru se připojí k timecodu prvního. Barva a komentář prvního markeru určuje způsob zápisu
a obsah poznámky. Znak pro spojení jde změnit v Config.ini (sekce `[znak_spojeni]`).

#### Příklad spojených markerů:<br/>
`INGEST	00:31:15:05	V1	green	*1 Výpadek A1-A4	1`<br/>
`INGEST	00:32:55:05	V1	blue	*1 Barva a komentář tohoto markeru nemá vliv na zápis	1`<br/>
Zápis v TL:<br/>
00:31:15:05 - 00:32:55:05 Výpadek A1-A4
___
### Zápis do souboru:

Excelové vzory pro vyplnění jsou uložené v `Data/Templates`. Název souboru musí být shodný s druhem technického listu
(TVB_Vstup, TVB_Vystup,...). V případě změny souboru, jde změnit přípona v Config.ini (sekce `[template]`),
pro načtení souboru.

Adresy buněk pro zápis v excelu určuje také druh technického listu. Všechny adresy jdou změnit v Config.ini
(sekce dle druhu TL např.`[TVB_Vstup]`). Je možné také přidat nové, pokud se např. změnil technický list.
Adresy poznámek jsou rozdělené na sloupec timecode, sloupec poznámek, první rádek a poslední rádek poznámek.
Pro každou poznámku se generuje nová dvojice adres v tomto rozmezí.

#### Příklad adres poznámek:<br/>
`Prvni_radek_poznamek: 23`<br/>
`Posledni_radek_poznamek: 26`<br/>
`Sloupec_timecode: A`<br/>
`Sloupec_poznamek: G`<br/>

Poznámky budou zapsány na adresy (TC, poznámka):<br/>
A23, G23 - 1. poznámka<br/>
A24, G24 - 2. poznámka<br/>
A25, G25 - 3. poznámka<br/>
A26, G26 - 4. poznámka<br/>

Pokud je poznámek více než počet volných řádků, vytvoří se nový list s názvem Dodatek TL.
Zbylé poznámky se zapíší do tohoto listu.

Vyplněný technický list je nakonec uložen do složky `Output`. Název uloženého souboru je stejný jako původní textový
soubor s markery (bez prefixu, pokud ho obsahoval). V případě potřeby, jde změnit přípona uloženého souboru v Config.ini
(sekce `[template]`).
