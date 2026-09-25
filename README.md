# Projet Casino — Craps

# 1. Présentation

Ce projet consiste à développer un jeu de casino en Python dans le cadre d'un projet de programmation.

Le jeu choisi est le Craps, un jeu de dés très connu dans les casinos.

Le programme permet :

de jouer au Craps en tant que joueur ;

de lancer deux dés ;

d'afficher les valeurs des deux dés et leur total ;

de gérer les situations de gain, de perte et de point ;

d'automatiser des parties avec une méthode bot() ;

de simuler un grand nombre de parties avec une méthode esperance() ;

d'utiliser une stratégie de mise configurable.

# 2. Règles du Craps utilisées

Le jeu utilise deux dés à six faces.

Premier lancer

Le premier lancer est appelé Come-Out Roll.

Total

Résultat

7 ou 11

Gain

2, 3 ou 12

Perte

4, 5, 6, 8, 9 ou 10

Point

Lorsqu'un total compris dans les valeurs de point est obtenu, un point est établi.

Après l'établissement du point

Une fois le point établi, le joueur continue de lancer les dés jusqu'à obtenir l'une des deux situations suivantes :

le même nombre que le point → gain ;

7 → perte.

Les autres résultats ne terminent pas la partie et les dés sont relancés.

Exemple

Si le premier lancer donne :

Dé 1 : 3
Dé 2 : 3
Total : 6
point

Le point est donc 6.

Les lancers suivants peuvent être :

Total : 8
Total : 5
Total : 9
Total : 6
gain

La partie se termine lorsque le 6 réapparaît.

Si un 7 apparaît avant le 6 :

Total : 8
Total : 5
Total : 7
perte

# 3. Structure du projet

Le projet contient notamment :

Python_Casino_Bradley/
│
├── craps.py
├── casino.py
└── Projet_casino.md

craps.py

Ce fichier contient la classe principale :

class Craps:

C'est le fichier consacré au jeu de Craps.

casino.py

Ce fichier contient une autre classe de jeu appelée JustePrix.

Il s'agit d'une autre partie du projet et elle n'est pas nécessaire au fonctionnement du Craps.

Projet_casino.md

Ce fichier contient les consignes générales du projet.

# 4. Classe Craps

La classe Craps contient les règles du jeu, la gestion du joueur, la stratégie de mise, le bot et le calcul de l'espérance.

class Craps:

Initialisation

Le constructeur est :

def __init__(
    self,
    gain=(7, 11),
    perte=(2, 3, 12),
    point=(4, 5, 6, 8, 9, 10),
    strategie=None
):

Il définit trois catégories de résultats :

self.gain = gain
self.perte = perte
self.point = point

Par défaut, la stratégie utilisée est :

{
    "mise": 10,
    "nombre_essais": 1,
    "multiplicateur de mise": 2
}

Cela signifie :

mise initiale : 10 € ;

nombre d'essais : 1 ;

multiplicateur après une perte : 2.

# 5. Méthode lancer_des()

La méthode :

def lancer_des(self, manuel=True):

permet de lancer deux dés.

Les deux valeurs sont générées aléatoirement avec :

random.randint(1, 6)

Le programme affiche :

Dé 1 : 4
Dé 2 : 2
Total : 6

La méthode retourne ensuite le total :

return total

Lorsque manuel=True, le programme attend que l'utilisateur appuie sur Entrée avant de lancer les dés.

# 6. Méthode joueur()

La méthode :

def joueur(self):

permet à un utilisateur de jouer une partie.

Le premier lancer est analysé :

if total in self.gain:
    print("gain")
    return "gain"

Si le résultat correspond à une perte :

elif total in self.perte:
    print("perte")
    return "perte"

Si un point est obtenu :

elif total in self.point:
    point = total
    print("point")

Le programme entre alors dans une boucle :

while True:

Cette boucle continue jusqu'à ce que :

le point réapparaisse → gain ;

un 7 apparaisse → perte.

# 7. Méthode mise()

La méthode :

def mise(self):

retourne simplement la mise définie dans la stratégie.

Par défaut :

10

Donc :

craps.mise()

retourne :

10

# 8. Méthode bot()

La méthode :

def bot(self):

permet d'automatiser la logique de jeu.

Elle récupère les paramètres de la stratégie :

mise = self.strategie["mise"]
multiplicateur = self.strategie["multiplicateur de mise"]
nombre_essais = self.strategie["nombre_essais"]

Le bénéfice est initialisé à :

benefice = 0

Gestion d'un gain

Lorsqu'une partie est gagnée :

benefice += mise

La mise revient ensuite à la mise initiale :

mise = self.strategie["mise"]

Gestion d'une perte

Lorsqu'une partie est perdue :

benefice -= mise

La mise suivante est multipliée par le multiplicateur :

mise *= multiplicateur

Avec une mise initiale de 10 € et un multiplicateur de 2, les mises peuvent donc suivre :

10 €
20 €
40 €
80 €
160 €
...

Cette logique correspond à une progression de mise de type Martingale.

# 9. Méthode esperance()

La méthode :

def esperance(self, n=100_000):

permet d'estimer l'espérance de gain grâce à un grand nombre de simulations.

Elle effectue :

n

simulations et additionne les bénéfices.

Le résultat final est :

return benef / n

L'espérance obtenue représente donc le bénéfice moyen par simulation.

Exemple

Si une simulation donne :

Espérance : -0.20

cela signifie qu'en moyenne, sur un grand nombre de parties simulées, le joueur perd environ :

