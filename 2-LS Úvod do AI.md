# 1
* Přeskočeno .)

# 2 Prohledávání (28-47)
* Agent
  * Vnímá podněty z prostředí prostřednictvím senzorů
  * Dělá akce, které mají vliv na prostředí, jehož změny pozoruje právě prostřednictvím senzorů
  * Racionální agent: $action=argmax_aE(utility(p,a))$
    * Optimalizujeme tak, abychom dostáhli maxima účelové funkce
* Typy prostředí
  * Agent zná prostředí úplně nebo jen částečně
  * Deterministické nebo stochastické nebo strategické (výsledek ovlivňují i jiní agenti)
  * Epizodické (jednotlivé akce jsou nezávislé) nebo sekvenční (aktuální stav ovlivněn předchozími)
  * Statické (prostředí se nemění) nebo dynamické nebo semidynamické (prostředí se nemění, ale délka přemýšlení ovlivňuje míru výkonu)
  * Diskrétní nebo spojité
  * Single-agent nebo multi-agent
* Typy agentů
  * Reflex agent
    * Vybere akci na základě aktuálního vstupu - v podstatě tabulka
    * Model-based reflex agent
      * Vybere akci na základě přechozích stavů a předchozích akcí
      * → rozhoduje se na základě aktuálního stavu
      * Obecně je rozhodnutí reflex agenta řízeno jen minulostí
  * Goal-based agent
    * Bere v potaz svůj cíl a podle něj vybírá akci
    * Kromě minulosti pracují i s budoucností - cílem
* Reprezentace stavů
  * Atomická - stav je určen číslem, používá se při prohledávání
  * Factored - stav je určen vektorem hodnot
  * Strukturovaná - stav je určen strukturou, tj. objekty a vztahy mezi nimi
* Problem solving agent
  * Typ goal-based agenta
  * Atomická reprezentace stavů
  * Cíl je množinou cílových stavů
  * Akce odpovídají přechodům mezi stavy
  * Úkolem je najít posloupnost akcí, která vede k cíli právě prohledáváním
    * Agent pak provede postupně akce z posloupnosti - znovu už nic neověřuje
    * Prostředí musí být plně pozorovatelné, diskrétní, statické, deterministické
* Abstrakce
  * Musím se rozhodnout, co budou stavy a co akce
  * Je nutno zvolit vhodnou úroveň abstrakce
    * Musí být dostatečně přesná, ale zároveň zpracovatelná
* Dobře definovaný problém
  * Výchozí stav
  * Přechodový model - v podstatě automat
  * Ověření, že jsem v cíli
* Řešení problému prohledáváním
  * Začnu v počátečním stavu - vygeneruji následníky a pokračuji v nich
  * Frontier - množina otevřených stavů
  * Tree search
    * Vzniká mi strom dosažutelných stavů
    * Problémem je, že se mi mohou opakovat stavy (pokud lze jednoho dosáhnout více cestami)
    * Ale neoupužívá se jen na stromy! ... jde to i na obecné grafy
  * Graph search
    * Tree search, kde si pamatuji navštívené stavy
    * Problémy: musím umět rychle ověřit, zda už je stav uzavřený
  * BFS
    * Používá FIFO 
    * Kompletní
    * Cíl najde v optimálním čase
    * Pracuje v čase $O(b^{d+1})$, kde $b$ je branching faktor a $d$ je hloubka cíle
      * $d+1$ protože v čase $d$ ho zařadím do frontieru a pokud je na konci, dojdu k němu v čase až $d+1$
      * I paměťová složitost je $O(b^{d+1})$ → a to je problém
  * DFS
    * Používá LIFO
    * Kompletní pro graph search (ne pro tree search, protože můžu jít do nekonečné větve)
    * Sub optimální (najde cestu do cíle, ale nemusí být nejkratší, lze upravit na branch-and-bound)
    * Časová složitost $O(b^m)$ kde $m >> d$, ale paměťová pouze $O(bm)$
    * Backtracking není DFS, v DFS v aktuálním stavu generuji všechny následníky, v backtrackingu jen jednoho
      * Backtracking má tak paměťovou složitost $O(m)$
  * Uniform-cost-seach
    * Dijkstrův algoritmus
    * Účelová funkce $f(n)$ bez heuristiky 
        * $f(n)=g(n)$
  * Greedy best-first search
    * Suboptimální - problémem typicky překážka
    * Neúplný - pokud nalezení cesty vyžaduje vzdálení se od cíle
    * $f(n)=h(n)$ ... vybírám následníky pouze podle heuristiky
  * A*
    * $f(n)=h(n)+g(n)$ ... vybírám následníky podle heuristiky a podle vzdálenosti od počátku 
    * Optimální a kompletní
    * Pro Tree search je optimální s přípustnou heuristikou
    * Pro Graph search je optimální s monotóní heuristikou
    * Nevýhoda: je to lepší BFS, takže mu často dojde prostor
  * Heuristiky
    * Je přípustná, když je menší než nejkratší délka do cíle
      * Pro tree seach dostačující pro optimalitu
    * Je monotónní (konzistentní), pokud splňuje trojúhelníkovou nerovnost
      * Implikuje přípustnost
      * Pro graph search dostačující pro optimalitu
    * Příklad: najít heuristiky, která je přípustná, ale není monotónní
  * Příklad heuristiky
    * Floydova osmička
      * Počet špatně umístěných kostiček
      * Vzdálenost špatně umístěných kostiček od cílové pozice - Manhatanská heuristika

