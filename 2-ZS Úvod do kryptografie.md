# Kryptografická primitiva

## Příklad na úvod :)

* Alice posílá zprávy Bobovi
* Eva poslouchá, co si Alice a Bob posílají
* Mallory nejen poslouchá, ale chce zprávy mezi Alicí a Bobem i upravit

## Symetrická šifra

* Prostý text -> zašifrování klíčem -> poslání zašifrované zprávy -> rozšifrování stejným klíčem -> prostý text
* Používáme stejný klič pro zašifrování i rozšifrování -> proto symetrická
* **Kerckhoffův princip**: "the secret should be only the key"

### Definice

Funkce pro zašifrování: E:{0,1}^n * {0,1}^k → {0,1}^n
Funkce pro dešifrování: D:{0,1}^n * {0,1}^k → {0,1}^n
Správnost: \forall x \in {0,1}^n \forall k \in {0,1}^k : D(E(x,k),k)=x

Pro náhodný klíč K platí, že E(_,k) je náhodnou permutací z množiny všech zpráv, tj {0,1}^n

### Césarova šifra

* Posun písmen v abecedě
* Formálně: E : {A ... Z} x {A ... Z}  → {A ... Z}; E(x,k)=(x+k)%|{A ... Z}|

## Asymetrická šifra

* Na rozdíl od symetrické máme šifrovací klíč a dešifrovací klíč
* Prostý text -> zašifrování klíčem k1 -> poslání zašifrované zprávy -> rozšifrování klíčem k2-> prostý text
* Obvykle je jeden klíč veřejný a druhý privátní
* Pomalé
* Stejně jako symetrická šifra nechrání zprávu před modifikacemi, pouze před čtením. Proto je třeba šifrované zprávy nějakým způsobem podepisovat.

### Zabezpečené posílání zpráv

* Ke je veřejný šifrovací klíč, Kd je privátní dešifrovací klíč
* Každý mi tedy může posílat zašifrované zprávy
* Problém je s ověřením, komu vlastně patří privátní klíč
* Složitost klíčem můžeme volit podle toho, jak dlouho potřebujeme, aby zůstala zpráva v bezpečí (někde stačí pár vteřin (nákupy na burze), jinde potřebujeme léta)

### Asymetrický elektronický podpis

* Ke je privátní šifrovací klíč, Kd je veřejný dešifrovací klíč
* Můžu tedy posílat zprávy, které každý může rozšifrovat -> ověřit, že jsem to já

## Hashovací funkce

* {0,1}^x → hashovací funkce → {0,1}^n
* Podle výstupu nelze určit vstup
* Pro jeden výstup existuje nekonečně mnoho vstupů (protože výstup má konečnou délku)
* Problém s kolizemi

### Symetrický elektronický podpis (MAC - message autentication code)

* sign(x,k)=H(X || K) ... kde || je spojení řetězců

### Vylepšení asymetrického elektronického podpisu

* sing(X,Ks)=E(H(x),Ks)
* Zprávu zahashujeme, hash zašifrujeme privátní asymetrickým klíčem. Posíláme originální zprávu a zašyfrováný hash, který si může každý rozšifrovat veřejným klíčem

## Generování náhodných čísel

* Generátor náhodných čísel -> stream náhodných bitů
* Ideálně naprosto nepředvídatelný

### Zrychlení asymetrických šifer

* Pro zašifrování samotné zprávy použiju **náhodný** symetrický klíč -> session key
* Symetrický klíč zašifruji pomocí privátního asymetrického klíče

### Challange-Response autentizace

* Alice chce potvrdit Bobovi, že zná heslo. Neví ale, jestli ho zná Bob, nechce ho tedy říct přímo
* Bob dá Alici zprávu, **challange**, a Alice ji podepíše symetrickým podpisem, kde klíčem je heslo, tj. MAC(challange, password)
* Challange musí být náhodná

## Problémy protokolu

* Zprávy lze opakovat -> použít pořadové číslo
* Lze převracet bity -> použít podpis
* Délka zprávy -> zarovnat zprávy na stejnou délku
* Používat pokaždé jiný klíč

# Útoky

## Typy
 * Získání zašifrované informace

* Získání klíče
  * Pokud mám dvojici zašifrovaný text - výsledný text

