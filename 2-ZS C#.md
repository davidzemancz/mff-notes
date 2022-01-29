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

* TODO - aneb jak se ukládají data to typu object































































