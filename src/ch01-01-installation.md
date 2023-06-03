# Installation

La première étape consiste à installer Cairo. Nous allons télécharger Cairo manuellement en utilisant le dépôt Cairo ou avec un script d'installation. Vous aurez besoin d'une connexion Internet pour le téléchargement.

### Prérequis

Tout d'abord, vous devrez avoir Rust et Git installés.

```bash
# Install stable Rust
rustup override set stable && rustup update
```

Installer [Git](https://git-scm.com/).

## Installer Cairo avec un script ([Installer](https://github.com/franalgaba/cairo-installer) by [Fran](https://github.com/franalgaba))

### Installer

Si vous souhaitez installer une version spécifique de Cairo plutôt que la dernière version disponible, définissez la variable d'environnement CAIRO_GIT_TAG (par exemple, export CAIRO_GIT_TAG=v1.0.0).

```bash
curl -L https://github.com/franalgaba/cairo-installer/raw/main/bin/cairo-installer | bash
```

Après l'installation, suivez [ces instructions](#set-up-your-shell-environment-for-cairo) pour configurer votre interface utilisateur.

### Mise à jour

```
rm -fr ~/.cairo
curl -L https://github.com/franalgaba/cairo-installer/raw/main/bin/cairo-installer | bash
```

### Désinstaller

Cairo est installé dans $CAIRO_ROOT (par défaut : ~/.cairo). Pour le désinstaller, il vous suffit de le supprimer :

```bash
rm -fr ~/.cairo
```

Ensuite, supprimez ces trois lignes du fichier .bashrc :

```bash
export PATH="$HOME/.cairo/target/release:$PATH"
```

et enfin, redémarrez votre interface utilisateur : 

```bash
exec $SHELL
```

### Configurez votre interface utilisateur pour Cairo.

- Définissez la variable d'environnement CAIRO_ROOT pour indiquer le chemin où Cairo stockera ses données. Par défaut, il s'agit de $HOME/.cairo. Si vous avez installé Cairo via une vérification Git, nous vous recommandons de la définir sur le même emplacement que celui où vous l'avez cloné.
- Ajoutez les exécutables `cairo-*` à votre variable PATH si ce n'est pas déjà fait.

La configuration ci-dessous devrait fonctionner pour la grande majorité des utilisateurs dans des cas d'utilisation courants.

- Pour **bash**:

 Les fichiers de démarrage Bash peuvent varier considérablement selon les distributions quant à la manière dont
   ils incluent les autres fichiers, dans quelles circonstances, dans quel ordre et quelle configuration supplémentaire ils effectuent
  Par conséquent, la manière la plus fiable d'obtenir Cairo dans tous les environnements consiste à ajouter les commandes 
  de configuration de Cairo à la fois dans le fichier .bashrc (pour les shells interactifs) 
  et dans le fichier de profil que Bash utiliserait (pour les shells de connexion).

  Tout d'abord, ajoutez les commandes à ~/.bashrc en exécutant la commande suivante dans votre terminal :

  ```bash
  echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.bashrc
  echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.bashrc
  ```

  Ensuite, si vous avez les fichiers ~/.profile, ~/.bash_profile ou ~/.bash_login, ajoutez également les commandes à l'un de ces fichiers.
  Si vous n'avez aucun de ces fichiers, ajoutez-les à ~/.profile.

  - pour ajouter à `~/.profile`:

    ```bash
    echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.profile
    echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.profile
    ```

  - pour ajouter à `~/.bash_profile`:
    ```bash
    echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.bash_profile
    echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.bash_profile
    ```

- Pour **Zsh**:

  ```zsh
  echo 'export CAIRO_ROOT="$HOME/.cairo"' >> ~/.zshrc
  echo 'command -v cairo-compile >/dev/null || export PATH="$CAIRO_ROOT/target/release:$PATH"' >> ~/.zshrc
  ```

  Si vous souhaitez utiliser Cairo également dans les shells de connexion non interactifs, ajoutez également les commandes à ~/.zprofile ou ~/.zlogin.

- Pour **l'interface Fish**:

  Si vous utilisez Fish 3.2.0 ou une version plus récente, exécutez ceci de manière interactive :

  ```fish
  set -Ux CAIRO_ROOT $HOME/.cairo
  fish_add_path $CAIRO_ROOT/target/release
  ```

  Sinon, exécutez le code ci-dessous :

  ```fish
  set -Ux CAIRO_ROOT $HOME/.cairo
  set -U fish_user_paths $CAIRO_ROOT/target/release $fish_user_paths
  ```

Sur MacOS, vous voudrez peut-être également installer [Fig](https://fig.io/), qui fournit des complétions alternatives pour de nombreux outils en ligne de commande avec une interface contextuelle de type IDE dans la fenêtre du terminal.
(Notez que leurs complétions sont indépendantes de la base de code de Cairo, il se peut donc qu'elles ne soient pas tout à fait à jour pour les changements d'interface les plus récents.)

### Redémarrez votre interface.

afin que les modifications de `PATH` prennent effet.

```sh
exec "$SHELL"
```

## Installer Cairo manuellement ([Guide](https://github.com/auditless/cairo-template) par [Abdel](https://github.com/abdelhamidbakhta))

### Étape 1 : Installer Cairo 1.0

Si vous utilisez un système Linux x86 et que vous pouvez utiliser la version binaire de sortie, téléchargez Cairo à partir d'ici : <https://github.com/starkware-libs/cairo/releases>.

Pour tous les autres, nous recommandons de compiler Cairo à partir des sources de la manière suivante :

```bash
# Start by defining environment variable CAIRO_ROOT
export CAIRO_ROOT="${HOME}/.cairo"

# Create .cairo folder if it doesn't exist yet
mkdir $CAIRO_ROOT

# Clone the Cairo compiler in $CAIRO_ROOT (default root)
cd $CAIRO_ROOT && git clone git@github.com:starkware-libs/cairo.git .

# OPTIONAL/RECOMMENDED: If you want to install a specific version of the compiler
# Fetch all tags (versions)
git fetch --all --tags
# View tags (you can also do this in the cairo compiler repository)
git describe --tags `git rev-list --tags`
# Checkout the version you want
git checkout tags/v1.0.0

# Generate release binaries
cargo build --all --release
```

.

**NOTE : Maintenir Cairo à jour**

Maintenant que votre compilateur Cairo se trouve dans un dépôt cloné, tout ce que vous avez à faire est de récupérer les dernières modifications et de reconstruire comme suit :

```bash
cd $CAIRO_ROOT && git fetch && git pull && cargo build --all --release
```

### Étape 2 : Ajouter les exécutables Cairo 1.0 à votre chemin d'accès (PATH)

```bash
export PATH="$CAIRO_ROOT/target/release:$PATH"
```

**NOTE : Si vous installez à partir d'une version binaire Linux, adaptez le chemin de destination en conséquence.**

### Étape 3 : Configuration du serveur de langage (Language Server)

#### Extension pour VS Code

- Désactiver l'ancienne extension Cairo 0.x
- Installez l'extension Cairo 1 pour une mise en évidence syntaxique adéquate et une navigation dans le code.
  Suivez simplement les étapes indiquées [ici](https://github.com/starkware-libs/cairo/blob/main/vscode-cairo/README.md).

#### Serveur de langage Cairo

Grâce à l'[Étape 1](#step-1-install-cairo-10-guide-by-abdel), le binaire `cairo-language-server` doit être compilé et l'exécution de cette commande copiera son chemin dans votre presse-papiers.

```bash
which cairo-language-server | pbcopy
```

Mettez à jour le `languageServerPath` de l'extension Cairo 1.0 en collant le chemin.
