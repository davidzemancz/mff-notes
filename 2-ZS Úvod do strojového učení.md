# Úvod

* Definice (Mitchell 1997): Program se zlepšuje s časem, který řeší danou úlohu.
* Úloha (Task T)
  * Klasifikace - rozdělení vstupů do tříd
    * Každému vstupu přiřadíme celé číslo -> přímo ho klasifikujeme
    * Nebo také můžeme vrátit celé rozložení pravděpodobnosti
  * Regrese - výstupem je číslo
    * Každému vstupu přiřadíme reálné číslo
* Míra (Mesuare M)
  * U klasifikace jednoduše úspešnost
* Zkušenost (Experiance E)
  * S učitelem (supervised) - porovnávám výsledek programu se správnými výsledky
  * Bez učitele (unsupervised)
  * Zpětnovazební učení - dostanu zpětnou vazbu na výsledek
* Data generating distribution
  * Zdroj dat, na kterých se chceme učit, se správným ohodnocením
* Optimalizace -> snažím se na známých datech o co nejmenší chybu
* Strojové učení -> snažím se o co nejmenší chybu na neznámých datech

## Značení

* *a,* ***a**, **A***, A ~ skalár, vektor, matice, tenzor
* a, **a**, **A** ~ náhodný skalár, vektor, matice
* Derivace, parciální derivace, gradient
* Trénovací množina $X^{N*D}$, kde N je počet vzorků a D je počet featur
* Target množina $t^N$, kde $t_i$ je buď reálné číslo (regrese) nebo celé číslo z intervalu (u klasifikace)

# Lineární regrese

* https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html
* Nejjednodušší lineární model
* $y(x,w,b)=x^Tw + b$
* $Y=Xw$ kde $w=(w_1,w_2,...,b)$, tedy bias dám nakonec vektoru vah, abych s ním nemusel pracovat zvlášť
* Chci postupně upravovat váhy tak, abych minimalizoval chybu
* Použiji funkci MSE pro vyjádření chyby $MSE(w)=\frac{1}{2}\sum_i^N(x_i^Tw-t_i)^2=\frac{1}{2}||Xw-t||^2$
  * Správně je to 1/N, ale protože monotonie, tak tam dám 1/2, aby se mi to zkrátilo, až to budu derivovat
* Existuje explicitní řešení pro minimalizace MSE (což je dost neobvyklé)
  * Pro nalezení minimální chyby chceme, aby $\grad MSE(w)=0$, tedy minimalizuji podle $w$
  * $\frac{\partial}{\partial w_j}\frac{1}{2}\sum_i^N(x_i^Tw-t_i)^2 = \frac{1}{2}\sum_i^N2(x_i^Tw-t_i)x_{ij}=\sum_i^N(x_i^Tw-t_i)x_{ij}=0$
  * $X^T(Xw-t)=0$
  * Řešení metodou nejmenších čtverců $w=(X^TX)^{-1}X^Tt$
  * Předpokládá, že matice $X^TX$ je regulární. Pokud by byla singulární, je nutno použít SVD
* **Overfitting** - malá chyba na testovacích datech, ale velká chyba na trénovacích datech
* **Underfitting** - velká chyba všude
* Pokud mám jen jednu vstupní featuru, můžu na ní udělat nějaké nelineární operace (mocnění) a tím vytvořit více featur (tzv. **polynomial features**)
* Naopak pokud mám vstupní featuru, která mi reprezentuje den v týdnu, je vhodné ji převést do **one-hot**

## Algoritmus

* Vstup: $X^{N*D}, t^N$
* Výstup: $w^{D+1}$, +1 je za bias
* Kroky:
  * $w=(X^TX)^{-1}X^Tt$

## Regularizace

* Obecně jde o jakoukoliv úpravu modelu (nejen lineární regrese), jejímž cílem je snížení chyby
* Například omezení kapacity modelu (zabraňuje overfittingu)

### L2 regularizace

* Snaží se, aby $w$ byly co nejmenší
* Neaplikuje se na bias, musím ho tedy uvažovat zvlášť
  * Pak je invariantní vůči tomu, když např. ke všem vstupům přičtu konstantu
* $MSE(w)=\frac{1}{2}\sum_i^N(x_i^Tw-t_i)^2+\frac{\lambda}{2}\norm{w}^2=\frac{1}{2}||Xw-t||^2+\frac{\lambda}{2}\norm{w}^2$
* Explicitní se pak liší jen přičtením jednotkové matice
  * $w=(X^TX+\lambda I)^{-1}X^Tt$
  * Mimochodem, matice $(X^TX+\lambda I)$ pro $\lambda > 0$ je vždy regulární

### Hyperparametry

* Jde o hodnoty, které nejsou součástí vstupních dat, ale upravují chování modelu
* Právě například regularizační parametr $\lambda$, stupeň polynomial features atd...

## SGD

### Náhodné veličny

* **Střední hodnota** (expectantion) funkce $f(x)$ je v podstatě vážený průměr
  * $\mathbb{E}_{x\sim P}[f(x)]=\sum_xP(x)f(x)$, kde $P(x)$ je pravděpodobnostní rozložení
* **Rozptyl** (variance) udává, jak se průměrně hodnoty liší od průměru
  * $Var(f(x))=\mathbb{E}[(f(x) - \mathbb{E}[f(x)])^2]$
* **Bias** (jiný bias, než ten, který se přičítá v LR)
  * Máme **estimator** (odhadce), pak $bias = \mathbb{E}[estimate] - true \space estimated \space value$
  * Pokud je $bias=0$ je estimator **unbiased**, jinak **biased**

### Gradient descent

