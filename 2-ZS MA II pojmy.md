# Metrické prostory

## Definice

* Metrický prostor
* Euklidovský prostor (metrický prostor nad reálnými čísly s euklidovskou metrikou)
* Diskrétní prostor (konstantní vzdálenost)
* Podprostor
* Spojité zobrazení
* Konvergence posloupnosti
* Okolí bodu
* Otevřená množina
* Uzavřená množina
* Vzdálenost od množiny
* Uzávěr
* Obraz
* Vzor
* Topologická vlastnost
* Ekvivalentní metriky
* Silně ekvivalentní metriky

## Věty

* **(Složení spojitých zobrazení)** Složení spojitých zobrazení je spojité.
* **(O konvergenci)** Zobrazení je spojité právě když zobrazí konvergentní posloupnost opět na konvergentní (včetně limity).
* **(Ekvivalence při spojitém zobrazení)** Pokud je f spojité, pak
  * Zobrazí okolí na okolí
  * Zobrazí otevřenou množinu na otevřenou
  * Zobrazí uzavřenou množinu na uzavřenou 
  * Kompaktní na kompaktní
* **(O spojitých zobrazeních)**
  * Projekce ze součinu metrických prostorů na jeden z nich jsou spojité
  * Pokud mám spojitá zobrazení po souřadnicích, pak existuje právě jedno spojité zobrazení (které mi po složení s projekcí dá zobrazení po souřadnicích) do součinu.

# Parciální derivace

## Definice

* Parciální derivace
* Totální diferenciál

## Věty

* **(TD → spojitost)** Pokud má funkce v bodě totální diferenciál, pak je v něm spojitá a má v něm všechny parciální derivace.
* **(Spojité PD → TD)** Pokud má funkce v okolí bodu spojité parciální derivace, pak v něm má totální diferenciál.
* **(Derivace složené funkce)**
* **(Řetízkové pravidlo)**
* **(Lagrangeova věta o střední hodnotě)**
* **(Záměnnost parciálních derivací)**

# Kompaktní prostory

## Definice

* Kompaktní prostor
* Omezený metrický prostor
* Cauchyovská posloupnost
* Úplný prostor

## Věty

* Podprostor kompaktního prostoru je kompaktní právě když je uzavřený.
* Kompaktní podprostor je vždy uzavřený.
* Součin kompaktních prostorů je kompaktní.
* **(Kompaktnost euklidovského podprostoru)** Podprostor euklidovského prostoru je kompaktní právě když je uzavřený a omezený.
* **(Obraz kompaktního prostoru)** Pokud je zobrazení spojité a vzor je kompaktní, pak je i obraz kompaktní.
* **(Extrémy na kompaktním prostoru)** Každá spojitá funkce na kompaktní prostoru (do reálných čísel) nabývá maxima i minima.
* Bijekce z kompaktního prostoru je homeomorfismus.
* **(Konvergence Cauchyovské posloupnosti)** Nechť má Cauchyovská posloupnost konvergentní podposloupnost. Pak i samotná posloupnost konverguje k tytéž limitě.
* **(Cauchyovská posloupnost součinu)** Posloupnost součinu je Cauchyovská právě když je Cauchyovská v každé složce.
* Součin úplných prostorů je úplný.
* **(Úplnost kompaktního prostoru)** Každý kompaktní prostor je úplný.

# Implicitní funkce

## Definice

* Jacobiho determinant
* Regulární zobrazení

## Věty

* **(Věta o implicitních funkcích v rovině)** Nechť je funkce v bodu *(x,y)* rovna 0 a nechť má na jeho okolí spojité parciální derivace řádu *k*. Pak pokud má nenulovou parciální derivaci podle y, pak existuje okolí *x*, na kterém je *y* jednoznačně určeno. Tato funkce v *y* má pak spojité derivace do řádu *k*.

* **(Věta o implicitních funkcích)** Nechť je funkce v bodu *(x,y)* rovna 0 a nechť má na jeho okolí spojité parciální derivace řádu *k*. Pak pokud má nenulový Jacobiho determinant podle y, pak existuje okolí *x*, na kterém je *y* jednoznačně určeno.

* **(Věta o Lagrangeových multiplikátorech)**
* **(Obraz regulární funkce)** Je-li zobrazení regulární, pak obraz otevřené množiny je otevřená množina

# Stejnoměrná spojitost

## Definice

* Stejnoměrná spojitost

## Věty

* Každé spojité zobrazení z kompaktního prostoru je stejnoměrně spojité

# Riemannův integrál

## Definice

* Objem
* Rozdělení
* Cihly
* Zjemnění
* Průměr
* Jemnost
* Horní/dolní součet
* Riemannův integrál (horní/dolní)

## Věty

* **(Kritérium existence)** Riemannův integrál v reálných číslech existuje právě když se k sobě horní a dolní součet limitně blíží.
* **(Existence pro spojité funkce)** Každá spojitá funkce v reálných číslech má Riemannův integrál.
* **(Integrální věta o střední hodnotě)**
* **(Základní věta analýzy)** Hodnota Riemannova integrálu spojité funkce je dána hodnotou primitivní funkce, jejíž derivací získáme původní funkci.
* **(Fubiniova věta)** Funkci více proměnných lze integrovat postupně.