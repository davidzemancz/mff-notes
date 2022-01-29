# Sylabus

1. Úvod, přehled podoborů počítačové lingvistiky a jejich obsah
2. Přirozený jazyk, jeho funkce a struktura
3. Zpracování jazyka na úrovni znaků, metody korekce překlepů
4. ASIMUT = metoda automatického skloňování českých slov
5. Syntaxe jazyka, přehled hlavních způsobů jejího formálního zachycení
6. Statistické metody v syntaxi a morfologii
7. Automatický překlad.
8. Jevy v přirozeném jazyce za hranicemi syntaxe, problematika zachycení těchto jevů, roviny popisu, logický zápis významu a smyslu vět, pravdivostní podmínky, užití jazyka v komunikaci, komunikační záměr a promluvový akt.
9. Od věty k souvislému projevu v přirozeném jazyce (mluvený a psaný diskurz, dialog), koreferenční mezivětné vztahy, užití zájmen a jiných prostředků zkráceného vyjádření.
10. Praktické problémy s automatizovaným zpracováním mezivětných koreferenčních vztahů na příkladech automatického překladu, dobývání informací z textu a uživatelského rozhraní v přirozeném jazyce.
11. Od věty k jejímu obsahu a zase zpět, reprezentace znalostí, odkazování, inference jako součást interpretace přirozeného jazyka.
12. Využití reprezentace znalostí v systémech pro automatizované vyhledávání v textech, dobývání informací z textů a automatizovanou tvorbu textů.
13. Přehled existujících systémů pro automatizované zpracování přirozeného jazyka podle hlavních metod, dosažené výsledky.
14. Formální prostředky vyvinuté pro popis souvislého projevu v přirozeném jazyce, reprezentace kontextu a jeho dynamického obohacování.

# Otázky

- Co je wordnet?
  - Sémantická síť
  - Obsahuje anglická slova seskupená do množin synonim

- Popište systém ASIMUT.
  - Vyhledávací a jazykový modul
  - Výrazy složené z slov v základním tvarů a z operátorů
  - Algoritmus čte slovo odzadu, dokud není jednoznačně možné určit způsob skloňování
- Podrobně popište systém MOSAIC.
  - Systém na vyhledávání v dokumentech
  - Využívá faktu, že přípony a koncovky nesou význam
  - Vstup je klasický text s typografií (ta se využívá při hodnocení)
  - Proběhne lemmatizace a syntaktická analýza jmenných skupin
- Používá MOSAIC syntaktickou analýzu? Proč?
  - Ano, na nalezení jmenných skupin (několikaslovných termínů)
- Co je a na co slouží strukturní index u Chomského (transformační) gramatiky?
  - Nevím
- Pražský závislostní korpus (PDT)
  - Anotační schéma aplikovatelné na jazyky různých typů
  - Sto tisíc vět
  - Anotace na úrovni morfologie a syntaxe + tektogramatická rovina
- Unifikační gramatiky - výhody/nevýhody
  - Nevím, ale obecně jsou unifikační gramatiky takové, kde je objekt reprezentován množinou jeho rysů
- Systém Česílko
  - Slouží k lokalizaci softwarových systémů
  - Do blízkých jazyků (SK, PL, RU)
  - Plně automatický a kvalitní překlad
  - Využívá morfologické slovníky
- Kontrola překlepů
  - Možnosti kontroly - slova nebo n-gramy
  - Heuristiky - blízká písmena na klávesnici, statistika 
- Co je morfém a jak ho klasifikujeme?
  - Nositel věcného nebo gramatického významu slova
  - Lexikální (věcný) a gramatický
- Nakreslete složkový a závislostní strom pro větu "Ve včerejším závodu startovali výborní skokani."
- Převeďte složkový strom na závislostní
- Co je překladová paměť?
  - Systém využívá dříve přeložené texty k zdokonalení překladu
- Co je vyhlazování?
  - Řešení problémů s velikostí dat
  - Nutnost umět v datech efektivně reprezentovat "nulové" pravděpodobnosti
- Brownův korpus
  - Jeden z prvních elektronických korpusů
  - Milion slov
  - Náhodné vzorkování
  - 15 druhů textu - noviny, literatura
