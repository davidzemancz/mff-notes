# Program v C#

## Soubory

* Soubory s kódem mají příponu .cs
* Důležitý je také soubor s projektem s příponou .csproj
  * Obsahuje závislosti na jiných knihovnách, nastavení kompilátoru apod.

## CIL (Common intermediate language)

* Specifikace všech jazyků z .NET
* Objektově orientovaný - tj. kromě klasický assemblerových instrukcí obsahuje i pokročilejší pro práci s objekty, heapem, stackem atp...
* V podstatě bytecode
* Univerzální sada instrukcí, do které jsou překládány C# programy
* Je spouštěn **CLR** (Common Language Runtime) - ta ho JITuje do native kódu pro aktuální procesor

### Metadata

* Doplňková data k našemu kódu, obsahují například jména tříd, atributů, metod a jejich parametrů
* Umožňuje použití tzv. *Reflection*, tj. například zavolání metody pouze podle jejího jména jako stringu

### Assembly

* Soubor s CIL kódem a metadaty
* Odpovídá .dll nebo .exe souboru
* V C# je třída Assemby, která právě umožňuje dynamické načítání knihoven za běhu volání tříd, metod pomocí reflexe

## CLR (Common language runtime)

* Spouští program napsaný v C# (nebo VB, F#) již přeložený do CIL
* Postupně ho JITuje do native kódu pro aktuální procesor
* Také obsahuje například GC
* CLR tedy musí být nainstalována na koncovém zařízení
* Používá tzv. lazy loading
  * Assembly se načítá, až když je potřeba (to není nutně hned při spuštění, protože se může používat v zatím nepřeložené části kódu)
  * Takže se snadno stane, že si CLR chybějící knihovny všimne až za běhu, což je pochopitelně nepříjemné 

## JIT (Just-In-Time)

* Jde o způsob překladu programů pro cílový stroj, zde překlad probíhá současně se spouštěním programu
* Základní jednotou překladu je Metoda
* Pro jeden proces proběhne překlad každé části kódu nejvýše jednou (ty nepoužité se ani přeložit nemusí), když ale proces zavřu a spustím znovu, překlad začíná také znovu
* Optimalizuje kód v CIL
* Funguje v něm tzv. **Tiered compilation**
  * Když CLR narazí na metodu, kterou nemá v native kódu, rychle ji JITuje
  * Pokud se daná metoda pak volá často, pustí CLR na pozadí JITování s více optimalizacemi

## AoT (Ahead-Of-Time)

* Překlad do native kódu pořád proběhne na cílovém zařízení, ale obvykle při instalaci
* Umožňuje důkladnější optimalizace
* Podpora od .NET 5 (Microsoft tomu říká Ready2Run)
  * Některé části kódu se ale pořád JITují

## Průběh překladu C# programu

* Překladač (jmenuje se Roslyn) přeloží C# soubory do CIL
  * Vygeneruje samotný CIL kód a k němu metadata
* Na koncovém zařízení uživatel spustí vygenerovaný program
  * CLR pozná, že jde o program v CIL a ujme se režije
  * JIT postupně překládá části kódu pro daný stroj
* Existují tzv. **Self-contained** .exe soubory
  * V souboru je náš program, CLR a všechny využívané .NET knihovny
  * Je sice větší, ale lze ho spustit kdekoliv (skoro)
  * Lze jej vytvořit příkazem *dotnet publish*

## Implementace .NET

* .NET = CLR + BCL (Base Class Library)
* .NET Framework (1.0 - 4.8)
  * Pro Windows
  * Mono ~ Xamarin pro Win, Linux, Android atd...
* .NET Core
  * OpenSource, multiplatformní
  * Měl nahradit .NET Framework a Xamarin
  * Na něj navazuje .NET 5
* .NET 6
  * Přinese podporu pro Android a iOS a WebAssembly

## Poznámky

* ildasm.exe - umí zobrazit CIL kód a meta data
* Dekompilátor ILSpy
* Obfuscator - program, který zabordeláři CIL kód

# Stringy

* Immutable datový typ (tj. po vzniku instance již nelze modifikovat)
  * Tedy pokus spojím dva stringy například operátorem + nebo metodou Concat, vznikne nová instance která zbaští další místo na heapu (protože pole jsou na heapu, jsou to referenční datové typy)
* Uvnitř je to vlastně pole charů
* Klíčové slovo string je syntaktická zkratka za System.String. Hodí se ho používat, kdybych si vytvořil vlastní třídu s názvem String.

## Internování

* Na jeden string na hladě můžu ukazovat z více míst programu (nemusím se bát, že bych ho rozvrtal, protože je immutable)
* Uvnitř je to hešovací tabulka
* Kompiler ho používá pro konstanty
* Statická metoda String.Intern()

## StringBuilder

* Slouží k dynamickému vytváření stringu
* Do stringu můžu přidávat znaky/jiné stringy a ony se přidávají do onoho interního pole
* Má ale určitou režii (je to prostě další objekt na heapu), takže se vyplatí až od většího (>10) množství operací

## Interpolace

