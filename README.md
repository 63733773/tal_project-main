## NOMEQUIPE : ATALDS_M1
Jordan TCHAMEDEU M1 DS

#                                      Projet : Traitement Automatique du Langage Naturel (TAL)

## Description de la tâche 

La tâche consiste à développer un système capable de classifier automatiquement des recettes de cuisine en déterminant, à partir du titre et des instructions de la recette, si celle-ci correspond à un Plat principal, une Entrée, ou un Dessert. Pour atteindre cet objectif, deux ensembles de données sont mis à disposition : un ensemble d'entraînement et un ensemble de test. L'ensemble d'entraînement a été subdivisé en deux parties : 80% des données servent à l'entraînement du modèle, tandis que les 20% restants sont utilisés pour la validation afin d'ajuster les paramètres et d'évaluer la performance du modèle avant de le tester sur l'ensemble de test.


## Exemples de documents (avec leur identifiant)

**ID:** recette_170277.xml
**Titre:** Langue de boeuf sauce piquante aux cornichons
**Type:** Plat principal
**Difficulté:** Facile
**Coût:** Moyen

**ID:** recette_21519.xml
**Titre:** Tarte aux raisins et amandes
**Type:** Dessert
**Difficulté:** Facile
**Coût:** Moyen

## Statistiques du corpus

**Ensemble d'Entraînement :** 9 978 documents
**Ensemble de Test :** 1 388 documents
**Ensemble de Validation :** 2 495 documents

La répartition des types dans l'ensemble combiné (entraînement, test, et validation) est la suivante :

**Plat principal :** 6 446
**Dessert :** 4 169
**Entrée :** 3 246

## Méthodes proposées

Pour aborder cette tâche de traitement du langage naturel, nous avons développé quatre méthodes distinctes, chacune caractérisée par des approches et des techniques spécifiques. Ces méthodes sont décrites comme suit :

### Run1 : Baseline (Classe Majoritaire)

La méthode décrite ci-dessus, nommée "Classe_Majoritaire", utilise une approche simple pour la classification des recettes de cuisine en Plat principal, Entrée, ou Dessert :

**Descripteurs utilisés :** Cette méthode ne s'appuie pas sur des descripteurs issus des données de recettes proprement dites. Au lieu de cela, elle adopte une stratégie de classification sans prendre en compte les caractéristiques des données d'entrée, en se basant uniquement sur la distribution des classes dans l'ensemble d'entraînement.

**Classifieur utilisé :** Le classifieur employé est DummyClassifier de la bibliothèque sklearn, configuré avec la stratégie "most_frequent". Ce classifieur choisit la classe la plus fréquente dans l'ensemble d'entraînement et prédit cette classe pour tous les exemples de l'ensemble de validation, sans effectuer d'analyse réelle du contenu ou des attributs des recettes.

### Run2 : BagOfWords

Pour la méthode de Bag of Words:

**Descripteurs utilisés :** Cette approche utilise le modèle Bag of Words pour transformer le texte des recettes en vecteurs de fréquence de mots. Chaque mot unique dans l'ensemble d'entraînement devient une caractéristique dans l'espace vectoriel, et chaque recette est représentée par un vecteur indiquant la fréquence de chaque mot.

**Classifieur utilisé :** Le classifieur choisi pour cette méthode est le MultinomialNB, un classifieur Naive Bayes multinomial. Ce modèle est bien adapté pour les tâches de classification de textes où les caractéristiques sont des compteurs ou des fréquences de termes.

### Run 3 : TF-IDF

Pour cette méthode utilisant TF-IDF et SVM :

**Descripteurs utilisés :** L'approche repose sur le modèle TF-IDF (Term Frequency-Inverse Document Frequency) pour convertir le texte des recettes en vecteurs. Ce modèle évalue l'importance d'un mot dans un document par rapport à une collection de documents, en attribuant plus de poids aux termes à la fois fréquents dans un document mais rares dans l'ensemble du corpus.

**Classifieur utilisé :** Le classifieur employé est SVC (Support Vector Classifier) de la bibliothèque sklearn, avec un noyau linéaire. Ce choix de noyau permet de traiter le problème de classification textuelle comme un problème d'optimisation linéaire, cherchant à établir la meilleure frontière de décision linéaire qui sépare les différentes catégories de recettes.

### Run 4 : Word2Vec

Pour cette méthode utilisant Word2Vec et la régression logistique:

**Descripteurs utilisés :** L'approche utilise Word2Vec pour convertir le texte des recettes en vecteurs numériques. Word2Vec est un modèle basé sur des réseaux de neurones qui apprend les représentations vectorielles des mots en fonction de leur contexte dans le corpus. Chaque recette est représentée par la moyenne des vecteurs de ses mots, offrant une représentation dense et significative du texte.

**Classifieur utilisé :** Le classifieur sélectionné est LogisticRegression de la bibliothèque sklearn, configuré pour effectuer un nombre maximal de 1000 itérations. Ce modèle effectue la classification en estimant les probabilités qu'une recette appartienne à chaque catégorie (Plat principal, Entrée, Dessert) en fonction de ses caractéristiques vectorielles, utilisant une fonction logistique pour modéliser cette relation.

## Résultats

|        Run       | f1 Score |
| -----------------| --------:|
|     baseline     |   0.21   |
|   Bag Of Words   |   0.86   |
|      TF-IDF      |   0.87   |
|    Word2Vec      |   0.83   |


## Analyse de résultats


#### Y-a-t-il des régularités dans les documents bien/mal classifiés ?:

**Baseline :** Tous les documents sont classés comme "Plat principal", indiquant que le modèle choisit la classe majoritaire sans égard pour les caractéristiques des données.
**Bag of Words et TF-IDF :** Ces méthodes semblent bien fonctionner pour la classe "Dessert", avec des taux de précision et de rappel élevés, mais elles ont plus de mal avec la classe "Entrée". Les erreurs pour "Entrée" pourraient indiquer des confusions avec les "Plats principaux", ce qui suggère une similitude de vocabulaire entre ces deux classes.
**Word2Vec :** A également un bon taux de reconnaissance pour "Dessert" mais montre des difficultés avec "Entrée", comme le reflète un taux de rappel plus bas.

### Où est-ce que l'approche se trompe ? (matrice de confusion):

**Baseline :** Prédit toujours "Plat principal", résultant en un grand nombre de Faux Positifs pour "Dessert" et "Entrée".
**Bag of Words :** Confond certains "Entrée" avec "Plat principal" (80 cas) et vice versa (99 cas).
**TF-IDF :** Similaire à Bag of Words, mais avec une légère amélioration dans la distinction entre "Entrée" et "Plat principal".
**Word2Vec :** Présente une amélioration par rapport à la baseline mais continue de confondre "Entrée" avec "Plat principal" (145 cas).

### Si votre méthode le permet: quels sont les descripteurs les plus décisifs ?:

Pour les méthodes **Bag of Words ** et **TF-IDF**, les termes avec les valeurs **TF**-IDF** les plus élevées ou les coefficients les plus significatifs dans le modèle sont les descripteurs les plus décisifs. Pour **Word2Vec**, cela n'est pas directement applicable car les vecteurs de mots captent des caractéristiques sémantiques qui ne sont pas aussi explicites que les coefficients ou les valeurs **TF-IDF**. Cependant, l'analyse des vecteurs proches des centres de classe peut donner des indications sur les mots clés pour chaque catégorie.



