- Co je ontologie a jak se používá?
  - Množina tříd objektů, která představuje klasifikaci univerza
- Chomskeho teorie
  - Způsob formálního popisu přirozených jazyků
- Co je alomorf?
  - Varianty kmene slova odvozené od stejného slovního základu
- Bickel-Schroderova metoda
  - Nevím
- PennTreebank
- Sestavy rysu a jejich použití.
- Co je transfér v automatickém překladu – přenos zanalyzované věty z jednoho jazyka do druhého (slovosled, morfologie)
- Jaký je rozdíl mezi interlinguou a pivotním jazykem?
- Co je TAG (velmi stručně popište)
- Popište model zašuměného kanálu.
- Funkční generativní popis stručně
- Statistické metody prekladu
- co je LFG?
- co je Two-Level morphology?
- BLEU
- rozdil intenze/extenze
- transparentní intenzionální logika
- co je ATN? (Augmented transition network)
- Stručně popište Český národní korpus(složení, velikost, typy značek).
- Popište Vauquoisův trojúhelník. (trojúhelník s interlinguou na vrcholu)
- Stručně popište systém METEO.
- Stručně popište rozdíl mezi hloubkovou a povrchovou rovinou analýzy syntaxe.
- rozdil mezi morfologickou analyzou a taggingem
- 3 hlavní přístupy k popisu morfologie
- Q systemy (k comu sluzia, kde su aplikované, ako funguju)
- dělení anafor a jak se řeší algoritmicky
- Co je to lemmatizace a kde se používá?
- ALPAC
- metody kontroly gramatickej spravnosti viet (hlavne javy, specificke javy pre cestinu, implementacia)

# Úvod

## Co je lingvistika

* Formální popis vlastností přirozených jazyků a jejich automatickým zpracováním (počítačová)
* Obory počítačové lingvistiky
  * Rozpoznávání a generování mluvené řeči
  * Fonetika (nauka o tvorbě hlásek jako zvuků)
  * Fonologie (nauka o funkci hlásek)
    * Základní jednotkou je foném (zvuková jednotka řeči mající rozlišovací funkci)
  * Morfologie (tvarosloví)
    * Základní jednotkou je morfém (nositel gramatického nebo věcného významu)
    * Zabývá se ohýbáním a pravidelným odvozováním slov pomocí předpon, přípon a vpon
  * Syntaxe (skladba)
  * Sémantika (význam)
  * Strojový (automatický) překlad
  * Formalismy (syntaktické)
  * Korpusová lingvistika
  * Statistická lingvistika (dříve kvantitativní, nyní modeluje užívání jazyka)

## Odložené poznámky

* **LINDAT** - digitální repozitář sloužící k uchování a sdílení dat a nástrojů v oblasti zpracování přirozených jazyků.

# Morfologie

* Předmětem morfologie je studium **vnitřní struktury slov** (skloňování, časování a stavba slov pomocí předpon, přípon, vpon)
  * **Lexikologie** - studium slov (resp. lexémů) jako jednotek slovní zásoby
  * **Lexikografie** - sestavování slovníků
* Základní jednotkou je **morfém** - nejmenší znaková jednotka jazyka nesoucí význam (gramatický nebo věcný)
  * **Lexikální morfém** - nese význam slova
  * **Gramatický morfém** - určuje gramatickou roli slovního tvaru
* Studuje způsoby **časování** (deklinace) a **skloňování** (konjugace)
* Rozlišujeme **autosémantická** (plnovýznamová) a **synsémantická** (pomocná) slova
* Tvaroslovný **dublet** - slovo, které je víceznačné a má různé slovní druhy
* **Alomorfy** jsou varianty kmene odvozené od stejného slovního základu (matka, matce)

## Morfologická typologie jazyků

* **Analytické**
  * Izolační
  * Slovo = morfém
  * **Bez předpon/přípon**
  * Vietnamština, Čínština

