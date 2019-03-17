# Git


## config

**git config option valeur**

git config --global user.name "Mon nom"
git config --global user.email "mon@email.fr"

**Le drapeau --global permet d'indiquer à git que cette configuration est globale et affectera ainsi tous nos futurs projets.**

## init

La commande git init permet d'initialiser un nouveau dépôt git vide. **Cela aura pour effet de créer un dossier .git** qui contiendra les informations sur notre versioning. Il ne sera pas possible de versionner sans ce dossier là.

cd "MON SUPER PROJET"
git init

## status

**Comme son nom l'indique cette commande permet d'obtenir un status sur l'état de notre versioning.** Elle donne un rapide résumé des fichiers qui sont en staging et des fichiers non suivis.

git status

N'hésitez pas à abuser de cette commande pour savoir où vous en êtes, ça vous évitera de mauvaises surprise par la suite

## add

Une particularité de git est son système de staging qui permet de sélectionner les fichier à suivre lors du prochain commit. 

git add  fichier    # Permet de *stage* le fichier
git add  dossier  # Stage tout le dossier
git add *.html        # Stage tous les fichier finissant par .html
git add --all            # Stage tous les fichiers (même les ajouts et les suppressions

## commit

Une fois que la zone de staging est prête on va pourvoir faire notre premier commit. 

git commit  # Ouvre un éditeur pour insérer le message de notre commit
git commit -m "Message pour le commit ^^"


## log

 La commande log permet d'obtenir des informations sur les différents commit de notre projet.

git log # Récupère et affiche les derniers commit

Il existe quelques options utiles

 - --oneline, permet d'afficher l'historique avec une ligne par commit (plus lisible)
 - -n  nombre , permet de sélectionner le nombre de commit à afficher
 - -p  fichier , permet de voir l'historique des commit affectant un fichier en particulier
 - --author  motif , permet de voir l'historique par rapport au nom de l'auteur

**On remarquera aussi ici que chaque commit fait dans le projet est identifié par une clef sha1, cette clef sert à la fois à s'assurer de l'intégrité du commit, mais aussi de clef unique.**

## diff

La commande diff permet de voir les différence qu'il existe sur un fichier

git diff 
git diff  fichier 

## revenir en arrière

Ecrire l'histoire c'est bien, revenir dans le temps c'est mieux ! Le gros intérêt du versioning c'est qu'il va nous permettre de revenir en arrière en cas de problème. 

## checkout

