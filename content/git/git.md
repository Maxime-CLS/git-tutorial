---
title: "Lab - Git"
date: 2020-08-19T10:09:13+02:00
draft: false
weight: 5
---
### Créer un nouveau dépôt en ligne de commande

```
git init    # Initialise le répertoire .git
touch README.md
git add README.md   # Ajout de la modification
git commit -m "first commit"    # Message liée à la modification
git remote add origin https://github.com/Maxime-CLS/git-tutorial.git    # Ajout d'un dépôt git distant
git push -u origin master   # Pousse les modifications sur le dépôt distant
touch .gitignore
git add .gitignore  # Ajout de la modification
git commit -m "Add .gitignore"  # Message liée à la modification
git push origin master  # Pousse les modifications sur le dépôt distant
```

### Ajouter des changements sur le code

```
echo "# Hello :smile:" > README.md
cat README.md
touch tuto.txt
git status  # Affiche le modification de la branche
```

```
Sur la branche master
Votre branche est à jour avec 'origin/master'.

Modifications qui ne seront pas validées :
  (utilisez "git add <fichier>..." pour mettre à jour ce qui sera validé)
  (utilisez "git restore <fichier>..." pour annuler les modifications dans le répertoire de travail)
	modifié :         README.md

Fichiers non suivis:
  (utilisez "git add <fichier>..." pour inclure dans ce qui sera validé)
	tuto.txt

aucune modification n'a été ajoutée à la validation (utilisez "git add" ou "git commit -a")
```

#### Afficher l'ensemble des modifications

```
git diff    # Affiche les modifications apportés dans les fichiers
```

```
diff --git a/README.md b/README.md
index 69974f6..2027b47 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
+# Hello :smile:
```

#### Afficher les modifications sur un fichier spécifique

```
git diff README.md  # Affiche les modifications apportés sur un fichier spécifique
```

```
diff --git a/README.md b/README.md
index 69974f6..2027b47 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
+# Hello :smile:
```

Configuration globale de l'outil git pour les diff et les merge :

```
git config --global diff.tool vimdiff
git config --global difftool.prompt false
git config --global difftool.vimdiff trustExitCode true

git config --global merge.tool vimdiff
git config --global mergetool.vimdiff trustExitCode true
```

```
git difftool    # Outil de visualisation des modifications dans des fichiers
```

Changer de fenêtre : `Ctrl + w`

#### Ajouter et visualiser les modifications

```
git add .
git diff    # Affiche les modifications apportés dans un fichier
git diff --staged
git log     # Affiche l'historique des modifications
git log --pretty=format:"%h %an %ar - %s"   # Formate l'affichage de l'historique des modifications
git commit -m "Add tuto.txt and modify README.md"
git push origin master
git show # Affiche les modifications apportés dans un fichier
```

#### Annuler des changements

Retour à la dernière modification de la branche distante

```
echo "test" > tuto.txt
git checkout .  # Annule les modifications non-ajoutés
echo "test" > tuto.txt
git add tuto.txt
git reset HEAD  # Annule les modifications ajoutés
echo "test" > tuto.txt
git reset --hard HEAD # Annule les modifications ajoutés
echo "test" > tuto.txt
git add tuto.txt
git commit -m "Modify tuto.txt"
git push origin master
git revert HEAD~1   # Supprimer le dernier commit
```

#### Récupérer les modifications de branches

Ajouter au travers de l'interface grahique de GitHub un nouveau fichier.

Ajouter en local un autre fichier :

```
touch script.sh
git add script.sh
git commit -m "Add script.sh"
git push orign master
```

```
 ! [rejected]        master -> master (fetch first)
error: impossible de pousser des références vers 'https://github.com/Maxime-CLS/git-tutorial.git'
astuce: Les mises à jour ont été rejetées car la branche distante contient du travail que
astuce: vous n'avez pas en local. Ceci est généralement causé par un autre dépôt poussé
astuce: vers la même référence. Vous pourriez intégrer d'abord les changements distants
astuce: (par exemple 'git pull ...') avant de pousser à nouveau.
astuce: Voir la 'Note à propos des avances rapides' dans 'git push --help' pour plus d'information.
```

Un problème pour pousser ces modifications car notre branche master n'est pas à jour...
Du coup on va récupérer les modifications de la branche master comme ci on avait fait un pull request avant de faire des commits.

```
git pull --rebase
git push origin master
git log
```

Pour configurer son outil git à faire des pull rebase par défaut :

```
git config --global branch.autoSetupRebase always
git config --global pull.rebase = preserve
```

#### Manipuler des branches