* **Syntetické** → flektivní nebo aglutinační
  * Slovo > morfém
  * **Flektivní**
    * Mají předpony, přípony, koncovky
    * Daný tvar **morfému určuje více významů** (tz. koncovka určí rod, číslo,...)
    * Latina, slovanské jazyky
  * **Aglutinační**
    * Jeden **morfém nese jeden význam** (tz. koncovka určí rod)
    * Japonština
* **Polysyntetické**
  * Slovo = věta
  * Eskymácké a indiánské jazyky

## Přístupy ke zpracování morfologie

* **Založený na morfémech**
  * Vidí slovo jako řetěz morfémů
  * Vhodné pro aglutinační jazyky
* **Založený na lexémech**
  * Slovo jako aplikace pravidel, které jej upravují a tím vzniká nový slovní tvar
  * Vhodné pro angličtinu
* **Založený na slovech**
  * Hlavní roli hrají vzory, pokud mám vzor, dokážu vygenerovat všechny tvary slova
  * V ČJ existuje asi 250 vzorů

## Two-level morphology

* **Systém na zpracování morfologie** z 80. let
* První obecný model zpracování morfologie přirozeného jazyka
* Založený na konečněstavových automatech
  * **Nezávislá na jazyce**, ale pro každý jazyk nutné vytvořit slovník a pravidla
  * Společný systém morfologie
* **Generování výsledných tvarů slov** ze základního tvaru (neřeší opačný postup)
* 2 úrovně - **lexikální a povrchová**
* Paralelní aplikace pravidel
* Nehodí se pro jazyky se vzory

## Česká morfologie

* Využívá **poziční značky** (15 pozic, 13 kategorií)
  * Každá kategorie má své pořadí ve značce (2 rezervní)
* **Lemma** je základní tvar slova
* **Jednoznačný identifikátor** je lemma a značka

## Činnosti využívající morfologii

* **Morfologická analýza** - výsledkem je **seznam lemmat a značek** popisujících jednotlivé kombinace gramatických kategorií spjatých s daným vstupním slovním tvarem
* **Morfologické značkování** (tagging) - proces **výběru správné značky** v daném kontextu
* **Lemmatizace** - proces **výběru základního tvaru**, ze kterého byl odvozen daný vstupní tvar. Je to klíčová operace pro vyhledávání v textech
  * Lemma - základní podoba lexému (slovo nebo fráze)
* **Stemming** - **odříznutí koncovky**, na rozdíl od lemmatizace je základním tvarem **kmen slova**
* **Generování** - proces výběru správného slovního tvaru, pokud známe lemma a příslušnou kombinaci gramatických kategorií

# Kontrola překlepů

## Požadavky

1. Musejí být nalezeny všechny překlepy a nalezené musejí být opraveny

2. Musejí být přezkoušeny kontextové podmínky korigované verze

3. Slova, která systém nezná, by neměla být hlášena jako chyby

4. Systém by neměl dávat falešná chybová hlášení

5. Korektura musí být co nejvíce automatická

6. Čas zpracování musí být velmi krátký 


→ Tyto požadavky je v praxi velmi obtížné splnit součastně

## Metody

1. **Porovnávání řetězců se slovy ve slovníku**
   * Seznam všech možných slovních tvarů (tzv. wordlist)
   * Slovník lemmat + morfologická analýza
   * **Výhody**: spolehlivé a jednoduché
   * **Nevýhody**: pomalé, náročné na kvalitu slovníku
   
2. **Srovnávání skupin znaků** (dvojice, trojice,…) a hledání nedovolených kombinací znaků
* **Výhody**: rychlé a nezávislé na slovníku
  
* **Nevýhody**: velmi neúplné, i řada chybných slov se skládá z vhodných kombinací znaků 

### Možná vylepšení

* Vzít v úvahu okolnosti vzniku chyb (blízké klávesy apod.)
* Zohlednit statistiku chyb
* Zohlednit pravopisná pravidla (mně x mě, jsem x jsme)
* Heuristika na oddělení chyb a neznámých slov
* Zapojení syntaxe a sémantiky
* Pracovat s kontextem (porovnávat s korpusy apod.)

### Možnosti interakce s uživatelem