# 3 Constraint Satisfaction Problem (49-68)
* https://ktiml.mff.cuni.cz/~bartak/constraints/constrsat.html
* Problém $N$ královen na šachovnici - mám je rozmístit tak, aby se neohrožovali
  * Jednoduchý algoritmus
    * Královny budu umisťovat po sloupcích a pro každou další budu hedat řádek, případně se vracet
  * Přecházíme z atomické reprezentace na faktorovou - každý stav je reprezentován vektorem, pozicích všech královen
    * To potřebuji, abych mohl ověřit, že je cílový
  * Heuristika: počet konfliktů
  * Popis problému
    * Stavy: pozice královen na šachovnici
    * Výchozí stav: prázdná šachovnice
    * Cílový stav: královny se nehorhožují
    * Akce: Umísti královnu na šachovnici tak, aby nehrožovala ostatní
      * Pokud umisťují po sloupcích, zbavím se simetrií → hodí se tree search
      * Protože cílový stav je v hloubce $N$, tak se hodí DFS
  * Fowrward-checking
    * Metoda, která kontroluje, zda je ještě možné dosáhnout cílového stavu
    * U problému n královen "škrtám" políčka, kam lze umístit královnu v dalších řádcích a kontroluji, zda vůbac
* Problém: sudoku
  * Pro každé políčko si spočítám kandidáty
* CSP (Constrains Satisfaction Problem)
  * Definice
    * Konečná množina proměnných - popisují vlastnosti světa
    * Domény - hodnoty, jichž mohou proměnné nabývat
    * Konečná množina podmínek
      * Relace na proměnými (podmnožiny kartézského součinu)
      * Arita - počet proměnných relace
  * Cíl
    * Hledám ohodnocení proměnných splňující podmínky
    * Mělo by být úplné (všechny proměnné ohodnoceny) a korektní
  * Přípustné řešení
    * Nastavení hodnot proměnných z domén tak, že splňuje podmínky
    * Úplné pokud všechny proměnné mají nastavenou hodnotu
    * Konzistentní pokud jsou všechny podmínky splněny
* Modelování problému
  * Problém je nutné namodelovat jako CSP
* Řešení CSP
  * Backtracking search
    * Nastavím hodnotu proměnné (podle určeného pořadí)
    * Zkontroluji podmínky
      * Pokud splňuji, nastavím další
      * Pokud ne, backtracking
* Arc consistency
  * Vrcholy grafu jsou proměnné, orientované hrany jsou podmínky
  * V tomto případě uvažujeme pouze binární CSP
  * Cílem je mít hrany konzistentní -> pak je Arc consistent
    * Hrana (i,j) je konzistentní, pokud pro každou proměnnou x z domény D(i) existuje hodnota y z D(j), které splňují binární podmínku na i,j
  * Algoritmus AC-3
    1. Mam frontu hran 
    2. Dokud neni prazdna:
    3.  Vezmu hranu z fronty (i,j)
    4.  Pokud (i,j) neni konzistentni:
    5.    Odstranim nekonzistentni hodnoty z D(i)
    6.    Pridam vsechny hrany vedouci do i (k,i) do fronty (krome k=j)
  * Forward checking
    * Kontroluji podminky pouze aktualni promenne
  * Look ahead
    * Kontroluji vsechny podminky
  * Rozdíl forward checking a look ahead
    * FC kontroluje pouze podmínky s aktuální proměnnou
    * LA kontroluje všechny podmínky - proto je efektivnější
  * Druh tzv. local consistency
    * Arc consistency zaručuje pouze lokální konzistenci vůči podmínkám