```
git branch develop  # Créer une branche
git checkout develop    # Changement de branche
git checkout -b devlop_bis  # Créer et change de branche
git branch  # Affiche les branches locales
git branch -va  # Affiche les branches locales et distantes
git checkout develop    #Changement de branche
echo "#/bin/bash" > script.sh
git add script.sh
git commit -m "modify script.sh"
git push origin develop
git checkout master # Changement de branche
git merge develop   # Fusion de la branche develop dans master
git branch -d develop_bis   # Supprimer une branche locale
git push origin --delete develop_bis    # Supprimer une branche distante
```

#### Reécriture de l'historique

La réécriture de l'historique des référentiels se fait en utilisant **git rebase -interactive**. En mettant le rebase en mode interactif, vous avez plus de contrôle sur les modifications que vous souhaitez apporter. Après le lancement en mode interactif, six commandes vous sont données pour chaque validation dans le référentiel.

Dans cet exemple, nous voulons changer le commit. Pour le mettre dans cet état, nous devons changer le mot "pick" à côté du commit pour qu'il corresponde à l'action que vous souhaitez effectuer en fonction de la liste affichée dans la fenêtre Vim, dans ce cas "reformuler".

##### Reword

```
git rebase -i --root
```

```
pick 23e26d2 first commit
pick cd08c24 Add .gitignore
pick 898f4a2 UPdate README.md
pick d7f9d0c remove REAME.md
pick 965fa4c Ajout de fichier
pick ac106ed Modify tuto.txt
pick d99ae1d Create tuto.txt
pick 4930601 Add test.sh
pick a016630 modify test.sh

# Rebasage de a016630 sur 4930601 (9 commandes)
#
# Commandes :
#  p, pick <commit> = utiliser le commit
#  r, reword <commit> = utiliser le commit, mais reformuler son message
#  e, edit <commit> = utiliser le commit, mais s'arrêter pour le modifier
#  s, squash <commit> = utiliser le commit, mais le fusionner avec le précédent
#  f, fixup <commit> = comme "squash", mais en éliminant son message
#  x, exec <commit> = lancer la commande (reste de la ligne) dans un shell
#  b, break = s'arrêter ici (on peut continuer ensuite avec 'git rebase --continue')
#  d, drop <commit> = supprimer le commit
#  l, label <label> = étiqueter la HEAD courante avec un nom
#  t, reset <label> = réinitialiser HEAD à label
#  m, merge [-C <commit> | -c <commit>] <label> [# <uniligne>]
#          créer un commit de fusion utilisant le message de fusion original
#          (ou l'uniligne, si aucun commit de fusion n'a été spécifié).
#          Utilisez -c <commit> pour reformuler le message de validation.
#
# Vous pouvez réordonner ces lignes ; elles sont exécutées de haut en bas.
#
# Si vous éliminez une ligne ici, LE COMMIT CORRESPONDANT SERA PERDU.
#
# Cependant, si vous effacez tout, le rebasage sera annulé.
#
```

On modifie le message de commit suivante  **pick a016630 modify test.sh**  :

```
...
reword a016630 modify test.sh
...
```

On enregistre et quitte et on affiche les modifications :

```
git log --oneline
```

##### Squash

Une alternative plus rapide pour modifier le dernier message de validation consiste à utiliser `git commit --amend`

Une série de 3 commits différents a été effectuée dans votre environnement local. À l'époque, ces engagements avaient du sens, mais ils ne doivent plus être qu'un seul commit. En utilisant Rebase, nous devons écraser les commits ensemble.

En utilisant git rebase --interactive HEAD~3, nous avons la plage de commits de HEAD au dernier 0. Pour écraser nous avons besoin d'un commit de base dans lequel tout sera écrasé. En tant que tel, laissez le premier commit sur "pick" mais changez le reste en "squash".

Après avoir enregistré, vous aurez la possibilité de modifier. Par défaut, le message de validation Git sera une combinaison des messages de validation précédemment écrasés.

```
echo "### Bom dia" >> README.md
git add README.md
git commit -m "Modify README.md"
echo "#### :stuck_out_tongue_winking_eye:" >> README.md
git add README.md
git commit -m "Modify README.md"
echo "#### :smiling_imp:" >> README.md
git add README.md
git commit -m "Modify README.md"
git rebase -i HEAD~3
```

```
pick b2df124 Modify README.md
pick aaa7398 Modify README.md
pick 73a8f53 Modify README.md
```

On modifie les pick en squash

```
pick b2df124 Modify README.md
squash aaa7398 Modify README.md
squash 73a8f53 Modify README.md
```

```
git log --oneline
```