* Obecná metoda pro hledání "minim" funkcí
* $w \leftarrow w - \alpha \grad_w E(w)$, kde $\alpha$ je tzv. **learing rate** (další hyperparametr) a $E(w)$ je obecně chybová funkce
* Definuji $\grad_w E(w)=\grad_w \mathbb{E}_{(x,t)}[L(y(x,t),t)]$
* **Standart gradient descent**
  * K výpočtu využiju všechna vstupní data, pak dostanu $\grad_w E(w)$ přesně
* Stochastic gradient descent 
  * K výpočtu gradientu použiju náhodné vstupní dato, tedy 
  * $\grad_w\mathbb{E}(w) \approx \grad_wL(y(x,w),t)$ pro náhodně vybrané $(x,t)$
* **Minibatch SGD**
  * K výpočtu gradientu použiju několik (batch) náhodně vybraných dat 
  * $\grad_w\mathbb{E}(w) \approx \frac{1}{B}\sum_i^B \grad_wL(y(x_i,w),t_i)$ 
* Řešení bude **konvergovat** k optimu za těchto podmínek
  * Chybová funkce $L(y(x,w),t)$ je **konvexní** a **spojitá**
  * Posloupnost learning rates $(\alpha_i)_i$ splňuje:
    * $\forall i: \alpha_i > 0$ ,  $ \sum_i \alpha_i = \infty$ , $ \sum_i \alpha_i^2 < \infty$, tedy $\alpha_i \rightarrow 0$
  * Pokud by byla $L$ nekonvexní, konverguje SGD pouze do lokálního minima

## SGD pro lineární regresi

* https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDRegressor.html
* Pro standartní GD: $E(w)=\mathbb{E}_{(x,t)}[\frac{1}{2}(x^Tw-t)^2] + \frac{\lambda}{2}\norm{w}^2$
* Pro Minibatch SGD: $E(w)=\frac{1}{b}\sum_i^b\frac{1}{2}(x_i^Tw-t_i)^2 + \frac{\lambda}{2}\norm{w}^2$
  * $\grad_wE(w)=\frac{1}{b}\sum_i^b((x_i^Tw-t_i)x_i) + \lambda w$

### Algoritmus

* Vstup: $X^{N*D}, t^N$, $\alpha \in \mathbb{R}^+, \lambda \in \mathbb{R}$
* Výstup: $w^{D}$, které minimalizují MSE (doufejme)
* Kroky:
  * $w ← 0$
  * Dokud jsem nezkonvergoval
    * Vyberu náhodně minibatch z dat velikosti $b$
    * $w ← w - \alpha\grad_wE(w)$, kde $\grad_wE(w)=\frac{1}{b}\sum_i^b((x_i^Tw-t_i)x_i) + \lambda w$

## Cross-validace

* https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html
* Pro vhodné nastavení hyperparametrů, preprocessing nebo výběr modelu potřebuji průběžně ověřovat, jak je můj model dobrý na datech, která nezná
* Také chci trénovat vždy na trochu jiných datech, aby byl model "objektivnější"
* K tomu použiju tzv. **k-fold cross validation**, kdy si trénovací množinu rozsekám na k částí
* Na k-1 částech natrénuji model s nějakým nastavením a na té poslední udělám validaci (tj. predikci)

# Klasifikace

## Perceptron

* Klasifikace do dvou tříd
* **Accuracy** (přesnost) budeme měřit jako poměr správně/špatně klasifikovaných vzorků
* Pro převedení lineární regrese na binární klasifikaci se určí **treshold**, který odděluje dvě třídy (obvykle 0)
* Předpokládáme, že data jsou lineárně separabilní (pokud ne, nebude existovat vhodný vektor $w$)
* Vektor $w$ je kolmý na dělící nadrovinu (protože pro dva body na ní platí $(x_1-x_2)^Tw=0$)
* **Vzdálenost vzorku x** od dělící nadroviny
  * Budˇ $x_\bot$ projekcí na nadrovinu
  * Pak $x= x_\bot+\frac{rw}{\norm{w}}$
  * Tedy $r = \frac{x^Tw}{\norm{w}}=\frac{y(x)}{\norm{w}}$

* **Nevýhoda**: najde nějakou dělící přímku, tedy muže být "blízko" jedné množiny a "daleko" od druhé

### Algoritmus

* Vstup: $X^{N*D}, t \in {-1;1}^N$, kde $X$ jsou lineárně separabilní
* Výstup: $w^{D}$, že $t_ix_iw>0$ (resp. $sign(x_i^Tw)=t_i$)
* Kroky:
  * $w ← 0$
  * Dokud nejsou všechny vzorky správně klasifikované
    * Pokud $t_ix_i^Tw\leq 0$
      * $w ← w + t_ix_i$
  
* Vlastně jde o **online GD** algoritmus, kde chybová funkce je $L(y(x,w),t)=ReLU(-tx^Tw)=max(0,-tx^Tw)$
  * Není nutno používat learing rate, neboť násobení vah konstantou nezmění predikce
* Algoritmus **konverguje**, tedy pro lineárně separabilní data vždy najde dělící přímku

## Logistická regrese

### Distribuce

#### Bernouliho distribuce

* $x \in \{0,1\}, \phi \in (0,1)$
* $P(x)=\phi^x(1-\phi)^x$ ... tj. buď $\phi$ nebo $(1-\phi)$

#### Kategorická distribuce

* $x \in \{0,1\}^k, p \in (0,1)^k, \sum_i^k p_i=1$
* $P(x)=\prod_i^kp_i^{x_i}$

#### Normální distribuce

* Suma náhodných pokusů s konečným rozptylem k ním konverguje
* Jde o distribuci s maximální entropií  

### Entropie