* Když chci di stringu vložit hodnotu proměnné
* V zásadě existují tři možnosti
  * "ab{0}cd{1}".Format(param1, param2)
    * Nepraktické, protože je založeno na speciálních znacích {0}
    * Vzniká pole pro parametry (klíčové slovo params)
  * "ab" + param1 + "cd" + param1
    * V paměti vznikají mezivýsledky
    * Pro pár hodnot je to ale jedno, zase se ušetří na instrukcích CALL apod...
  * string.Concat("a",param1,"b",...)
    * Dopředu známá délka → na haldě se alokuje jeden string
* Shurntí: pro pár hodnot je to jedno, pro větší počty je to záhodno vyzkoušet a změřit si výkon třeba Profilerem ve VS

## Char

* 2bytový hodnotový datový typ
* Reprezentuje jeden znak v UTF-16
  * Ale některé znaky v UTF-16 mají 2 byty, tj. jsou reprezentovány více chary - na to je třeba myslet, když čtu soubor po znacích

# Čtení a zápis do souboru

 ## StreamWriter

* Převádí textovou reprezentaci do bytové, tj. řeší za nás kódování apod...
* Uvnitř má buffer, pole fixní velikosti, které když se mu zaplní, zavolá FileStream a data zapíše do souboru
  * Protože syscall pro zápis do souboru je drahý
* Je IDisposable, tedy je nejlepší ho používat s klíčovým slovem **using**, které na konci bloku zavolá Dispose()
  * OS ví, že může zpřístupnit soubor k editaci atd.

# Testování kódu

* Tzv. unit testy, unit, protože testují menší jednotky kódu (což jsou obvykle metody)
* Testovací frameworky
  * MSTest - starší a jendoduchý
  * xUnit - nový, používá jej MS
* Mockup třída - falešná třída, která implementuje nějaký interface
  * Pro testování vrací nějaká data ... testovací
  * Co nejjednodušší implemetnace
* Test Driven Development (TDD)
  * Nejdříve píšu testy, pak samotnou implementaci

# Třídy

## Konstruktor

* Volá se při vytvoření nové instance třídy

* Pokud není specifikovaný, výchozí je bezparametrický a public

* Na začátku se inicializují globální proměnné

* Z pohledu CIL jde o klasickou metodu se jménem .ctor

* Konstruktor mohu volat z jiného konstrukotru

  ``` c#
  class A{
      int x;
      int x;
      A(x) { this.x = x; }
      A(x,y) : this(x) { this.y = y; }  // this(x) zavolá konstruktor s jedním parametrem a ten nastaví atribut x
  }
  ```

* Nedědí se

### Statický konstruktor

* Konstruktor s prefixem static
* Slouží k inicializaci statických proměnných
* Volá jej CLR za běhu, až když je potřeba
* V CLR reprezentován metodou .cctor

## Dědičnost

* Třída může dědit nejvýše z jedné třídy (na rozdíl od C++), ale může implementovat více rozhraní (interface)

### Klíčová slova

* Virtual
  * Označí metodu jako virtuální a tedy ji lze v potomku změnit její implementaci a označit ji klíčovým slovem override.
  * Klíčové slovo override upraví záznam ve VTM, aby ukazoval na metodu potomka.
* New
  * Pokud chci v potomku přepsat metodu rodiče, která není virtual, musím použít klíčové slovo new. Jinak by nebylo jasné, zda volat metodu potomka či rodiče.
  * Klíčové slovo new vytvoří nový záznam ve VTM (pro potomka), kde uvede metodu potomka
  * V čem je teda rozdíl?
    * Pokud je metoda označena virtual, její autor explicitně uvádí, že je možno ji v potomku přepsat, aniž by to rozbilo zbytek třídy
* Base
  * Stejné jako this, akorát odkazuje na instanci rodiče (a tedy na jeho metody, konstruktory atp.)
* Sealed
  * Zakazuje dědit od dané třídy (například string, int ... jsou seald)
  * sealed override zakazuje overridovat metodu

### VMT (Virtual method table)

* Tabulka virtuálních metod
* Každá instance třídy má pointer do paměti na její VMT

## Vlastnosti (Properties)

* "Náhrada" za koncept getterů a setterů v Javě - efektivnější syntax

* V CIL se ale místo vlastností vygenerují metody 

  * get_*
  * set_*