La commande checkout permet de faire plusieurs choses

 - Passer de branche en branche (on en parlera dans un prochain chapitre
 - Revenir sur un fichier par rapport à un commit
 - Revenir sur un commit

git checkout

Permet de transformer le  fichier  tel qu'il était lors du  commit  et l'ajoute au staging.

git checkout  commit 
Transforme tous les fichiers pour reproduire l'état du  commit . Cette commande nous place dans un état particulier appellé detached HEAD. En résumé vous êtes revenu en arrière en tant que spectateur. Vous pouvez voir le projet tel qu'il était au moment du commit tout en ayant la possibilité de revenir dans le "présent". On utilisera cette commande pour observer des vieux commit, si on souhaite réellement revenir en arrière on utilisera plutôt la commande reset.

## revert

Revert permet d'inverser un commit.

git revert  commit
Cette commande va défaire ce qui avait été fait au moment du  commit  en créant un nouveau commit. 

## reset

 il faudra faire très attention lors de l'utilisation de cette commande car elle altère l'historique et peut dans certains cas supprimer vos modifications (si vous voyez --hard, vérifiez 6 fois ce que vous voulez faire).

**git reset fichier**
Supprime un fichier de la zone de staging, mais ne supprime pas les modifications qui sont faites

**git reset**
Supprime tous les fichiers de la zone de staging, sans supprimer les modifications.

**git reset --hard**
Cette commande est à utiliser avec extrème précaution, elle renvoit le dossier de travail au niveau du dernier commit. Toutes les modifications non commit seront perdues.

**git reset commit**
Permet de revenir en arrière jusqu'au  commit , réinitialise la zone de staging tout en laissant votre dossier de travail en l'état.  Cette commande vous permet surtout de nettoyer l'historique en resoumettant un commit unique à la place de commit trop éparses.

**git reset commit --hard**
Permet de revenir au  commit  et réinitialise la zone de staging et le dossier de travail pour correspondre. Toutes les modifications, ainsi que tous les commits fait après le  commit  seront supprimés. 

**La commande reset ne devra jamais être utilisé après avoir publié (push) vos modifications. En revanche, elle peut être utile pour nettoyer votre historique local avant de l'envoyer en ligne.**

## les branches

**branch**
La commande branch permet de gérer tout ce qui a attrait aux branches (ajout, listing, suppression, renommage).

git branch              # Permet de lister les branches
git branch branche    # Permet de créer une nouvelle branche  branche 
git branch -m branche # Renomme la branche courante en  branche 
git branch -d  branche # Permet de supprimer une branche

Attention ! On ne peut pas supprimer une branche qui n'aurait pas été fusionné avec une autre (on perdrait alors les modifications de la branche). **Si on souhaite forcer cette suppression, et perdre tout le travail effectué dessus il faudra utiliser un D majuscule.**

**git branch -D branche**
Supprime la branche même si elle n'a pas été fusionnée

**git checkout branche**
Permet de se rendre sur une branche existante. **En revanche, si vous le souhaitez vous pouvez demander à git de sauter sur une branche qui n'existe pas en la créant au préalable.**

**git checkout -b branche** 
équivalent à 
git branch  branche 
git checkout  branche 

## merge

Merge permet de ramener une branche sur une autre et ainsi de la fusionner. La fusion de 2 branche se fait toujours à partir de la branche principale.

La branche fusionnée ne sera pas affectée

**git merge branche**
Fusionne la branche branche avec la branche courante. Git choisira automatiquement l'algorithme de fusion à utiliser.

**git merge --no-ff branche**
Fusionne la branche branche avec la branche courante en générant un commit de fusion (même si un fast-foward est possible).

## historique

Manipuler l'historique peut parfois s'avérer utile pour corriger un commit mal effectué mais aussi pour préparer une branche avant la fusion.

## amend

L'argument --amend permet de rajouter les fichier en staging dans le commit précédent. Ceci permet de corriger un oubli et d'éviter de faire 10 commits pour la même chose.

git commit --amend

## rebase

Comme son nom l'indique rebase permet **de déplacer une branche et de changer son commit de départ (sa base).** 

**git rebase nouvelle-base**
Permet de changer la base de la branche courante pour la nouvelle-base .

**rebase -i**
Le drapeau -i permet de déclencher un rebase interactif. Ce mode nous permet de guider git dans son rebase et de lui dire comment organiser les nouveaux commits.

**git rebase -i nouvelle-base**
Cette commande va ouvrir un éditeur nous permettant de dire à git comment réorganiser les différents commit à partir de la nouvelle base

## remisage

Imaginons, vous avez des modifications en cours de création et vous avez envie de les mettre de côté temporairement, plutôt que de commit les différentes modifications vous alliez les stocker grâce au remisage.

## git stash

**Cette commande va mettre de côté toutes les modifications qui ont été apportées au projet depuis le dernier commit .**
 Vous pouvez alors continuer à travailler sur autre chose (faire vos commits de manière traditionnelle) et ré appliquer plus tard ces modifications avec la commande :

**git stash apply**
Il est possible de voir l'ensemble des stash sauvegardés avec

**git stash drop**
enfin il est possible de combiner les commandes apply et drop en utilisant la commande pop

 **git stash pop**
  équivaut à 
 git stash apply
 git stash drop



## remote

Utiliser un dépôt git en local c'est bien, mais le gros intérêt du versionning c'est de pouvoir travailler à distance mais aussi de collaborer à plusieurs.

## --bare

Lorsque l'on fait un git init on a la possibilité d'ajouter le drapeau --bare. Cette option permet de préciser que ce dossier ne contiendra pas de dossier de travail mais seulement l'historique de notre projet. Ces dossiers --bare peuvent être utilisés comme dépôt distant.

cd mon-remote
git init --bare


## remote

La commande remote vous permet de créer voir et supprimer des connexions. 
git remote # Liste les dépôts distants
git remote -v # Liste les dépôts distants et les chemins associés
git remote add  alias   chemin/url  # Ajoute un nouveau dépôt distant
git remote rm  alias  # Supprimé un dépôt distant
git remote rename  old   new 


## push

La commande push permet de transférer les commits locaux vers le dépôt distant.

git push  remote   branche 
git push  remote  --all # Permet d'envoyer toutes les branches
git push  remote  --force 

Ce drapeau doit être utilisé en dernier recours (normalement vous n'en aurez jamais besoin) car il va modifier l'historique distant et peu ainsi affecter tous les collaborateurs. Mais ça peut être utile en cas de problème, si par exemple vous envoyez un commit en oubliant de retirer une clef d'API ou une information sensible.

## fetch

La commande fetch permet d'importer les informations du dépôt distant. 

git fetch remote # Récupère toutes les branches et tous les commits 
git fetch remote branche

git fetch origin master 
git log master..origin/master # Permet de voir les commits entre ma branche master et celle du remote

Si les modifications me semble acceptable pour une fusion je peux alors fusionner manuellement
git merge origin/master


## pull

Récupérer les infos du remote avec un fetch puis un merge n'est pas forcément très "user-friendly". **La commande pull permet de faire un git fetch suivi d'un git merge en une seule commande.**

git pull  remote
git pull remote branche

## Fork & pull request

Nous allons maintenant parler des services tiers Bitbucket et GitHub qui permettent d'héberger vos projets versionnés avec Git.

## Fork

**Un fork désigne une copie d'un dépôt.** En effet, par défaut il n'est pas possible de faire de commit sur un dépôt qui ne nous appartient pas (heureusement sinon ça serait l'anarchie). **Du coup, les services ont introduit cette notion de fork qui permet de se retrouver avec un dépôt sur lequelle on aura la permission d'écriture**

## Pull request

La notion de pull request va de paire avec le système de Fork. **Une fois que l'on a travaillé sur notre fork on souhaite souvent proposer à l'auteur original nos modifications.** 
On fera alors un pull request qui consiste tout simplement à demander à l'auteur original de merge nos modifications. Ce processus est géré de manière quasi automatique par GitHub et Bitbucket.

## GitHub ou Bitbucket

 - Si votre projet est open source, GitHub est plus adapté car il met
   mieux en avant le code et parceque, soyons franc, tout les devs ont
   un compte GitHub.
 - Si votre projet est fermé, Bitbucket propose une tarification qui
   peut s'avérer plus intéréssante.