* **Surprise**, **Self-information** (míra překvapení)
  * Pro jisté jevy (Pr=1) je nulová
  * Roste pro méně pravděpodobné jevy
  * Pro nezávislé jevy musí být aditivní (tj. když mě překvapí dvě věci, tak se překvapení sečtou)
  * $I(x)=-\log{P(x)}=\log{\frac{1}{P(x)}}$
* **Entropie** je pak střední hodnota self-information v celém rozložení (distribuci)
  * $H(P)=\mathbb{E}_{x\sim P}[I(x)]=-\mathbb{E}_{X \sim P}[\log{P(x)}]$
  * $H(P)=-\sum_xP(x)\log{P(x)}$
  * **Motivace**
    * Chci posílat zprávy kanálem, kudy chodí 0,1
    * Mám pravděpodobnosti jednotlivých znaků a místo, které zabírají
    * Pak průměrný počet bitů na znak je $\sum_xP(x)S(x)$, kde $P$ je pravděpodobnost a $S$ je velikost
    * Tedy entropie mi říká, kolik bitů potřebuji, abych mohl reprezentovat výsledek náhodného pokusu (nejen to, platí pro $\log_2$)
      * Je to střední hodnota "self-information"
* **Corss-Entropie**
  * Mám dvě distribuce, $P,Q$
    * K motivační příkladu: znaky mi chodí z $P$, ale kód jsem postavil na znacích z $Q$
  * $H(P,Q)=-\mathbb{E}_{X \sim P}[\log Q(x)]$
* **Gibbsova nerovnost**
  * $H(P,Q)\geq H(P)$
  * $H(P,Q) = H(P) \iff P=Q$
  * ... vypadá to skoro jako metrika, ale není to metrika, protože obecně neplatí $H(P,Q)=H(Q,P)$
* Pro kategorickou distribuci o $n$ prvcích platí $H(P)=\log n$ (u motivačního příkladu bych každému prvku přiřadil sekvenci o $\log n$ bitech)
* **K čemu to a co chceme?**
  * Máme definovanou cross-entropii $H(P,Q)$
  * Klasifikační model vrátí distribuci ... a já chci, aby se co nejvíc blížila distribuci v datech
  * Takže chci spočítat cross-entropy (nejlépe spojitá funkce, kterou pak zderivuji)
* Obecně je rozložení s **největší entropií** to nejobecnější → a to je právě normální rozložení

### KL divergence

* $D_{KL}(P||Q)=H(P,Q)-H(P)=\mathbb{E}_{x\sim P}[\log P(x) - \log Q(x)]$
* Budeme na ní měřit, jak je distribuce $Q$ daleko od $P$ ... tedy jak moc jsem k ním blízko
* Nenulová
* Nesymetrická, protože $H(P,Q)\neq H(Q,P)$
  * Záleží, přes co kterou distribuci se počítá střední hodnota
  * V praxi pak $P$ budou data a $Q$ predikce
    * → chceme, aby se predikce s daty shodovala tam, kde data máme ... nezajímá nás, co predikce predikuje tam, kde data nemáme

### MLE

* Maximum likelihood estimation
* https://www.youtube.com/watch?v=I_dhPETvll8
* **Empirické rozložení dat** z množiny $X=\{x_1,...x_N\}$ je $\hat p_{data}(x) = \frac{|\{i:x_i=x\}|}{N}$
* **Likelihood** je pak $L(w)=p_{model}(X,w)=\prod_i^Np_{model}(x_i,w)$, kde $p_{model}(x,w)$ je množina různých rozložení
  * Zafixoval jsem x - to je trénovací množina
  * Nabývá hodnoty z <0,1> (to je likelihood)
* **Maximum likelihood estimation** pro $w$ je pak 
  * $w_{MLE}=\arg \max_w p_{model}(X,w)=\max_w \prod_i^Np_{model}(x_i,w)$ (je to součin pravděpodobností, protože data jsou nezávislá)
  * $w_{MLE}=\arg \min_w \sum_i^N-\log p_{model}(x_i,w)$ → **Negative Log Likelihood**
  * $w_{MLE}=\arg \min_w \mathbb{E}_{x \in p_{data}}[-\log p_{model}(x_i,w)]$ (přidám si $1/N$, minimum to neovlivní)  → to je **cross-entropy**
  * $w_{MLE}=\arg \min_w H(p_{data}(x),p_{model}(x,w))$ ($\pm H(p_{data}(x))$)
  * $w_{MLE}=\arg \min_w D_{KL}(p_{data}(x)||p_{model}(x,w))$ (navíc ještě $+ H(p_{data}(x))$, ale to je konstanta, takže nemá na minimum vliv)
* MLE pak lze zobecnit pro případ, kdy sis pro dané $x$ předpovídat $t$
  * $w_{MLE}=\arg \max_w p_{model}(t|X,w)=\max_w \prod_i^Np_{model}(t_i|x_i,w)$
  * $w_{MLE}=\arg \min_w \sum_i^N-\log p_{model}(t_i|x_i,w)$ ... NLL (negative log likelihood)
  * $w_{MLE}=\arg \min_w H(\hat p_{data},p_{model}(t|x,w))$
* MLE je **konzistentní**, tj. s rostoucím počtem dat konverguje ke skutečnému rozložení dat
* MLE má také **nejmenší MSE**

### Binární klasifikace

* Funkce **sigmoid** $\sigma(x)=\frac{1}{1+e^{-x}} : \mathbb{R} → [0,1]$
  * Chápeme ji jako pravděpodobnost pozitivního výsledku
  * **Derivace sigmoid** $\sigma´(x)=\sigma(x)(1-\sigma(x))$

