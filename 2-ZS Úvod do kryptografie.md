# Úvod

## Kryptografická primitiva

### Symetrická šifra

* Zpráva se šifruje i dešifruje stejným klíčem
* Typicky zachovává délku zprávy
* **Kerckhoffův princip**: tajný má být klíč, ne algoritmus

### Asymetrická šifra

* Zvlášť klíč na šifrování a dešifrování
* Obvykle jeden veřejný, druhý privátní

### Hešovací funkce

* $H_b: \{0,1\}^n → \{0,1\}^b$, kde například $b=256$
* Chceme, aby z výstupu nebyl odvoditelný vstup
* Nechceme kolize

### Náhodné generátory

* Stačí, aby výstupem bylo $\{0,1\}$
* Chceme nepredikovatelnost, neovlnivnitelnost

## Obecné vlastnosti dobrého protokolu

* **Padding** - všechny zprávy zarovnám na stejnou délku
* **Timestamp** - proti replikace
* **Nonce** - k planitextu přilepím náhodné číslo, aby útočník nemohl zprávy provonávat
* **Sessin id** - identifikace kanálu

## Útoky obecně

### Modely útoku

* Proti komu se bráníme
* Jak dlouho musí tajemství vydržet

### Typy útoku

* Chceme získat zašifrovaný text
* Chceme získat klíč

### Měření obtížnosti útoku

* Tzv. security level

## Jednorázové klíče



## Dělení tajemství



# Symetrické šifry

## Blokové šifry

### Césarova šifra

* Bloky o velikost 1, permutace je posun abecedy o klíč
* Problémy
  * Málo klíčů → snadný brute-force útok
  * Krátké bloky a žádná interakce mezi nimi → frekvenční analýza

### Vigenerova šifra

* Bloky o velikost délky klíče
* Používá 26 abeced, postupně se posouvám po textu a klíči

### Bezpečnost blokových šifer

* Neexistuje definice, obecně je dobrá šifra taková, kterou nelze efektivně odlišit od náhodné permutace
  * Verifikátor dostane text buď s $E_k$ pro náhodné $k$, nebo náhodnou permutaci a má odpovědět, co dostal
  * Chceme, aby pravděpodobnost úspěchu $\leq 2/3$ s lepší složitostí, než je $2^{scl}$, kde $scl=$ security level

### Konstrukce blokových šifer

#### Iterované šifry

![krypto_iterovaneSifry](C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_iterovaneSifry.jpg) 

#### Substitučně-permutační sítě (SPN)

- **S-Boxy** - malé invertibilní tabulky, které mění bity
- **Whitenig** - počáteční a koncový xor s klíčem (zamezuje útočníkovi kontrolu nad vstupy s-boxů)
- **P-Boxy** - obecná permutace na pozicích v bloku
- **Runda**
  - Na vstupu xor s částí klíče
  - Následně S-boxy
  - Následně P-Box

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_spn.jpg" alt="krypto_spn" style="zoom:50%;" /> 

### Feistelovy sítě

* Konstrukce s neinvertibilními S-boxy

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_faistelovySite.jpg" alt="krypto_faistelovySite" style="zoom: 67%;" /> 

### DES (Digital Encryption Standard)

#### Historie

* Vyvinut v 70. letech v IBM na základě požadavku NSB (National Bureau of Standarts)
* 56-bitový klíč (technicky 64, ale každý byte má paritní bit) → security level 56
* NSA na poslední chvíli vyměnila S-boxy

#### Struktura DES

* Feistlova síť s 16 rundami pracující s 32-bitovými půlbloky
* Navíc počáteční a koncový P-box (zbytečný)

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_des.jpg" alt="krypto_des" style="zoom:67%;" /> 

#### Problémy DES

* Existují slabé klíče
* Komplementarita $E_{\bar k}(\bar x) = \overline{E_k(x)} $
* Příliš krátké klíče (v roce 2012 možno prolomit za 26 hodin)
* Krátké bloky (kolize jednou za $2^{32}$ bloků)

#### Vylepšení

* 2-DES - nepomůže, security level 57
* 3-DES - 168bitový klíč, security level 112

### AES (Advanced Encryption Standard)

* Z roku 2001 (mezitím se NBS přejmenovat na NIST)
* 128bitové bloky, 128,192,256bitové klíče (10,12,14 rund)

#### Struktura AES

* Je to SPN s lineární transformací navíc
* Bytově orientovaná (kvůli efektivní implementaci)
* Stav je matice 4x4 bytů, předávaná mezi rundami
  * Rundový klíč má stejný tvar
* Před první rundou AddRoundKey

##### Runda

* ByteSub - byty proženeme identickými s-boxy
* ShiftRow - itý řádek rotujeme o i bytů doleva
* MixCol - na každý sloupec aplikujeme stejnou lineární (a invertibilní) transformaci
* AddRoundKey - xor s klíčem

##### Inverzní runda

* AddRoundKey
* InvMixCol
* InvShiftRow
* InvByteSub
* První a druhá dvojice komutují

#### Problémy a kritika

* Jednoduchá algebraická struktura (lze převést na řešení soustavy rovnic ... které zatím neumí nikdo (asi?) řešit)
* Příliš malá rezerva v počtu rund
* Zarovnává na byty
* Zatím se ale žádný rozumný útok neumí

### Padding

* Zarovnání všech šifrovaných zpráv na stejnou délku
* Chceme kontrolovat jeho správnost a strukturu? ... zdržuje, ale kdoví, co tam může být
* Různé možnosti
  * Dát na konec 0, číslo určující délku paddingu apod ...

### Šifrovací módy

#### ECB (Electronic Code Book)

* Nemá žádný IV
* Vynechání nebo prohození bloku není poznat

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_ecb.jpg" alt="krypto_ecb" style="zoom: 67%;" /> 

#### CBC (Cpiher Block Changing)

* Vyžaduje náhodný IV
* Změna bitu změní zbyte zprávy
* Lze se vyhnout padding tak, že zprávu doplním 0 a prohodím $Y_n$ a $Y_{n-1}$

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_cbc.jpg" alt="krypto_cbc" style="zoom:67%;" /> 

### Útoky na blokové šifry

#### Padding oracle attacks

* Příklad pro CBC s paddingem typu $p...p$ délky $p$

### Prosakování informací



## Proudové šifry

### LFSR

- Linear-Feedback shift registers

### Trivium

### RC4

### ChaCha20



### Šifrovací módy

#### CTR (Counter)

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_ctr.jpg" alt="krypto_ctr" style="zoom:67%;" /> 

#### CFB (Output feedback)

<img src="C:\Users\david.zeman\OneDrive\MFFUK\_notes\images\krypto_cbf.jpg" alt="krypto_cbf" style="zoom:67%;" /> 

# Hešovací funkce

