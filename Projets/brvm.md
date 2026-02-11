
# Guide BRVM, Actions et Investissement

## Table des matières


## Concepts de base

### Action (Stock / Share)

**Définition** : Une part de propriété d'une entreprise cotée en bourse. Quand tu achètes une action, tu deviens actionnaire, c'est-à-dire copropriétaire d'une fraction de l'entreprise.

**Analogie dev** : Un token qui représente une fraction de propriété. Comme des parts dans un projet open source où chaque contributeur détient un pourcentage.

```
Investissement total = Nombre d'actions x Prix par action

Exemple :
10 actions SONATEL x 15 000 FCFA = 150 000 FCFA investis
```

---

### Dividende (Dividend)

**Définition** : Part des bénéfices annuels qu'une entreprise redistribue à ses actionnaires. Ce n'est pas systématique, c'est une décision du conseil d'administration chaque année.

**Analogie dev** : Un return annuel. L'entreprise te renvoie une partie de ce qu'elle a gagné.

```
Rendement du dividende (%) = (Dividende annuel par action / Prix de l'action) x 100

Exemple :
Dividende par action = 500 FCFA
Prix de l'action     = 15 000 FCFA
Rendement            = (500 / 15 000) x 100 = 3,33%
```

---

### Plus-value (Capital Gain)

**Définition** : Profit réalisé quand tu vends une action à un prix supérieur à son prix d'achat.

```
Plus-value = Prix de vente - Prix d'achat - Frais de transaction

Exemple :
Achat  : 10 actions x 15 000 FCFA = 150 000 FCFA
Vente  : 10 actions x 18 000 FCFA = 180 000 FCFA
Gain   = 180 000 - 150 000 = 30 000 FCFA soit +20%
```

---

### Moins-value (Capital Loss)

**Définition** : Perte réalisée quand tu vends une action moins cher que son prix d'achat. C'est le risque principal de la bourse.

```
Moins-value = Prix de vente - Prix d'achat (résultat négatif)

Exemple :
Achat  : 10 actions x 15 000 FCFA = 150 000 FCFA
Vente  : 10 actions x 12 000 FCFA = 120 000 FCFA
Perte  = 120 000 - 150 000 = -30 000 FCFA soit -20%
```

---

### Capitalisation boursière (Market Cap)

**Définition** : Valeur totale d'une entreprise selon le marché à un instant T. C'est le prix qu'il faudrait théoriquement payer pour acheter 100% de l'entreprise.

```
Market Cap = Nombre total d'actions en circulation x Prix actuel par action

Exemple SONATEL :
100 000 000 actions x 15 000 FCFA = 1 500 milliards FCFA
```

---

### Liquidité (Liquidity)

**Définition** : Facilité avec laquelle on peut acheter ou vendre une action rapidement sans que cela impacte le prix de façon significative.

**Analogie dev** : Comme le temps de réponse d'une API. Haute liquidité = transaction quasi instantanée. Faible liquidité = tu peux attendre des jours pour trouver un acheteur.

L'indicateur principal de liquidité est le volume quotidien de transactions.

---

### Courtier / Broker

**Définition** : Intermédiaire agréé qui exécute tes ordres d'achat et de vente en bourse. Sika Finance joue ce rôle. Tu ne peux pas acheter des actions directement sans passer par un courtier.

---

### Portefeuille (Portfolio)

**Définition** : L'ensemble de tes positions financières (actions, obligations, etc.) à un moment donné.

**Analogie dev** : Ton `package.json` des investissements. La liste de tout ce que tu détiens.

---

## La BRVM

### Présentation

La BRVM (Bourse Régionale des Valeurs Mobilières) est la bourse commune de 8 pays de l'UEMOA.

- **Localisation** : Abidjan, Côte d'Ivoire
- **Pays membres** : Bénin, Burkina Faso, Côte d'Ivoire, Guinée-Bissau, Mali, Niger, Sénégal, Togo
- **Monnaie** : FCFA (code ISO : XOF)
- **Site officiel** : www.brvm.org

