## [**NIVEAU 1 - HELLO, WORLD OF XSS**](https://xss-game.appspot.com/level1)

Dans ce premier exercice, il s’agit d’injecter un script qui va générer une alerte de type `popup`.

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/Niveau_1/img/lvl1_home.png"
        style="width:100%">
</div>

<br>Pour ce faire, il est possible d’utilser la barre d’adresse ou la barre de recherche dans la page directement.

Nous choisirons cette dernière option.

Nous allons injecter le script d’alerte comme suit :

```
<script>alert("hacked");</script>        
```

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/Niveau_1/img/lvl1_injected_script.png"
        style="width:100%">
</div>

Une fois le code écrit, on clique sur `Search` pour l’envoyer.
<br>Bravo, vous venez de passez le premier niveau. Vous pouvez maintenant passer au niveau suivant. 

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/Niveau_1//img/lvl1_complete.png"
        style="width:100%">
</div>

<br>Voici les indices pour ce niveau :

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/Niveau_1//img/lvl1_hint.png"
        style="width:100%">
</div>