# PROJET SLAM1

![Logo Brasserie](Image1.png)

Pour le titre de chaque parties, nous allons mettre un code couleur pour différencier le travail de chacun.

**Équipe :**
- Desforges Alexandre (représenté par la couleur bleu) [Lien du github d'Alexandre](https://github.com/lugzz/projet_Slam)
- Issaoui Khaoula (représentée par la couleur rose)
- Kuras Maxime (représenté par la couleur verte)

## 🔵🟢I) Présentation du projet

Actuellement en première année à l'EPSI nous avions un projet pour l'atelier SLAM1. Nous avions comme projet d'offrir une solution dans le but de développer la présence en ligne à une organisation, ainsi que de lui offrir des outils de gestion adaptés à ses besoins. Cette organisation est La Brasserie Terroir & Saveurs, située dans les Hauts-de-France.

### a) Les missions

Les missions étaient en multiple :

- Voici les principales missions à réaliser pour le projet Brasserie T&S :
- Mise en place d'un outil de suivi et d'hébergement du projet
- Réalisation d'un site vitrine pour la brasserie
- Développement d'un système de gestion des utilisateurs
- Implémentation des fonctionnalités spécifiques pour chaque profil :
  - Brasseur (outils de brassage, gestion des stocks)
  - Direction (bilans financier et commercial)
  - Caissier (système de caisse, gestion des clients)
  - Client (fidélité, achats, réservations)
  - Administrateur (gestion utilisateurs, logs système)
- Production du rendu final avec documentation technique et présentation

###🌸b) Stratégie


La stratégie que nous avons utilisée était la méthode agile. Nous nous donnions souvent de lundi à vendredi pour réaliser les tâches que nous avions prévu le weekend auparavant. Cela nous permettait d'être le plus réactif envers le client (en fonction des évolutions de ses directions). Notre équipe était composé d'une product owner / manageuse et de deux développeurs. Nous avions aussi un tableau Kanban pour visualiser les tâches en cours et leur progression sur Trello.

![Tableau Kanban](image-2.png)

## 🟢II) Présentation de la structure de données

Nous avons eu des soucis concernant les associations entre les différentes tables sur PHP My Admin cela provient du fait que le type des tables défini par défaut est en "MyISAM" et nous avons modifier le type en "InnoDB".

![Structure de la base de données](image-3.png)

![Types des tables](image-4.png)

| **Table catégorie produit** | **Type** | **Description** |
|----------------------------|----------|----------------|
| id | int(11) | Clé primaire |
| nom | varchar(50) | Nom de la catégorie |

| **Table client_fideliter** | **Type** | **Description** |
|---------------------------|----------|----------------|
| id | int(11) | Clé primaire |
| date_fidelisation | date | Date de la fidélisation |
| cagnotte | decimal(10,2) | Montant de la cagnotte |
| id_utilisateur | int(11) | Clé étrangère vers user(id_utilisateur) |

| **Table produits** | **Type** | **Description** |
|-------------------|----------|----------------|
| id | int(11) | Clé primaire |
| nom | varchar(25) | Nom du produit |
| description | varchar(255) | Description du produit |
| prix | decimal(15,2) | Prix du produit |
| url_img | text | URL de l'image du produit |
| etat_produit | tinyint(1) | État actif/inactif |
| stock_produits_finis | varchar(50) | Quantité en stock |
| id_categorie | int(11) | Clé étrangère vers categorie_produit(id) |

| **Table recettes** | **Type** | **Description** |
|-------------------|----------|----------------|
| id | int(11) | Clé primaire |
| volume | decimal(10,2) | Volume de la recette |
| pourcentage | decimal(5,2) | Pourcentage d'alcool |
| ebc_grains | text | Valeur EBC des grains |
| brassage | decimal(10,2) | Quantité pour le brassage |
| rincage | decimal(10,2) | Quantité pour le rinçage |
| MCU | decimal(10,2) | MCU (couleur du moût) |
| EBC | decimal(10,2) | EBC final |
| SRM | decimal(10,2) | SRM final |
| amerisant | decimal(10,2) | Houblon amerisant utilisé |
| aromatique | decimal(10,2) | Houblon aromatique utilisé |
| levure | decimal(10,2) | Quantité de levure |

| **Table role** | **Type** | **Description** |
|---------------|----------|----------------|
| id_role | int(11) | Clé primaire |
| nom_role | varchar(50) | Nom du rôle |
| Description_role | varchar(510) | Description du rôle |

| **Table Stock** | **Type** | **Description** |
|----------------|----------|----------------|
| id_stockmp | int(11) | Clé primaire |
| quantite | int(11) | Quantité en stock |
| prix_achat_ht | decimal(15,2) | Prix d'achat hors taxe |
| nom_matiere | varchar(50) | Nom de la matière première |
| date_entree_stock | date | Date d'entrée en stock |
| description_mp | text | Description de la matière première |

| **Table user** | **Type** | **Description** |
|---------------|----------|----------------|
| id_utilisateur | int(11) | Clé primaire |
| nom | varchar(20) | Nom de l'utilisateur |
| prenom | varchar(20) | Prénom de l'utilisateur |
| email | varchar(50) | Adresse e-mail |
| creer_le | timestamp | Date de création |
| etat_compte | tinyint(1) | État du compte |
| premiere_co | tinyint(1) | Statut première connexion |
| login | varchar(15) | Identifiant |
| password | varchar(255) | Mot de passe (hashé) |
| id_role | int(11) | Clé étrangère vers role(id_role) |

| **Table vente** | **Type** | **Description** |
|----------------|----------|----------------|
| id_vente | int(11) | Clé primaire |
| reductions | decimal(15,2) | Montant de la réduction appliquée |
| date_vente | date | Date de la vente |
| total | decimal(15,2) | Montant total de la vente |
| type_vente | varchar(50) | Type (ex. réservation, achat, etc.) |
| id_utilisateur | int(11) | Clé étrangère vers user(id_utilisateur) |

| **Ventes_details** | **Type** | **Description** |
|-------------------|----------|----------------|
| id_produit | int(11) | Clé étrangère vers produits(id) |
| id_vente | int(11) | Clé étrangère vers ventes(id_vente) |
| quantite | int(11) | Quantité du produit vendu |
| prix_unitaire | decimal(15,2) | Prix unitaire (non défini dans le CREATE, mais présent dans les données) |

Initialement, notre projet reposait sur une base de données existante. Cependant, cette dernière présentait de nombreuses limites, notamment en matière de gestion des ventes, de flexibilité des rôles utilisateurs et d'évolutivité. Nous avons donc choisi de repartir sur une base propre, entièrement repensée.

Structure de la base de données :

- **user** : Table centrale contenant les informations des utilisateurs.
- **role** : Permet d'attribuer un rôle à chaque utilisateur, déterminant ses droits d'accès aux fonctionnalités de la plateforme (ex. : administrateur, direction, brasseur, caissier, client).
- **client_fideliter** : Enregistre les points de fidélité des clients (rôle n°4) pour un futur système de récompenses.

Gestion des ventes :

- **ventes** : Permet l'enregistrement des ventes, avec des fonctionnalités adaptées aux profils *caissier* (vente directe, gestion des réductions) et *direction* (consultation des chiffres, ajout de dépenses).
- **ventes_details** : Table de liaison entre les ventes et les produits, précisant les quantités et prix unitaires, afin de suivre précisément les transactions.

Produits et affichage :

- **produit** : Utilisée par :
  - le *caissier* pour la vente de produits,
  - le *brasseur* pour la création ou modification des produits,
  - la *vitrine* du site pour l'affichage des produits, avec gestion des images (via url_image).

Fonctionnalités évolutives :

Pour anticiper les besoins futurs ou permettre des extensions de fonctionnalités, d'autres tables ont été prévues :

- **categorie_produit** : Organisation des produits par catégorie.
- **fournisseurs** : Gestion des fournisseurs de matières premières.
- **achats** : Historique des achats effectués auprès des fournisseurs.
- **stock_matière_première** : Suivi du stock des ingrédients nécessaires à la production.
- **brassage** : Traçabilité de la production brassicole, étape par étape.

Cette base de données assure donc une gestion complète, centralisée et évolutive de l'ensemble des besoins de la brasserie, tant au niveau de la vente que de la production ou de la gestion client.

## III) Présentation de la structure du programme

### 🔵Arborescence :

```
/
├── admin.php
├── bar.jpg
├── blonde.png
├── brasserie_logo.png
├── brasseur.php
├── brune.png
├── caissier.css
├── caissier.php
├── changement_mdp.php
├── client.php
├── config.php
├── connexion.php
├── dashboard_direction.php
├── direction.php
├── gin.png
├── index.php
├── inscri.php
├── inscription.php
├── ipa.png
├── log.php
├── login.php
├── logout.php
├── logs.txt
├── panier.php
├── produits.php
├── whiskey.png
```

### Fonctions/fonctionnalité récurrente :

[Fonction de logging]

Fonction permettant d'ajouter un message de type évènement dans un fichier log.txt (exemple : [2025-04-09 09:54:33] Tentative de connexion par un utilisateur)

[Connexion BDD]

Connexion à la base de données avec gestion d'erreur.

### 🔵a) Connexion (connexion.php)

[Formulaire connexion]


Formulaire permettant de se connecter avec les bons types et aussi le required qui oblige l'insertion d'une valeur dans l'entrée utilisateur. (Ligne 122 à 131)

[Vérification saisies]


Vérification des saisies utilisateur côté serveur. (Ligne 10 à 13)

[Récupération info utilisateur]

On récupère les informations de l'utilisateur grâce à son login et on vérifie avec la similitude du mot de passe enregistré haché dans la bdd et l'entrée utilisateur. (Ligne 28 à 32)

Si tout est bon, on attribue à cette session des données qui nous seront utiles sur toutes les autres pages comme l'id, le role ainsi que son login.

[Vérification première connexion]

On récupère la valeur dans la bdd qui nous indique si c'est ça première connexion alors qu'il doit changer de mot de passe (lors de la création d'un compte, le mot de passe est automatiquement "motdepasse" et l'utilisateur doit le changer lors de sa première connexion). Il sera donc redirigé vers une page qui lui permettra de changer son mot de passe sinon il pourra accéder à son profil. (Ligne 35 à 42)

### 🔵b) Changement mot de passe

[Formulaire changement mdp]

Formulaire permettant d'entrée son nouveau mot de passe. Deux fois entrée pour plus de sécurité. (Ligne 120 à 135)

[Vérification et mise à jour mdp]

On vérifie l'entrée du formulaire puis on vérifie la similitude entre les deux mots de passes et si oui alors on met à jour la base de données avec le nouveau mot de passe haché et on le redirige vers l'index et on n'oublie pas de modifier aussi l'état de sa première connexion pour ses prochains venus sur le site. (Ligne 29 à 49)

### 🔵c) Admin

[Mise à jour utilisateur]

Ici on update la base de données en changeant les infos que l'admin aura indiqué dans le form et on se referera à l'id utilisateur. (Ligne 28 à 50)

[Ajout utilisateur]

Ici on va ajouter un utilisateur à la base de donnée en prenant comme login premiere lettre du prénom + nom de famille+25, on va hacher le mot de passe via la fonction php, et aussi on verifie que le login n'existe pas déjà dans la bdd (unicité). (Ligne 53 à 91)

[Suppression utilisateur]

Ici on va venir supprimer un utilisateur via son id. (Ligne 95 à 110)

[Tableau utilisateurs]

On aura ici un tableau affichant les colonnes pour chaque utilisateur avec des actions de sois modifié l'utilisateur ou soit le supprimer. (Ligne 234 à 270).

[Formulaire ajout utilisateur]

Ici on va afficher un form quand l'utilisateur cliquera sur le bouton ajouter utilisateur qui envoyer en post les infos. (Ligne 289 à 306)

### 🔵d) Brasseur

[Requêtes tables]

Requêtes permettant de récupérer la table produits et recettes. (Ligne 52 à 67).

[Calculs pour recette]
Ici on récupère les infos entrées par le brasseur et on applique les calculs recommandés par Mr. Lecomte pour ensuite l'afficher au brasseur les ingrédients de sa recette. (Ligne 70 à 91)

[Enregistrement recette]
On va venir enregistrer dans la bdd la recette créer par le brasseur si celui-ci clique sur enregistrer la recette. (Ligne 94 à 115)

[Formulaire recette]

Il s'agit du formulaire pour que le brasseur ait une recette. (Ligne 372 à 390)

[Affichage produits]
On va afficher les produits avec leurs caractéristiques. (Ligne 279 à 311).

[Affichage résultats calcul]

On va afficher les résultats du calcul du brasseur et lui proposer l'option de l'enregistrerLigne(392 à 427)

[Affichage recettes]

Affichage des recettes (Ligne 433 à 473)

[Affichage stock]

Affichage du stock des matières premières. (Ligne 475 à 503)

### 🔵e) Client

[Gestion panier]

On va récupérer depuis le form l'id du produit ainsi que sa quantité puis on rentrera le tout dans la variable $_SESSION['PANIER'] en vérifiant qu'il existe bien et que le produit sélectionné n'est pas déjà dans le panier auquel cas on rajoute sa nouvelle quantité. (Ligne 24 à 65)

[Récupération fidélité et ventes]

Ici on va pouvoir récupérer les points de fideliter du client ainsi que ses ventes et on pourra sélectionner les détails des ventes en fonction alors de l'id de la vente. (Ligne 69 à 88)

[Sélection produits disponibles]

On sélectionne les produits disponibles, alors ou leur état = 1 lorsque le client clique sur commander. (Ligne 90 à 100)

[Affichage points fidélité]

Affichage points de fidélité. (Ligne 243 à 287)

[Affichage commandes]

[Affichage commandes avec détails]

Affichage des commandes avec une colonne action permettant de visualiser les détails de la commande. (Ligne 295 à 317)

[Affichage détails vente]

Affichage des détails de la vente en fonction de l'id de la vente sélectionnée. 

[Affichage produits disponibles]

Affichage des produits disponibles avec possibilité de commander et l'id, et la quantité passeront dans le form.(Ligne 325 à 338)

### 🟢f) Direction

[Ajout recette]
Ajout d'une recette en vérifiant que l'input nommé « ajout_recette » est appelé par l'utilisateur, il lance donc le « try / catch », dans la try nous avons la requête SQL qui est placée dans la variable $sql. Il y a une requête préparer avec le bout de code $stmt (statement). Dans le catch nous avons la gestion d'erreur lié au PHP data object. (Ligne 15 à 35)

[Ajout dépenses]

Même processus pour l'ajout des dépenses. (Ligne 38 à 51)

[Calcul recettes, dépenses, ventes]

Pour ce qui est du calcul des recettes, dépenses et ventes il y a un processus identique avec la préparation des requêtes SQL ici dans la variable : « $SQL _recettes », « SQL_dépenses », « sql_ventes_produits ».

Nous avons le calcul des recettes, dépenses et ventes dans les requêtes SQL et elles seront définies comme : « total_recettes », « total_depenses », « total_vendu ».

A chaque requête j'ai défini une variable ($recettes, $depenses, $ventes_produits) dans laquelle on aura un tableau associatif pour l'affichage sur la page HTML qui est construit par l'argument du fetch. On le voit ici : 

(Ligne 56 à 86)

[Fetch PDO]

Une fois le calcul des recettes et dépenses, je défini une variable $bilan qui sera la soustraction entre recettes et dépenses. (Ligne 75)

Deux variables ont été créées pour l'affichage par moi ou année et qui sera mis. (Ligne 53 et 54)

[Variables mois et année]

On voit sur la partie HTML les deux appels aux variables $mois et $année. (Ligne 204 à 216)

[Affichage bilan]

Affichage du total recette et dépenses avec le bilan, ils auront un affichage à 2 décimal après la virgule. (Ligne 232 à 245)

[Formulaire ajout recette]

Le formulaire pour l'ajout d'une recette. (Ligne 247 à 264)

[Formulaire ajout dépense]

Le formulaire pour l'ajout d'une dépense. (Ligne 265 à 286)

[Tableau associatif ventes]

Nous avons ici le tableau associatif pour les ventes de chaque produit. (ligne 288 à 306).

### 🟢g) Caissier

[Récupération données]

Nous avons la récupération des données des tables produits, réservations, clients. (Ligne 15 à 60)

[Création compte client]

Nous avons ici la création d'un compte client avec la création du mot de passe, du hachage, et l'implémentation dans la table user et client_fideliter. On utilise la fonction beginTransaction(). Elle va nous servir a ce qu'il n'y ait pas de mauvais enregistrement des données dans les tables client_fideliter et user. Puisque que nous avons l'utilisation de l'id utilisateur pour créer un compte fidélité. Avec la variable : « $user_id = $pdo->lastInsertId(). (Cette fonction nous permet de récupérer l'id qui vient d'être créer pour être sur qu'il s'agit bien du bon utilisateur.

Nous avons donc une gestion d'erreur avec le try catch mais également avec le beginTransaction, commit et rollback. (Ligne 64 à 115).

[Validation réservation]

Nous avons la validation de la réservation avec le même prince de gestion d'erreur et de requêtes. (Ligne 118 à 134).

[Enregistrement vente]

L'enregistrement d'une vente par un caissier avec la vérification de l'existence du client puis le calcul du total de la vente avec la gestion du stock. (ligne 137 à 267)

[Gestion remises et fidélité]

Ici la gestion des remises et de l'utilisation des points de fidélité puis l'enregistrement de la vente en base de données.(Ligne 190 à 211)

[Incrémentation tables]

Puis incrémentation de la table vente détails et du stock dans la table produit fini.

Ajout des points de fidélité défini par 10% du montant total payé par le client. (ligne 137 à 267)

[Onglets caissier]

La partie HTML qui avec les 3 onglets qui représente les grandes fonctionnalités du profil caissier. (Ligne 330 à 332)

[Formulaire enregistrement vente]

Le formulaire pour enregistrer la vente à un client. (ligne 336 à 399).

[Formulaire création client]

Formulaire de création d'un compte client. Qui ne peut qu'enregistrer le nom, le prénom et le mail. (Ligne 402 à 433)

[Gestion validations réservations]

Gestion des validations des réservations de l'ensemble des clients. (Ligne 436 à 473)



Une fonction qui permet d'afficher les onglets que l'on souhaite et cacher les autres qui contiennent une class = « tab-content ». (Ligne 478 à 486)

## IV) Présentation des fonctionnalités du programme

### 🟢🔵A) Site Vitrine

Nous retrouvons sur le site vitrine, un message de bienvenue, les produits que l'on présente et qui sommes-nous.

Nous avons la navbar sur la gauche avec le logo et un message explicitant le rôle avec lequel on se connecte, en dessous de celle-ci, plusieurs onglets de navigation ; "Accueil, Nos Produits, Qui Nous Sommes, l'onglet du rôle et le bouton déconnexion".

![Site vitrine](image-56.png)

### 🔵B) Système de Connexion

Pour ce qui est du système de connexion, nous avons un simple formulaire qui renvoie au serveur mot de passe et login, une vérification est faite par la base de données et la page qui est associé à notre rôle sera déployée. 

![Système connexion](image-57.png)

### 🔵C) Administration des Comptes (Profil Administrateur)

Pour ce qui est de la page administrateurs, nous avons bien les fonctionnalités de gestions des différents utilisateurs, la possibilité de modifier, supprimer ou ajouter des comptes. Seul l'administrateur peut créer des comptes ayant un rôle autre que client. Nous avons donc le tableau qui renvoie l'ensemble des utilisateurs de la base de données, et à droite des actions permettant de supprimer ou modifier les comptes.

En bas se trouve l'onglet d'ajout d'utilisateur avec un simple formulaire renvoyant ; "Nom, Prénom, E-mail, Rôle, Etat du compte". L'ajout du rôle et de l'état du compte est unique à l'interface administrateur.

![Admin interface](image-58.png)

### 🔵D) Espace brasseur

Le profil brasseur nous permet de gérer le stock de bières présent, il peut modifier ou supprimer du stock.

Il a également le formulaire de calcul du brassage, il rentre les valeurs souhaiter pour créer une bière en particulier et la formule lui permet de connaître les dosages exacts. La bière créer avec la formule peut être ajouter avec le formulaire d'ajout de produit.

La recette de bière est ainsi créée et enregistrée en base de données.

![Interface brasseur 1](image-59.png)
![Interface brasseur 2](image-60.png)

Il a également la possibilité de voir le stock de matière premières à savoir s'il a la possibilité de produire la bière souhaitée.

![Stock matières premières](image-61.png)

### 🟢E) Espace Direction

Le profil Direction a deux grosses fonctionnalités principales, la possibilité de gérer le bilan financier, avec un filtre par mois et années. Le bilan est donc dû au calcul suivant : Total recettes -- Total Dépenses.

Il a également la possibilité d'ajouter une recette (ex : vente de produit pour une autre entreprise) et d'ajouter une dépense (ex : achat de matières premières chez un fournisseur).

![Interface direction](image-62.png)

La seconde fonctionnalité majeure est le bilan commercial qui n'est ni plus ni moins que l'ensemble des produits vendus à un moment donné, en l'occurrence le mois d'avril 2025.

![Bilan commercial](image-63.png)

### 🟢F) Espace Caissier

Le profil caissier possède 3 grandes fonctionnalités qui sont ; "L'enregistrement d'une vente à un client, Créer un compte client et Valider une réservation"

La première permet de vendre grâce à un formulaire, les différents produits proposés par la brasserie mais également il lui permet de sélectionner un client s'il a déjà un compte chez nous et y mettre une remise en pourcentage.

![Interface caissier vente](image-64.png)

Dans le cas où le client n'a pas de compte, la seconde fonctionnalité prends son sens, le caissier peut en créer un avec un formulaire, qui, contrairement au profil administrateur, n'a que les champs nom, prénom et e-mail. Le rôle et l'état du compte sera automatiquement gérer dans le code.

![Création compte client](image-65.png)

La dernière fonctionnalité est la réservation. Sachant que le profil client peut passer une réservation, le profil caissier peut valider celle-ci.

![Validation réservation](image-66.png)

### 🔵G) Espace Client

Pour le dernier profil, le client a la possibilité de visualiser l'ensemble des commandes qu'il a effectué. Mais également de passer commande, d'où l'importance de la fonctionnalité du profil caissier.

![Interface client](image-67.png)

## V) Démonstration d'un exemple de fonctionnement (vidéo)

[Lien vers la vidéo de démonstration](https://youtu.be/uNSb7ul4Q2I)

## VI) Vidéo de présentation sur ce que vous avez réalisé (que vous aviez en charge)

[Lien vers la vidéo de présentation de nos parties individuelles](https://www.youtube.com/watch?v=Iv5gFbdfd-k&ab_channel=AlexandreDesforges)

## VII) Sources

- W3C (Doc générale)
- PHP (Site de la doc PHP)
- MDN Web Docs (Site de la doc HTML)
- W3Schools (Site de la doc CSS)
- Looping (MCD et MLD de la BDD)
- ChatGPT, Claude, Copilot
- StackOverflow
