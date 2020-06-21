# AutoTL

###### _Python 3.8, IDE: PyCharm 2020.1.2 (Community Edition)_

Program má za úkol vyplnit technický list pořadu.
Data pro vyplnění přebírá z markerů Avid Media Composeru vyexportovaných do textového souboru.

Soubory přebírá ze složky Input_TXT a vyplněné listy uloží do složky Output.
Šablony pro vyplnění jsou uloženy ve složce Data/Templates. Množství nastavení (zkratky poznámek,
adresy buněk pro vyplnění, výchozí hodnoty atd.) lze upravit v Data/Config.ini.

## Postup:

#### Soubor markerů:

Kódování textu souboru určuje prefix ve jméně souboru oddělený podtržítkem (např. UTF_{jméno souboru}).
Pokud jméno souboru prefix neobsahuje,
otevře se v kódování MacCentralEurope. Prefixy jede změnit/přidat v Config.ini (sekce [encoding]).

Výchozí nastavení je:<br/>
default - MacCentralEurope<br/>
PC - windows-1250<br/>
MAC - MacCentralEurope<br/>
UTF - UTF-8

Zápis markerů v souboru je následující:

(název pc)/t(timecode)/t(stopa markeru)/t(barva markeru)/t(komentář markeru)/t1/n

INGEST	00:05:24:10	V1	green	Dialogový titulek	1

Zpracovává se pouze timecode(TC), barva a komentář.

#### Filtrování a úprava metadat:

Nejdříve se oddělí markery obsahující metadata pořadu
od markerů s poznámkami. Hlavní marker s metadaty musí mít barvu určenou v Config.ini (sekce [markery],
výchozí je fialová - magenta) a dále se zpracovává samostatně.
Zbytek markerů metadat se oddělí, pokud komentář odpovídá zkratce určené v Config.ini (sekce [zkratky]).
Kde jdou též změnit nebo přidat alternativní zkratky.

Výchozí zkratky jsou:<br/>
in - začátek pořadu<br/>
zt - stopáž pro zkrácené titulky<br/>
out - konec pořadu<br/>
tl - začátek textless<br/>
end - konec textless

Metadata v hlavním markeru musí být zapsána každé na novém řádku. První řádek musí vždy obsahovat druh
technického listu. Zkratku pro druh listu jde změnit/přidat v Config.ini (sekce [zkratky]).

Výchozí zkratky jsou:<br/>
TVB_Vstup - Vstupní materiál pro TV Barrandov<br/>
TVB_Vystup - Výstupní materiál pro TV Barrandov<br/>
PRIMA_Vstup - Vstupní materiál pro TV Prima<br/>
PRIMA_Vystup - Výstupní materiál pro TV Prima

Zbytek metadat musí být zapsáno ve tvaru {klíč}:{hodnota}. Pokud řádek obsahuje pouze klíč (bez dvojtečky),
přidělí se výchozí hodnota určená v Config.ini (sekce [metadata]). Prázdné řádky a řádky začínající '#' se ignorují.
Datum se přidá automaticky. Šablony metadat jsou uloženy ve složce Šablony.

Příklad markerů metadat:<br/>
INGEST	00:02:00:00	V1	green	in	1<br/>
INGEST	00:02:00:01	V1	magenta	TVB_Vstup<br/>
Nazev_ORIG: Law and order<br/>
Nazev_EP_ORIG: Loco parentis<br/>
Serie: S10<br/>
EP: 10<br/>
HD<br/>
16x9<br/>
Stereo<br/>
A1: CZmix<br/>
A2: CZmix<br/>
Technik_zapis: Tschernoster<br/>

Následuje příprava na zápis. Technické listy se navzájem liší, proto úpravy určuje druh listu
(TVB_Vstup, TVB_Vystup,...). Většina úprav probíhá automaticky bez možnosti nastavení.
Například spojení jména pořadu a čísla série, spojení čísla a jména epizody atd..
Metadata která se mají zapsat velkými písmeny jde nastavit v Config.ini (sekce [caps_meta]).

Výchozí metadata vypsaná velkými písmeny:<br/>
Nazev_CZ<br/>
Nazev_EP_CZ<br/>
Nazev_ORIG<br/>
Nazev_EP_ORIG

Pokud je druh listu TVB_Vystup, ověřuje se formát obrazu. Techniský list pro 16:9 Pillarbox se liší od standartního.

#### Úprava poznámek:

Pokud komentář markeru odpovídá zkratce určené v Config.ini (sekce [zkratky]), celý komentář se vymění.
Zkratky jde změnit/přidat Config.ini.

Výchozí zkratky poznámek:<br/>
dt - Dialogový titulek, je v textless<br/>
dt_ne - Dialogový titulek, NENÍ v textless<br/>
as - Asynchron<br/>
mt - Místopisný titulek, je v textless<br/>
mt_ne - Místopisný titulek, NENÍ v textless<br/>
ct - Časový titulek, je v textless<br/>
ct_ne - Časový titulek, NENÍ v textless<br/>
tit - Titulek, je v textless<br/>
tit_ne - Titulek, NENÍ v textless<br/>
lup - Lupanec

Barva markeru určuje způsob zapsání. Barvu lze nastavit v Config.ini (sekce [markery]).
Font a velikost písma jde také nastavit Config.ini (sekce [pismo]).

Výchozí nastavení:<br/>
Font - Arial<br/>
Velikost - 11

bílý marker (white) - zapíše se do technického listu jako poznámka bez timecodu<br/>

černý marker (black) - marker označující černou v pořadu, přidá před komentář 'Černá '<br/>
Např. INGEST	00:34:59:17	V1	black	2s	1 - se zapíše jako: 'Černá 2s'

červený marker (red) - zapíše se červenou barvou

modrý marker (blue) - zapíše se tučným písmem

Pokud je na začátku komentáře markeru 