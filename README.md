# Installation Python x86_64 pour Mac M1

Ce tutoriel s'adresse aux personnes qui comme moi, se seraient heurtées à ce message d'erreur, en essayant d'installer un package `pip` (dans un environement virtuel ou non) sur leur tout nouveau Mac M1 :
![error_msg](https://drive.google.com/uc?export=view&id=1MDm-dYdFg8wE-FyLF5TiFXj4807Cm9rc)

En l'occurrence, dans mon cas je suis dans un environement Conda, et je cherche à installer un package uniquement disponible sur PyPi. Ce package n'étant pas encore disponible dans l'architecture ARM, `pip` ne parvient pas à le trouver et donc à l'installer. Pour ce faire, nous allons installer une version de Python compilée en architecture x86_64 plutôt que ARM pour pouvoir retravailler avec nos packages préférés comme avant ! 🤗

⚠️ **DISCLAIMER :**
Dans ce tutoriel, je ne prêche pas la vérité absolue mais simplement une solution de secours pour ceux qui comme moi, se seraient retrouvés confrontés à cette problématique. Je me suis inspiré librement de la solution proposée pour cette [question](https://stackoverflow.com/questions/68659865/cannot-pip-install-mediapipe-on-macos-m1) sur StackOverflow.

Par conséquent, je me déleste de toute responsabilité si jamais cette installation venait à vous causer un quelconque tort (puisque moi aussi je serais concerné par ce tort :upside_down_face:), mais me tiens disponible pour toute question complémentaire.

Aussi, ce tutoriel a été rédigé à date du 18/10/2021, à savoir avant la sortie de la nouvelle puce M1 Max. J'espère que ce tutoriel viendra a être obsolète et que les problèmes de compatibilités seront résolus dans un futur proche pour toutes les puces Apple Silicon. En attendant, vous pouvez toujours vous servir de ce tutoriel comme bon vous semble.

## (Optionel) Installer iTerm2

Ce n'est pas une obligation, mais ce terminal, c'est quand même le feu :fire:

## (Pré-requis) Avoir installé correctement `brew` et `miniforge`

Avant de commencer ce tutoriel, il est nécessaire d'avoir installé `brew` et `miniforge` sur votre Mac. Si cela a été fait correctement, vous devriez avoir ce dossier : `/opt/homebrew/bin` indiquant que `brew` a été installé dans le répertoire `/opt`.

Auquel cas, vous pouvez toujours vérifier le *path* en tapant la commande:

```bash
which -a brew
```

Si vous voyez `/opt/homebrew/bin` en tête de liste, tout va bien.

Si vous ne le voyez pas au début, vous avez deux choix :

- ajouter le path `/opt/homebrew/bin` au début du `PATH`

```bash
echo 'export PATH=/opt/homebrew/bin:$PATH' >> ~/.zshrc
```

- créer un alias `mybrew` (ou que sais-je `marcel` par exemple ?) qui va pointer vers l'exécutable dans `/opt/homebrew/bin`

```bash
echo 'alias mybrew=/opt/homebrew/bin/brew'
```

Et si rien ne s'affiche bah... Installez `brew` 🤷‍♂️

## Installer Python compilé en x86_64

### 1. Préparation du terminal

Dans cette section, je parle bien de l'application "Terminal" et non "iTerm2". Dans le doute, je vous suggère de séparer l'utilisation des deux. Pour ceux déjà sur iTerm2, aucun changement dans vos habitudes. Pour les autres, je ne sais pas si les changements auront un impact important pour vous par la suite. Si vous observez des difficultés à la fin de ce tutoriel annulez les changements de cette étape une fois les installations de brew et Python (x86_64) terminées.

Localisez l'application Terminal dans Finder sous le dossier `Applications>Utilities` (ou `Applications>Utilitaires` pour les plus réfractaires à l'anglais :smirk:) :

![utilities_terminal](https://drive.google.com/uc?export=view&id=1-Nt54D08gFRWfShDPFawtqzu6XAkhL83)

Si toutefois l'application était ouverte, fermez la (Cmd+Q). Ensuite faites un clic droit sur l'application Terminal, puis "Get info" (ou "Obtenir informations") :

<img src="https://drive.google.com/uc?export=view&id=1rbVgRpTaG0f0Z_4T9AeTYpkrFu5XhVvz" width="377" height="752">

Et cochez la case "Open using Rosetta" (ou "Ouvrir avec Rosetta").

### 2. Installation de `brew` compilé en x86_64

Et là vous me dites : "Mais quoi ?? Tu nous demande d'avoir `brew` d'installé correctement en pré-requis et là tu veux nous faire installer `brew`, c'est quoi cette embrouille ?!". Ce à quoi je vous réponds : "Oui.".

Tout simplement pour la simple et bonne raison qu'il vous faut une version de `brew` compilée en x86_64 pour installer Python compilé en x86_64.

Pour cela, ouvrez simplement le Terminal précédemment configuré, puis tapez la commande suivante :

```bash
arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### 3. Installation de Python

Logiquement, la version de `brew` que vous venez d'installer, devrait se trouver dans le dossier `/usr/local/homebrew/bin/brew` (ou en tout cas dans le dossier `/usr`). Encore une fois, la commande `which` est votre amie, sinon vous devriez voir le dossier d'installation apparaître dans les logs d'installation sur votre Terminal.

Si le chemin est bien celui décrit au-dessus, tapez cette commande :

```bash
arch -x86_64 /usr/local/homebrew/bin/brew install python@3.8
```

Sinon, changez le path vers l'executable du nouveau `brew` installé.

### 4. Créer un environement virtuel

**Note :** si vous souhaitez une utilisation plus simple que celle proposée dans cette partie, sautez cette étape et allez jeter un oeil à la [partie suivante](#pour-aller-plus-loin-et-vous-simplifier-la-vie-relieved).

Une fois l'installation de Python terminée (cela peut prendre plusieurs minutes), vous êtes désormais prêts à créer votre environement virtuel pour travailler avec cette version de Python.

Tapez donc la commande suivante, en remplaçant le nom `myvenv` par le path vers où vous souhaitez installer votre environement virtuel :

```bash
arch -x86_64 /usr/local/homebrew/opt/python@3.8/bin/python3 -m venv myvenv
```

Encore une fois, <u>vérifiez bien le chemin d'installation</u> du Python x86_64 lors de son installation et remplacez le dans la commande précédente.

Et voilà ! Vous êtes prêts à profiter pleinement de ce nouvel environement virtuel. Il est désormais accessible là où vous l'avez installé, et vous pouvez l'activer le désactiver avec les commandes habituelles :

```bash
# Activer votre environement virtuel
source path/to/myvenv/bin/activate
# Le désactiver (sans blague)
deactivate
```

## Pour aller plus loin et vous simplifier la vie :relieved:

### Installer `virtualenvwrapper`

Cette section s'adresse aux plus téméraires d'entre vous, et vous offre une solution intéressante pour utiliser vos environements virtuels sous `virtualenv`. On se rapproche de `conda` dans son utilisation mais avec ses particularités, que j'apprécie tout particulièrement.

#### Pourquoi `virtualenvwrapper` ?

L'avantage principal de cette extension, est qu'elle vous permet de gérer vos environements virtuels, de la même façon que `conda`, mais en y associant un dossier de travail. Lors de la création d'un "project", `virtualenvwrapper` vous crée un environement virtuel associé dans le répertoire où tous vos environements virtuels sont enregistrés, et en parallèle, crée un dossier de travail dans un dossier "projects", stipulé dans votre fichier `.zshrc`.

Dans la suite de cette section, je vous accompagnerai pour configurer `virtualenvwrapper` afin de vous faciliter la tâche avec cette extension de folie.

Lancez iTerm2 (ou Terminal) et tapez les commandes suivantes :

```bash
# Vérifier version/installation de pip
which -a pip # ou which -a pip3
# Vérifier version/installation de python
which -a python # ou which -a python3
```

Si vous obtenez un path pour chacune des deux commandes précédentes, tant mieux ! Vérifiez quand même que `pip` référence bien `pip3` et `python` référence Python 3 (par exemple tapez les dans le terminal).
Sinon c'est que vous avez un soucis d'installation avec votre pip/Python (essayez bien avec et sans le 3 pour `which`).

**Pour les utilisateurs de PyEnv**👇

Si vous avez déjà installé PyEnv correctement sur votre machine, vous ne devriez pas avoir de soucis lors de l'installation (en plus d'être sacrément smart d'utiliser ce gestionnaire de version Python 😉).

-----

**Pour les autres**👇

[Installez Pyenv](https://github.com/pyenv/pyenv#installation). Non plus sérieusement, PyEnv c'est franchement très sympa et hyper utile pour manipuler différentes versions de Python de la même manière que des environements virtuels. Rien ne vous y oblige bien sûr !

-----

Si tout fonctionne bien, vous pouvez lancer cette dernière commande :

```bash
pip install virtualenvwrapper # ou pip install virtualenvwrapper
```

### Configuration du fichier .zshrc

Dans un éditeur de texte (comme Nano ou Vim pour les puristes) ouvrez votre fichier de configuration `~/.zshrc` et ajoutez-y les lignes suivantes à la fin :

```bash
# Python x86_64
export ARCH_PYTHON="/usr/local/Homebrew/opt/python@3.8/bin/python3"

# Virtualenvwrapper
source /path/to/virtualenvwrapper.sh
```

En remplaçant le chemin vers le fichier `virtualenvwrapper.sh` selon son installation sur votre machine. Normalement il devrait être affiché au cours de son installation par `pip`. Pour les utilisateurs de PyEnv vérifier dans le dossier : `~/.pyenv/versions/3.9.7/bin/virtualenvwrapper.sh`.

Redémarrez votre terminal, et vous devriez avoir plusieurs messages lors du redémarrage comme quoi plusieurs script ont été instanciés. Réouvrez le fichier `.zshrc` et ajoutez les lignes à la suite dans la section Virtualenvwrapper :

```bash
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/Documents/projects
export VIRTUALENVWRAPPER_PYTHON=$(which python3)
export VIRTUALENVWRAPPER_VIRTUALENV=$(which virtualenv)
```

Ici j'ai spécifié mon dossier `projects` pour mon `PROJECT_HOME`, mais vous pouvez choisir n'importe quel dossier. Ce sera là où vos dossiers de travail associés aux environements virtuels seront créés.

<details>
<summary><u><b>Plugin ZSH</b></u></summary>
Trouvez la section des plugins zsh et ajoutez-y "virtualenvwrapper" comme ceci :

```bash
plugins=(
    ... # vos autres plugins
    virtualenvwrapper
)
```

Note : si toutefois lors du prochain redémarrage, votre terminal se ferme tout seul d'un coup, enlenvez le `virtualenvwrapper` de vos plugins.
</details>

Redémarrez une nouvelle fois votre terminal et...

### Enjoy!

Désormais, pour profiter pleinement de `virtualenvwrapper`, il vous suffit de mémoriser les quelques commandes associées :

- `mkproject venv`: crée un *project* nommé `venv` (environement virtuel + dossier de travail du même nom)
- `mkvirtualenv venv`: crée un environement virtuel `venv` isolé
- `rmvirtualenv venv`: supprime l'environement virtuel `venv`
- `lsvirtualenv`: liste vos environements virtuels situés dans `WORKON_HOME`
- `workon venv`: active un environement virtuel et vous déplace dans le dossier de travail associé (depuis n'importe où !)
- `deactivate`: désactive l'environement virtuel actif (depuis n'importe où !)

Avec ce set-up de malade, vous pouvez maintenant créer votre environement virtuel avec la version Python compilée x86_64 (ou la version Python par défaut sur votre machine !) en utilisant la commande :

```bash
mkproject -p $ARCH_PYTHON myvenv
```

Et c'est là que la variable qu'on a ajouté plus haut prend tout son sens. Si vous avez besoin d'un environement virtuel sous Python x86_64, vous pouvez spécifier directement la version de cette dernière depuis n'importe quel terminal. Si l'option `-p` n'est pas spécifiée, l'exécutable Python spécifié dans `VIRTUALENVWRAPPER_PYTHON` sera utilisé.