* Klíčová slova get, set, lze opatřit modifikátory, případně parametry (to umí CIL, C# má pouze osekané indexery)

  * Uvnitř metody klíčového slova set se využívá kontextové klíčové slovo **value**

* Auto-implemented properties - metody se vygenerují automaticky

  ```c#
  class A{
      public string Name { get; protected set; }
  }
  ```

  * V CIL to výše odpovídá tomu níže

  ```c#
  class A{
      private string name;
      public get_name() {	return name; }
      public set_name(string value) {	name = value;  }
  }
  ```

### Indexery

* Nemají jméno, parametry se uvádí do hranatých závorek
* Využívají se například při přístupu do kolekcí (jako je Array, List<T>)

```c#
class A{
    private List<string> list;
    public int this [int index]{
        get { return list[index]; }
	    set { list[index] = value; }
    }
}
```



## Konverze

* Převod mezi datovými typy (obvykle ve vztahu rodič-potomek)
* Klíčové slovo **is** ověří, zda je objekt určitého typu (prochází celý strom hierarchie objektů, což může chvíli trvat)
* Klíčové slovo **as** stejně jako () provede přetypování, ale při nezdaru vrátí null namísto toho, aby program spadl na InvalidCastException

## System.Object

* Výchozí třída, od které dědí všechny ostatní třídy (implicitně)
* Metody
  * protected object **MemberwiseClone**()
    * Vytvoří kopii objektu byte po bytu - takže pro referenční typy nemá význam
    * Obvykle se implementuje s rozhraním IClonable, které ovšem nepodporuje generické typy
  * public Type **GetType**()
    * Vrátí třídu odpovídajícího typu
    * Obsahuje různé parametry třídy, jako například jméno, seznam metod, vlastností, jejich atributy...
    * Také obsahuje VMT
  * public virtual bool **Equals**(object o)
    * Porovnává dva objekty, zda jsou si rovny
    * U třídy object jde o porovnání referencí, ale protože je virtual, třeba u **string** je overrideovaná a porovnává znaky
  * public virtual string **ToString**()
    * Vrací string reprezentující objekt, ve výchozím stavu vrací jeho název
  * public virtual int **GetHashCode**()
    * Hash objektu
  * public static bool **ReferenceEquals**(object objA, object objB)
    * Statická metoda, která porovnává reference dvou objektů

## System.ValueType

* Třída, od níž dědí všechny hodnotové typy, tj. **struct**, jako je int, bool, Nullable...
* Overriduje virtuální metodu Equals, místo referencí porovnává byty

# Výjimky

* Všechny výjimky jsou v C# typu System.Exception
* Užitečné atributy třídy Exception
  * Message - chybová hláška
  * StackTrace - cesta v programu (v podstatě callstack) od vyhození výjimky k jejímu vypadnutí
  * Source - jméno objektu, který výjimku vyhodil
* Syntax

``` c#
try {
    // Kód
} catch (Exception e) {
    // Zachytím výjimku, pokud nastane
} finally {
    // Vyvolá se vždy, ať výjimka nastane nebo ne
	// Hodí se pro uvolnění systémových zdorjů (soubory, atp...)
    // POZOR: Pokud v try nastane výjimka, kterou nezachytí catch a již není žádný nadřazený try-catch, výjimka vypadne z procesu a OS ten proces killne, aniž by se stihl vyvolat finally
}
```

* Umí vypisovat StackTrace
  * Při volání funkce se na zásobník uloží parametry, návratová adresa a pak lokální proměnné

# Typový systém v CIL

* Narozdíl od C++ si nemůžu vybrat, co bude hodnotový (stack) a co referenční (heap) typ - je to pevně učeno

## Hodnotové typy

* Všechny typy s klíčovým slovem **struct**, enumy
* Pracuje se s nimi jako s hodnotou ... takže když hodnotu jedné proměnné přiřadím do jiné, tak se prostě na stacku zkopíruje
  * Totéž platí pro předávání structu jako parametr - zkopíruje se
* Nemohou být **null** (mohou, když je udělám Nullable, syntaktická zkratka ?)
* Hodnoty jejich atributů se ukládají postupně na stack (vzpomeňme na **Počítačové systémy** a věci o tom, jak se co v paměti odsazuje a zarovnává)
* Protože se ukládají na stack, hodí se pro menší typy, <16 bytů  
* Klíčové slovo **new** jen zavolá konstruktor - bezparametrický vždy existuje
* Vždy je dělat **immutable**, jinak se v kolekcích chovají neintuitivně (indexer mi vrací kopii, takže eventuální metodu volám na kopii)

## Referenční typy

* Všechny typy s klíčovým slovem **class**, interface, pole, delegáty
* Fungují jako reference ... takže vždycky předávám referenci a když chci vytvořit kopii, musím použít něco jako Clone
* Mohou být **null**
  * Od C# 8.0 to lze zakázat

* Na haldě mají objekty určitý overhead
  * Jedna na stacku je uložen pointer na ně, který sám o sobě zabírá 4B nebo 8B podle architektury
  * Také je na haldě u objektu popis jeho typu (objekt Type)
  * SyncBlock (nevím přesně, k čemu to, ale Ježek se o tom nějak zmiňoval)


### Pole

* Jednorozměrná, vícerozměrná, pole polí
* Vícerozměrné pole je prostě matice
* V poli polí mohou mít "podpole" různé velikosti 

### Boxing

* Pokud uložím int to objectu, tak je to trochu divné, protože int je hodnotový a object je referenční typ
* Stane se to, že int se "zaboxuje" do objectu a ulože se na heap, protože object je obecnější typ.
* Když k němu pak chci přistoupit, int se "odboxuje"
* Obecně náročná operace

# ------------------------------------------------------------------------------------------------------------------------------------------------------

# Konverze
## Implicitní konverze
* Z konkrétnějšího typu na obecnější
* Také mezi některými číselnými hodnotovými typy
  * Klíčové slovo checked - hlídá, zda při konverzi z většího typu na menší nedošlo ke ztrátě informace
* Obecně jsou v C# implicitní konverze tam, kde se nemá co rozbít a kde se nemohou ztratit žádná data
## Explicitní konverze
* Přetypování nebo klíčové slovo as
* Může při nich za runtime nastat chyba (lze ošetřit klíčovým slovem is)

# Extension metody
* Způsob, jak lze typu přiřadit další nestatické metody
* Před první parametr statické metody přidám klíčové slovo this
* Na úrovni CIL se to přeloží jako klasické volání statické metody
* Mohou být pouze ve statické třídě
* Instatnční metoda má před extentsion metodou vždy přednost

# Přetěžování metod
* Volba volání metody na úrovní překladače
  1. Podle arity
  2. Podle kontextu (typu proměnné)
    * Volí nejspecifičtější podle typu parametrů a s "co nejmenší prací" za runtime
      * Pokud existuje implicitní konverze z typu A na B a volám metodu s parametrem typu A, ale existuje jen pro B, tak překladač sám provede konverzi
      * Taková uživatelská konverze se ale provede nejvýše jedna
    * Pokud neexistuje metoda s parametrem pro konkrétní typ, hledá se s co nejbližším rodičem ve stromu dědičnosti
  3. Podle obecnějšího kontextu (rodiče typu)
    * Nastane pouze, pokud se neuplatnilo nějaké z pravidel v 2.

# Přetěžování operátorů
* Lze přetežit vpodstatě libovolný operátor

# Generika
* Specifikace generického typu pomocí klíčového slova where
## Genereické metody
* V C++ jsou šablony. Pokud použiji metodu šablony, C++ překladač vezme implemntaci (musí ji mít k dispozici) a znovu ji přeloží pro použití s konkrnétním typem
* V C# se do CIL zapíše generická metoda obecně. Až při jejím volání se řekne, s jakým typem se volá
* Duck typing - stačí mi, aby objekt splňovat pouze ty vlastnosti, které zrovna potřebuji ... a nezajímá mě jeho typ
  * V Pythonu a dynamicky typovaných jazycích funguje obecně
  * V C++ funguje také, protože se generická metoda vždy překládá znovu
* Místo Duck typingu je v C# klíčové slovo where
  * V C++ apod. je použití sice pohodlnější, ale v C# je explicitně řečeno, co přesně potřebuji
* Generická může být i Exntension metoda nad generickým typem
## Generické typy
* Použití stejné jako u generických metod
* Pro každou použitou specializaci JIT vygeneruje zvláštní typ
  * Pokud mám tedy ve třídě statický konstruktor, zavolá se pro každou specializaci
  * Platí to pouze pro hodnotové typy, pro referenční se pořád recykluje jedna metoda ... aby se ušetřila paměť

* Metody generického typu nejsou nutně všechny generické
* Generické mohou být jak třídy, tak struktury

# Explicitní implementace interface
* Pro každý typ a interface, který implementuje, existuje tabulka, kde je uvedenou, která metoda typu implementuje kterou metodu interface
* Syntax void IInterface.Method(){...}

# Kovariance typů
* Hodí se, pokud mám A<T> a B<T>, kde A je rodič B a chci předat B do metody, která příjmá A ... pak to jde, pokud jsou A,B kovariantní
* B je typově kompatibilní s A, pokud lze do proměnné typu A lze přiřadit hodnotu typu B
* Pak pokud C<B> je typově kompatibilní s C<A>, pak C je kovariantní podle T
  * Čtení bezpečné, zápis se musí kontrolovat
* Nebo pokud C<A> je typově kompatibilní s C<B>, pak C je kontravariantní podle T
  * Zápis bezpečný, čtení se musí kontrolovat
* Jinak je C invariantní podle T
* Generické typy jsou invariantní
## Pole
* Pole referenčních typů jsou kovariantní
* Pole hodnotových typů jsou invariantní
* Příklad: pole objectů může ukazovat na pole stringů
* Čtení pak funguje dobře, při zápisu se musí kontrolovat typ → a to musí být k každého pole referenčních typů
* Důsledek: proto to nefunguje u hodnotových typů, aby pole nebyla pomalá
## Generické interface
* Mohou být invariantní, kovariantní (out) nebo kontravariantní (in)
* Variance funguje pouze pro generické typy
* Příklad: k variantnímu interface IList existuje kovariantní IReadOnlyList

# Interface vs. abstraktní třídy
* Mohu dědit pouze z jedné třídy, ale z libovolně mnoha interface
* Prolémy s násobnou dědičnosti (Diamon inheritance problem)
  * Problém s datovými poli
* V C++ virtuální a nevirtuální dědičnost
* Interface neumožňuje popisovat fieldy. Vlastnosti ano, ale to jsou v CIL stejně metody
* Od C# 8.0 povoluje interface výchozí implementace metod
## Virtuální metody
* U každého typu je odkaz na VMT
* Při volání metody se najde podle typu a VMT, která metoda se má volat
* Zaznámy ve VMT ukazují na implementaci
* Dědí se
## Abstraktní třídy
* Pokud mám v abstraktní třídě metodu, pak ji mohu v dědících třídách overridovat
* Mohu to zakázat klíčovým slovem sealed
## Interface
* Každý typ, který implementuje interface má vlastní intefrace tabulku virtuální metod, v níž je pro každou metodu interface jeden záznam
* Záznamy v IVMT ukazují na metodu
* Dědí se
* Implementace nevyžaduje virtuální metodu
## Kdy používat interface nebo abstraktní třídu
* Abstraktní třída umožňuje definovat datové položky a defaultní implementace
* Abstraktní metody také mohou mít všechny modifikátory
* Interface umožňuje vícenásobnou dědičnost
* Interface podporuje pouze veřejné modifikátory, ale v rámci třídy můžu jeho metody implementovat explicitně

# ArraySegment<T>
* Struktura pro pohled na část pole
* V .NET Coje je Span<T>

# Benchmark v .NET

# Delegáty
* Chytré ukazatele na funkce. Definuji návratový typ a parametry metody
* Referenční typ - reference ukazuje na delegáta, ne na metodu
* Překladač vygeneruje třídu podle názvu delegátu, která dědí z MulticastDelegate a metodu Invoke
* V každém delegátu jsou dva ukazatele
  1. Na metodu
  2. Na instanci, na které se má metoda zavolat
* Delegát je immutable
* Funguje i pro virtuální metody, konkrétní implementace se vybere při inicializaci delegáty za runtime
* V C++ jsou memberpointery, kam uložím odkaz na metodu a odkaz na instanci předám až při volání
* Delegát může být generický
## Multicast delegate
* Delegát může ukazovat na více metod
* Metoda Combine - přidání metody, +=
* Metoda Remove - odstraní delegáta, který ukazuje na stejnou metodu a instanci, -=
* Metody přímo mění hodnotu proměnné s delegátem
## Předdefinované delegáty v .NET
* Delegát Action bere parametry a vrací void
* Delegát Func bere parametry a vrací hodnotu
* Delegát Predicate bere parametry a vrací bool

# Anonymní metody
* Syntaktická zkratka delegate (parametry) { tělo metody }
* Překladač si domyslí typ návratové hodnoty
* V praxi překladač v rámci typu vygeneruje normální metodu

# Lambda funkce
* Syntaktická zkratka za anonymní metodu 
* Překladač si domyslí typ návratové hodnoty a typy parametrů
* V lamba funkci se můžu odkazovat na proměnnou ven - volná proměnná, ostatní vázané
* Capture - přiřazení hodnoty volné proměnné
* Clousure - lambda funkce bez volných proměnných
* Ve skutečnosti C# i C++ nepodporuje lambda funkce, ale pouze clouseres
* Capture se provede v rámci kontextu
  * Pokud dělám capture lokální proměnné, tak se dělá capture by reference a životnost proměnné se prodlouží sna životnost lambda funkce
  * Opět se vytvoří Scope třída a lambda funkce bude její metodou - C# překladač tomu říká DipslayClass
  * Lokální proměnné, které se používají v lambda funkci se nahradí datovými položkami scope třídy - předtím se tedy musí vytvořit instance scope
  * Třída je referenční typ - má delší životnost než lokální proměnné
* Důsledek: lambda funkce s vnějším scope mají netriviálí overhead
## Lambda funkce v C++
* [](args){ code }; ... mohou ji rovnou zavolat [](args){ code }();
* vector<T> zhruba odpovídá List<T>
* & pro referenci
* Iterátory v C++ umí chodit z zpátky
* Capture
  * Zakázáno - []
  * By value - [=] ... vytvoří se kopie proměnné. Jak ji ale předat?
    * Trik: pro lambda funkci vytvořím třídu, kde volné proměnné budou jako datové položky a lambda funkce se stane její metodou. Tzv. Scope
    * U definice lambda funkce v programu se pak vytvoří instance té třídy Scope, do jejíchž datových položek se uloží kontext
  * By reference - [&]
    * Do scope se místo hodnoty uloží reference
    * Problém, pokud lambda funkce žije déle, než např. lokální proměnná, s jejíž referencí pracuji

# Anonymní typy
* Syntax: var t = new { x = a, y = b }
* V praxi se vytvoří nový typ s příslušnými vlastnostmi
* Pokud C# překladač najde v rámci assembly více anonymních tříd se stejnými datovými položkami ve stejném pořadí, použije pro něj jeden typ
* Overriduje se metoda ToString, Equals (na strukturální porovnání)
* Naopak neimplementuje IEq<T> a operátor ==
* Obvykle se dají použít pouze v rámci metody
* Debilní featura: třída Tuple<T1, T2, ...> - nevýhoda je overhead
* Od C# 7.0 existuje struct ValueTuple<T1, T2, ...> - syntaktická zkratka n-tice (...)
* Podtržítkem discard
* Dekonstruktor
  * V typu musím definovat metodu Deconstruct(), která vrací ValueTuple
  * Pak můžu typ syntakticky přiřadit do n-tine, C# sám zavolá Deconstruct

# Jmenné prostory
* Mohou být vnořené
* Koncepčně podobné, jako adresářová struktura
* Z pohledu .NET žádné namespaces nejsou, jména tříd se doplní o namespace
* Je to syntaktická zkratka C#
* Hledání jmen typů vždy začíná v aktuálním kontextu, pokud se nenajde, postoupí se o kontext výš
* Kořenem stromů jmenných prostorů je global
* Pokud chci zadat absolutní cestu k namespace, použiju global::
* Stromů můžu mít více
  * Můžu jednotlivým assembly přiřadit vlastní alias (a do zdrojáku přidat extern alias)
* Klíčové slovo using naimportuje do aktuálního jmenného prostoru všechny typy (!pouze)
  * Lze také používat pro vlasntí aliasy
## Nested types
* Vnořené typy 
* Podporuje je i CIL
* Pro jejich oddělení používá + (namespace se ve jméně typu oddělují tečkami)
* U vnořeného typu můžu používat všechny modifikátory, u typu pouze v namespace pouze public nebo internal
* Nadřazený typ má přístup k private věcem vnořeného typu

# Kolekce
* Rozhraní IList<T> : ICollection<T> : IEnumerable<T> : IEnumerable
* IEnumerable<T> umožňuje procházení
  * Stav procházení není přímo v kolekci (kvůli více vláknům)
  * Typ IEnumerator<T>, jeho instance prochází kolekci
    * Položka Current, metoda MoveNext(), Reset() ... Reset() často vyhazuje výjimku, protože ho nikdo nepoužívá :)))
    * Během enumerování by nemělo být možné kolekci modifikovat (po dobu existence enumerátoru) - vyhodit InvalidOperationException
    * Syntatická zkratka foreach za GetEnumerator() a while cyklus, umí explicitní přetypování
    * Může být lazy, hodnoty se vytváří až během enumerování
