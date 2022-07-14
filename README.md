# Kivy
Cross-platform Python framework for NUI development.
<br/>

## Installation
Pour utiliser ce framework, il vous suffit d'exécuter la commande
suivante :

```sh
# si ton OS est Ubuntu 20.04 et plus, install d'abord python3-pip
sudo apt install python3-pip
sudo pip3 install kivy
```

## Utilisation
Voici un exemple de code écrit en servant du framework. Ce code 
doit être mit dans un fichier que tu dois nommer `main.py`.

```python
from kivy.app import App
from kivy.uix.gridlayout import GridLayout
from kivy.uix.label import Label
from kivy.uix.image import Image
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput


class SayHello(App):
    def build(self):
        self.window = GridLayout();
        self.window.cols = 1;
        self.window.size_hint = (0.6, 0.7);
        self.window.pos_hint  = {
            "center_x": 0.5,
            "center_y": 0.5
        }

        # create widget
        self.image = Image(source='logo.png');
        self.greeting = Label(
            text="Yammah's Virtual World",
            font_size=18,
            color='#00FFCE',
        );
        self.user     = TextInput(
            multiline=False,
            padding_y=(20, 20),
            size_hint=(1, 0.5),
        );
        self.button   = Button(
            text="GREET",
            size_hint=(1, 0.5),
            bold=True,
            background_color='#00FFCE',
        );
        self.button.bind(on_press=self.callback);

        # add widgets to window
        self.window.add_widget(self.image);
        self.window.add_widget(self.greeting);
        self.window.add_widget(self.user);
        self.window.add_widget(self.button);
        return self.window;

    def callback(self, instance):
        self.greeting.text = "Hello " + self.user.text + "!";


if __name__ == '__main__':
    app = SayHello();
    app.run();

```

Pour exécuter, écrit simplement la commande suivant :

```sh
python3 main.py
```

>**NOTE** :<br>
> Au fait, si je t'ai suggérer de nommer le fichier principal avec 
> `main.py` pour des raisons mensionnées beaucoup plus bas.


## Compilation sous Android
Ici, on va tenter de générer le `.apk` de notre application réalisée
dans la section précédente. Pour ce faire, suit moi attentivement, et ne ratte aucune étape. Je tiens à t'avertir que j'utilise
actuellement l'OS `Ubuntu 20.04.3 LTS` pour buider notre `.apk`.
Du coup, je ne te garenti rien, si tu utilise un autre OS différent.<br>
Donc, c'est partie !

### Installation des dépendances
```sh
# quelque lib (obligatoire)
sudo apt install git cython autoconf openjdk-13-jdk

# quelque lib de dev (obligatoires)
sudo apt install build-essential libltdl-dev libffi-dev libssl-dev python-dev

# compression et décompression
sudo apt install zip unzip

# enfin install le module python de cython
sudo pip3 install --upgrade cython
```

### Installation de `buildozer`
Buildozer est un outil qui automatise l'ensemble du processus de compilation. Il télécharge et configure tous les prérequis pour `python-for-android`, y compris le `SDK` et le `NDK android`, puis génère un `.apk` qui peut être automatiquement poussé vers l'appareil (Android).

```sh
# clonner le projet buildozer avec git
git clone https://github.com/kivy/buildozer.git

# Entre dans le dossier de buildozer
cd ./buildozer

# Install le avec la commande suivante
sudo python3 setup.py install

```

Maintenant, vas à la racine de ton projet pour initialiser les
configuration de buildozer.

```sh
# Vas dans ton projet sur le terminal
cd ./monappli

# Exécute cette commande pour initialiser la config de buildozer
buildozer init

```

### Compilation
Maintenant, il est temps qu'on compile notre `.apk`. Très simplement,
exécute la commande suivante :

```sh
sudo buildozer android debug
```

Si tout s'est bien passé, tu pourras récupérer ton `.apk` dans le 
dossier `bin` créé automatiquement par la commande précédente à la 
racine de ton projet.

Voici à peut prêt à quoi ressemble mon dernier log sur mon terminal:

```sh
[DEBUG]:        
[DEBUG]:        > Task :compileDebugJavaWithJavac
[DEBUG]:        warning: [options] source value 7 is obsolete and will be removed in a future release
[DEBUG]:        warning: [options] target value 7 is obsolete and will be removed in a future release
[DEBUG]:        warning: [options] To suppress warnings about obsolete options, use -Xlint:-options.
[DEBUG]:        Note: Some input files use or override a deprecated API.
[DEBUG]:        Note: Recompile with -Xlint:deprecation for details.
[DEBUG]:        Note: Some input files use unchecked or unsafe operations.
[DEBUG]:        Note: Recompile with -Xlint:unchecked for details.
[DEBUG]:        3 warnings
[DEBUG]:        
[DEBUG]:        Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
[DEBUG]:        Use '--warning-mode all' to show the individual deprecation warnings.
[DEBUG]:        See https://docs.gradle.org/6.4.1/userguide/command_line_interface.html#sec:command_line_warnings
[DEBUG]:        
[DEBUG]:        BUILD SUCCESSFUL in 2m 58s
[DEBUG]:        25 actionable tasks: 25 executed

[INFO]:    <- directory context /home/dmk/Documents/repositories/kivy/.buildozer/android/platform/python-for-android
[INFO]:    Of the existing distributions, the following meet the given requirements:
[INFO]:         myapp: min API 21, includes recipes (hostpython3, libffi, openssl, sdl2_image, sdl2_mixer, sdl2_ttf, sqlite3, python3, sdl2, setuptools, six, pyjnius, android, kivy, certifi), built for archs (armeabi-v7a, arm64-v8a)
[INFO]:    myapp has compatible recipes, using this one
[INFO]:    # Copying android package to current directory
[INFO]:    # Android package filename not found in build output. Guessing...
[INFO]:    # Found android package file: /home/dmk/Documents/repositories/kivy/.buildozer/android/platform/build-arm64-v8a_armeabi-v7a/dists/myapp/build/outputs/apk/debug/myapp-debug.apk
[INFO]:    # Add version number to android package
[INFO]:    # Android package renamed to myapp-debug-0.1-.apk
[DEBUG]:   -> running cp /home/dmk/Documents/repositories/kivy/.buildozer/android/platform/build-arm64-v8a_armeabi-v7a/dists/myapp/build/outputs/apk/debug/myapp-debug.apk myapp-debug-0.1-.apk
WARNING: Received a --sdk argument, but this argument is deprecated and does nothing.
No setup.py/pyproject.toml used, copying full private data into .apk.
Applying Java source code patches...
Applying patch: src/patches/SDLActivity.java.patch
# Android packaging done!
# APK myapp-0.1-arm64-v8a_armeabi-v7a-debug.apk available in the bin directory

```
Et ça marche !
