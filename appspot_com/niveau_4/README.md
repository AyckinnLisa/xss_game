### NIVEAU 4 - [CONTEXT MATTERS](https://xss-game.appspot.com/level4)

<pre>
Chaque donnée fournie par l'utilisateur doit être correctement échappée dans le contexte de la page dans laquelle
elle apparaîtra. Ce niveau montre pourquoi.

Objectif de la mission
Injecter un script pour faire apparaître un alert() JavaScript dans l'application.
</pre>

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_4/img/lvl4_home.png"
        style="width:100%">
</div>

La page génère un timer pour lequel vous choisissez la durée, passé ce délai, un popup apparaît pour vous signaler la fin du décompte.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_4/img/lvl4_timer_demo.png"
        style="width:100%">
</div>

**Premier indice**

<pre>
1. Take a look at how the startTimer function is called.
1. Regardez comment la fonction "startTimer est appelée."
</pre>

**Second indice**

<pre>
2. When browsers parse tag attributes, they HTML-decode their values first. &lt;foo bar='z'&gt; is the same as 
&lt;foo bar='&amp#x7a;'
2. Lorsque les navigateurs analysent les attributs des balises, ils décodent d'abord leurs valeurs en HTML. 
&lt;foo bar='z'&gt; est la même chose que &lt;foo bar='&amp#x7a;'
</pre>

**Troisième indice**

<pre>
3. Try entering a single quote (') and watch the error console.
3. Essayez d'entrer un guillement simple (') et observez la console d'erreur.
</pre>

Ouvrons la console du navigateur. Clic droit sur la page puis `Inspecter...` (suivant le navigateur), puis, cliquez sur `Console`.
<br>L'indice nous dit d'essayer d'entrer un simple guillement en tant que valeur pour le timer:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_4/img/lvl4_simple_quote.png"
        style="width:100%">
</div>

Nous pouvons voir que cela génère une erreur dans la console, de plus, le compteur tourne indéfiniement, logique puisqu'il n'a pas de valeur numérique pour démarrer le décompte.

Voyons à quoi fait référence cette erreur:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_4/img/lvl4_error_console.png"
        style="width:100%">
</div>

<br>Il semblerait que l'erreur se produise à la ligne 21, cliquez sur `frame:21`, ce qui, normalement, vous place directement sur la ligne concernée:

```html
1   <!doctype html>
2   <html>
3     <head>
4       <!-- Internal game scripts/styles, mostly boring stuff -->
5       <script src="/static/game-frame.js"></script>
6       <link rel="stylesheet" href="/static/game-frame-styles.css" />
7
8       <script>
9         function startTimer(seconds) {
10          seconds = parseInt(seconds) || 3;
11          setTimeout(function() { 
12              window.confirm("Time is up!");
13              window.history.back();
14          }, seconds * 1000);
15        }
16      </script>
17    </head>
18    <body id="level4">
19      <img src="/static/logos/level4.png" />
20      <br>
21      <img src="/static/loading.gif" onload="startTimer('&#39;');" />
22      <br>
23      <div id="message">Your timer will execute in &#39; seconds.</div>
24    </body>
25  </html>
```

L'idée va donc être de placer le payload dans la fonction `startTimer`. 
<br>Essayons le script dans la barre d'adresse, le champ de saisie du timer ne fonctionne pas:

```html
https://xss-game.appspot.com/level4/frame?timer=');alert("ACCES AUTORISE !");('
```

Hum... Cela n'a pas d'autre effet que faire tourner le timer indéfiniement.

<br>Il s'agit donc de comprendre ce qui ne va pas dans ce script.
<br>Après plusieurs essais et de multiples recherches, j'ai essayé en remplaçant le `;` par son code URL : `%3B`:

```html
https://xss-game.appspot.com/level4/frame?timer=')%3Balert("ACCES AUTORISE !")%3B('
```

> [!TIP]
> - Le signe `'` va générer une erreur puisque le compteur attend une valeur numérique. C'est cette erreur que nous exploitons avec notre alerte.
> - Les `;` semblant être interdits dans la barre d'adresse (contrairement à l'exercice précédent), nous n'avons pas d'autres choix que de les encoder: `%3B`.
> - `(')` Permet de continuer l'exécution de la commande et lance le timer.

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
> Bravo, vous venez de passez le quatrième niveau. Vous pouvez maintenant passer au niveau suivant. 