## Enumerátor jako struct
* Pokud bude enumerator struct, tak se vrací jako rozhraní IEnumerator, takže se stejně zaboxuje a bude na haldě
* Udělám enumerátor jako nested public struct, přidám metodu GetEnumerator, která vrací přímo můj struct a implementaci pro interfaces udělám explicitně
* foreach cyklus si s tím poradí, v prvním kroku (běhěm překladu) se pokusí najít na enumerované kolekci metodu GetEnumerator, bez ohledu na interface ... tedy zavolá tu, co vrací struct
## Iterátorové metody
* Libovolná metoda, která vrací IEnumerator<T> a obsahuje klíčové slovo yield return
* C# překladač za nás vytvoří třídu enumeratoru, z lokálních proměnných se vytvoří její datové položky
* V enumerátoru se vygeneruje stavový automat - jednotlivá volání jsou přechody mezi stavy stavového automatu
* Klíčové slovo yield break. MoveNext() pak vrátí false.
* Ježek: pokud implementuji debilní algoritmus, stavový automat bude taky debilní
* Pozor na práci s vyhazováním výjimek. Pokud provedu kontrolu parametrů až v metodě GetEnumerator s yield return, tak se kód zavolá až při volání MoveNext()
  * Řešením je napsat dvě metody, jedna kontroluje parametry a vrací enumerátor, druhá enumeruje

