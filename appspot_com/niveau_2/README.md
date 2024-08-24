## NIVEAU 2 - [PERSISTENCE IS KEY](https://xss-game.appspot.com/level2)

### DESCRIPTION DE LA MISSION
<pre>
Les applications web conservent souvent les données des utilisateurs dans des bases de données côté serveur et,
de plus en plus, côté client, et les affichent ensuite aux utilisateurs.
Quelle que soit l'origine de ces données contrôlées par l'utilisateur, elles doivent être manipulées avec précaution.

Ce niveau montre à quel point les bogues XSS peuvent être facilement introduits dans des applications complexes.
</pre>

<br>

### OBJECTIF DE LA MISSION
<pre>
Injecter un script pour faire apparaître une [alert()] dans le contexte de l'application.
</pre>

> [!NOTE]
> <pre>
> L'application enregistre vos messages, donc si vous insérez du code pour exécuter l'alerte, ce niveau sera
> résolu à chaque fois que vous rechargerez l'application.
> </pre>

<br>

### INDICES

**Premier indice**:
> [!TIP]
> <pre>
> 1. Note that the "welcome" post contains HTML, which indicates that the template doesn't escape the contents of
> status messages.
> 1. Notez que le message de "bienvenue" contient du HTML, ce qui indique que le modèle n'échappe pas au contenu des
> messages d'état.
> </pre>

Vérifions cela dans le code source du cadre:

```html
<!doctype html>
<html>
  <head>
    <!-- Internal game scripts/styles, mostly boring stuff -->
    <script src="/static/game-frame.js"></script>
    <link rel="stylesheet" href="/static/game-frame-styles.css" />
    [...]

          html += '<b>You</b>';
          html += '<span class="date">' + new Date(posts[i].date) + '</span>';
          html += "<blockquote>" + posts[i].message + "</blockquote";
          html += "</td></tr></table>"
          containerEl.innerHTML += html; 

    [...]
        <td class="message-container">
          <div class="shim"></div>
          <form action="?" id="post-form">
            <textarea id="post-content" name="content" rows="2" 
              cols="50"></textarea>
            <input class="share" type="submit" value="Share status!">
            <input type="hidden" name="action" value="sign">
          </form>
        </td>
      </tr>
    </table>

  </body>
</html>
```

Bien, ce sera notre faille.

Ce qu'il faut comprendre de ce code, c'est qu'il y a une fonction de remplacement de code HTML: `containerEl.innerHTML`.
<br>Ce qui signifie que, quel que soit le script que vous entrerez dans le champ de post, il ne sera pas traité en l'état, car la fonction empêche l'utilisation des balises &lt;script&gt;.

<br>

**Deuxième indice**:
> [!TIP]
> <pre>
> 2. Entering a &lt;script&gt; tag on this level will not work. Try an element with a JavaScript attribute instead.
> 2. Utiliser une balise &lt;script&gt; sur ce niveau ne fonctionnera pas. Essayez un élément avec un attribut JavaScript
> à la place.
> </pre>


<br>

**Troisième indice**:
> [!TIP]
> <pre>
> 3. This level is sponsored by the letters i, m and g and the attribute onerror.
> 3. Ce niveau est sponsorisé par les lettres i, m et g et l'attribut onerror.
> </pre>

L'élément à garder dans ce troisième indice est l'attribut `onerror`.

<br>

### MISSION

Comme il ne nous est pas possible d'utiliser de balises &lt;script&gt;, nous utiliserons une balise &lt;img&gt;.

> [!NOTE]
> Peut importe ce que nous mettrons comme lien d'image puisque le but est précisément de générer une erreur, nous mettrons donc un simple `x`.

<br>Il est maintenant temps d'injecter le `payload` dans le champs de texte.

```
<img src='x' onerror='alert("ACCES AUTORISE !!")'>
```

<div align="center">
    <img
        src="https://github.com/AyckinnLisa/xss_game/blob/main/appspot_com/niveau_2/img/lvl2_hacked.png"
        style="width:100%">
</div>

Puis cliquez sur le bouton `Share status`.

<br>

### Explications

La page a essayé d'afficher l'image `x`. Comme cette image n'existe pas, elle a continué d'exécuter la commande, en l'occurence, l'alerte concernant une éventuelle erreur de chargement, ici, notre popup.

<br>

> [!IMPORTANT]
> <pre>
> Congratulations, you executed an alert:
>
> ACCES AUTORISE !!
>
> You can now advance to the next level.
> </pre>

<br>

> [!NOTE]
> Bravo, vous venez de passer le deuxième niveau. Vous pouvez maintenant passer au niveau suivant. 