* K-consistency
  * Konzistence k proměnných
  * Věta: pokud je problém i-konzistentní pro i=1..n (n je # proměnných), pak ho lze řešit DFS bez backtrackingu
  * Nevhodné, časová složitost exponenciální podle k
* Globální podmínky
  * TODO
* Pořadí ohdonoceování proměných
  * DOM heurstika: vyberu proměnnou, pro kterou mám nejméně možností ohodnocení
  * DEG heurstika: vyberu proměnou, která figuruje v nejvíce podmínkách
  * Obvykle se používá kombinace (dom/deg, dom+deg)
* Pořadí přiřazování hodnoty proměnné
  * Succeed-fist: vyberu hodnotu, která má více podpor nebo heuristiku vhodnou pro daný problém
  * Constraing programin předmět v zimě

## Cvičení
* Algoritmů je spočetně mnooho
* Rozhodovacích problémů je nespočetně mnoho
* Příklad problémů, pro které neexistuje algoritmus
  * Halting problem
* Chceme obecný algoritmus na řešení problémů
* Příklady tříd úloh
  * Lineární programování
  * Konvexní optimalizace
  * CSP (Constrains satisfaction programming)
  * SAT
  * Automatické plánování
* Formulace CSP
  * Konečná množina proměnných
  * Konečná množina hodnot každé proměnné - domény
  * Množina podmínek
    * k-tice proměnných
    * Podmnožina kartézského součinu domén těchto proměnných
    * Podmínkou je, aby k-tice hodnot těchto proměnných ležela v této podmnožině
  * Řešiče CSP pak hledají takové ohodnocení, které splňuje všechny podmínky
  * Příklady řešičů: python-constrains, IBM CPLEX CP, Google OR-Tools
  * Budu hledat perfektní párování v grafu
    * Může být bipartitní - pokud každé proměnné přiřazuji hodnotu
    * Obecný graf 

# 4 Logika a SAT (71-88)
* Výroková (i predikátová) je formální framework pro reprezentaci znalostí a odvozování nových faktů
* Problém královen s binárními proměnými
  * Nevýhoda: domény jsou jenom dvouprvkové → při filtrování, pokud odstraním jednu hodnotu, dozvím se druhou ... tak silná ale filtrace obvykle není
* CNF
  * Proč místo CNF neopužívám DNF, kde to jde rychleji? 
    * Protože formule v DNF jsou obvykle delší než formule v CNF
    * DNF vlastně vyjmenovává všechny modely
* Algoritmus DPLL
  * Najde splňující ohodnocení
  * Úplný (bud vydá řešení, nebo řekne, že neexistuje)
  * Dva základní principy
    * Hledá pure symbols - proměnné, které se ve všech klauzilích vyskytují pouze pozitivně nebo negativně
    * Hledá unit clauses - jednotková klauzule, ta má jen jediné možné ohodnocení
  * Heuristiky
    * Degree - vezmu proměnnou, která je v nejvíce klauzulích
* Moderní SAT solvery
  * Rozdělení formule na více nezávislých
  * Vhodná volba proměnné k ohodnocení
  * Random restart
  * Clause learning - pokud najdu konflikt, přidám jej jako novou klauzuli
* Konwladge-based agent
  * Racionální agent
  * Pomocí logických pravidel umí odvodit z pozorování dodatečné informace
  * Knowledge base
  * Operace TELL, ASK
* Pojmy
  * Rezoluce
    * Zamítací procedura
  * Hornova klauzule
    * Klauzule (tj. disjunkce literálů), kde je nejvýše jeden literál pozitivní
    * Vznikají z implikací
  * Odvoditelnost
* Rezoluční algoritmus
  * Zamítací procedura
  * Rezoluční pravidlo
* Forward chaining
  * Z faktů k důsledkům
  * Používá Hornovy klauzule
  * Data-driven
* Backward chaining
  * Od tvrzení k faktům
  * Query je decomposed na Hornovy klauzule
  * Goal-driven

# 5 Plánování (90-110)
* Fluent - proměnná měnící se s časem (s parametrem t)
* Observation model
  * Spojue informace ze senzorů (pozorování) a vlastnosti světa
* Transition model
  * Využívá tzv. effect axiomy - akce v čase t
    * Neříkají ale, co se nezměnilo
  * Popisuje přechody mezi stavy
* Frame problém: effect axiomy neříkají, co se akcí nezměnilo
  * Řešením jsou frame axiomy - popisují, co po akci zůstane stejné
  * Musím pospat, že věci se mezi $t$ a $t+1$ nezměnili
  * Frame axiomů je ale mnoho - nepraktické
  * Forward^t → (HaveArrow^t ↔ HaveArrow^t+1)
* Successor-state axiomy
    * Popíšu axiomy v čase t pomocí těch stejných v čase t-1 
    * Popisují změny stavů
    * Můžu snadno popisovat více akcí ve stejném čase (proměnné nastavené na True)
    * Hybridní agent plánovač
    * F^t+1 ↔ ActionCausesF^t or (F^t and not ActionCausesNotF^t)
* Plánování pomocí odvozování
  * Kombinuje logiku a A*
  * Idea:
    1) Zkontruuji logickou formuli, kde popíšu:
      * Init^0 - výchozí stav
        * Musím jej znát úplně (tj. ne jen nastavit vhodné proměnné na true, ale i ostatní na false - kromě toho, kde agent je, musím umět říct, kde není)
      * Transition^1..Transition^t - succesor-state axiomy pro všechny akce
      * Goal ^t - cíl
        * Nemusím znát jeho přesný čas - budu t postupně inkerementovat
    2) Nechám ji vyřešit SAT solver
    3) Pokud najdu model, mám plán. Jinak neexsistuje