```
b585e82 (HEAD -> master) Modify README.md
94c0e8f modify script.sh
4930601 Add test.sh
d99ae1d Create tuto.txt
ac106ed Modify tuto.txt
```

#### Diviser un commit

On souhaite maintenant diviser un commit en deux :

```
echo "#### :fire:" >> README.md
echo "echo Hola" >> script.sh
git add -A
git commit -m "Modify README.mm and script.sh"
git rebase -i HEAD~1
```

```
pick 90464ca Modify README.md and script.sh
edit 90464ca Modify README.mm and script.sh
```

```
git reset HEAD^
git add README.md
git commit -m "Update README.md"
git add script.sh
git commit -m "Update script.sh"
git rebase --continue
git log --oneline
```

#### SubModule

Il arrive souvent lorsque vous travaillez sur un projet que vous deviez utiliser un autre projet comme dépendance. Cela peut être une bibliothèque qui est développée par une autre équipe ou que vous développez séparément pour l’utiliser dans plusieurs projets parents. Ce scénario provoque un problème habituel : vous voulez être capable de gérer deux projets séparés tout en utilisant l’un dans l’autre.

Git gère ce problème avec les sous-modules. Les sous-modules vous permettent de gérer un dépôt Git comme un sous-répertoire d’un autre dépôt Git. Cela vous laisse la possibilité de cloner un dépôt dans votre projet et de garder isolés les commits de ce dépôt.

##### Démarrer un sous-module

```
git submodule add -b master https://github.com/Maxime-CLS/git-submodule-tutorial.git
```

Par défaut, les sous-modules ajoutent le sous-projet dans un répertoire portant le même nom que le dépôt.

```
cd git-submodule-tutorial/
echo "## tech-lunch :start:" >> README.md
git status
git add README.md
git commit -m "Update README.md"
git push origin master
cd ../
git status
git submodule update
```

#### Remote

```
git remote -v

origin	https://github.com/Maxime-CLS/git-tutorial.git (fetch)
origin	https://github.com/Maxime-CLS/git-tutorial.git (push)


git remote add client https://github.com/Maxime-CLS/git-tutorial-client.git
git remote -v

origin	https://github.com/Maxime-CLS/git-tutorial.git (fetch)
origin	https://github.com/Maxime-CLS/git-tutorial.git (push)
client	https://github.com/Maxime-CLS/git-tutorial-client.git (fetch)
client	https://github.com/Maxime-CLS/git-tutorial-client.git (push)


git push client master
```

#### Alias

```
git config --global alias.st status
git config --global alias.ci commit
git config --global alias.co checkout
git config --global alias.glog \
'log --graph --oneline --decorate --branches --tags --remotes'
```

## GitOps

### Terraform

[docs d'installation](https://learn.hashicorp.com/tutorials/terraform/install-cli)


**Ajouter** la clé GPG HashiCorp, **Ajouter** le référentiel Linux officiel HashiCorp, **Metter** à jour et **installer**.

```
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
terraform -help
terraform -install-autocomplete
```

Créez un répertoire nommé gitops-docker-demo et le fichier main.tf


```
mkdir gitops-docker-demo && cd $_
touch main.tf
```

Avec votre IDE, écrire la configruation suivante dans main.tf :

```
terraform {
  required_providers {
    docker = {
      source = "terraform-providers/docker"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "tutorial"
  ports {
    internal = 80
    external = 8000
  }
}
```

Initialiser le projet, qui télécharge un plugin qui permet à Terraform d'interagir avec Docker. Provisionner le conteneur de serveur NGINX avec apply. Lorsque Terraform vous demande de confirmer le type yes appuyer sur ENTER.

```
terraform init
terraform apply
```

Accéder au serveur web

```
http://localhost:8000/
```

Pour arrêter le conteneur, exécuter terraform destroy.

```
terraform destroy
```

Créer un dépôt Git pour vos configurations :

```
git init
vim .gitignore
```

```
.terraform
terraform*
```

```
git add -A
git ci "first commit"
git remote add origin https://github.com/Maxime-CLS/gitops-docker-demo.git
git push -u origin master
```

### Créer une modification de ma configuration


```
git checkout -b feature
```

```
terraform {
  required_providers {
    docker = {
      source = "terraform-providers/docker"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  count = 2
  image = docker_image.nginx.latest
  name  = "tutorial-${count.index+1}"
  ports {
    internal = 80
  }
}
```

```
terraform apply
```

```
git add main.tf
git ci "Add feature"
git push origin feature
```

```
git branch -D feature
docker ps
```

```
http://localhost:XXXXX/
http://localhost:XXXXX/
```

```
git revert HEAD
terraform apply
docker ps
```

```
http://localhost:XXXXX/
```