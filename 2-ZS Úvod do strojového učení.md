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
  * Tedy $r = \frac{x^Tw}{\norm{w}}$

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

### Entropie

* **Surprise** (míra překvapení)
  * Pro jisté jevy (Pr=1) je nulová
  * Roste pro méně pravděpodobné jevy
  * $I(x)=-\log{P(x)}=\log{\frac{1}{P(x)}}$
* **Entropie** je pak míra překvapení v celém rozložení (distribuci)
  * $H(P)=\mathbb{E}[I(x)]=-\mathbb{E}_{X \sim P}[\log{P(x)}]$
  * $H(P)=-\sum_xP(x)\log{P(x)}$
* **Corss-Entropie**
  * $H(P,Q)=-\mathbb{E}_{X \sim P}[\log Q(x)]$
* **Gibbsova nerovnost**
  * $H(P,Q)\geq H(P)$
  * $H(P,Q) = H(P) \iff P=Q$
  * ... vypadá to skoro jako metrika, ale není to metrika, protože obecně neplatí $H(P,Q)=H(Q,P)$
* Obecně je rozložení s **největší entropií** to nejobecnější → a to je právě normální rozložení

### MLE

* Maximum likelihood estimation
* **Empirické rozložení dat** z množiny $X=\{x_1,...x_N\}$ je $\hat p_{data}(x) = \frac{|\{i:x_i=x\}|}{N}$
* **Likelihood** je pak $L(w)=p_{model}(X,w)=\prod_i^Np_{model}(x_i,w)$, kde $p_{model}(x,w)$ je množina různých rozložení
  * Nabývá hodnoty z <0,1>
* **Maximum likelihood estimation** pro $w$ je pak 
  * $w_{MLE}=\max_w p_{model}(X,w)=\max_w \prod_i^Np_{model}(x_i,w)$
  * $w_{MLE}=\min_w \sum_i^N-\log p_{model}(x_i,w)$
* MLE pak lze zobecnit pro případ, kdy sis pro dané $x$ předpovídat $t$
  * $w_{MLE}=\max_w p_{model}(t|X,w)=\max_w \prod_i^Np_{model}(t_i|x_i,w)$
  * $w_{MLE}=\min_w \sum_i^N-\log p_{model}(t_i|x_i,w)$ ... NLL (negative log likelihood)
  * $w_{MLE}=\min_w H(\hat p_{data},p_{model}(t|x,w))$
* MLE je **konzistentní**, tj. s rostoucím počtem dat konverguje ke skutečnému rozložení dat
* MLE má také **nemenší MSE**
* Česky (metoda maximální věrohnodnosti)

### Binární klasifikace

* Funkce **sigmoid** $\sigma(x)=\frac{1}{1+e^{-x}}$
  * **Derivace sigmoid** $\sigma´(x)=\sigma(x)(1-\sigma(x))$
* $P(C_1|x)=\sigma(x^Tw+b)$
* $P(C_0|x)=1-P(C_1|x)$
* Pokud má platit  $y(x,w)=P(C_1|x)=\frac{1}{1+e^{\hat y(x,w)}}$, tak musí $\hat y(x,w)=\log(\frac{P(C_1|x)}{1-P(C_1|x)})=\log(\frac{P(C_1|x)}{P(C_0|x)})$
  * To se jmenuje **logit** - ta část, která je parametr nelineární funkce (tj. lineární část modelu)
    * Co je logit? Logaritmus poměru pravděpodobnost. také inverz sigmoidu
* Použiji chybovou funkci $E(w)=\frac{1}{N}\sum_i-\log(P(C_{t_i}|x_i,w))$ ... **NLL**
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

* 