* SATPlan
  * Plánování pomocí SAT
  * Vždy najde nejkratší plán - právě díky tomu, že čas inkrementujeme postupně
  * Výroková logika je dost neefektnvní → ale "zachrání" nás predikátová :)
  * Musím umět efektivně generovat výrokové formule
  * Preconditions axioms
    * Podmínky pro danou akci
* Porovnání řešení plánování
  * Prohledávání: nevýhoda je atomická reprezentace stavů, potřebujeme kvalitní domain-specific heuristiku
  * Logika: nevýhoda je počet proměnných, ale lze využít heuristiku vzhledem k logikcké povaze problému
* Situační kalkulus
  * Nevýhoda využití pouze výrokové logiky je počet proměnných
  * Situační kalkulus využívá predikátovou logiku (prvního řádu)
  * Popis:
    * Term pro situace: Result(state, action)
    * Term pro stav: At(robot, location)
      * Pokud se stav meni, pridame dalsi argument (fluents) : At(robot, locaiton, s)
      * Naopak pokud se stav nemeni, rikame tomu rigit predicate
    * Term pro preconditions: Phi(s) → Poss(a, s)
  * Typy formuli:
    * Stavy (fluent nebo rigid)
    * Preconditions (possibility axiom)
    * Succesor-state axiomy
  * Opět hledám takové ohodnocení, že cílová podmínka je splněna
* Typy axiomu
  * Effect axiomy: popisuji zmeny stavu
  * Frame axiomy: popisuji, co efekt nezmeni ... je jich hodne
  * Succesor-state axiomy: lepsi nez frame axiomy. Popisuji fluent v case t-1 a t
  * Preconditions axiomy: podminky akce (pokud chci strilet, musim mit sip)
  * Action-exclusion axiomy: zabrani mi udelat vice akci soucastne v jednom case
* Klasické plánování
  * Faktorová reprezentace (stav určen vektorem hodnot)
  * Stav jsou instantiated atoms (ne promenne)
    * Fluents: hodnota se ve stavu meni
    * Rigit atom: hodnota se nemeni
  * Closed world assumption 
    * Co neni nastaveno na true, predpokladam, ze je false
  * Jsem v cili, pokud je goal+ podmnozina stavu a zaroven g- prunik stav je prazdny
  * Operátor - reprezentace akcí agenta ve formátu (name(o), preconds(o), effects(o))
    * Akce je plně instanciovaný operátor
  * Aplikace akcí
    * Akce je aplikovatelná, pokud jsou splněny podmínky a žádná z nich není ve sporu s aktuálním stavem
    * Nový stav je pak sjednocením původního, přidáním positivních efektů a odebráním negativních
  * Relevance akcí
    * Akce je relevantní pouze pokud její efekty mají s cílem neprázdný průnik
  * Regression set - operace, pomocí nichž se dostanu do cíle
  * Planning domain model - popis operátorů
  * Planning problem P = (O,s_0,g)
    * O je planning domain model
    * s_0 je počáteční stav
    * g je cílová podmínka
    * Plán je pak n-tice akcí (a_1,..,a_k)
  * Příklad na slide 105