* Zašifrování zprávy naší klíčem

##  Úroveň bezpečnosti

* Úroveň bezpečnosti k → úspěšný útok vyžaduje alespoň 2^k kroků

## Challange-response autentizace

* N možných challanges
* M pokusů 
* f je funkce M → N
* Pr[f je injektivní]  = (# injektivních f) / (# všech f) = (n*(n-1)...*(n-m+1)) / (n^m) 
* ... nějáké další kroky, které má Lucka v sešitě
* ... = e^-(m^2/2n)
* Pokud chceme, aby pravděpodobnost opakování challange byla alespoň 1/2, pak musíme udělat sqrt(n) pokusů
* Pro k bitové challanges je tedy bezpečnost <= k/2

## Zašifrování symetrického klíče asymetrickým

* A vygeneruje náhodný klíč a pošle ho B zašifrovaný jeho veřejným klíčem
* Útočník si může vygenerovat hromadu náhodných klíčů a zašifrovat je B veřejným ... a pak porovnávat, zda A někdy nevygeneruje stejný, jako nějaký z jeho 
* N všech ožných klíčů
* D vygenerovaných klíčů úročníkem
* M pokusů utočníka
* Pr[A nevygeneruje nějaký stejný klíč, jako má utočník] = (1-d/n)^m

# Vernamova šifra (One-Time pod)

* Zpráva: x \in {0,1}^n
* Náhodný klíč: k \in {0,1}^n
* Šifrovací funkce: y = E(x,k)=x xor k
* Dešifrovací funkce: x = D(y,k)=y xor k
* Dokážeme, že každý bit y je s pravděpodobností 1/2 0 a s pravděpodobností 1/2 1 -> y je náhodné
  * To znamená, že každý bit je nezávislý na ostatních
  * A taky, že ze zašifrovaného textu nemám žádnou informaci o původním textu -> "Perfect Security"
  * Důkaz je v Lucky sešitě

* Omezení

  * Nesmím opakovat klíče
  * Klíč musí být stejně dlouhý jako text
  * y1 xor y2 = x1 xor x2 -> získám xor dvou různých zpráv
  * Není odolná vůči modifikacím -> úprava jednoho bit v zašifrované zprávě upraví jeden bit v dešifrované

# Tajné sdílení (Shamirovo sdílení tajemství)

## Pro dva

* Rozdělím klíč na dvě části, jednotlivě bezvýznamné
* x je klíč, x1,x2 jsou jeho části, kde x1+x2=x, ale x1 nebo x2 samostatně nic neznamenají
* x1 = náhodný text, x2 = x xor x1
  * x2 xor x1 = x xor x1 xor x1 = x

## Obecně

* Lemma: Pro každých d bodů v tělese F existuje právě jeden polynom takový, že prochází právě body x
  * Tj. pokud mám polynom stupně l a znám ho v l bodech, pak ho můžu jednoznačně určit
* (k,l), kde k je počet kousků, na které rozdělím tajemství a l je počet kousků, které potřebuji, abych ho zjistitl
  * k = # shares
  * l = # needed shares
* Definujeme polynom stupně l
  * f(0) = tajemství
  * f(1) ... f(l-1) hodnoty, které rozdělíme
  * f(1) ... f(k) - rozdělené klíče

# Symetrické šifry

## Block cipher

* Symetrická šifra pro zprávy fixní velikosti
* b - block size, k - key size
* E: {0,1}^b x {0,1}^k → {0,1}^b 
* Ek je permutací na {0,1}^b
  * Tj. je bijekcí z  {0,1}^b → {0,1}^b

### Bezpečnost

* D - Distinguisher
  * Dostane buď permutaci na {0,1}^b nebo Ek pro náhodný klíč
  * V čase 2^sec.lvl. umí rozlišit, zda je o zašifrovaný text nebo náhodnou permutaci
* E není bezpečná právě když by existoval distinguisher s pravděpodobností úspěchu alespoň 2/3

### Iterated cipher

* Více kol, ve kterých šifruji (Round R1 ... Rk)
  * Pro každé kolo potřebuji zvláštní klíč
    * Existuje Key Schedulery, která z jednoho klíče vygenerují klíče k1 ... kk
* Whiteing - před spuštěním cyklu XOR textu s klíčem
  * Útočník tak nemůže do cyklu posílat vstupy jako třeba 000010000...
* Dešifrovací funkce je stejná jako šifrovací, jen s inverzními S boxy

#### SPM (Substitution permutation network)

* Viz Lucky sešit
* Jedno kolo iterační šifty
* Více S boxů a jeden P box, který permutuje výstupy S boxů
* Výstup XOR s klíčem

### Feistelova šifra

* Zprávu rozdělíme na dvě stejné část → L0, R0
* f - Feistelova funkce
* Provedeme více kol, v každém 
  * L_i+1 = R_i
  * R_i = L_i XOR f(Ri, Ki)

### DES (Data Encryption Standart)

* Vyvinutá v 70. letech firmou IBM
* 64bitové bloky, 56bitové klíč
* Feistel netowrk s 16 koly
* ![image-20211013101228355](C:\Users\david.zeman\AppData\Roaming\Typora\typora-user-images\image-20211013101228355.png)
* Problémy a slabiny
  * Slabé klíče jsou samé 1 nebo samé 0
  * Dk = Ek
  * E_NEG(k)(NEG(x))
  * Krátký klíč
  * Malý blok
* 2-DES
  * 112bitový klíč
  * Ek2(Ek1(x))=y
  * DÚ - najít slabinu
    * Pokusím se zašifrovat x všemi možnými klíči k1 a zapamatuji si je ... to je 2^56 kroků
    * Pak vezmu y a pokusím se ho dešifrovat všemi možnými klíčky k2 ... to je 2^56 kroků
    * Celkem tedy 2^57 kroků
* 3-DES
  * 168bitový klíč
  * Security level = 112

### AES (Advanced Encryption Standart)

* Výsledek soutěže z roku 1997
  * Vybrán návrh Rijndael
* Požadavky
  * bezpečnost
  * Rychlá v HW i SW implementaci
* Parametry
  * Klíč 128/192/256 bitů
  * Kol: 10/12/14
* Jedno kolo šifrování
  * Stav je jeden blok - 128 bitů, tj. 16 bytů v matici 4x4 ~ GF(2^8)
  * Před prvním kolem se provede jednou AddRoundKey
  * Kroky
    1. ByteSub (identické 8bitové S-boxy)
    2. ShoftRow - řádky se posunou vlevo (1. o 0, 2 o 1, ...)
    3. MixColumn
    4. AddRoundKey
* Dešifrování
  * AddRoundKey (používá inverzní sboxy)
  * InverseMixColumn
  * InverseShiftRow
  * InverseByteSub
* Nevýhody
  * Jednoduchá algebraická struktura
  * Počet kol 10 je možná málo
  * Byte-aligned
  * Grover's algorithm na kvantovém počítači najít inverzní funkci v čase O(sqrt(n)) ... takže pro 128bitový klíč v čase 2^64

### Padding

* U blokových šifer je nutno rozdělit zprávu na bloky
* Pokud není zpráva dělitelná velikostí bloku → potřebuju padding (který je reversible)
* Typy paddingu
  1. 80 00 00 00 00 ...
  2. P P P P ... kde P je velikost paddingu
  3. 00 00 00 ... P kde 0 je P-1 a P je délka paddingu

### Módy

#### ECB (Electronic codebook)

* Máme bloky X1 ... Xn a všechny proženeme Ek(X...) a dostaneme Y1 ... Yn
* Špatná volba
  * Stejné bloky na vstupu budou stejné jako na výstupu
    * Xi = Xj ↔ Yi = Yj
  * No IV → deterministická
  * Snadná manipulace s bloky - pokud je prohodím, nikdo si toho nevšimne

#### CBC (Cipher Block Changing)

* Máme bloky X1 ... Xn a všechny XOR s s předchozím Y1...  a ty proženeme Ek(X...) a dostaneme Y1 ... Yn
* X1 → XOR IV → Ek(...) → Y1
* X2 → XOR Y1 → Ek(...) → Y2
* ...
* IV musí být random
* Bitflip v Y1 se zpropaguje dál
* Nejpoužívanější

##### Leak

* Po ~ 2^(keySize/2) krocích bude pravděpodobně existovat i,j, že Yi=Yj
* Pro více info viz Lucky sešit
* Řešení → nepoužívat stejný klíč moc dlouho 

#### CTR (Counter)

* Z blokové šifry udělá stream šifru
* Mám IV, IV+1, IV+2 .... a proženu je Ek(...). S výsledky udělám XOR X1, XOR X2 ... a dostanu Y1, Y2...

##### Leak

* Viz Lucky sešit

#### OFB (Output feedback)

* Stejné jako CTR, ale nešifruju IV+1, ale místo toho Ek(IV), ne IV+2, ale Ek(Ek(IV)) atd...

### Padding oracle Attack

1. 
   * Padding jako k bytů hodnoty k
   * Na konci je 0
   * Změním padding hodnoty k na k+1
   * ... atd. Viz Lucky sešit. ... nebo Barči

### Ciphertext stealing

* CBC
* ECB

## Stream cipher

* PRNG (Pseoudo random number generator) parametrizovaný klíčem generuje pseudo náhodné bity
* Ty XOR s bity zprávy a dostáváme "zašifrované bity" ... klíč by se ale neměl opakovat
* Proto PRNG parametrizujeme navíc "nonce", počáteční inicializační hodnotou (aby se mohl opakovat klíč)
* Vlastnosti
  * Self-inverse - dešifrovací funkce je stejná jako šifrovací (Ek = Ed)
  * Převrací bity
  * Komutativní - Ek(Ek'(x)) = Ek'(Ek(x))

### RC4

* Vznik v roce 1987, 2015 čas na prolomení 75 hodin

### eStream

* Evropa, 2004-2008
* Požadována jednoduchá SW implementace

### ChaCha20

* Zlepšení Salza20


# Hešovací funkce

* $\{0,1\}^* → \{0,1\}^b$

* Vlastnosti

  1. Nemělo by být možné snadno najít kolizi, tj, $x,y : h(x)=h(y)$

  2. Nemělo by být možné najít druhý předobraz jednoho hashe

  3. Nemělo být být možné z hashe odvodit původní zprávu
  * 1 → 2 → 3

## Compression function

* Vstup $x_1$, $x_2$ a výstup $y_1$, vše o velikosti bloku b
* Chceme, aby byla collison-free
* Merkle-Damgard construction
  * Colision fucntion → hash fucntion
* **Věta**: Pokud je $g$ bez kolizí, pak i $MD(g)$ je bez kolizí.
* DÚ: Tossing a coin over phone call
  * Řešení 1.
    * Alice vygeneruje random bit A, Bob random bit B
    * A uděláme A xor B
    * Funguje ale, jen pokud si pošlou A,B navzájem ve stejný okamžik
  * Řešení 2.
    * Alice vygeneruje random bit A, Bob random bit B
    * Alice udělá commitment
      * Tj. nějak zaznamená svoji volbu
      * Obvykle hash(A || nonce)
    * Bob pošle B Alice a ta udělá A xor B

## Útoky na hešovací funkce

### Bitrhday attack

* $h:\{0,1\}^n→\{0,1\}^b$
* Můžeme zkoušet všechny heše
  * Ptřebujeme alespoň $2^{\frac{b}{2}}$ různých hešů
  * Náročné na paměť
* Parametrizované zprávy
  * K parametrů - bitů
  * Každý z nich určuje část zprávy (zda je použije verze 0 nebo verze 1)
  * Celkem tedy $2^k$ zpráv

### Birthday attack s úsporou paměti

* Děláme x → h(x) → h(h(x)) → ....
* Po čase najdeme v grafu všech možných hešů cyklus
  * A s ním i vrchol, kde dochází ke kolizi (tj. vedou do něj dvě hrany)
  * Jak ho najít?
    * Pošlu grafem dva pointery H, T z x
      * T skáče vždy o jeden vrchol
      * H skáče vždy o dva vrcholy
      * Po lineárně mnoha krocích se v grafu potkají
        * Až se potkají, pošlu z x ještě želvu
        * Ti dva potkaní a želva se pak budou vždy pohybovat po jednom kroku a jistě se potkají ve vrcholu, kde dochází ke kolizi

## Davies-Mayer construction

* Jak získat kompresní funkci?
* E_k (bloková šifra) → f (kompresní funkce)
* f(x,y)=E_x(y) xor y
* Problém, pokud bychom za E_k použli DES
* Věta: Pro D-M s ideální blokovou šifrou by útok s q pokusy našel kolizi s pravděpodobností <= q^2 / 2^b 
*  

## Praktické hešovací funkce

### MD5

* 128b výstup
  * Problém
* V roce 2004 nalezen způsob, jak hledat kolizce
* M-D konsturkce

### SHA-1

* 160b výstup
* V roce 2015 byla objevena kolize, v roce 2017 plná kolice

### SHA-2

* 224b, 256b, 384b, 512b

## Sponge consrukction

* 

* tady jsem chyběl

# Náhodné generátory

* Chceme aby proces byl nepředídatelný a neovlivnitelný

## Pseudonáhodné generátory (PRNG)

* Vezmu blokovou šifru a postupně do ní strkám vstupy jako iv, iv+1...

## Fyzikální

* Napříkla $\alpha$ rozpad (budu měřit intervaly mezi emitovanými částicemi)
* Měření např. rádiového nebo tepelného šumu
* Založené na principu přesného měření času
* Ring oscilator (cyklický obvod se třemi hradly pro negaci)

## Kombinovaný přístup

* Vezmu PRGN a jeho výstup zkombinuji s nějakým fyzikálním generátorem
* V praxi si fyzikální entropii hromadím někde v bazénku (pool) a když jí mám dost (128 bitů), tak jí zkombinuji s PRGN hodnotou

## Fortuna

* Dvě komponenty
  * Generátor
    * Používá AES s 256bitový klíčem, šifruje 128bitový čítač
    * Po $2^{16}$  blocích změna klíče, ale neresetuje se čítač
      * Klíče se změní na základě nějakého fyzikálního generátoru (hashe jeho dat)
  * Entropy akumulator
    * Má 32 poolů (bazénků), v němž hromadí entropii
    * Ty se postupně prohrazjí
    * Pro i-té změné klíče se použije i-tý bazének

# Secure Chanel

* A ↔ B
* $m_1 ... m_n$ → podposloupnost
  * Tz. B vždy dostane podposloupnost zpráv od A a umí určit, které zprávy chybí (pokud nějaké)
* Postup
  * Náhodný klíč
  * Symetrická šífra (AES)
  * MAC po zašifrování
  * Sequence number pro určení pořadí zpráv
  * Key derivation pomocí hash funkce

# Složitost

* Uvažujeme b-bitová čísla
* Operace
  * ADD, SUB, CMP - O(b)
  * MUL O($b^2$)
  * DIV, MOD O($b^2 * log(b)$)
  * N-th PWR O($b^2 * log(n)$)

## Největší společný dělitel (Greatest common divisor - gcd(x, y))

* Euklidův algoritmus O($b^3$)
* $\forall x,y \exists \alpha \beta : gcd(x, y)=\alpha x + \beta y$
* Kde $\alpha, \beta$ jsou Bezouts coeficients

**... tady jsem nebyl na přednášeče**

# RSA (Rivest, Shamir, Adleman)

* 1978 (MI6 ji znala už v roce 1973)

* Asymetrická šifra

* Klíč: p,q náhodná velká prvočísla

* n=p*q

* $\phi(n)=(p-1)(q-1)$ 

* e ... $\phi(n)$ - šifrovací exponent

* d ... ed = 1 mod $phi(n)$ - dešifrovací exponent

* E(x) = $x^e \mod n$

* D(y) = $y^d \mod n$

* Věta:

  $\forall x \in Z_n : D(E(x))=x$

  * Důkaz:

    $x^{ed}= x^{1+k\phi(n)}=x * (x^\phi(n))^k=x$

* Časová složitost: $\Omega(b^3)$ - $O(b^3)$
* Zrychlení
  1) Můžu jeden z exponentů zafixovat a dopočítat druhý - obvykle ten veřejný, protože privátní by byl snadno dopočitatelný
* Vlastnosti
  * Můžu prohodit e,d
  * Komutuje (protože exponenty exponentu je součin)
  * $E(x_1*x_2)=E(x_1)*E(x_2)$
* Blind signature
  * Alice 