### Les indices

Un indice boursier est une mesure statistique qui résume l'évolution d'un groupe d'actions. C'est comme un benchmark de performance.

**BRVM 10** Moyenne pondérée des 10 actions les plus actives du marché. C'est l'indice de référence, celui qu'on regarde en premier pour savoir si le marché monte ou descend globalement.

**BRVM Composite** Inclut toutes les actions cotées sur la BRVM. Donne une vision plus complète du marché.

### Horaires de trading

- Du lundi au vendredi : 8h30 - 15h30 (heure d'Abidjan, GMT+0)
- Fermé les week-ends et jours fériés

### Secteurs représentés sur la BRVM

```
Télécommunications  : SONATEL, Orange CI
Banques             : SGBCI, NSIA Banque, BOA, Coris Bank
Distribution        : CFAO, BERNABE
Energie             : CIE (Compagnie Ivoirienne d'Electricité)
Agro-industrie      : SIFCA, PALMCI, SAPH
Transport           : SITAB, BOLLORE TRANSPORT
```

---

## Types d'actifs financiers

### Actions (Stocks)

**Définition** : Part de propriété d'une entreprise. Rendement variable, risque plus élevé, potentiel de gain plus important.

Tu gagnes de deux manières :

- Les dividendes (si l'entreprise en verse)
- La plus-value (si le prix monte avant que tu vendes)

---

### Obligations (Bonds)

**Définition** : Un prêt que tu fais à une entreprise ou à un Etat. En échange, ils te remboursent avec un taux d'intérêt fixe sur une durée déterminée.

**Analogie dev** : Une Promise avec un return garanti et une date d'expiration connue à l'avance.

```
Rendement obligation = Taux d'intérêt annuel fixe

Exemple :
Capital investi          : 1 000 000 FCFA
Taux                     : 6% par an
Durée                    : 5 ans
Intérêt annuel           : 1 000 000 x 0,06 = 60 000 FCFA
Intérêt total sur 5 ans  : 60 000 x 5 = 300 000 FCFA
A terme tu récupères     : 1 000 000 + 300 000 = 1 300 000 FCFA
```

Avantages : revenus prévisibles, risque plus faible que les actions. Inconvénients : rendement généralement plus faible, moins flexible.

---

### ETF (Exchange Traded Fund)

**Définition** : Un fonds qui regroupe un panier d'actions ou d'obligations et se négocie en bourse comme une action ordinaire.

**Analogie dev** : Un package npm. Au lieu d'installer chaque dépendance manuellement, tu installes un package qui en contient plusieurs.

Avantage principal : diversification automatique avec un seul achat.

---

## Indicateurs et métriques

Ces indicateurs servent à évaluer si une action est chère, bon marché, ou correctement valorisée.

### PER (Price to Earnings Ratio) — Ratio cours/bénéfice

**Définition** : Compare le prix d'une action au bénéfice par action qu'elle génère. Indique combien d'années de bénéfices sont nécessaires pour "rembourser" le prix de l'action.

```
PER = Prix de l'action / Bénéfice par action (BPA)

Exemple :
Prix action = 15 000 FCFA
BPA         = 1 200 FCFA
PER         = 15 000 / 1 200 = 12,5

Interprétation :
PER < 15        : action potentiellement sous-évaluée (opportunité)
PER entre 15-25 : valorisation raisonnable
PER > 25        : action potentiellement surévaluée (risque de correction)
```

Attention : le PER varie selon les secteurs. Il faut toujours comparer entre entreprises du même secteur.

---

### ROE (Return on Equity) — Rendement des capitaux propres

**Définition** : Mesure l'efficacité avec laquelle une entreprise utilise l'argent de ses actionnaires pour générer des bénéfices.

```
ROE (%) = (Bénéfice net / Capitaux propres) x 100

Exemple :
Bénéfice net     = 500 000 000 FCFA
Capitaux propres = 2 000 000 000 FCFA
ROE              = (500 / 2 000) x 100 = 25%

Interprétation :
ROE > 15%        : excellente rentabilité
ROE entre 10-15% : rentabilité correcte
ROE < 10%        : rentabilité faible
```

---

### Ratio d'endettement (Debt to Equity)

**Définition** : Mesure la proportion de dettes par rapport aux capitaux propres. Indique le niveau de risque financier de l'entreprise.

```
Ratio = Dette totale / Capitaux propres

Exemple :
Dette      = 1 000 000 000 FCFA
Capitaux   = 2 000 000 000 FCFA
Ratio      = 1 / 2 = 0,5

Interprétation :
< 0,5   : endettement sain
0,5 à 1 : endettement modéré, à surveiller
> 1     : entreprise très endettée, risque élevé
```

---

### Rendement du dividende (Dividend Yield)

**Définition** : Revenu généré par les dividendes rapporté au prix de l'action. Permet de comparer les actions entre elles comme tu comparerais des taux d'intérêt.

```
Dividend Yield (%) = (Dividende annuel par action / Prix actuel de l'action) x 100

Exemple :
Dividende  = 600 FCFA par action
Prix       = 15 000 FCFA
Yield      = (600 / 15 000) x 100 = 4%

Un yield de 4% signifie que tu reçois 4% du prix de l'action chaque année
juste en dividendes, indépendamment des variations de prix.
```

---

### Volume de transactions

**Définition** : Nombre d'actions échangées sur une période donnée, généralement un jour. Mesure l'activité et la liquidité d'une action.

```
Volume moyen quotidien = Total des actions échangées / Nombre de jours de bourse

Interprétation :
Volume > 10 000 actions/jour  : bonne liquidité
Volume < 500 actions/jour     : liquidité faible, difficile de vendre rapidement
```

---

## Analyse technique

L'analyse technique consiste à étudier les graphiques de prix et les indicateurs mathématiques pour anticiper les mouvements futurs. Elle ne prédit pas l'avenir avec certitude, elle calcule des probabilités.

### Graphiques et chandeliers japonais (Candlestick)

**Définition** : Chaque bougie sur un graphique représente l'évolution du prix sur une période donnée (1 minute, 1 heure, 1 jour, etc.).

```
Structure d'une bougie :

    |        <- Mèche haute : prix le plus haut atteint dans la période
  [   ]      <- Corps : va du prix d'ouverture au prix de clôture
    |        <- Mèche basse : prix le plus bas atteint dans la période

Corps vert (ou blanc) : clôture > ouverture => le prix a monté
Corps rouge (ou noir) : clôture < ouverture => le prix a baissé
```

**Analogie dev** : Un objet OHLC (Open, High, Low, Close) loggué pour chaque intervalle de temps.

```javascript
type Candle = {
    open   : number, // Prix à l'ouverture
    high   : number, // Prix le plus haut
    low    : number, // Prix le plus bas
    close  : number, // Prix à la clôture
    volume : number  // Nombre d'actions échangées
}
```

---

### Tendance (Trend)

**Définition** : Direction générale du prix sur une période.

- **Tendance haussière (Bull / Uptrend)** : le prix fait des sommets et des creux de plus en plus élevés
- **Tendance baissière (Bear / Downtrend)** : le prix fait des sommets et des creux de plus en plus bas
- **Tendance latérale (Sideways / Range)** : le prix oscille dans une fourchette sans direction claire

---

### Moyennes mobiles (Moving Averages — MA)

**Définition** : Moyenne des prix de clôture sur N jours. Lisse les fluctuations pour révéler la tendance de fond.

**Analogie dev** : Un algorithme de rolling average appliqué sur un tableau de prix.

**SMA — Simple Moving Average**

```
SMA(N) = (Prix_j1 + Prix_j2 + ... + Prix_jN) / N

Exemple SMA 20 jours :
SMA(20) = somme des 20 derniers prix de clôture / 20
```

**EMA — Exponential Moving Average**

Comme la SMA mais donne plus de poids aux prix récents. Réagit plus vite aux changements de tendance.

```
EMA = Prix_actuel x k + EMA_precedente x (1 - k)
k   = 2 / (N + 1)

Pour N = 12 : k = 2 / 13 = 0,1538
```

**Utilisation des croisements de moyennes mobiles :**

```
Signal d'achat  : MA courte (20j) croise au-dessus de MA longue (200j)
Signal de vente : MA courte (20j) croise en-dessous de MA longue (200j)

if (SMA_20 > SMA_200) {
    // Tendance haussière confirmée, potentiel d'achat
} else {
    // Tendance baissière, prudence ou sortie de position
}
```

Moyennes les plus utilisées : 20, 50, 100, 200 jours.

---

### RSI — Relative Strength Index

**Définition** : Oscillateur entre 0 et 100 qui mesure la vitesse et l'amplitude des mouvements de prix. Indique si une action est en zone de surachat ou de survente.

```
RSI = 100 - (100 / (1 + RS))
RS  = Moyenne des hausses sur N jours / Moyenne des baisses sur N jours
N   = 14 jours par défaut

Interprétation :
RSI > 70       : zone de surachat, risque de correction à la baisse
RSI < 30       : zone de survente, potentiel de rebond à la hausse
RSI entre 30-70 : zone neutre, pas de signal fort
```

Attention : le RSI peut rester au-dessus de 70 longtemps lors d'une forte tendance haussière. Ce n'est pas un signal de vente automatique.

---

### MACD — Moving Average Convergence Divergence

**Définition** : Indicateur de momentum qui montre la relation entre deux moyennes mobiles exponentielles. Utile pour détecter les changements de tendance.

```
Ligne MACD    = EMA(12) - EMA(26)
Ligne Signal  = EMA(9) appliquée sur la ligne MACD
Histogramme   = MACD - Signal

Signaux :
MACD croise Signal vers le haut    => signal d'achat (momentum haussier)
MACD croise Signal vers le bas     => signal de vente (momentum baissier)
Histogramme positif et croissant   => tendance haussière qui s'accélère
Histogramme négatif et décroissant => tendance baissière qui s'accélère
```

---

### Support et Résistance

**Support** : Niveau de prix plancher où l'action a tendance à rebondir. Les acheteurs sont nombreux à ce niveau.

**Résistance** : Niveau de prix plafond où l'action a du mal à passer. Les vendeurs sont nombreux à ce niveau.

```
Exemple :
Prix actuel  : 15 000 FCFA
Support      : 13 500 FCFA  (l'action a rebondi 3 fois depuis ce niveau)
Résistance   : 17 000 FCFA  (l'action n'a pas réussi à dépasser ce niveau)

Stratégie classique :
- Acheter proche du support
- Vendre proche de la résistance
- Si le prix casse la résistance avec du volume => forte hausse probable
- Si le prix casse le support avec du volume    => forte baisse probable
```

---

### Corrélation Volume / Prix

Le volume confirme ou infirme un mouvement de prix. Un mouvement sans volume est fragile.

```
Prix monte  + Volume fort    => tendance haussière confirmée
Prix monte  + Volume faible  => hausse fragile, méfiance
Prix baisse + Volume fort    => tendance baissière confirmée
Prix baisse + Volume faible  => baisse sans conviction, potentiel rebond
```

---

## Stratégies d'investissement

### Buy and Hold (Acheter et conserver)

**Principe** : Acheter des actions solides et les conserver plusieurs années sans se soucier des fluctuations court terme.

Convient pour : les débutants et les personnes qui n'ont pas le temps de suivre le marché quotidiennement.

```
Horizon     : 3 à 10 ans minimum
Objectif    : capitalisation long terme + dividendes
Actions     : entreprises stables avec historique de dividendes

Exemple :
Achat SONATEL en 2020 à 12 000 FCFA
Prix en 2025             : 18 000 FCFA
Gain en capital          : +50%
Dividendes recus          : environ 3 000 FCFA cumulés sur 5 ans
Rendement total approximatif : +75%
```

---

### DCA — Dollar Cost Averaging (Investissement périodique)

**Principe** : Investir un montant fixe à intervalles réguliers (chaque semaine ou chaque mois), quel que soit le prix du moment. Permet de lisser le prix d'achat moyen.

```
Exemple sur 4 mois avec 50 000 FCFA par mois :

Mois 1 : prix = 10 000 FCFA  =>  50 000 / 10 000 = 5,00 actions achetées
Mois 2 : prix = 12 500 FCFA  =>  50 000 / 12 500 = 4,00 actions achetées
Mois 3 : prix =  8 000 FCFA  =>  50 000 /  8 000 = 6,25 actions achetées
Mois 4 : prix = 11 000 FCFA  =>  50 000 / 11 000 = 4,55 actions achetées

Total investi    : 200 000 FCFA
Total actions    : 19,80 actions
Prix moyen payé  : 200 000 / 19,80 = 10 101 FCFA par action

Sans DCA (achat unique au mois 2) :
Prix payé        : 12 500 FCFA par action

Le DCA a permis de réduire le prix moyen de 2 400 FCFA par action.
```

---

### Value Investing (Investissement dans la valeur)

**Principe** : Repérer des actions dont le prix du marché est inférieur à leur valeur réelle. Stratégie popularisée par Warren Buffett.

```javascript
// Critères de sélection value investing
function isValueStock(stock) {
    return (
        stock.PER < 15          &&  // Prix raisonnable par rapport aux bénéfices
        stock.ROE > 12          &&  // Bonne rentabilité
        stock.dividendYield > 3 &&  // Dividendes attractifs
        stock.debtRatio < 0.6       // Endettement maîtrisé
    );
}
```

---

### Diversification

**Principe** : Répartir son capital entre plusieurs actifs pour réduire le risque global. Si une action s'effondre, les autres limitent la perte.

**Analogie dev** : Comme les microservices. Si un service tombe, les autres continuent de fonctionner.

```
Exemple de portefeuille diversifié :

Télécommunications (SONATEL)  : 20%
Banques (SGBCI ou BOA)        : 20%
Energie (CIE)                 : 15%
Agro-industrie (SIFCA)        : 15%
Obligations                   : 15%
Réserve de liquidités          : 15%

Règle de base : jamais plus de 20-25% du capital sur un seul actif.
```

---

## Gestion du risque

La gestion du risque est la partie la plus importante. Ne pas perdre de l'argent est plus important que d'en gagner.

### Stop Loss

**Définition** : Ordre automatique de vente déclenché si le prix descend à un niveau prédéfini. Limite les pertes sans que tu sois obligé d'être devant ton écran.

```
Exemple :
Prix d'achat : 15 000 FCFA
Stop loss    : 13 500 FCFA  (perte max acceptée : -10%)

Si le prix descend à 13 500 FCFA, l'ordre de vente s'exécute automatiquement.
La perte est limitée à 10%, quelle que soit la suite.
```

---

### Take Profit

**Définition** : Ordre automatique de vente déclenché si le prix monte à un niveau cible. Sécurise les gains automatiquement.

```
Exemple :
Prix d'achat : 15 000 FCFA
Take profit  : 18 750 FCFA  (gain cible : +25%)

Si le prix monte à 18 750 FCFA, la vente s'exécute automatiquement.
```

---

### Ratio Risque / Rendement (Risk / Reward Ratio)

**Définition** : Compare le gain potentiel à la perte potentielle d'une position. Un bon trade a toujours un ratio favorable.

```
Ratio = Gain potentiel / Perte potentielle

Exemple :
Prix d'achat : 15 000 FCFA
Stop loss    : 13 500 FCFA  => perte potentielle = 1 500 FCFA
Take profit  : 19 500 FCFA  => gain potentiel    = 4 500 FCFA

Ratio = 4 500 / 1 500 = 3:1

Règle : ne jamais prendre une position avec un ratio inférieur à 2:1.
Avec un ratio de 2:1, on peut se tromper une fois sur deux et rester rentable.
```

---

### Position Sizing

**Définition** : Calcul du montant à allouer sur chaque position en fonction du risque acceptable sur le capital total.

```
Règle des 2% : Ne jamais risquer plus de 2% de son capital total sur une seule position.

Exemple :
Capital total        : 1 000 000 FCFA
Risque max par trade : 1 000 000 x 2% = 20 000 FCFA

Prix achat           : 15 000 FCFA
Stop loss            : 13 500 FCFA  => risque par action = 1 500 FCFA

Nombre d'actions max  = 20 000 / 1 500 = 13 actions
Investissement        = 13 x 15 000 = 195 000 FCFA

Si le stop loss se déclenche : perte = 13 x 1 500 = 19 500 FCFA soit ~2% du capital.
```

---

### Volatilité

**Définition** : Mesure statistique de l'amplitude des fluctuations de prix. Calculée avec l'écart-type des rendements journaliers.

```
Volatilité annuelle < 15%  : action stable (secteur utilities, obligations)
Volatilité annuelle 15-30% : volatilité modérée (grandes capitalisations)
Volatilité annuelle > 30%  : action très volatile (risque élevé, potentiel élevé)
```

Plus la volatilité est haute, plus le risque est grand, et plus le potentiel de gain l'est aussi.

---

## Sika Finance : Guide pratique

### Types d'ordres disponibles

**Ordre au marché (Market Order)** L'ordre s'exécute immédiatement au meilleur prix disponible. Pas de garantie sur le prix exact. A utiliser sur les actions très liquides uniquement.

**Ordre à cours limité (Limit Order)** Tu fixes un prix maximum à l'achat ou minimum à la vente. L'ordre ne s'exécute que si ce prix est atteint.

```
Exemple :
Action SONATEL : prix actuel 15 200 FCFA
Tu places un ordre limite à 14 800 FCFA
L'achat ne se fera que si le prix descend à 14 800 FCFA ou moins.
```

**Ordre Stop (Stop Order)** L'ordre se déclenche automatiquement quand le prix atteint un seuil. Utilisé pour implémenter un stop loss.

---

### Checklist avant d'acheter une action

```
Analyse fondamentale :
[ ] PER inférieur à 20
[ ] ROE supérieur à 10%
[ ] Ratio d'endettement inférieur à 0,7
[ ] Dividendes versés régulièrement
[ ] Secteur stable ou en croissance

Analyse technique :
[ ] Tendance générale haussière (prix au-dessus de la MA 200 jours)
[ ] RSI entre 30 et 70 (ni suracheté ni survendu)
[ ] Volume suffisant pour la liquidité
[ ] Support identifié pour placer le stop loss

Gestion du risque :
[ ] Montant de la position inférieur à 20% du portefeuille total
[ ] Stop loss défini avant d'acheter
[ ] Ratio risque/rendement supérieur à 2:1
[ ] Raison d'achat clairement formulée et condition de sortie définie
```

---

### Tableau de suivi de portefeuille

A tenir dans Google Sheets ou Excel.

```
Date       | Action  | Qté | Prix achat | Prix actuel | Valeur    | Gain/Perte | Stop Loss | Notes
-----------|---------|-----|------------|-------------|-----------|------------|-----------|------
2025-02-11 | SONATEL | 10  | 15 000     | 16 500      | 165 000   | +10%       | 13 500    | Hold
2025-02-11 | SGBCI   | 5   | 22 000     | 21 500      | 107 500   | -2,3%      | 20 000    | Watch
```

---

### Plan d'action pour débutant sur compte démo

**Phase 1 — Mois 1-2 : Observer sans investir**

- Suivre 5 à 10 actions quotidiennement
- Lire les graphiques (chandeliers, tendances, supports/résistances)
- Calculer le PER et le ROE de 3 à 5 entreprises manuellement
- Tester les indicateurs RSI et MACD sur TradingView
- Ne pas encore investir, même sur le démo. Juste observer.

**Phase 2 — Mois 3-4 : Premiers investissements en démo**

```javascript
const portfolio_demo = {
    capital_total    : 1_000_000, // FCFA virtuel
    allocation: {
        action_telecom : 200_000, // 20% sur une action solide type SONATEL
        action_banque  : 150_000, // 15% sur un autre secteur
        action_energie : 150_000, // 15% troisième secteur
        reserve        : 500_000  // 50% en réserve, ne pas tout investir d'un coup
    },
    strategie        : "Buy and hold 3 à 6 mois",
    stop_loss        : "Défini sur chaque position avant l'achat"
};
```

**Phase 3 — Mois 5-6 : Bilan et optimisation**

- Calculer la performance réelle en pourcentage
- Identifier les erreurs (entrées trop tôt, stop loss mal calibré, etc.)
- Tester le DCA sur une position
- Condition pour passer en réel : performance positive sur 3 mois consécutifs avec discipline sur les stop loss

---

## Formules récapitulatives

```
Performance totale (%)
= ((Valeur finale - Valeur initiale) / Valeur initiale) x 100

Rendement annualisé — CAGR (Compound Annual Growth Rate)
= ((Valeur finale / Valeur initiale) ^ (1 / nombre_annees)) - 1

Rendement total incluant dividendes
= Plus-value + Total des dividendes recus

Ratio de Sharpe (rendement ajusté au risque)
= (Rendement portefeuille - Taux sans risque) / Volatilité
Un ratio > 1 est considéré comme bon.

Bénéfice par action (BPA / EPS — Earnings Per Share)
= Bénéfice net de l'entreprise / Nombre total d'actions

Valeur comptable par action (Book Value Per Share)
= Capitaux propres / Nombre total d'actions

Rendement réel (après inflation)
= Rendement nominal - Taux d'inflation
```

---

## Erreurs à éviter

### FOMO (Fear Of Missing Out)

Acheter une action après une forte hausse par peur de rater l'opportunité. C'est souvent le meilleur moyen d'acheter au plus haut et de vendre au plus bas.

```
Mauvaise approche :
"L'action a pris +25% cette semaine, il faut que j'achète maintenant."
=> Arriver trop tard. La hausse est déjà consommée dans le prix.

Bonne approche :
Attendre une correction ou le prochain point d'entrée technique identifié.
Analyser si les fondamentaux justifient le nouveau niveau de prix.
```

---

### Sur-trading (Over-trading)

Faire trop de transactions génère des frais qui détruisent les performances.

```
Frais typiques d'un courtier BRVM : environ 0,5% à 1% par transaction

Capital initial : 1 000 000 FCFA
20 allers-retours par mois à 1% chacun
Frais mensuels  : 1 000 000 x 1% x 20 = 200 000 FCFA de frais

Règle pratique en débutant : moins de 10 transactions par mois.
```

---

### Investir de l'argent dont on a besoin

La bourse peut baisser et rester basse pendant des mois ou des années. Si tu investis de l'argent dont tu auras besoin prochainement, tu seras forcé de vendre à perte.

Règle : n'investir que de l'argent dont tu n'as pas besoin pour les 3 à 5 prochaines années minimum.

---

### Ne pas avoir de plan de sortie

Beaucoup de gens savent quand acheter mais pas quand vendre. Les conditions de sortie doivent être définies avant d'entrer dans une position.

```javascript
const trade_plan = {
    action       : "SONATEL",
    prix_achat   : 15_000,
    stop_loss    : 13_500,   // Vente automatique si le prix descend ici (-10%)
    take_profit  : 19_500,   // Vente automatique si le prix monte ici (+30%)
    horizon      : "6 mois",
    raison_achat : "PER attractif, dividendes réguliers, tendance haussière confirmée"
};
```

---

### Négliger l'inflation

Un gain de 5% avec une inflation de 4% ne représente qu'un gain réel de 1%.

```
Rendement réel = Rendement nominal - Taux d'inflation

Exemple :
Gain sur l'année : +8%
Inflation        :  4%
Rendement réel   : +4%
```

---

## Ressources et outils

### Sites essentiels

```
BRVM officiel   : www.brvm.org
                  Cotations officielles, rapports financiers des entreprises cotées

Sika Finance    : www.sikafinance.com
                  Plateforme de trading, compte démo

Zone BRVM       : www.zonebrvm.com
                  Analyses, actualités marché, fiches des entreprises

Agence Ecofin   : www.agenceecofin.com
                  Actualités économiques et financières Afrique
```

### Outils d'analyse

```
TradingView     : www.tradingview.com
                  Graphiques avancés, tous les indicateurs techniques, version gratuite suffisante
                  Rechercher "BRVM:SNTS" pour SONATEL par exemple

Google Sheets   : Tableau de suivi de portefeuille personnalisé
```

### Glossaire Français / Anglais

```
Action              : Stock / Share
Obligation          : Bond
Dividende           : Dividend
Plus-value          : Capital Gain
Moins-value         : Capital Loss
Portefeuille        : Portfolio
Courtier            : Broker
Carnet d'ordres     : Order Book
Marché haussier     : Bull Market
Marché baissier     : Bear Market
Liquidité           : Liquidity
Volatilité          : Volatility
Capitalisation      : Market Cap
Rendement           : Yield / Return
Cours               : Price / Quote
Séance              : Trading Session
Indice              : Index
Momentum            : Momentum
Support             : Support
Résistance          : Resistance
Volume              : Volume
Bénéfice par action : EPS (Earnings Per Share)
```

---

## Checklist avant de passer en réel

```
Connaissances :
[ ] Je comprends ce qu'est une action, une obligation, un dividende
[ ] Je sais calculer et interpréter le PER, le ROE, le dividend yield
[ ] Je comprends les indicateurs RSI, MACD, moyennes mobiles
[ ] Je connais les types d'ordres (marché, limite, stop)

Pratique sur démo :
[ ] Compte démo utilisé pendant au moins 3 mois
[ ] Performance positive sur 3 mois consécutifs
[ ] Stop loss respectés à chaque position
[ ] Journal de trading tenu régulièrement

Psychologie et discipline :
[ ] Pas de trading émotionnel (achat par peur de rater)
[ ] Signaux attendus avant chaque entrée en position
[ ] Capacité à supporter une perte de 10% sans paniquer

Finances personnelles :
[ ] Capital investi non nécessaire à court terme
[ ] Epargne de sécurité couvrant 3 à 6 mois de dépenses
[ ] Aucun emprunt pour investir
```

---

## Conclusion

Les marchés financiers ne sont pas un moyen de s'enrichir rapidement. Ce sont des outils de création de richesse sur le long terme pour ceux qui comprennent ce qu'ils font, qui gèrent leur risque et qui restent disciplinés.

Le compte démo est une ressource précieuse. L'utiliser jusqu'à être régulièrement profitable avant de mettre de l'argent réel.

---

_Ce guide est à but éducatif. Les marchés financiers comportent des risques de perte en capital. Ne jamais investir plus que ce que l'on peut se permettre de perdre._