* Nic neopravovat (kdo nic nedělá, nic nezkazí)
* Dávat na výběr
* Vyžadovat potvrzení opravy
* Automatická oprava (ne)

### Komerční řešení

* K nabízení vhodných oprav se používá Levenshteinova vzdálenost
* Upřednostňuje se přesnost (nenalezené chyby stejně autor nevidí) a rychlost
* Vylepšení jdou směrem k vyššímu uplatnění kontextu, než k rozšiřování slovníků

# ASIMUT

* ASIMUT - Automatická Selekce Informací Metodou Úplného Textu
* Metoda **automatického skloňování** českých slov
* Hledané výrazy jsou složené ze slov a speciálních operátorů

## Jazykový modul

* Neobsahuje žádný rozsáhlý slovník, založený pouze na **retrográdním slovníku**
  * Retrográdní slovník znamená, že slova jsou v něm uspořádána abecedně odzadu podle pravé strany slova
* Mnoho slov, která mají stejný koncový segment, se stejně skloňuje
* Výjimky jsou v **seznamu výjimek**


## Vyhledávací modul (algoritmus)

* **Porovnávají se jednotlivé znaky základního tvaru slova odzadu** dokud není možné jednoznačně určit, jak slovo skloňovat
* Když získáme kořen slova, přidáme všechny vhodné koncovky

## Problémy

* Není vždy možné určit vzor
* Příliš hrubá klasifikace, pádové koncovky mají varianty → přegenerovávání (tj. vygenerují se i nesprávné tvary)
* Malý rozsah retrográdního slovníku → je nutné přidávat výjimky
* Nefunguje příliš dobře pro slovesa (víceznačnost koncových segmentů základních slovesných tvarů)

# MOZAIKA

* MOSAIC - Morphemic Oriented System of Automatic Indexing and Condensation
* Systém pro **indexaci dokumentů**, tvoření souhrnů a seznamů klíčových slov
* Měl standartní přístup k indexaci
  * Dokumenty indexovány slovy + četnost
* Využívá, že řada koncovek a přípon (v angličtině) nese význam
* Ani ASIMUT ani MOSAIKA nepoužívají rozsáhlé slovníky, al lingvistické poznatky

## Algoritmus

* Vstupem je text s typografickým členěním
* Kroky
  * **Lemmatizace** a morfologická analýza
  * Nalezená **lemmata profiltrována** a odstraněna ta, jejichž kořen nemá vztah k dané tématické oblasti
  * **Syntaktická analýza jmenných skupin** pomocí jednoduché gramatiky (tj. víceslovná spojení)
  * **Přiřazení vah termínům** na základě vah a typografie
  * **Normalizace** vah vzhledem k délce dokumentu
* Výstupem je seznam nevýznamnějších termínů

## Shrnutí

* **Výhody**: není nutné vytvářet slovníky odborných termínů, pouze relevantní přípony doplněné o negativní slovník
* **Problémy**: vytváření slovníků pro tematické oblasti je pracné, neřeší zájmena

# Syntax

* **Závislostní strom věty**
  * Velmi dobře a přehledně **zachycuje vztahy** mezi jednotlivými větnými členy
  * Není ovšem jednoduché jej získat
  * Ne vždy je bez kontextu jasné, co na čem závisí (**závislosti pomocí šipek**)
  * **Přísudek v kořeni**
  * Speciální případy
    * **Koordinace** - dva větné členy se stejnou sémantickou rolí
    * **Apozice** - dva větné členy se stejnou syntaktickou rolí
    * **Pareze** - vsuvka
* **Složkový strom věty**
  * Odpovídá derivačnímu stromu bezkontextové gramatiky
  * **Slova jsou v listech, ve vrcholech závislosti**
  * Je méně přehledný, obsahuje mnohdy velké množství nadbytečných uzlů.
  * Přirozené jazyky nebývají bezkontextové
  * Dá se vyjádřit uzávorkováním, v každé závorce právě dva prvky
* Problém **neprojektivní konstrukce**
  * Není jasné, co na čem vlastně závisí

## Formální gramatické systémy

### Transformační  (Chomského) gramatika

