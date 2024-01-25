# Projet "QCM"

## Documentation joueur

Pour jouer, vous devez appartenir au groupe `QCM-joueur`.

Il vous suffit de lancer le script [bin/menu](bin/menu).

## Documentation administrateur

### Pour ajouter un niveau de difficulté

Vous devez appartenir au groupe `QCM-admins`

Il vous suffit de créer un répertoire dans [share/questions](share/questions).

Vous pouvez aussi passer par le script [bin/ajout_questions](bin/ajout_questions), accessible via le menu.

### Pour ajouter un niveau

Vous devez appartenir au groupe `QCM-admins`

Il vous suffit de créer un répertoire dans [share/questions/<<le niveau de difficulte>>/](share/questions/).

Vous pouvez aussi passer par le script [bin/ajout_questions](bin/ajout_questions), accessible via le menu.

### Pour ajouter une question

Vous devez appartenir au groupe `QCM-admins`

Il vous suffit de créer un fichier dans `share/questions/<<le niveau de difficulte>>/<<le niveau>>` :
- La première ligne est l'intitulé de la question
- Les lignes suivantes sont les choix de réponses possibles
- La ligne contenant la bonne réponse doit se terminer par " #bonneReponse"

Par exemple :
```
Quel fils de Zeus est le protecteur des marchands, des voyageurs et des voleurs ?
Héphaïstos
Apollon
Hermès #bonneReponse
Hestia
```

Vous pouvez aussi passer par le script [bin/ajout_questions](bin/ajout_questions), accessible via le menu.

## Documentation développeur

### Scripts

Les scripts sont intensément commentés.

### Permissions

La consigne demande que :
- tous les membres du groupe QCM-joueurs puissent jouer au jeu
  > Cela nécéssite qu'ils aient le droit d'éxécuter les scripts, mais aussi que les scripts puissent faire tout  ce dont ils ont besoin (créer un répertoire au nom du joueur dans var/parties, créer des fichiers dedans, lire les fichiers question etc...)
- Seul vous pourrez modifier les scripts, les scores etc...
  > Il faut que personne d'autre que vous ne puisse modifier les scripts, modifier ou supprimer les résultats des parties etc...
- Les membres du groupe « QCM-admin » devront pouvoir ajouter de nouvelles questions, même sans connaître le bash.
  > Les membres du groupe "QCM-admin" doivent pouvoir modifier le contenu de share/questions

```console
$ cd projet
$ sudo chgrp -R QCM-joueurs bin lib
$ sudo chmod -R 750 bin lib
$ ls -l bin lib
bin:
total 40
drwxr-x--- 2 prof QCM-joueurs 4096 janv. 20 22:01 .
drwxr-xr-x 7 prof prof        4096 janv. 21 19:14 ..
-rwxr-x--- 1 prof QCM-joueurs 1030 janv. 20 22:11 _afficher_scores_joueur
-rwxr-x--- 1 prof QCM-joueurs 2528 janv. 20 23:40 _afficher_tableau_scores
-rwxr-x--- 1 prof QCM-joueurs 2535 janv. 20 22:10 _calculer_scores_joueur
-rwxr-x--- 1 prof QCM-joueurs 2437 janv. 20 20:24 _enchainement_questions
-rwxr-x--- 1 prof QCM-joueurs 2859 janv. 21 00:22 menu
-rwxr-x--- 1 prof QCM-joueurs 1771 janv. 20 20:20 _nouvelle_partie
-rwxr-x--- 1 prof QCM-joueurs 2440 janv. 21 00:22 _poser_question
-rwxr-x--- 1 prof QCM-joueurs 1364 janv. 20 20:29 _reprendre_partie

lib:
total 12
drwxr-x--- 2 prof QCM-joueurs 4096 janv. 20 20:12 .
drwxr-xr-x 7 prof prof        4096 janv. 21 19:14 ..
-rwxr-x--- 1 prof QCM-joueurs  347 janv. 20 22:48 traitement_options
```