* Forward planning (progresivní)
  * Algoritmus Forward search
  * Jde o počítečního stavu k cílovému
  * Obvykle A* s dobrou heuristikou
  * FwS(O,s_0,g)
    1. Dokud nemám plán nebo není nesplnitelný:
    2.  Vyber akce z O, které splňují podmínky
    3.  Pokud neexistují, tak nelze
    4.  Vyber vhodnou akci (heuristika)
    5.  Aplikuji akci a pokračuj
* Backward planning (regresivní)
  * Algoritmus Backward search
  * Prohledává pouze od cíle - výhodné, protože prohledává pouze větve vedoucí k cíli
  * Je ale obtížejší vymyslet vhodnou heuristiku
  * BwS(O,s_0,g):
    1. Dokud nemám plán nebo není nesplnitelný:
    2.  Vyber ty akce, které za vedou k cíli
    3.  Pokud neexistují, tak nelze
    4.  Vyber vhondou z nich (heuristika)
    5.  Nastav nový cíl (resp. cíle)
* Heuristiky
  * Potřebujeme heuristiku pro prohledávací algoritmus
  * Odhad vzdálenosti do cíle 
  * K odvození heuristiky můžeme použít tzv. relaxaci problému - jeho zjednodušenou verzi
  * Nebo můžu ignorovat preconds
  * Nebo můžu ignorovat negativní efekty
* Jiné formy plánování
  * Plan-space planning
  * Hierarchical planning

# 6 Bayeskové sítě (112-134)
* Nejistota
  * Zpusobená nemožností úplného pozorování nebo nedeterminismem (nevíme, jak akce dopadne)
* Logical agent může pracovat s:
  * Belief states - stavy, ve kterých se agent může nacházet	
  * Contingency plan - nevíme, v jakém stavu budeme po akci, tak počítáme všechny možnosti
    * Místo sekvence možných budeme uvažovat každou možnost (strom)
  * Obě reprezentace mohou být netriviálně velké
  * Logical agent ale přesně ví, ve kterém stavu se nachází (hodnoty 0,1)
* Probabilistic agent
  * O každém stavu má reálnou hodnotu 0-1
* Opakování pravděpodobnosti
  * Prostor elementárních jevů (sample space)
  * Pravděpodobnost (probability)
  * Prosor jevů (event space)
  * Nepodmíněná pravděpodobnost (prior)
  * Podmíněná pravděpodobnost (posterior)
  * Náhodné proměnné (veličiny)
    * Každá má doménu (obor hodnot)
  * Sdružená distribuce (náhodné vektory)
    * Distribuce
  * Marginální distribuce 
  * Podmíněná pravděpodobnost
    * $P(Y|E=e) = P(Y,E=e)/P(E=e)$
    * Zároveň platí $\sum_{y \in Y}P(Y=y,E=e)=1$, takže $\alpha=1/P(E=e)$ lze považovat za tzv. normalizační konstantu
    * Pak $P(Y|E=e)=\alpha P(Y, E=e)$
* Pravděpodobnostní distribuci můžu reprezentovat pomocí tabulky
  * Můžu ale ušetřit místo, protože některé náhodné veličiny jsou nezávislé (absolutně nebo podmíněně)
  * Absolutní nezávislost: P(X|Y) = P(X) nebo P(Y|X) = P(Y) nebo P(X,Y) = P(X).P(Y)
  * Podmíněná nezávislost (s n.v. Z): P(X|Y,Z) = P(X|Z) nebo P(Y|X,Z) = P(Y|Z) nebo P(X,Y|Z) = P(X|Z) P(Y|Z)
  * Důsledek: místo jedné velké tabulky budu mít řadu menších
* Popis problému:
  * Boolovské náhodné proměnné
  * Query: pravděpodobnost, kterou chci znát
  * Evidence: známé hodnoty
* Bayesovo pravidlo: $P(a|b) = P(b|a) P(a) / P(b)$
  * Obecně $P(Y|X) = P(X|Y) P(Y) / P(X) = \alpha P(X|Y) P(Y)$
  * V praxi: $P(cause|effect) = P(effect|cause) P(cause) / P(effect)$
* Naive Bayes model
  * Předpokladá, že všechny effects jsou nezávislé (naivně předstírá a přeje si, aby to byla pravda - NENÍ)
  * $P(Cause,Effect_1,…,Effect_n) = P(Cause) Pi P(Effect_i|Cause)$
