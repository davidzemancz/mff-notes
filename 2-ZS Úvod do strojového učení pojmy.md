# Lineární regrese

* MSE
* Explicitní řešení lineární regrese metodou nejmenších čtverců
* $L_2$ regularizace
* Střední hodnota
* Rozptyl
* Bias estimatoru (biased a unbiased estimator)
* Gradient descent (standardní, stochastický, batch)
* Podmínky konvergence SGD
* SGD algoritmus pro lineární regresi

# Perceptron

* Algoritmus pro perceptron
* Perceptron jako SGD
* Predikce

# Logistická regrese

* Entropie, self-information
* Cross-entropie
* KL-divergence
* Gibbsovy nerovnosti
* Emirical data distribution
* Likelihood
* MLE jako minimalizace NLL
* Odvození MSE z MLE
* Věta o Lagrangeových multiplikátorech
* Kategorická distribuce je ta s maximální entropií

## Binární klasifikace

* Funkce sigmoid (derivace)
* Logit a jeho význam
* Predikce
* Chybová funkce NLL
* SGD algoritmus

## Klasifikace do více tříd

* Funkce softmax
* Logit a jeho význam
* Predikce
* SGD algoritmus
* Regiony tříd jsou konvexní
* Derivace softmax

# MLP

* Struktura MLP a predikce
* Aktivační funkce na výstupní vrstvě (a jakou distribuci produkují)
* Aktivační funkce na skryté vrstvě
* Odvození gradientů pro trénování MLP
* Universal approximation theorem

# K-nn

* Accuracy, recall, precision
* $F_1$ skóre, $F_\beta$ skóre
* Micro-averaged a macro-averaged $F_1$ score
* K-nn algoritmus pro regresi a klasifikaci, metriky a váhy

# Kernel lineární regrese

* Definice vah
* Polynomiální kernel pro rysy stupně $d$
* Polynomiální kernel pro rysy stupně alespoň $d$ 
* RBF kernel
* SGD algoritmus pro kernel lineární regresi
* Predikce

# Naive bayes

* TD, IDF, TF-IDF
* Podmíněná pravděpodobnost
* Podmíněná cross-entropie
* Mutal information (kdy je nulová)
* Posterior, prior, likelohood
* MAP
* Vztah MF-IDF a mutal information
* Základní předpoklad Naive bayes klasifikátoru ($p(x|C_k)=?$)
* Gaussian Naive Bayes 
* Multinomial Naive Bayes
* Bernoulli Naive Bayes
* Generativní a diskriminativní modely

# Korelace

* Kovariance
* Korelované a nekorelované náhodně proměnné
* Nezávislé proměnné jsou nekorelované (důkaz)
* Pearsonův korelační koeficient (rozsah)
* Spearmanův rank korelační koeficient
* Kendallův rank korelační koeficient
* Nekorelované chyby více modelů se navzájem vyruší

# Rozhodovací stromy

* Bagging, bootstraping, omezování featur
* Rozhodovací strom pro regresi (typy vrcholů, dělení vrcholů, predikce, hodnoty v listech)
* Rozhodovací strom pro klasifikaci (typy vrcholů, dělení vrcholů, predikce, hodnoty v listech)
* Gini index
* Entropy criterion
* Dělení vrcholů stromu během trénování
* Odvození Gini indexu z MSE
* Odvození entropy criterion z NLL
* Random forest

## Gradient-boosted rozhodovací stromy

* Chybová funkce
* Predikce
* Predikce pro t-tý strom

# PCA

* Princip PCA
* Whitening
* Power iteration algoritmus

# K-Menas

* Algoritmus pro K-menas
* Kmeans++

# Gausian mixture

* 





