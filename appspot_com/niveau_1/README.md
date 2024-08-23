## [**NIVEAU 1 - HELLO, WORLD OF XSS**](https://xss-game.appspot.com/level1)

Dans ce premier exercice, il s’agit d’injecter un script qui va générer une alerte de type `popup`.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_1/img/lvl1_home.png"
        style="width:80%">
</div>

<br>Pour ce faire, il est possible d’utilser la barre d’adresse ou la barre de recherche dans la page directement.

Nous choisirons cette dernière option.

Mais avant, regardons le **premier indice**:

<pre>
1. To see the source of the application you can right-click on the frame and choose View Frame Source from the context menu or use your browser's developer tools to inspect network traffic.
1. Pour voir la source de l'application, vous pouvez cliquer avec le bouton droit de la souris sur le cadre et choisir "Afficher la source du cadre" dans le menu contextuel ou utiliser les outils de développement de votre navigateur pour inspecter le trafic réseau.
</pre>

**Second indice**:

<pre>
2. What happens when you enter a presentational tag such as &lt;h1&gt;?
2. Que se passe-t-il lorsque vous saisissez une balise de présentation telle que &lt;h1&gt; ?
</pre>

**Troisième indice**:

<pre>
3. Alright, one last hint: &lt;script&gt; ... alert ...
3. Très bien, un dernier indice: &lt;script&gt; ... alert ...
</pre>

Nous allons injecter le script d’alerte ci-dessous dans le champs de saisie:

```
<script>alert("ACCES AUTORISE !");</script>        
```

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_1/img/lvl1_injected_script.png"
        style="width:80%">
</div>

Une fois le code écrit, on clique sur `Search` pour l’envoyer.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_1//img/lvl1_complete.png"
        style="width:80%">
</div>

<br>Bravo, vous venez de passez le premier niveau. Vous pouvez maintenant passer au niveau suivant. 