* Bayesovská síť
  * DAG
  * Odovzuje podmíněnou pravděpodobnost n.v. X na základě pozorování (evidence)
  * Relativně efektivní reprezentace sdružené pravděpodobnosti
  * Vrcholy odpovídají náhodným veličinám
    * Každý vrchol X má podmíněnou distribuci $P(X | Parents(X))$
  * Sdružená distribuce: $P(x_1,…,x_n) = P_i * P(x_i | parents(X_i))$
  * Vytvoření bayeskovské sítě
    * Vrcholy
      * Uspořádání podle závislostí (funguje každé pořadí, v nejhorším případě dostanu velkou tabulku)
    * Hrany
      * Vyberu si vrchol a najdu jeho rodiče, že $P(X_i | Parents(X_i)) = P(X_i | X_i-1..X_1)$
* Inference by enumeration (odvození enumerací)
  * $P(X|e) = a P(X,e) = \alpha \sum_y P(X,e,y)$, kde $P(X,e,y)$ je $P(x_1,…,x_n) = P_i P(x_i | parents(X_i))$
* Inference by variable elimination (odvození eliminací)
  * Dynamické programování
  * TODO
* Monte Carlo
  * Aproximace
  * TODO

# 7 Dynamické Bayeskové sítě (136-151)
* Situační kalkulus
  * Pohled na svět jako na sérii okamžiků
  * Každý okamžik je soubor náhodných veličin
    * Skryté náhodné veličiny
    * Pozorovatelné náhodné veličiny $X_t$
  * Přechodový model, kde aktuální stav závisí na všech ostatních předchůdcích $E_t$
* Formální model
  * Transition mode
    * Popisuje pravděpodobnostní ditribuci $P(X_t | X_{0:t-1})$
    * Markovský předpoklad $P(X_t | X_{0:t-1}) = P(X_t | X_{t-1})$ a zároveň $P(X_t | X_{t-1})$ je stejné pro všechna $t$
      * Tj. vývoj světa nezávisí na konrétním čase
  * Sensor (observation) model
    * Popisuje, jak evidence (observed) variables $E_t$ zásiví na ostatních $P(E_t | X_{0:t}, E_{1:t-1})$
    * Sensor Markov Assumption $P(E_t | X_{0:t}, E_{1:t-1}) = P(E_t | X_t)$
      * Tedy aktuální pozorování odpovídá aktuálnímu stavu
* Transition i sensor model lze popsat pomocí Bayesovské sítě
* Úlohy
  * Filtrace (Filtering)
    * Zjištění aktuálního stavu na základě všech předchozích pozorování
    * $P(X_t | e_{1:t})$
    * Příklad: kde jsem teď?
    * Filtrovací algoritmus by se měl udržovat aktuální stav a umět ho aktualizovat, abych nemusel vše počítat pořád znovu
    * Hodí se pro rozhodování, kam jít dál, pokud v čase $t+1$ bude evidence $e_{t+1}$
      * $P(X_t+1|e_{1:t+1}) = f(e_{t+1},P(X_t|e_{1:t}))$
      * Co je funkce f? Odvození:
        * $P(X_{t+1}|e_{1:t+1}) = P(X_{t+1}|e_{1:t},e_{t+1})$
        *   = $\alpha P(e_{t+1}|X_{t+1},e_{1:t}) * P(X_{t+1}|e_{1:t})$  - Bayesovo pravidlo
        *   = $\alpha P(e_{t+1}|X_{t+1}) * P(X_{t+1}|e_{1:t})$          - Sensor markovský předpoklad
        *   = $\alpha P(e_{t+1}|X_{t+1}) * \sum_{x_t} P(X_{t+1}|x_t)P(x_t|e_{1:t})$
      * $f_{1:t}$ se prograguje vpřed - forward message
        * $P(X_{t}|e_{1:t}) = f_{1:t}$
        * $f_{1:t+1}=\alpha FORWARD(f_{1:t},e_{t+1})$
  * Predikce (Prediction)
    * Výpočet budoucí hodnoty n.v. (budoucího stavu) na základě všech předchozích pozorování
    * $P(X_{t+k} | e_{1:t})$, kde $k>0$
    * Příklad: kde budu v budoucnosti?
    * Je to jako filtering bez nové evidence
      * $P(X_{t+k+1}|e_{1:t+1}) = \sum_{x_{t+k}} P(X_{t+k+1}|x_{t+k})P(x_{t+k}|e_{1:t})$
  * Vyhlazování (Smoothing)
    * Výpočet minulého stavu
    * $P(X_k | e_{1:t})$ pro $k < k < t$
    * Příklad: kde jsem byl?
    * Backward message passing
  * Most likely explanation: the task to f
    * Nejpravděpodobnšjí vysvětlení - když znám pozorování, chci zjistit nejpravděpodobnější posloupnost stavů
    * $ \argmax x_{1:t} P(x_{1:t} | e_{1:t})$ 
    * Viterbiho algoritmus