On voit que seul moi (prof) peut créer de nouveaux scripts ou modifier les existants, et que seuls les membres de QCM-joueurs peuvent les éxécuter.


sudo chgrp -R QCM-joueurs var
sudo chmod 755 var
sudo chmod 3775 var/parties

On utilise les "sticky bits" "SGID" et "sticky" (voir cours 4 slide 32) pour que :
- Les répertoires créés dans parties aient, eux aussi, pour groupe "QCM-joueurs"
- Les répertoires des joueurs, bien qu'ils aient pour groupe "QCM-joueurs" et que le groupe ait le droit d'écriture dans var/parties, ne puissent être supprimés que par le joueur en question.

Donc, quand un joueur "joueurX" va lancer une nouvelle partie :
- Un répertoire "joueurX" va être créé dans var/parties/
- Le script (qui, rappelons le, est exécuté en tant "joueurX") aura le droit de créer ce répertoire car le joueur est dans le groupe "QCM-joueurs" et le groupe a le droit d'écriture dans var/parties.
- Le répertoire var/parties/joueurX appartient au joueur joueurX, et a pour groupe propriétaire "QCM-joueurs"
- Les fichiers créés dans var/parties/joueurX ne seront modifiables que par "joueurX" et seront lisibles par tout le monde.
- Seul "joueurX" peut supprimer son répertoire.


sudo chgrp QCM-admins bin/ajout_questions
sudo chmod ug+x bin/ajout_questions

On utilise le sticky bit "SGID" pour que tout ce qui est créé dans share appartienne au groupe QCM-admins.