# LINQ
* Language integrated queries
* Deklerativní dotaz
* Pouze syntax, překladač jej přepíše na volání metod z System.Linq
* Příklad: from c in customers where c.City == "Praha" select c
* Mohu si vytvořít vlastní třídy LinqTo..., která implementuje vhodné metody (Where, Select, OrderBy)
* V System.Linq ve třídě Enumerable je LinqToObjects
  * Je lazy, pokud možno -> vyrobí se datová struktura, která reprezentuje dotaz (například WhereEnumerable), uloží si parametr (lambda fci) a odkaz na zdroj dat
    * Můžu si uložit odkaz, ten má referenci na zdroj dat a když pak zdroj dat změním, projeví se to při dalším enumerování dotazu
  * Dotazy se začnou vyhodnocovat, až když začnu enumerovat
  * Pozor, například pokud zavolám OrderBy, tak to rozhodně není lazy

# Vlákna
* Vlákno - stav výpočtu v programu
  * Registry procesoru reprezentují stav vlákna
    * Stack pointer - ukazuje na vršek volacího zásobníku
  * Každé vlákno má vyhrazenou vlastní část adresového prostoru a v ní má volací zásobník
  * Halda a globální statické proměnné jsou mez vlákny sdílené
* Jsou plánována OS
  * Vlákno běží, dokud se neukončí nebo ho nepřeruší operační systém (např. každých 10 ms)