* Hidden Markov Model (HMM)
  * Pokud je stav popsán jedinou n.v. $X_t$ (a jednou pozorovatelnou veličinou $E_t$), pak to nazýváme HMM
  * Jednoduchý pohled - jednoduchá implementace pomocí maticových operací
    * Nechť $X_t$ nabývá hodnot z $\{1..S\}$
    * Pak transition model $P(X_t|X_{t-1})$ je matice $T^{S*S}$, kde $T_{ij}=P(X_t=j | X_{t-1}=i)$
    * Sensor model je pak matice $O_{t(i,i)}=P(E_t=e_t | X_t=i)$
  * Maticová formulace algoritmů
    * Forward message propagation (filtering)
    * Backward message propagation (smoothing)
  * Příklad s lokalizací robota
    * Mám robota na mřížce, má mapu světa a (nepřesné) senzory, které mu říkají, kde jsou překážky
    * Ranodm variables: $X_t$ - pozice robota v čase t
    * Transition model: náhodný pohyb po mřížce $P(X_{t+1}=j|X_t=i) = 1/ |Nb(i)|$ kde $Nb(i)$ jsou sousedé $i$, pro $j \in Nb(i)$
    * Sensor model: pozorování v čase t jako $E_t$
    * Sensor tables: nepřesnost
* Dynamická Bayesovská síť
  * Bayesovská síť reprezentující dočasný stav pravděpodobnostního modelu
  * Prior distribution: $P(X_0)$
  * Transition model: $P(X_1|X_0)$
  * Sensor model: $P(E_1|X_1)$
* DBN vs. HMM
  * HMM je speciální případ DBN
  * DBN ale můžu převést na HMM tak, že hodnoty n.v. budou n-tice
  * Obecně je DBN úspornější a efektivnější

# 8 Decision making (154-168)
* Racionální agent (připomenutí)
  * Dělá racionální rozhodnutí na základě svých pozorování a svého cíle
    * Jak dělat racionální rozhodnutí při nejistotě?
      * Budeme měřit utilitu
      * Budeme dělat jednoduchá rozhodnutí za sebou
* Decision theory
  * Vybírám tu akci, která mi přinese co nejvyšší aktuální užitek
  * V deterministickém světě: Result(s,a) - výsledek akce a ve stavu s
  * V nedeterministickém světe:
    * Result(a) - n.v. popisující výsledky akce a
    * P(Result(a)=s | a, e) - pravděpodobnost, že výsledkem akce a je stav s při evidenci e
* Utilita
  * Preference agenta zachycuje utilita U(s)
  * Expected utility $EU(a|e)=\sum_s P(Result(a)=s|a,e) U(s)$
  * Maximum expected utility princip: $action = \argmax_a EU(a|e)$
    * Racionální agent si vybere akci s maximální utilitou
  * Utility theory
    *  Je jednodušší porovnat dva prvky, než všechny ohodnotit
    *  Musím zachovat tranzitivitu
    * Normalized utility function
* Rozhodovací síť
  * Vylepšená Bayesovská síť
    * Desicion node - místo, kde se agent rozhoduje
    * Chance node - reprezentuje náhodnou veličinu
    * Utility node
  * Vyhodnocení rozhodovací sítě
    1. Nastavím evidence variables podle aktuálního stavu
    2. Pro každou možnou akci:
    3. Spočítám posterior a utilitu 
    4. Vrátím akci s nejvyšší utilitou
  * Vytváření rozhodovací sítě
    * Vytvořím model situace
    * Nastavím pravděpodobnosti
    * Nastavím utility akcí
    * Ověřím správnost
    * Ověřím sensitivitu na malou změnu vstupu
* Sequential decision problems
  * Utilita závisí na sérii rozhodnutí
  * Uvažujme plně pozorovatelné, ale nedeterministikcé prostředí
