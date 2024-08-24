## NIVEAU 3 - [THAT SINKING FEELING...](https://xss-game.appspot.com/level3)

### DESCRIPTION DE LA MISSION

<pre>
Comme vous l'avez vu au niveau précédent, certaines fonctions JS courantes sont des puits d'exécution, ce qui signifie 
qu'elles entraînent l'exécution par le navigateur de tous les scripts qui apparaissent dans leur entrée.
Parfois, ce fait est caché par des API de niveau supérieur qui utilisent l'une de ces fonctions sous le capot.

L'application à ce niveau utilise un de ces puits cachés.
</pre>

<br>

### OBJECTIF DE LA MISSION

<pre>
Comme précédemment, injectez un script pour faire apparaître un alert() JavaScript dans l'application.
</pre>

<br>

> [!NOTE]
> <pre>
> Puisque vous ne pouvez pas entrer votre charge utile n'importe où dans l'application, vous devrez modifier
> manuellement l'adresse dans la barre d'URL ci-dessous.
></pre>

<br>

### INDICES

**Premier indice**:
> [!TIP]
> <pre>
> 1. To locate the cause of the bug, review the JavaScript to see where it handles user-supplied input.
> 1. Pour localiser la cause du bug, examinez le JavaScript pour voir où il traite les données fournies par 
> l'utilisateur
> </pre>

**Deuxième indice**:
> [!TIP]
> <pre>
> 2. Data in the window.location object can be influenced by an attacker.
> 2. Les données de l'objet window.location peuvent être influencées par un attaquant.
> </pre>

**Troisième indice**:
> [!TIP]
> <pre>
> 3. When you've identified the injection point, think about what you need to do to sneak in a new HTML element.
> 3. Quand vous aurez identifié le point d'injection, réfléchissez à ce qu'il faut faire pour introduire un nouvel 
> élément HTML.
> </pre>

**Quatrième indice**:
> [!TIP]
> <pre>
> 4. As before, using &lt;script&gt; as a payload won't work because the browser won't execute scripts added after 
> the page has loaded.
> 4. Comme précédemment, l'utilisation de &lt;script&gt; comme charge utile ne fonctionnera pas car le navigateur n'exécute
> pas les scripts ajoutés après le chargement de la page.
> </pre>

<br>

### MISSION

Regardons, maintenant, les photos. On peut voir que l'ancre de l'URL change à chaque photo... logique.

<pre>
https://xss-game.appspot.com/level3/frame#1
https://xss-game.appspot.com/level3/frame#2
https://xss-game.appspot.com/level3/frame#3
</pre>

C'est donc après l'ancre que nous allons injecter le code.

Mais avant, nous allons essayer de comprendre un peu la logique de l'application. Pour ce faire, cliquez sur `Target code (toogle)` juste en dessous de la page.
<br>Cela nous affiche le code source de la page `index.html`. En analysant ce code, nous pouvons observer ceci:

```html
<script>
      function chooseTab(num) {
        // Dynamically load the appropriate image.
        var html = "Image " + parseInt(num) + "<br>";
        html += "<img src='/static/level3/cloud" + num + ".jpg' />";
        $('#tabContent').html(html);
 
        window.location.hash = num;
 
        [...]
</script>
```

"En quoi c'est interéssant" me direz-vous ?
<br>Nous pouvons constater que le paramètre `num` est utilisé pour afficher l'image choisie par l'utilisateur. L'idée va donc être de casser les guillemets et d'inserer le script à la place du nom de l'image.

<br>

> [!TIP]
>Si vous n'êtes pas familier avec les langages de programmation, sachez que `num` (dans ce cas) est un paramètre variable. Cela signifie que le programme ne sait pas, à l'avance, quelle valeur afficher puisqu'elle dépend du choix de l'utilisateur au >moment 'T'.

C'est précisément ce paramètre variable qui va nous servir de faille.

Dans la mesure où l'attribut `onerror` a bien fonctionné dans l'exercice précédent, nous allons le réutiliser dans l'URL:

```
https://xss-game.appspot.com/level3/frame#3' onerror='alert("ACCES AUTORISE !")';
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
> Bravo, vous venez de passer le troisième niveau. Vous pouvez maintenant passer au niveau suivant. 