0,20 € par partie

Une espérance négative ne signifie pas que chaque partie est perdue. Certaines parties peuvent être gagnantes et d'autres perdantes.

# 10. Loi des grands nombres

L'objectif de esperance() est d'utiliser la loi des grands nombres.

Plus le nombre de simulations augmente, plus la moyenne observée tend à se rapprocher de l'espérance théorique du jeu.

Par exemple :

10 parties
100 parties
1 000 parties
10 000 parties
100 000 parties

Les résultats peuvent varier fortement avec peu de parties.

Avec davantage de simulations, la moyenne devient généralement plus stable.

# 11. Exemple d'utilisation

Le programme crée une instance de Craps :

craps = Craps()

La stratégie peut être affichée :

print("Stratégie :", craps.strategie)

La mise peut être récupérée avec :

print("Mise :", craps.mise())

Le joueur peut ensuite jouer :

joueur = craps.joueur()
print("Joueur :", joueur)

Le bot peut être exécuté avec :

bot = craps.bot()
print("Bot :", bot)

Enfin, l'espérance peut être calculée avec :

print("Espérance :", craps.esperance())

# 12. Exemple de déroulement

Une partie peut produire :

Stratégie : {'mise': 10, 'nombre_essais': 1, 'multiplicateur de mise': 2}
Mise : 10

Lancer les dés...
Dé 1 : 3
Dé 2 : 3
Total : 6
point

Lancer les dés...
Dé 1 : 4
Dé 2 : 2
Total : 6
gain

Joueur : gain

Autre exemple :

Lancer les dés...
Dé 1 : 3
Dé 2 : 3
Total : 6
point

Lancer les dés...
Dé 1 : 5
Dé 2 : 2
Total : 7
perte

Joueur : perte

# 13. Probabilités du premier lancer

Avec deux dés à six faces, il existe 36 combinaisons possibles.

Les probabilités des principaux résultats sont :

Total

Nombre de combinaisons

Probabilité

2

1

2,78 %

3

2

5,56 %

4

3

8,33 %

5

4

11,11 %

6

5

13,89 %

7

6

16,67 %

8

5

13,89 %

9

4

11,11 %

10

3

8,33 %

11

2

5,56 %

12

1

2,78 %

Le premier lancer donne donc :

gain immédiat avec 7 ou 11 ;

perte immédiate avec 2, 3 ou 12 ;

établissement d'un point avec 4, 5, 6, 8, 9 ou 10.

# 14. Stratégie de mise

La stratégie par défaut est :

{
    "mise": 10,
    "nombre_essais": 1,
    "multiplicateur de mise": 2
}

Elle peut être modifiée lors de la création de l'objet.

Exemple :

strategie = {
    "mise": 20,
    "nombre_essais": 5,
    "multiplicateur de mise": 2
}

craps = Craps(strategie=strategie)

La mise initiale est alors de 20 €.

# 15. Limites de la stratégie Martingale

La progression utilisée par le bot augmente fortement les mises après une perte.

Avec une mise initiale de 10 € :

10 €
20 €
40 €
80 €
160 €
320 €
640 €
...

Cette progression peut devenir très importante après plusieurs pertes consécutives.

Dans un casino réel, elle est limitée par :

le capital disponible du joueur ;

la limite maximale de la table ;

le nombre de pertes consécutives.

La stratégie ne supprime donc pas le risque de perte.

# 16. Installation

Le projet utilise la bibliothèque standard Python.

Aucune installation externe n'est nécessaire pour le fichier craps.py.

Il faut disposer de Python installé sur l'ordinateur.

Pour vérifier l'installation :

python --version

# 17. Lancer le programme

Depuis le dossier contenant craps.py :

python craps.py

Le programme demande ensuite d'appuyer sur Entrée pour lancer les dés.

# 18. Technologies utilisées

Le projet utilise principalement :

Python

random

la programmation orientée objet ;

les classes ;

les méthodes ;

les dictionnaires ;

les boucles while et for ;

les conditions if / elif;

les simulations aléatoires ;

l'estimation d'une espérance par simulation.

La bibliothèque random fait partie de la bibliothèque standard de Python.

# 19. Objectifs pédagogiques

Ce projet permet de travailler plusieurs notions de programmation :

Programmation orientée objet

Création et utilisation de la classe :

Craps

Encapsulation des données

Les règles du jeu et la stratégie sont stockées dans l'objet.

Conditions

Le programme distingue :

gain
perte
point

Boucles

La boucle while permet de continuer une partie lorsqu'un point est établi.

Aléatoire

Les deux dés sont générés aléatoirement.

Simulation

La méthode esperance() permet de simuler un grand nombre de parties.

Stratégie

La mise peut être modifiée après une victoire ou une défaite.

# 20. Conclusion

Ce projet permet de reproduire une version simplifiée du Craps sous forme d'un programme Python.

Le joueur peut lancer les dés manuellement et observer le déroulement de chaque partie.

Le programme distingue correctement :

Premier lancer :
7 ou 11       → gain
2, 3 ou 12    → perte
4, 5, 6, 8, 9, 10 → point

Après un point :
point          → gain
7              → perte
autre résultat → nouveau lancer

La classe contient également un système de mise et une simulation permettant d'étudier le bénéfice moyen obtenu sur un grand nombre de parties.

L'objectif final est de comparer les résultats du jeu avec ceux d'autres jeux de casino et d'analyser son espérance de gain.
