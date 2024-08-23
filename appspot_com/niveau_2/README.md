## [**NIVEAU 2 - PERSISTENCE IS KEY**](https://xss-game.appspot.com/level2)

Il s’agit, pour ce niveau, de faire la même chose que dans l’exercice précédent, à savoir, injecter un script qui va générer un popup d’alerte.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_2/img/lvl2_home.png"
        style="width:80%">
</div>

<br>Il est à noter que l’application WEB enregistre tous les posts donc si vous insérez du code pour exécuter l'alerte, ce niveau sera résolu à chaque fois que vous rechargerez l'application.

Regardons le premier indice:

<pre>
1. Note that the "welcome" post contains HTML, which indicates that the template doesn't escape the contents of status messages.

1. Notez que le message de "bienvenue" contient du HTML, ce qui indique que le modèle n'échappe pas au contenu des messages d'état.
</pre>

Vérifions cela dans le code source du cadre:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_2/img/lvl2_html_code.png"
        style="width:80%">
</div>

Bien, ce sera notre faille.
<br>Ce qu'il faut comprendre de ce code, c'est que les posts ne passent par aucun filtre. autrement, dit, nous pouvons utiliser n'importe qu'elle méthode pour attaquer la page.

Voyons le deuxième indice:

<pre>
2. Entering a script tag on this level will not work. Try an element with a JavaScript attribute instead.

2. Utiliser une balise script sur ce niveau ne fonctionnera pas. Essayez un élément avec un attribut JavaScript à la place.
</pre>

Cet indice est suffisamment clair, je ne m'étends pas dessus.

Regardons le troisième indice:

<pre>
3. This level is sponsored by the letters i, m and g and the attribute onerror.

3. Ce niveau est sponsorisé par les lettres i, m et g et l'attribut onerror.
</pre>

L'élément à garder dans ce troisième indice est l'attribut `onerror`.

Il est maintenant temps d'injecter le `payload` dans le champs de texte:

```
<img src='x' onerror='alert("Hacked... again!")'>
```

On clique sur le bouton `Share status`:

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_2/img/lvl2_hacked.png"
        style="width:80%">
</div>

#### Explications

La page a essayé d'afficher l'image `x`. Comme cette image n'existe pas, elle a continué d'exécuter la commande, en l'occurence, l'alerte concernant une éventuelle erreur de chargement, ici, notre popup.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_2/img/lvl2_complete.png"
        style="width:80%">
</div>

Bravo, vous venez de passez le second niveau. Vous pouvez maintenant passer au niveau suivant. 