* Noam Chomský
* Způsob **formálního popisu přirozených jazyků**
* Využívá **povrchové** a **hloubkové** struktury
  * Povrchové - zápis
  * Hloubkové - význam
  * Jedné povrchové reprezentaci může odpovídat více hloubkových

* **3 základní komponenty**
  * **Báze** - soubor bezkontextových pravidel, generující složkové stromy (tzv**. frázové ukazatele**)
  * **Transformační komponent** (operují na frázových ukazatelích)
    * Obsahuje pravidla, která z frázových ukazatelů **vytváří povrchovou strukturu věty**
  * **Fonologický komponent** (obsahující regulární přepisovací pravidla přidělující řetězům morfémů fonetické interpretace)
* Množina vět daného jazyka je vytvářena **generativní procedurou**, souborem konečného počtu pravidel
  * Nezachycuje vztahy mezi variantami vět

### Tree Adjoining Grammars  (TAG)

* Elementárními strukturou je **složkový strom**
* Za listy (označené šipkou) je možné substituovat (a to i strom)
* Typy změn
  * **Substituce**
    * List stromu nahrazen jiným, který se shoduje
    * Pořadí substitucí nehraje roli
  * **Adjunkce**
    * Vnitřní uzel nahrazen za pomocný strom

### Lexical Functional Grammar  (LFG)

* Sada dvojic **atribut-hodnota**
* Rozlišuje dva základní typy struktur
  * **c-struktury** (constituent) - spojují slova do frází
  * **f-struktury** (functional) - popisují významové vztahy ve větě
* Každá f-struktura může obsahovat více c-struktur, ale každá c-struktura nejvýše jednu f-strukturu

### Kategoriální gramatiky

* Každému slovu je přiřazena **kategorie**, která popisuje syntaktické vlastnosti dané slovní formy

### Unifikační gramatiky

* Každý objekt (lexém) je reprezentován **množinou vlastností**
* Sjednocování množin vlastností (**unifikace**) může proběhnout tehdy, pokud si žádné dva neodporují (tj. i nesouvisející)
* Hodnota rysu může být také sestava rysů
* **Problém**: lze unifikovat i nesouvisející vlastnosti

### Funkční generativní popis (HPGS)

* 5 rovin – fonetická, fonologická, morfematická, povrchová a tektogramatická 
* Formy a funkce – jednotka na vyšší rovině reprezentuje funkci jednotky na rovině nižší (tektogramatická rovina je nejvyšší)
* Závislostní reprezentace
* Teorie valence (vazby, vyžadované nebo povolené řídícími slovy, zejména slovesy)  

#### Valence

* Obecně je valence schopnost lexikální jednotky na sebe vázat jednotky další
* Dva základní druhy členů na tektogramatické forvině
  * **Aktanty** - Konatel, Patient, Adresát, Origo, Efekt  
    * Každý z nich ve větě zastoupen pouze jednou
  * **Volná doplnění**
    * Mohou se vyskytovat vícektár
* **Vallex**

## Nástroje pro syntaktickou analýzu

### Augmented Transition Networks  

* Hrany typu CAT (přechod do dalšího stavu po nalezení kategorie), JUMP, SEEK

### Q-systémy

* Grafový analyzátor
* Typy objektů
  * Atomy
  * Stromy
  * Seznamy stromů

# Kontrola gramatické správnosti

* **Problémy specifické pro češtinu**
  * Gramatická shoda (podmět a přísudek)
  * Interpunkce
  * Neprojektivní konstrukce (syntax)
  * Zájmena (mně, mě)
* Možnosti kontroly
  * **Chybové vzorky** - vhodné u jazyků s pevným slovosledem
  * **Gramatika** - problém s určením, kdy algoritmus nezná gramatická pravidla a kdy je konstrukce špatně
* Btw, pusťte se nějakou přednášku od **Karla Olivy**, kde mluví o tvorbě korektoru pro MS Word 

## Metoda redukční analýzy

* Vstupní věta se postupně redukuje s při zachování invariantu **"redukce nesmí chybnou větu spravit a správnou zkazit"**
* Hledá se tzv. minimální chybová konfigurace
* Problém s homonymií (souzvučnost slov)