* Např. GC běží na vlastním vlákně, finalizery také
* Motivace: rychlost, chci provést dva algoritmy paralelně
* Jmenný prostor System.Threading, třída Thread
  * Pokud CIL hostuji sám, lze nakonfigurovat, co to znamená vlákno
  * Thread.CurrentThead - aktuální vlákno
    * Statická proměnná, ale pro různá vlákna různé hodnoty - používá se thread local storage 
* new Thread(void ThreadStart ...)
  * Vyrobením instance ještě nevznikne vlákno, to až voláním metody Start()  
* Stav vlákna z pohledu .NET
  * Unstarted
  * Running - po zavolání metody Start() ... to nemusí znamenat, že právě fyzicky běží na procesoru
  * Ended - lze zjistit z vlastnosti IsAlive
* Zabíjení vláken
  * Metoda Abort() - vyvolá výjimku ThreadAbortException, tedy skutečně nezabíjí
  * Skutečné zabíjení vláken v .NET není (CIL má RudeAbort)
* Konec vlákna
  * Akitvní čekání - neefektivní
  * Metoda Yield() - vyskočím z aktuálního vlákna
  * Metoda Sleep(milis)
  * Metoda Join - az joinovane vlákno skončí, tak mě OS probudí - nejlepší řešení
* Mutual exclusion
  * Ochrana kritické sekce
  * V .NET pomocí zámků (Mutex)
  * Klíčové slovo lock(obj) { ... }
    * .NET si pamatuje, které vlákno jej zamklo
    * Pokud je odemčený, tak se atomicky zamkne
    * Pokud je zamčený, zahájí se pasivní čekání
    * Vystoupení ze zámku jednoho vlákna probudí všechna ostatní vlákna, která na něj čekají a OS nějaké z nich naplánuje
    * Funguje rekurzivně - můžu stejný zámek jedním vláknem zamknout vícekrát - nedojde k Deadlocku
    * Kde vzít zámek?
      * Libovolný referenční typ
      * Na haldě jsou s každým objektem dva pointery 
        1. Instance třídy Type
        2. SyncBlock - právě ten zámek (počítadlo zamykání, vlákna ve frontě)
          * Zámek nemá žádný vztah s původním objektem
          * Defaultně je tam null
          * Vlastně je to Monitor
            1. Zámek
            2. Condition variable
              * Pro použití musím být v lock
                * Aby se podmínka, po které se volá Wait nebo Pulse provedlo atomicky
              * Monitor.Wait(thread) - uspí vlákno
                * Monitor.Exit(thread); Monitor.Wait(...); Monitor.Enter(thread); - atomicky
                * Podmínku musím ověřovat while
                * Obvykle po ověření podmínky, nutno předtím použít zámek
              * Seznam spících vláken (ne ty, která se mají probudit)
              * Monitor.Pulse(thread) - probudí jedno z vláken
              * Monitor.PulseAll()
      * Pokud chci jen basic zámek, vystačím si s new object()
      * Ve skutečnosti se příkaz lock přepíše na 
        * Monitor.Enter(obj);
        * try { ... } 
        * finnaly { Monitor.Exit(obj); }
        * Může se hodit, pokud by se mi bloky loc překrývali
  * Nepraktické v případě, že jsou kritické sekce krátké - protože vlákno, které narazí na zamčený zámek se odplánuje a pak se musí znovu naplánovat
  * Problémy mohou nastat při zamykání více zámků současně - například Deadlock
  * Ve třídě Monitor je metoda TryEnter(obj) - ale místo deadlocku můžu vytvořit livelock ... lock ve while
  * Výjika ThreadAbortException se na konci catch blocku vyhazuje opakovaně
