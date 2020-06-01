# LIRE UNE DOC/TROUVER UNE INFORMATION SUR INTERNET

## Utilité de ce document

Dans le quotidien d'un développeur, trouver des informations à propos de tels langages/libraries/frameworks est commun, et le faire efficacement est une compétence qui s'acquiert avec le temps. Malgré tout, je penses qu'il y a une manière efficace de le faire qui n'est pas forcément si évidente, et ce document essaie de montrer, pas à pas, comment faire.

## Contribuer

Si vous repérez une faute d'orthographe, une tournure floue, avez une proposition d'amélioration, quelque à ajouter/retirer, ou tout simplement avez une question, ouvrez une issue/PR !

## Avant de commencer

J'utiliserais google et mozilla firefox dans ce document, si vous utilisez quelque chose de différent, il est possible qu'il y ai de petits changement à faire pour adapter certaines choses (n'hésitez pas à contribuer pour compléter pour d'autres moteurs de recherches/navigateurs)

### TROUVER UNE INFORMATION SUR UN MOTEUR DE RECHERCHE A PARTIR D'UN MESSAGE D'ERREUR

Supposons que je sois en train d'apprendre le Python, et que j'essaie de résoudre un exercice. Voici mon code actuel:

```py
print("a" + 1)
```

Malheureusement, lorsque j'essaie de le lancer, l'interpréteur Python n'a pas l'air d'accord avec moi:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```
Première étape ? Commençons par analyser le message d'erreur. Première ligne:

> Traceback (most recent call last):

Okay, donc, d'abord `Traceback`. Qu'est ce que ça veut dire ? Google va nous le dire ! 
Utilisons Google Traduction. Coup de chance, en tapant simplement `traduction` sur Google, on tombe sur Google Traduction:

![_](https://raw.githubusercontent.com/shika-blyat/lire_une_doc/master/assets/traceback_trad.JPG)

Okay, donc Python nous dit qu'il remonte la trace en mettant le `most recent call` (appel le plus récent) en `last` (dernier). J'imagines qu'il remonte la trace du message d'erreur ?
Soit, continuons:

> File "main.py", line 1, in <module>

Okay, donc dans le fichier `main.py`, à la ligne `1`. Bien, on avance, on sait où se trouve l'erreur

> TypeError: can only concatenate str (not "int") to str

`Type Error` Mmh, erreur de type ? Keskessé ? Concatenate ? Mmh, demandons à google, copions le message et ajoutons python devant, ainsi:
`python TypeError: can only concatenate str (not "int") to str`

Premier link que nous renvoie google: https://stackoverflow.com/questions/51252580/getting-a-typeerror-can-only-concatenate-str-not-int-to-str une question stack overflow, un forum anglophone de développeur très connu et qui vous aidera souvent quand vous avez un problème :p. Regardons le contenu de ce link:
> Python working a bit differently to JavaScript for example, the value you are concatenating needs to be same type, both int or str... 

Mmh, okay :). Donc visiblement on ajoute une valeur de type str et une valeur de type int ensemble. D'accord, on avance. La suite montre un code qui ressemble beaucoup au notre et qui lève la même erreur. Génial il nous dis même comment résoudre le problème ! Il suffit de convertir le int en str, avec un appel à `str`. Essayons:
```py
print("a" + str(1))
```
résultat: `a1`
Ca fonctionne !

#### Conclusion

- Lorsqu'on ne connaît pas un mot ou deux dans un message d'erreur, on le traduit, et on essaie de lui donner du sens dans le contexte
- Lorsqu'on ne pige rien du tout à plusieurs phrases, on essaie de demander à google si y'a pas quelqu'un qui a déjà posé la question pour nous et quelqu'un qui y a déjà répondu (ça sera très souvent le cas)

### TROUVER UNE INFORMATION DANS UNE DOCUMENTATION

J'ai trouvé comment résoudre les différents que mon cours proposait, et je l'ai fini. Je me sens assez confiant en Python pour faire un petit projet, un bot discord. Je vais utiliser la libraire discord.py pour cela. Mais, petit problème, je ne sais pas par où commencer. Cherchons rapidement sur internet `discord.py tutorial`. Le premier link nous mène à:
https://discordpy.readthedocs.io/en/latest/quickstart.html qui semble donner un bout de code "template" pour commencer.

```py
import discord

client = discord.Client()

@client.event
async def on_ready():
    print('We have logged in as {0.user}'.format(client))

@client.event
async def on_message(message):
    if message.author == client.user:
        return

    if message.content.startswith('$hello'):
        await message.channel.send('Hello!')

client.run('your token here')
```

Ca fonctionne ! Mais bon, j'aimerais ping la personne en répondant `Hello!`, ça serait bête qu'elle ne voit pas le message de mon bot, sauf que je ne sais pas comment ping une personne avec discord.py. Tapons `discord.py documentation` sur google :). Le link de la documentation en question: https://discordpy.readthedocs.io/en/latest/
Bon bah génial, mais j'en fais quoi de cette documentation ? J'vais commencer par essayer de taper `mention` dans la barre de recherche à gauche, vu que ça m'a l'air d'être la meilleure chose à faire.

Oula oula, c'est quoi tous ces résultats, faisons le tri. Qu'est ce que nous avons nous ? On a un `client.user` et un `message.author`. Parmis tous les résultats, j'en vois deux qui m'intéresse:
`discord.Role.mention` et `discord.User.mention`. En fait, non, après réflexion je n'ai qu'un user, pas un rôle. Cliquons donc sur `discord.User.mention`, et regardons les informations que ça nous donne:
![_](https://raw.githubusercontent.com/shika-blyat/lire_une_doc/master/assets/disc_mention_doc.JPG)

Je vois que c'est de type string, et que ça permet visiblement de mentionner l'user en question. Par contre, je ne sais pas si c'est une méthode ou un attribut. Dois je faire `user.mention` ou `user.mention()` ? Essayons:

```py
    if message.content.startswith('$hello'):
        await message.channel.send(client.user.mention + ' Hello!')
```
Oh, ça fonctionne !

### Conclusion

- Lorsqu'on ne connaît pas une lib, un bon réflexe c'est d'avoir un onglet avec la doc de la lib en question ouvert dans son navigateur
- Un peu de jugeote permet souvent d'éliminer la majorité des résultats et de faire le tri
- La majorité des documentations ont un outil de recherche intégré. Parfois il fonctionne bien, parfois `<lib> <mot clé>` dans google donnera de meilleurs résultats que utiliser l'outil de recherche de la doc