* $P(C_1|x)=\sigma(x^Tw+b)$
* $P(C_0|x)=1-P(C_1|x)$
* Pokud má platit  $y(x,w)=P(C_1|x)=\frac{1}{1+e^{\hat y(x,w)}}$, tak musí $\hat y(x,w)=\log(\frac{P(C_1|x)}{1-P(C_1|x)})=\log(\frac{P(C_1|x)}{P(C_0|x)})$
  * To se jmenuje **logit** - ta část, která je parametr nelineární funkce (tj. lineární část modelu)
    * Co je logit? Logaritmus poměru pravděpodobnost. také inverz sigmoidu
* Použiji chybovou funkci **NLL** $E(w)=\frac{1}{N}\sum_i-\log(p_{model}(C_{t_i}|x_i,w)$
* Algoritmus opět používá SGD

#### Algoritmus

* Vstup: $X^{N\times D}, t \in \{0,1\}^N$, learing rate $\alpha \in \mathbb{R}^+$
* Kroky
  * $w ← 0$
  * Dokud jsem nezkonvergoval
    * $g ← \frac{1}{b} \sum_i^b \grad_w -\log(P(C_{t_i}|x_i,w)) = \frac{1}{b}\sum_i^b((x_i^Tw-t_i)x_i)$$
    * $w ← w - \alpha g$

### Odvození MSE z MLE

* Pokud náš model generuje normální rozložení, pak lze z ML odvodit MSE

### Klasifikace do více tříd

* Klasifikátor se jmenuje multinomial logistic regression nebo maximum entropy classifier nebo softmax regression  
* Místo do dvou chceme klasifikovat do *K* tříd
* Budeme generovat *K* výstupních parametrů, pro každý bude existovat sada vah → $W \in \mathbb{R}^{D\times K}$
  * $\hat y(x,W)=x^TW$, tedy $\hat y(x,W)_i=x^TW_{*i}$
* Zobecním funkci sigmoid na **softmax**
  * $softmax(z)_i=\frac{e^{z_i}}{\sum_je^{z_j}}$
  * Pak $P(C_i|x,W)=y(x,W)_i=softmax(\hat y(x,W))_i=softmax(x^TW)=\frac{e^{(x^TW)_i}}{\sum_j e^{(x^TW)_j}}$
  * Kde $\hat y(x,W)_i=\log (P(C_i|x,W)) + c$
    * Na konstantě $c$ nezáleží, protože ze softmaxu můžu vytknout $\frac{e^c}{e^c}=1$, tedy $softmax(z+c)_i=softmax(z)_i$

#### Algoritmus

* Vstup: $X^{N\times D}, t \in \{0,1,...K-1\}^N$, learing rate $\alpha \in \mathbb{R}^+$
  * $w$ reprezentuje jak matici vah $W$, tak eventuálně bias $b$
* Kroky
  * $w ← 0$
  * Dokud jsem nezkonvergoval
    * $g ← \frac{1}{b} \sum_i^b \grad_w -\log(P(C_{t_i}|x_i,w)) = \frac{1}{b}\sum_i^b((x_i^Tw-1_t)x_i^T)$$
    * $w ← w - \alpha g$

#### Regiony

* Všechny regiony tříd při klasifikaci do k-tříd jsou **konvexní**

## F1-score

* **accuracy** $=\frac{TP + TN}{TP+TN+FP+FN}$
* **precision** $=\frac{TP}{TP+FP}$
* **recall** $=\frac{TP}{TP+FN}$
* **F1** $=\frac{2}{precision^{-1}+recall^{-1}}=\frac{TP+TP}{TP+FP+TP+FN}$
* Také je nutné použít vhodný průměr
  * Aritmetický $AM(p,r)=\frac{p+r}{2}$
    * $AM(100,1)=50.5$
  * Geometrický $GM(p,r)=\sqrt{p*r}$
    * $AM(100,1)\approx 10$
  * Harmonický $HM(p,r)=\frac{2}{\frac{1}{p}+\frac{1}{r}}$
    * $AM(100,1)\approx 2$
* F1 skóre může být zobecněno na $F_\beta$, kde recall je $\beta$-krát důležitější než precision
  * $F_\beta=\frac{1+\beta^2}{precision^{-1}+\beta^2 * recall ^{-1}}$

# MLP

* Obecně reprezentuje lineární modely
* Multilayer perceptron (vícevrstvý perceptron)

## Bez skryté vrstvy

* Mám *D* vstupních vrcholů pro featury
* Mám *K* výstupních vrcholů (v případě regrese $K=1$, jinak počet tříd-1)
* Každý vstupní vrchol je propojený se všemi výstupními a hrany jsou ohodnoceny váhami
* Hodnota výstupního vrcholu $y_i=a(\sum_jx_jw_{ij}+b_i)$, kde $a$ je aktivační funkce

## Se skrytou vrstvou

* Navíc uvažuji skrytou vrstvu *h* s aktivační funkcí *f*
* $h=f(x^TW^{(h)}+b^{(h)})$
* $y=a(h^TW^{(y)}+b^{(y)})$

## Aktivační funkce výstupní vrstvy

* Lineární regrese - identita nebo $exp(x)$ pro Posionovksou regresi
* Binární klasifikace - sigmoid $\sigma(x)$
* Klasifikace do více tříd - $softmax(x)$

## Aktivační funkce skryté vrstvy

* Žádná - pak skrytá vrstva nepomůže, model zůstane pořád stejný (zachová se linearita)
* $tanh(x)$ - symetrický sigmoid
* $ReLU(x)=max(0,x)$ - používá se dnes

## Algoritmus trénovaní MLP pomocí SGD

* Vstup: $X^{N\times D}, t^N$, learing rate $\alpha \in \mathbb{R}^+$
* $w$ reprezentuje jak váhy $W$, tak bias $b$
* Kroky:
  * Inicializace $w$
    * Váhy nastavím náhodně z $\left< -1/\sqrt{M};1/\sqrt{M} \right>$, kde $M$ je velikost vrstvy
    * Biasy nastavím na 0
  * Dokud jsem nezkonvergoval
    * $g ← \frac{1}{b} \sum_i^b \grad_w -\log(P(t_i|x_i,w))$$
    * $w ← w - \alpha g$

## Universal approximation theorem

Buď $\phi(x) : \mathbb{R} → \mathbb{R}$ rostoucí, spojitá a omezená. Pak $\forall \epsilon >0$ a spojitou funkci $f:\left<0,1\right>^D→ \mathbb{R}$ existuje $H \in \mathbb{N}, v\in \mathbb{R}^H, b \in \mathbb{R}^H, W \in \mathbb{R}^{D\times H}$, že pro funkci $$F(x)=v^T\phi(x^TW+b)$$ platí $\forall x \in \left<0,1\right>^D : \abs{F(x)-f(x)}<\epsilon$.

# K-nearest neigbors

* Výsledek určí podle "nejbližších" známých prvků v trénovací množině
* Jednoduchý, ale často efektivní algoritmus jako pro klasifikaci, tak pro regresi
* V algoritmu v podstatě není trénovací fáze, vše probíhá až při predikci
* Hyperparametry
  * **k** - hledaný počet sousedů
  * **metrika** - jak najít nejbližší sousedy (obvykle $L_p$ norma, kde $d(x,y)=\norm{x-y}_p=\sqrt[p]{\sum_i\abs{x_i}^p}$)
  * **váhy**
    * Uniformní - všech k sousedů má stejnou váhu
    * Inversní - $\frac{1}{d(x,y)}$
    * Softmax
* Výsledek $t=\sum_i^k\frac{w_i}{\sum_j^k w_j}t_i$

# Kernel metody

## Kernel lineární regrese (SGD)

* Cíl: zrychlení SGD (i lineární regrese) pro data, kde potřebujeme polynomial features
* Vyjádřím $w$ jako lineární kombinaci vstupních vektorů s polynomial features
  * $w = \sum_i \beta_i \phi(x_i)$, kde $\phi : \mathbb{R}^D → \mathbb{R}^{D^p}$
* Upravím SGD update
  * $w ← w - \frac{\alpha}{b}\sum_i^b(\phi(x_i)^Tw-t_i)\phi(x_i) ← \sum_i^b(\beta_i-[i \in b]\frac{\alpha}{b}(\phi(x_i)^Tw-t_i))\phi(x_i)$
  * Tedy pro $i \in \{1,...,b\}$ se upraví $\beta_i=\beta_i-\frac{\alpha}{b}(\sum_j^n(\beta_j\phi(x_i)^T\phi(x_j))-t_i)$

### Algoritmus

* Vstup: $X^{N\times D}, t^N$, learing rate $\alpha \in \mathbb{R}^+$
* Kroky:
  * $\beta_i=0$
  * $K_{ij}=\phi(x_i)^T\phi(x_j)$
  * Dokud jsem nezkonvergoval:
    * Tedy pro $i \in \{1,...,b\}$ se upraví $\beta_i=\beta_i-\frac{\alpha}{b}(\sum_j^n(\beta_jK_{ij})-t_i)$
* Predikce: $y(z)=\phi(z)^Tw=\sum_i\beta_i\phi(z)^T\phi(x_i)$
* **Bias**: průměr přes trénovací data

### Kernel trick

* $\phi(x)^T\phi(y)=1+x^Ty+(x^Ty)^2+...+(x^Ty)^p$

## Kernely

* Polynomiální kernel stupně alespoň $d$ $K(x,y)=(\gamma x^Ty)^d$
* Polynomiální kernel stupně alespoň $d$ $K(x,y)=(\gamma x^Ty +1)^d$
* Gaussian radial basis kernel (RBF) $K(x,y)=e^{-\gamma\norm{x-y}^2}$
  * Odpovídá "nekonečným" polynomial features

# SVN

* Support vector machine
* Cílem je při binární klasifikaci najít takovou dělící nadrovinu, která je maximálně vzdálená od obou skupin (**maximum margin**)
  * Vzdáleností rozumíme normu rozdílu prvku skupiny a jeho projekce na dělící nadrovinu → chceme ji maximalizovat pro obě skupiny
* Vzdálenost bodu od dělící nadroviny $r=\frac{\abs{y(x_i)}}{\norm{w}}$
  * Chceme tedy ${\arg\max}_{w,b}\frac{1}{\norm{w}}\min_i[t_i(\phi(x_i)^Tw+b)]$ za předpokladu $\frac{\abs{y(x_i)}}{\norm{w}} = \frac{t_iy(x_i)}{\norm{w}}$, což platí, pokud jsou všechny vzorky správně klasifikovány
* Protože model je invariantní k násobení $w,b$, můžu předpokládat, že nejbližší bod dělící nadroviny bude splňovat $t_iy(x_i)=1$
  * S předpokladem $t_iy(x_i)\geq1$ můžu upravit ${\arg\min}_{w,b}\frac{1}{2}\norm{w}^2$
    * Použiju větu o Lagrangeových multiplikátorech
* K řešení SVM se obvykle používá **SMO** (Sequential Minimal Optimization) agoritmus

## Coordinate descent agloritmus

* Chceme řešit $\arg\min_w L(w_1,...,w_D)$
* Kroky
  * Dokud jsem nezkonvergoval
    * $\forall i \in \{1,...,D\}$ nastav $w_i← \arg\min_{w_i} L(w_1,...,w_D)$

## SMO algoritmus

* https://en.wikipedia.org/wiki/Sequential_minimal_optimization

# Strojové zpracování textu

* **Term** - slovo, obecně sekvence znaků
* Obvykle máme featuru pro každý term. Její hodnotu lze reprezentovat různě.
  * Binárně - 1/0, zda se term v dokumentu vyskytuje
  * Frekvence (TF) - $TF(t,d)=\frac{ \abs{ \{ t : t \in d \} }}{\abs{d}}$
  * Inverse document frequency (IDF) - $IDF(t)=\log \frac{\# \space docs}{\# \space docs \space containing \space t } = I(P(t \in d))$
    * Vyjadřuje, jak "unikátní" slovo je
      * Například "a" bude téměř v každém a často, zatímco "mastodont" je unikátnější
  * Nejčastěji se používá **TF-IDF** $=TF \cdot IDF$

## TD-IDF

* Podmíněná entropie $H(Y|X)=\mathbb{E}_{x,y}[I(y|x)]==\mathbb{E}_{x,y}[-\log P(y|x)]\sum_{x,y}P(x,y)P(y|x)$
* Pak $I(x,y)=H(Y)-H(Y|X)=\mathbb{E}_{x,y}[\frac{P(x,y)}{P(x)P(y)}]$ je **mutal information**
  * Je symetrická $H(Y)-H(Y|X)=H(X)-H(X|Y)$
  * Je nezáporná $I(X,Y)\geq0$
  * Pokud jsou $X,Y$ jsou stejné (tj. náhodné veličiny jsou nezávislé $P(X,Y)=P(X)P(Y)$), pak je nulová
* Odvození TD-IDF z mutal information
  * Buď $D$ množina dokumentů velikost $N$ a $T$ množina termů. Dokumenty vybírám uniformě.
  * Pak $P(d)=1/N$ a $I(d)=H(d)=\log N$
  * $P(d|t)=1/\abs{\{d \in D : t \in d \}}$ a $I(D|T = t)= \log \abs{\{d \in D : t \in d \}}$
  * $I(d)-I(d|t)=H(D)-H(D|T=t)=\log \frac{N}{\abs{\{d \in D : t \in d \}}}=IDF(t)$
    * Aneb kolik self-ifnormation se dozvím o dokumentu, když v něm je nějaký term - relevance mezi dokumentem a termem

## Bayesovská pravděpodobnost

* Pravděpodobnost uvažuje jako míru nejistoty
* $P(B|A)=\frac{P(A|B)P(B)}{P(A)}$
* **Vsuvka:** Franta je spořádaný a klidný člověk, která má rád pořádek. Je spíš knihovník, nebo farmář?
  * Ačkoliv popis sedí spíš na knihovníka, musíme uvážit, že farmářů je zhruba 30krát více
* $p(w|X)\propto p(X|w)\cdot p(w)$, kde:
  * $p(w|X)$ - posterior
  * $p(X|w)$ - likelihood
  * $p(w)$ - prior 
* Pak **Maximum a posterior** $w_{MAP}=\arg\max_w p(w|X)=\arg \max_w p(X|w)p(w)$

# Naive Bayes klasifikátor

* $p(C_k|x)=\frac{p(x|C_k)P(C_k)}{p(x)}$
* Pak $\arg \max_k p(C_k|x)= \arg \max_k \frac{p(x|C_k)P(C_k)}{p(x)}= \arg \max_kp(x|C_k)P(C_k)$ protože $p(x)$ je konstantní (data jsou dána)
* Předpokládá, že všechny features jsou nezávislé, když už znám třídu (což není moc pravda, proto naive)
  * Kdybych to nepředpokládal, tak $p(x|C_k)=p(x_1|C_k)\cdotp(x_2|C_k,x_1)\cdotp(x_3|C_k,x_1,x_2),...$ ... podle dimenze dat
  * Ale s předpokladem $p(x|C_k)=\prod_d^Dp(x_d|C_k)$ ... distribuce v jedné dimenzi
* Klasifikátorů je více, podle toho, jak modelují $p(x_d|C_k)$

## Gaussian

* Předpokládá, že $p(x_d|C_k)$ má normální rozdělení, tedy $\mathcal{N}(\mu_{d,k},\sigma^2_{d,k})$
  * Potřebuji si tedy určit střední hodnotu $\mu_{d,k}$ a rozptyl $\sigma^2_{d,k}$ pro $1\leq d \leq D, 1\leq k \leq K$ (tj. pro všechny rysy a třídy) a použiji na to MLE
* Predikce
  * Vím, že $p(x|C_k)=\prod_d^Dp(x_d|C_k)$
  * Každou $p(x_d|C_k)$ spočtu z hustoty normálního rozdělení
  * Nakonec určím třídu podle $\arg \max_k p(C_k|x)= \arg \max_kp(x|C_k)P(C_k)$, tedy tu, která má maximální součin prioru a likelihoodu
* Jak odhadnout parametry $\mu_{d,k}, \sigma^2_{d,k}$? Zafixuji si $d,k$, vezmu množinu vstupních dat třídy $k$ jako $x_1,...x_{N_k}$
  * Mám teda data a (podle předpokladu) jsou z normálního rozdělení → a já chci zjistit, jaké rozdělení to bylo
  * Udělám to prostě tak, že na datech $x_1,...x_{N_k}$ spočítám střední hodnotu a rozptyl (koho by to překvapilo?)
* Může se stát, že rozptyl bude malý - pak ho prostě zvětším o konstantu

## Bernoulli

* Binární rysy
* $p(x_d|C_k)=p_{d,k}^{x_d}(1-p_{d,k})^{1-x_d}$
* $p(C_k|x)\propto (\prod_d^D p_{d,k}^{x_d}(1-p_{d,k})^{1-x_d})p(C_k)$ 
  * Zlogaritmuji → $\log p(C_k|x)=\sum_d^D(x_d\log\frac{p_{d,k}}{1-p_{d,k}}+\log(1-p_{d,k}))=b_k+x^Tw_k$
  * Tedy predikce nakonec vypadá jako lineární logistická regrese, ale model se trénuje jinak
* Jak odhadnout pravděpodobnosti $p_{d,k}=\frac{\text{number of focs of class k with feature d}}{\text{number of docs of class k}}$
  * Snadno se ale stane, že $p_{d,k}=1$ nebo $p_{d,k}=0$
  * Použije se vyhlazování $p_{d,k}=\frac{\text{number of focs of class k with feature d} + \alpha}{\text{number of docs of class k}  + 2\alpha}$ pro pseudo-count $\alpha > 0$

## Multinomial

* Odpovídá počtům něčeho, které soutěží mezi sebou
* Hodí se pro TF-IDF

## Generativní a diskriminativní modely

* **Diskriminativní modely** modelují podmíněnou pravděpodobnost $p(t|x)$
  * Vytvoří dělící nadrovinu
  * Obvykle fungují lépe, protože určit dělící nadrovinu je snažíš, než popsat celou oblast
  * Obecně, aby se "efektivně" natrénoval, potřebuje $\Omega(D)$ trénovacích dat
* **Generativní modely** modelují sdruženou pravděpodobnost $p(t,x)$ s pomocí Bayesovy věty odhadují $p(x|t)\cdotp(t)$
  * Vytvoří oblasti, které popisují daná data
  * Umí generovat data → třeba i obrázek (psa například)
  * Obecně, aby se "efektivně" natrénoval, potřebuje $\Omega(\log D)$ trénovacích dat

# Rozhodovací stromy

## Kovariance

* **Linearita střední hodnoty** $\mathbb{E}[\sum_ix_i]=\sum_i\mathbb{E}[x_i]$
* **Rozptyl** $Var(\sum_ix_i)=\mathbb{E}[(\sum_ix_i-\sum_i\mathbb{E}[x_i])^2]=...=\sum_i\sum_j\mathbb{E}[(x_i-\mathbb{E}[x_i])(x_j-\mathbb{E}[x_j])]$
* **Kovariance** $cov(x,y)=\mathbb{E}[(x-\mathbb{E}[x])(y-\mathbb{E}[y])]=\mathbb{E}[xy]-\mathbb{E}[x]\mathbb{E}[y]$
  * Rozptyl je tedy součet všech možných kovariancí
  * $cov(x,x)=Var(x)$

## Korelace

* Dvě náhodné veličiny jsou nekorelované, pokud $c(x,y)=0$, jinak jsou korelované
* Nezávislé veličiny → nekorelované. Ale neplatí nekorelované → nezávislé.

### Pearsonův korelační koeficient

* Značí se $r$ nebo $\rho$
* $\rho = \frac{cov(x,y)}{\sqrt{Var(x)Var(y)}}$ ... používá se při výpočtu z definic
* $r = \frac{\sum_i(x_i-\mathbb{E}[x])(y_i-\mathbb{E}[y])}{\sqrt{\sum_i(x_i-\mathbb{E}[x])^2}\sqrt{\sum_i(y_i-\mathbb{E}[y])^2}}$ ... používá se při výpočtu ze vzorku dat
* Platí, že $-1 \leq \rho \leq 1$
* Vyjadřuje vzájemnou lineární závislost hodnot

### Spearmanův korelační koeficient

* Značí se také $\rho$
* Data si setřídím a očísluji → **ranky** a spočítám Pearsonův korelační koeficient na nich

### Kendal rank korelační koeficient

* Značí se $\tau$
* $\tau=\frac{\sum_{i<j}sign(x_j-x_i)sign(y_j-y_i)}{n \choose 2}$ ... počet dovjic, které oboje stoupají nebo klesají dělený počtem všech

## Kombinování modelů

* Ensembling
* Motivací je dosáhnout většího výkonu
* Každý musím natrénovat jinak a jejich předpovědi zprůměruji v případě regrese (hard voting)
  * V případě klasifikace, pokud model vrací třídy, udělám voting. Pokud model vrací distribuci, udělám průměry. (soft voting)
* Myšlenka: pokud mají modely **nekorelované chyby**, tak při více modelech se navzájem vyruší
* Pro MLP snadné - stačí náhodně inicializovat a modely se obvykle chovají jinak, protože chybová funkce má mnoho lokálních minim

### Bagging (Bootstrap aggregation)

* Pro každý model vytvoříme nový dataset (tedy, z původního datasetu vyberu vždy pro jednom prvku pro každý)

## Rozhodovací stromy

* Obvykle rozdělím prostory na krychle a všem objektům v jedné krychli přiřadím hodnotu
* Typy vrcholů
  * List - predikovaná hodnota (resp. průměr přes všechny hodnoty v listu)
    * $I_T$ - množina všech dat, která list reprezentuje
    * $\hat t_T=frac{1}{\abs{I_T}}\sum_{i \in I_T}t_i$ - hodnota listu, tj. predikce
  * Vnitřní vrchol - dvě hodnoty
    * Klasifikační rys
    * Hodnota, kdy všechno menší jde doleva, všechno větší jde doprava

### Regrese

#### Trénovaní

* Na začátku mám jeden vrchol, který obsahuje všechny cílové hodnoty (→ predikce bude jejich průměr)
* **Kritérium** - říká nám, jakým způsobem list rozdělit
  * $c_{SE}=\sum_{i\in I_T}(t_i-\hat t_T)^2$ ... součet čtvercových chyb, ne průměr (aby reprezentoval, kolik prvků v listu je)
    * $\hat t_T=\frac{1}{\abs{I_T}}\sum_{i \in I_T}t_i$
* **Dělení listů**
  * Hrubou silou, vyzkouším všechny features a všechny hodnoty
  * Budu minimalizovat kritéria podstromů, resp. minimalizovat $c_{T_L}+c_{T_R}-c_{T}$ (když mám více vrcholů)
    * Chci tedy maximalizovat pokles neuspořádanosti 

#### Parametry trénování

* Minimální počet vzorků v listu k rozdělení - regularizace
* Maximální hloubka stromu - omezuje kapacitu modelu
  * Trénuji rekurzivně
* Maximální počet listů- také omezuje kapacitu modelu
  * Musím vhodně vybrat list, který štěpit

### Klasifikace

* Pravděpodobnost, že prvek je třídy $k$ v regionu $T$ je $p_T(k)=\frac{\abs{\{i \in I_T : i =k\}}}{\abs{I_T}}$ 
* **Kritéria**
  * **Gini index** $c_{Gini}(T)=|I_T|\sum_kp_T(k)(1-p_T(k))$
    * Lze jej odvodit z MSE
  * **Entropy criterion** $c_{entropy}(T)=|I_T|\cdot H(P_T)=-|I_T|\sum_{k :p_T(k)\ne0}p_T(k)\log p_T(k)$
    * Lze jej odvodit z NLL

 ## Random forest

* Natrénuji více stromů a nakonec udělám voting
* 2 triky
  * Použiji bootstraping a bagging, abych každý strom natrénoval různě
  * Při dělení listu vyberu náhodně $D/2$ rysů, které budu používat (a zbytek nesmím)

## Gradient boosted decision trees

*  Stromy trénuji sekvenčně, přičemž každý další strom se snaží opravovat chyby těch předchozích
* Predikce: $y(x_i,W)=\sum_t^Ty_t(x_i,W_t)$, kde
  * $y_t$ je predikce t-tého stromu
  * $W_t$ je vektor parametrů (hodnot v listech) t-tého stromu
* Chybová funkce $L(W)=\sum_i\mathcal{l}(t_i,y(x_i,W))+sum_t^T\frac{1}{2}\lambda\norm{W_t}^2$
  * Pro regresi $l(t_i,y(x_i,W))=(t_i-y(x_i,W))^2$
* Predikce pro t-tý strom
  * Chci takovou, která minimalizuje loss, přičemž předchozí stromy jsou už známé
  * $L^{(t)}(W_t,W_{1..t-1})=\sum_i\mathcal{l}(t_i,y^{(t-1)}(x_i,W_{1...t-1})+y_{t}(x_i,W_t))+\frac{1}{2}\lambda\norm{W_t}^2$
  * Chceme ji minimalizovat, tedy chceme znát, jaké má být $y_t(x_i,W_t)$ (predikce nového stromu) → použijeme Newtonovu metodu a rozepíšeme přes Taylorův rozvoj
    * $g_i=\frac{\part\mathcal{l}(t_i,y^{(t-1)}(x_i))}{\part y^{(t-1)}(x_i)}$ - první derivace
    * $h_i=\frac{\part^2\mathcal{l}(t_i,y^{(t-1)}(x_i))}{\part y^{(t-1)}(x_i)^2}$ - druhá derivace
    * $L^{(t)}(W_t,W_{1..t-1})=\sum_i(\mathcal{l}(t_i,y^{(t-1)}(x_i))+g_iy_{t}(x_i)+\frac{1}{2}h_iy^2_{t}(x_i))+\frac{1}{2}\lambda\norm{W_t}^2$
    * A upravuji ....
      * $L^{(t)}(W_t,W_{1..t-1})\approx\sum_i(g_iy_{t}(x_i)+\frac{1}{2}h_iy^2_{t}(x_i))+\frac{1}{2}\lambda\norm{W_t}^2 + const$
      * $L^{(t)}(W_t,W_{1..t-1})\approx\sum_T((\sum_{i\in I_T}g_i)w_T+\frac12(\lambda+\sum_{i\in I_T}h_i)w^2_T) + const$
    * Jak nastavit $w_T$, abych minimalizovat $L^{(t)}(W_t,W_{1..t-1})$
      * $0=\frac{\part L^{(t)}}{\part w_T}=(\sum_{i\in I_T}g_i)+(\lambda+\sum_{i\in I_T}h_i)w_T)$
      * $w_T^*=-\frac{\sum_{i\in I_T}g_i}{\lambda+\sum_{i\in I_T}h_i}$ ← nejlepší nastavení vah
  * Nyní chci loss $L^{(t)}(W_t,W_{1..t-1})$ co nejmenší, tedy dosadím do ní $w^*_T$
    * 
* data subsampling - každému stromu ukážu jen část dat
* feature subsampling - dovolím pracovat jen s některými rysy
* shirnkage - predikce každého natrénovaného stromu přenásobím parametrem $\alpha < 1$, aby za většinu predikce nebyl zodpovědný jediný strom 

### Newtonova metoda hledání kořene

* Mám $f:\mathbb{R}→\mathbb{R}$ a chci najít její kořen
* Iteruji $x_{i+1}=x_i-\frac{f(x_i)}{f´(x_i)}$ (předpokládám, že funkce se chová jako přímka)
* Když chci hledat minimum funkce, minimalizuji derivaci
  * $x_{i+1}=x_i-\frac{f´(x_i)}{f´´(x_i)}$
  * V podstatě SGD, ale mám určený learing-rate (který odpovídá tomu předpokladu, že funkce se chová jako přímka)
* Funguje dobře pro málo parametrů ... protože bych potřeboval inverzi Hesiánu

### Klasifikace

* GBDT budou dohromady tvořit lineární část modelu - logit
* Výsledek pak proženu sigmoidem
* Pak použiji NLL