* Markov Decision Process
  * Sekveční rozhodovací problé pro plně pozorovatelné, ale stochastické prostředí s markovským přechodovým modelem a rewardem
  * Transition model:  $P(s'|s,a)$ - pravděpodobnost dosažení stavu s'ze stavu s při akci a
  * Reward $R(s)$ - zisk ve stavu s
    * Krátkodobý zisk
  * Utilita $U([s_0,..])=R(s_0)+\gamma R(s_1)+\gamma R(s_2)+...$, kde $0 < \gamma < 1$
    * Dlouhodobý zisk
  * Policy $\pi (s)$ - funkce vracející nejlepší akci pro každý stav
    * Je optimální, pokud vrátí tu s nejvyšší utilitou
  * Expected utility s policy $\pi$
    * $U^{\pi}(s) = E[\sum_i \gamma_i R(S_i)]$ kde $S_t$ je n.v. popisující stav v čase $t$
    * Optimalní policy je pak $\pi_s = \argmax_\pi U^\pi(s)$
      * Nezávisí na počátečním stavu
    * True utility of state je pak ta s nejlepší policy
  * Bellmanova rovnice: $U(s) = R(s) + \gamma \max_a \sum_{s'} P(s'|s,a) U(s')$
    * Řešení
      * Value iteration
        * $U_{i+1}(s) = R(s) + \gamma \max_a \sum_{s'} P(s'|s,a) U_i(s')$
        * Opakuji, dokud $|U_{i+1}(s) - U_{i}(s)| > \delta$
      * Policy iteration
        * Upravuji policy, dokud se zlepšují
  * Pokud mám částečně pozorovatelné prostředí, zavedu belief states (pravděpodobnostní distribuci nad všemi možnými stavy)

# 9 Teorie her (171-186)
* Teorie her z pohledu AI
* Místo jednoho agenta jich máme víc
* Svým způsobem ostatní agenty vůbec nepotřebuji - prostě mám jen nejistý výsledek akcí
* Ale můžu využít toho, že ostatní jsou racionální agenti a třeba i znám jejich účelové funkce
* Nejčastější typ hry: determenistické, dva hráči se střídají, s nulovým součtem a s úplnou informací
* Strom hry
  * Tahy hráčů odpovídají vrstvám
  * Celkový užitek hráče lze zjistit z listů
* Algoritmus minimax
  * Zkontruuji storm hry
  * Ohodnotím listy (mohou být v různých hloubkách)
  * Minimax hodnota záleží, kdo zrovna hraje (chci buď minimalizovat nebo maximalizovat) 
  * Problém: strom roste exponencionálně
  * Optimální strategie je nejlepší možná (ne nutně výherní)
* Vylepšení minimaxu
  * Alfa-beta ořezávání
    * Neperspektivní větve neprohledávám
  * Neprohledávám až k listům, ale ohodnotím pozice ve hře
  * Evaluační funkce
    * V šachách materiální hodnota - ohodnotím figurky a jejich pozice
    * Funguje na šachy, nefunguje na go
      * V šachách cca 30 možných tahů
      * V go cca 256 možných tahů a nemáme dobrou evaluační funkci
    * Efekt horizontu - algoritmus doběhne do určité hloubky a dál už se nedívá
* Stochastický prvek ve hrách
  * Příklad: házení kostkou
  * Mohu vyřešit tak, že do minimax stromu přidám chance úrovně
* Single-move hry
  * Hráči: agenti, kteří dělají akce
  * Akce: vybírají je hračí
  * Payoff funkce: ohodnocení akce hráče podle akcí všech ostatních hráčů
  * Hráči hrají podle strategie (policy)
    * Pure strategie: deterministická
    * Mixed strategie: randomizovaná, používá pravděpodobnostní distribuci
  * Řešení hry je pak najít optimální strategii pro každého hráče
* Vězňovo dilema
* Nashovo equilibrium
  * Žádnému hráči se nevyplatí změnit strategii
  * Paretooptimální - TODO
* Maximin
  * TODO
* Repeated games
  * TODO
* Game theory - analizuje agenty a ohodnocuje jejich rozhodnutí
* Inverse game theory - nastavuje pravidla tak, aby všichni agenti získali maximum
* Aukce
  * Dobrý aukční mechanismus je
    * Výhodný pro prodávajícího
    * Vyhraje ten, kdo si cení předmětu akce nejvíc
  * Anglická aukce
    * Začíná se minimálním ceně (bid)
    * Běží, dokud někdo přihazuje, prodá se za poslední příhoz
  * Holandská aukce
    * Začíná se na vysoké ceně a ta se snižuje, dokud ji někdo nezaplatí
    * Rychlá
  * Sealed-bid aukce
    * Pokud mám odhah, kolik dají ostatní, je lepší dát víc
  * Sealed-bid second-price aukce (Vickrey auction)
    * Nejlepší je dát tolik, na kolik si cením zboží

# 10 Machine learning (189-202)

# 11 Machine learning 2 (205-215)

# 12 Etika AI
* Přeskočeno