```
~/projet$ tree -pug .
[drwxr-xr-x prof     prof    ]  .
|-- [-rw-rw-r-- prof     prof    ]  README.md
|-- [drwxr-x--- prof     QCM-joueurs]  bin
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _afficher_scores_joueur
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _afficher_tableau_scores
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _calculer_scores_joueur
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _calculer_tableau_scores
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _enchainement_niveaux
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _enchainement_questions
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _nouvelle_partie
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _poser_question
|   |-- [-rwxr-x--- prof     QCM-joueurs]  _reprendre_partie
|   |-- [-rwxrwxr-- prof     QCM-admins]  ajout_questions
|   `-- [-rwxr-x--- prof     QCM-joueurs]  menu
|-- [drwxrwxr-x prof     QCM-joueurs]  etc
|-- [drwxr-x--- prof     QCM-joueurs]  lib
|   `-- [-rwxr-x--- prof     QCM-joueurs]  traitement_options
|-- [drwxr-sr-x prof     QCM-admins]  share
|   `-- [drwxrwsr-x prof     QCM-admins]  questions
|       |-- [drwxrwsr-x prof     QCM-admins]  difficile
|       |   |-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 1\ :\ Geographie
|       |   |   |-- [-rw-rw-r-- prof     QCM-admins]  1
|       |   |   |-- [-rw-rw-r-- prof     QCM-admins]  2
|       |   |   `-- [-rw-rw-r-- prof     QCM-admins]  3
|       |   |-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 2\ :\ Histoire
|       |   |   |-- [-rw-rw-r-- prof     QCM-admins]  1
|       |   |   |-- [-rw-rw-r-- prof     QCM-admins]  2
|       |   |   `-- [-rw-rw-r-- prof     QCM-admins]  3
|       |   `-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 3\ :\ Informatique
|       |       |-- [-rw-rw-r-- prof     QCM-admins]  1
|       |       |-- [-rw-rw-r-- prof     QCM-admins]  2
|       |       `-- [-rw-rw-r-- prof     QCM-admins]  3
|       `-- [drwxrwsr-x prof     QCM-admins]  facile
|           |-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 1\ :\ Geographie
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  1
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  2
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  3
|           |   `-- [-rw-rw-r-- prof     QCM-admins]  4
|           |-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 2\ :\ Histoire
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  1
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  2
|           |   |-- [-rw-rw-r-- prof     QCM-admins]  3
|           |   `-- [-rw-rw-r-- prof     QCM-admins]  4
|           `-- [drwxrwsr-x prof     QCM-admins]  Niveau\ 3\ :\ Informatique
|               |-- [-rw-rw-r-- prof     QCM-admins]  1
|               |-- [-rw-rw-r-- prof     QCM-admins]  2
|               `-- [-rw-rw-r-- prof     QCM-admins]  3
`-- [drwxr-x--- prof     QCM-joueurs]  var
    |-- [drwxrws--T prof     QCM-joueurs]  parties
    |   |-- [drwxrwsr-x joueur2  QCM-joueurs]  bob
    |   |   `-- [drwxrwsr-x joueur2  QCM-joueurs]  2024-01-25_22-17-03
    |   |       |-- [drwxrwsr-x joueur2  QCM-joueurs]  Niveau\ 1\ :\ Geographie
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  1
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  2
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  3
    |   |       |   `-- [-rw-rw-r-- joueur2  QCM-joueurs]  score
    |   |       |-- [drwxrwsr-x joueur2  QCM-joueurs]  Niveau\ 2\ :\ Histoire
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  1
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  2
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  3
    |   |       |   `-- [-rw-rw-r-- joueur2  QCM-joueurs]  score
    |   |       |-- [drwxrwsr-x joueur2  QCM-joueurs]  Niveau\ 3\ :\ Informatique
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  1
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  2
    |   |       |   |-- [-rw-rw-r-- joueur2  QCM-joueurs]  3
    |   |       |   `-- [-rw-rw-r-- joueur2  QCM-joueurs]  score
    |   |       |-- [-rw-rw-r-- joueur2  QCM-joueurs]  difficulte
    |   |       `-- [-rw-rw-r-- joueur2  QCM-joueurs]  score
    |   `-- [drwxrwsr-x joueur1  QCM-joueurs]  zoro
    |       |-- [drwxrwsr-x joueur1  QCM-joueurs]  2024-01-25_22-14-13
    |       |   |-- [drwxrwsr-x joueur1  QCM-joueurs]  Niveau\ 1\ :\ Geographie
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  1
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  2
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  3
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  4
    |       |   |   `-- [-rw-rw-r-- joueur1  QCM-joueurs]  score
    |       |   |-- [drwxrwsr-x joueur1  QCM-joueurs]  Niveau\ 2\ :\ Histoire
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  1
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  2
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  3
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  4
    |       |   |   `-- [-rw-rw-r-- joueur1  QCM-joueurs]  score
    |       |   |-- [drwxrwsr-x joueur1  QCM-joueurs]  Niveau\ 3\ :\ Informatique
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  1
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  2
    |       |   |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  3
    |       |   |   `-- [-rw-rw-r-- joueur1  QCM-joueurs]  score
    |       |   |-- [-rw-rw-r-- joueur1  QCM-joueurs]  difficulte
    |       |   `-- [-rw-rw-r-- joueur1  QCM-joueurs]  score
    |       `-- [drwxrwsr-x joueur1  QCM-joueurs]  2024-01-25_22-15-52
    |           |-- [drwxrwsr-x joueur1  QCM-joueurs]  Niveau\ 1\ :\ Geographie
    |           |   `-- [-rw-rw-r-- joueur1  QCM-joueurs]  1
    |           `-- [-rw-rw-r-- joueur1  QCM-joueurs]  difficulte
    |-- [drwxrwsr-x prof     QCM-joueurs]  scores
    |   |-- [drwxrwsr-x joueur2  QCM-joueurs]  bob
    |   |   `-- [-rw-rw-r-- joueur2  QCM-joueurs]  facile
    |   `-- [drwxrwsr-x joueur1  QCM-joueurs]  zoro
    |       `-- [-rw-rw-r-- joueur1  QCM-joueurs]  facile
    `-- [drwxrwsr-x prof     QCM-joueurs]  scores-generaux
        |-- [-rw-rw-r-- joueur2  QCM-joueurs]  difficile
        `-- [-rw-rw-r-- joueur2  QCM-joueurs]  facile

31 directories, 69 files
```