## RFODG (Robust Free-Order Dependency Grammar )

* Pravidla gramatiky popisují správné i chybné konstrukce
* Výpočet probíhá ve fázích, interpret gramatiky rozhoduje, jak se bude stejné gramatické pravidlo používat
* 3 fáze
  * Pozitivní projektivní
  * Negativní projektivní
  * Negativní neprojektivní


## LanGR

* Primárně vyvíjen pro desambiguaci české morfologie
  * **Desambiguace** je proces anotace jazykových dat, vstupujících do korpusu	
    * Anotace znamená přiřazení lemmat a jiných atributů
* Pracuje s pozitivními a negativními desambiguačními pravidly
* Pravidla mohou mít neomezený kontext
* **Redukční metoda** - snaha o 100% přesnost
* Pravidla se musí vkládat ručně (výcházejí z dat korpusu)
* Pravidla jsou vzájemně nezávislá a neuspořádaná, aplikují se v cyklech
* Každé pravidlo má 4 části: kontext, desambiguační část, report a akce
* Postup
  * **Segmentace** - rozdělení na věty
  * **Tokenizace** - rozdělení na slova
  * **Morfologická analýza** - každému tokenu přiřadí seznam dvojic lemma-tag
  * **Morfologická desambiguace** - každému tokenu z jeho seznamu vybere ideálně jednu dvojici lemma-tag
  * **Syntaktická analýza** - větný rozbor

* Používá jej MS Word

# Překlad

* Problémy překladu
  * Práce s morfologií (obyčejný slovník nestačí)
  * Závislost na kontextu
  * Víceznačnost
* Současné trendy
  * Statistické metody (využívají paralelní označkované korpusy)
  * Systémy využívající dříve přeložené texty - tzv. **překladová paměť**

## Významné překladové systémy

### Systran

* Překlad dokumentů EU
* Mezi cca 20 páry (kvalitně pouze A-F-N)

### Eurotra

* Oficiální projekt EU v 80. letech
* 72 jazykových párů - nezvládnutá modularita

### Vertbomil

* Německý nástupce Eurotry
* Překlad mluvené řeči na téma plánování příští schůzky dvou obchodníků

### České systémy

#### APAČ

* Překlad z angličtiny do češtiny v oblasti vodních pump (slovník s cca. 1500 výrazy)

#### Ruslan

* Překlad manuálů operačních systémů velkých počítačů
* Slovník velikosti cca 8500 slov
* Překlad jedné věty trval čtyři minuty

#### Česílko

* Lokalizace velkých softwarových systémů
* Snaha o minimalizaci lidského podílů
* Typ překladu
  * Velmi blízké jazyky (např. CZ-PL, CZ-RU,...)
  * Plné morfologické slovníky
  * Statistická analýza četšiny

# Korpusová lingvistika

* Studium jazyka obecně
  * **Studium struktury**
    * Identifikuje strukturní jednotky jazyka (morfémy, lexémy, gramatické třídy)
    * Popisuje, jak menší jednotky tvoří větší
  * Studium užití
    * Jak mluvčí a pisatelé využívají prostředky jazka k předání informace
    * Nutné velké množství dat (nejlépe ohodnocených)
* **Korpus** - soubor textů považovaný za reprezentativní projev jazyka
* Korpusová lingvistika se zabývá metodami, jak **popisovat, strukturovat a propojovat** velké množství jazykových dat
  * Využívá metody **morfologické** a **syntaktické** analýzy


## Moderní korpusy

* Důležitý je výběr vzorků a **reprezentativnost**
* Obvykle jsou konečné (u **monitorovacích korpusů** se data neustále přidávají)
* Jsou ve strojme čitelné formě
* Nejsou čisté, tj. obsahují i gramatické chyby, cizí slova...

## Známé korpusy

### Borwnův korpus

* Brown Corpus of Standard American English 
* 1 milion slov
* **15 druhů** textu, 500 textů, každý cca 2000 slov, v různých
  kategoriích různý počet textů, pečlivě **náhodně vybrané**
* **Nebyl anotovaný**

### PennTreebank