* Lock-free algoritmy bez zámků - lepší, ale s možným dlouhým čekáním
* Wait-free algoritmy bez zámků a se zaručenou délkou čekání - nejlepší¨
* Ukončení vlákna
  * Abort() - nepraktické, výjimka může vzniknout v nevhodném místě
  * Do datové struktury, se kterou vlánko pracuje, dám příznak - otrávená pilulka

## Vlákna na pozadí a na popředí
* Příznak IsBackground
* Proces se neukončí, dokud běží nějaké vlákno na popředí
* Vlákna na pozadí ukoční CLR potom metodou Abort()
## Vstupy a výstupy
* Do metody Thread.Start() lze předat object - to není moc praktické
* Lepší je si vytvořit třídu, například Job se vstupy a výstupy a předat vláknu instatnčí metodu instance té třídy
* Předat vláknu lambda funkci, v ní předat parametry - třída se vytvoří za nás
## Simulace vláken bez vláken
* Iterátorové metody - yield return
* Zavolá se MoveNext a tím se provede jeden krok (CoRoutines) - používá se například v Unity pro simulaci mnoha entit
## Problémy
* Deadlock - A čeká na B a B čeká na A
## Synchronizační primitiva v C#
* Monitor
* Třídy *Slim - dobré
* Potomci třídy WaitHandle (například třída Mutex)
  * Implementované v jádře OS - musí se přejít do admin řežimu procesoru
  * Velký overhead
  * Výhoda: jsou globální, hodí se, pokud pracuji s více procesy
* Semaphore
  * Jako mutex, ale do sekce může pustit více vláken
## WinForms a vlákna
* Application.Run pustí nekonečnou smyčku a obsluhuje události
* S Control můžu pracovat pouze z vlákna, na kterém vznikla
* Na Control můžu zavolat Invoke, která se zavolá synchronně, je blokující (BeginInnvoke neblokující)
* WinFormsSynchronizationContext : SynchronizationContext
  * Vlastnost Current
    * Liší se podle vlákna, ThreadLocalStorage
      * Atribut [ThreadStatic] - pozor, výchozí hodnota platí pouze na hlavním vlákně
  * Post - asynchornní, ekvivelent BeginInvoke
  * Send - synchronní, ekvivalent Invoke
## ThreadPools
* Vyrábění a ukončení vlákna ma velký overhead -> je výhodnější vlákna recyklovat
* V C# třída ThreadPool, metoda QueueUserWorkItem(delegate)
  * Má seznam background vláken, na kterých se postupně pouští položky z fronty
  * Pokud nemají nic na práci, tak se vlákna zase uspí
  * Počet vláken cca. odpovídá počtu jader procesoru
  * Hodí se pro krátce běžící úlohy
* Když mu vlákna dojdou, umí vytvořit další
  * Pokud mu dám Task s příznakem LongRunning, tak vždycky vyrobí další
* Nevýhoda - nemá pořádně API
* Má globální frontu a každé jeho vlákno má ještě svojí
  * Vlákno se vžycky prvé dívá do svojí lokální fronty
* Nad abstrakcí TaskScheduler
  * Abstraktní třída pro plánování vláken
  * Má metodu FormCurrentSyncContex()
  * Do TaskFactory.StartNew() lze předad konkrétní TaskScheduler
    * Jako výchozí bere TaskScheduler.Current
    * Task.Run jako výchozí bere TaskScheduler.Default
## Task
* Ve jmenném prostoru System.Threading.Tasks
* Hlavní je třída Task, poskytuje API k ThreadPoolu
* Narozdíl od Threadu jich lze vyrábět hodně relativně levně
* Metoda Start zafrontí úlohu do ThreadPoolu
* Task lze vyrobit v TaskFactory.NewTask(...)
* Metoda Task.Run(...) - není ekvivalentní s new Task(...).Start(), jiná konfigurace
* Pokud pouštím Taks z jiného Tasku, tak se primárně frontí do fronty vlákna toho mateřského tasku
  * Předpoklad, že mají stejný kontext a tedy data jsou nakešovaná v keši procesoru
  * Lze nastavit v TaskCreationOptions.PrefareFairnes - Task se zafrontí do globální fronty
* Metoda Wait() blokuje, dokud nedoběhne
* Trik:
  * Pokud v Tasku čekám na jiný Task z ThreadPoolu, který se ještě nezačal provádět, tak se provede synchronně
* Aktuální vlákno: Thread.ManagedThreadId
* Pokud potřebuji, aby Task počkal na jiný Task, který se v něm vyrobil, tak můžu zavolat Wait nebo použít TaskCreationOptions.AttachToParent = true
* Vlastnost result, je blokující
* Metoda ContinueWith(delegate)
  * Lepší než Wait(), task v parametru se vytvoří a naplánuje, až doběhne
  * Parametr je tělo Tasku, který se pustí až když původní doběhne, původní task jako parametr
  * Pokud potřebuji čekat na více Tasku, existují metody Factory.WaitAll a Factory.WaitAny
  * Můžu jich vytvořit více a podmínit je stavm Tasku
* Task s návratovou hodnotou, Future, je asynchronní -> vrací příslib 
* Viz příklad s kukuřicí, prasítky a hamburgry
* Je vhodné metodu pojmenovat se sufixem *Async
* Promise
* Metoda Task.FormResult<T>(T) - vrátí hotovou Task s danou hodnotou, hodí se pokud chci do metody s parametrem Task předat statickou hodnotu
* Rušení Tasku - kooperativní
  * Běžící kód se musí ptát, zda ho někdo nechce přerušit
  * struct CancellationToken
  * Výjimka vyhozená v Tasku se nevyšíří, ale uloží se do objektu Tasku
    * Pokud se zeptám na task.Result a byla vyhozena výjimka, tak se vyhodí znovu
    * Stav Faulted
    * CancelationToken.ThrowIfCancelationRequested() vyhodí výjimku OperationCanceledException, která nastaví stav Canceled (ne Faulted)
* V CancelationToken je reference na CanceletionTokenSource, kde je metoda Cancel
## Asynchornní metody
* Klíčové slovo async, musí vracet Task
* Řeší problém, kdy nechci blokovat vlákno -> použil bych ContinueWith, ale to je otravný a náročný
* V ní musí být alespoň jednou klíčové slovo await před proměnnou, do které vracím Task - řez v metodě
* Překladač z toho opět postaví nějaký stavový automat
## Parallel
* Třída Parallel, umoňuje zavolat paralelně více delegátů
## Paralel Linq (PLINQ)
* Nadstavba LinqToObjects
* Extension metoda AsParallel : IParallelEnumerable
* Všechny metody od AsParallel doprava jsou z PLINQ
  * Opět lazy, pokud možno
  * Ale protože je to paralelní, tak každé vlákno předspracovává data do bufferu
  * AsOrdered nebo AsUnordered - zachovávat při paralelním zpracování pořadí

# TCP/IP
* Socket (IP adresa + port)
  * Obousměrný komunikační kanál
  * Metody Listen(), Send(), Recive()
* Spolehlivý protokol
* Byte stream
* V .NET třída Socket
* Nad třídou Socket se dá vytvořit NetworkStream : Stream
* Nadstavby na třídou Socket jsou TcpClient a TcpHost
* Třídy HttpRequest, WebClient
* Existuje i UdpClient - hodí se na live stream videa, hry apod
* Hodí se pro požadavky od klientů vytvářet Tasky
* Paralelní operace na úrovni OS (Windows mají overlap io)
  * Socket funguje asynchronně, například FileStream ve výchozím ne - musí se nastavit