* První a nejznámější **syntakticky anotovaný korpus**  
* Opět 1 milion slov (2500 novinových článků)

### A další ...

* British National Corpus - 100 miliónů slov
* Oxford English Corpus - 2 biliony slov
  * Morfologická anotace a gramatické vztahy pomocí nástroje Sketch Engine

### Český národní korpus (ČNK)

* Spolupráce UK, MUNI a UJČ
* **Anotovaný** na morfologické úrovní (strojově, nástroji **morfologické analýzy**)
* 600 miliónů slov (15% literatura, 60% noviny, 25% odborné texty)

#### Pražský závislostní korpus

* **Anotační schéma** aplikovatelné na jazyky různých typů
* 100 tisíc vět
* **Anotace na úrovni morfologie, analytické roviny a tektogramatické roviny**
* Založený na teorii **funkčního generativního popisu**
* Původně anotovaný strojově, pak **opravován ručně**

# Pravděpodobnostní a statistické metody

* Využití
  * Překlad (víceznačnost samostatných slov, předložky,...)
    * Tedy například okolní slova nám mohou při překladu napovědět, kterou variantu překládaného slova zvolit
  * Modelování jazyka
    * Hlavním úkolem je **předpovědět následující slovo** pomocí podmíněné pravděpodobnosti na základě konextu
    * $P(A|B)=\frac{P(B|A)P(A)}{P(B)}$
    * $p(w|h)$ kde, w je předpovídané slovo a h je historie
* **N-gramy**
  * Historie předcházejících (případně následující) slov, znaků
* **Vyhlazování**
  * Problém je velikost dat
  * Nutné umět efektivně reprezentovat možnosti, které nenastávají
  * Nevýhoda je, že zmizí rozdíl mezi špatnými a málo pravděpodobnými n-ticemi

## Statistický překlad

* Použít **paralelní korpus** jako trénovací množinu
  * Například dokumenty EU

### Metoda zašuměného kanálu

* Uvažujeme **jazykový** a **překladový** model
  * Jazykový je například trigramový založený na obřím korpusu cílového jazyka
  * Překladový model je založený na menším paralelním korpusu
  * Překladový model vrátí omezený překlad
  * Jazykový model jej upraví, aby byl "pěkný"

### Evaluace systémů automatického překladu

* Metrika **BLEU**
* Tři složky
  * **Unigramová** přesnost C/N, kde N je počet slov v kandidátu a C je počet slov z kandidáta, které se vykytují alespoň v jednom referenčním překladu
  * N-gramová přesnost - zobecnění
  * **Penalizace** za stručnost (Brevity penalty) - pouze n-gramová přesnost by měla tendenci preferovat stručné věty
* $BLEU = BP * (p_1p_2p_3p_4)^{1/4}$ ... penalizace * geometrický průměr za n-gramy 1 až 4
* **Výhody**
  * Rychlá, relativně vysoká míra korelace s lidským překladem
* **Nevýhody**
  * Drahá na referenční překlady
  * Statistické systémy se vůči ní ladí

# Sémantika

* Popis syntaxe umožňuje rozlišit gramaticky správně a nesprávně utvořené věty. Gramaticky správně jde ale napsat i nesmysl.
* **Význam a pravdivost**
  * Pravdivost je dána kontextem, není součástí jazyka
  * I nepravdivá sdělení mají výzanm
  * Obecně je těžké říct, zda věty mají stejný význam
  * Formální jazyky často spojují pravdivost a význam v jedno - přirozené ne

## Lexikální sémantika

* Význam slov můžeme popsat pomocí nějakého metajazyka
  * Sémantické rysy (významové třídy)
  * **Ontologie** - množina tříd objektů, která představuje klasifikaci objektů univerza
* Význam slova ale často závisí na kontextu

### Popis významu slov

* Synonyma, definice
* Sémantické rysy (rod, číslo, vzor, slovní druh ...)

#### Sémantická síť

* Například **WordNet** (angličtina), EuroWordNet (Evropa)
* Obsahuje **množiny slov propojených sémantickými a lexikálními relacemi**
* Aplikace sémantických sítí
  * Automatický překlad
  * Reprezentace znalostí
  * Vyhodnocování kvality překladu

##### WordNet

* **Lexikální databáze** anglických slov
* Sdružuje slova do **synsetů** (každý synset vyjadřuje nějaký koncept)



### Popis významu věty

* **Predikátová logika 1. řádu**
  * Konstruuje logické formule z jednotlivých výrazů vět
  * Problém s modalitou, časem a postoji (výroky typu "myslím si")

#### Intenze a extenze

* **Intenze** - samotný popis výrazu, jeho charakteristika (intenzí pojmu čtverec je pravoúhlost a stejná délka stran)
* **Extenze** - souhrn věcí, které pod výraz spadají

## Základní přístupy k sémantice

### Modelově-teoretická sémantika

* Pracuje s pravdivostními podmínkami vztaženými k určitému modelu

#### Montagueovská gramatika

* **Teorie sémantiky přirozeného jazyka**
* Založená na **formální logice**, lambda kalkulu a teorii množin
* Tvrdí, že neexistuje rozdíl mezi sémantikou přirozených a formálních jazyků
* Rozlišuje syntaktické kategorie
  * t - truth, declerative sentence
  * e - entity expression
  * ...
  * Obecně ve tvaru X/Y, kde sémantika Y je vyjádřena pravdivostní hodnotou X

#### TIL (transparentní intenzionální logika)

* Založena na modifikaci lambda kalkulu
* Je transparentní - tj. nemá formální aparát, nepreferuje žádná slova jako "logická"
* Je založena na pojmu **možných světů**
* Univerzum je chápáno jako množina společná všem možným světům
* Věty v jazyce rozkládá na atomy

### Kompozicionální sémantika

* Vychází z principu kompozicionality, používá různé reprezentace
  * Sémantické rysy
  * Logickou reprezentaci a zjišťování pravdivosti


## Rozpoznávání vztahů v textu

### Anafora

* Výraz, jehož interpretace závisí na kontextu
* Rozlišujeme **Exoforu** (mimo text) a **Endoforu** (v rámci textu) a **Kataforu** (odkaz v textu, ale dopředu)
* Typické výrazy
  * Zájmena a nevyjádřený podmět
  * Elipsa - vypuštěné části výrazů na základě paralelismu s předchůdcem
    *  *Včera jsem šel pěšky. Kam? Domů.*
* Setkáváme se s ní ve všech oblastech zpracování jazyka
  * Získávání informací z textu
  * Automatický překlad
  * Dialogové systémy

* Řešení anafory - nutné využít řadu informací
  * Morfologické značky (např. u zájmen shoda v rodě)
  * Syntaktická struktura věty
  * Statistické přístupy

## Zásoba sdílených znalostí

* Modeluje zásobu znalostí, o které mluvčí předpokládá, že ji sdílí s posluchačem
* Závisí na tom, o čem se aktuální mluví

# Skryté markovské modely (HMM)

* **Metoda analýzy řad** (posloupností událostí v čase) na základě kontextu (řečový signál, posloupnost morfologických značek)
* Aplikace
  * **Analýza řeči**
  * **Morfologické značkování**
  * **Rozpoznávání značek aut**
* Markovova hypotéza - kontext je možno zkrátit na délku, která je spočitatelná
* V podstatě stochastický spočetný automat
* Markovův automat je určen strukturou, přechody mezi stavy a počátečním a koncovým stavem
* 3 úkoly
  * **Rozpoznávání** (ohodnocení) statistického modelu
    * Forward-backward algoritmus 
    * Například určení pravděpodobnosti jevu (například značky auta)
  * **Dekódvání**
    * Viterbiho algoritmus  
      * V podstatě hledání nejkratší cesty
      * Ohodnocení hran odpovídá pravděpodobnosti přechodu
    * Hledání nejpravděpodobnější posloupnosti skrytých stavů   
  * **Učení**
    * Baum-Welshův algoritmus
    * Cílem je najít parametry modelu, tedy pravděpodobnosti přechodů mezi stavy a pravděpodobnosti jednotlivých prvků posloupnosti na trénovací množině.
