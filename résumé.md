# Résumé de ce qu'on a fait avec Charlie

Bon voilà tu vas voir c'est pas si compliqué que ça, je sais dis comme ça c'est nul mais tu constatera par toi même que c'est un peu toujours pareil, cest toujours le même shéma qu'on applique.

## <u>Objectif</u> *( du moins ce que nous on a compris qu'il fallait faire ) ​*

- Installer Lumen
- Importer la base de donné SQL
- Créer les templates/layout (ton html qui saffiche sur ta page)
- Créer les Controller en fonction du fichier qui été donné Route.md
- Créer tes routes
- Afficher la liste des quizz en page daccueil ( on fait une boucle en allant chercher les elements de la BDD)
- Bonus: toi qui est bon en font, tu peux t'amuser avec le Css et intégrer boostrap

### Installer Lumen 

- Tu clone ton classroom, et a l'intérrieur de ton classroom tu ouvre un terminal et tu tape ça :
- `composer create-project --prefer-dist laravel/lumen Oquizz`

### Importer la BDD

- Bon la j'imaginque que tu sais faire,mais  **petite précision**, tu crées manuellement une base de donné Oquizz ( en utf8), et ensuite en la selectionant tu importe d'abord ton fichier import-oquiz.sql puis après import-data-oquiz.sql. Ne t'embête pas avec le 3 eme fichier c'était facultatif.

## Avant de commencer

- dans ton Arborescence il faut configurer ton **.env**  

  APP_KEY= (*pour ça je te file un générateur de pass* ) https://passwordsgenerator.net/

  DB_DATABASE= *Nom de ta base de donnée*
  DB_USERNAME= *Nom D'utilisateur*
  DB_PASSWORD= *Pass de la BDD*

- Dans ton dossier **bootstrap** il y a un fichier **app.php**, dedans tu dois décommenter:

  `$app->withFacades();`

  `$app->withEloquent();` *ça permet d'activer Facade Et Eloquent des fonction de Lumen*

## Créer les templates

Pour ça c'est le plus simple, Dans **resources/views** tu créer un dossier **layout** avec ton **header et footer** les parties de ton html qui restent statiques. Moi j'ai mis également mon Dossier CSS (mais c'est peu etre pas la qu'il est recommandé de le mettre, peu importe il faudra juste faire attention au lien de ton css dans le header) ! Maintenant **dans ton dossier views tu as besoin de créer t'es page php** qui <u>seront affichés en fonction de tes routes</u>. Pour les connaitres il faut aller dans le fichier **<u>route.md</u>** 

**ex:** *home.php / account.php / tag.php / quizz.php/  ...*

## Créer les Controller

Pour connaitre la liste des controller que l'on va utiliser idem il faut regarder dans le fichier **route.md**

Donc dans **HTTP/Controllers** <u>tu créer t'es nouveaux controllers</u>: *MainController.php / QuizController.php / UserController.php et TagController.php*



<u>Par exemple</u> pour ton MainController qui d'après le fichier route.md ne contien que ta page daccueil ;



`<?php`

`namespace App\Http\Controllers;`

`use App\Models\Quizzes;`

`use Illuminate\Http\Request;`

`use Illuminate\Support\Facades\DB;`

`use Log;`



`class MainController extends Controller`

`{`

​    	`public function homeAction()`

​    `{`

​         `return view('home');`

​    `}`



`}`



ici on a tout le début début du code avec **namespace et les uses** que <u>tu peux copier coller a chaque Controller que tu créer</u>, c'est les include 2.0 en quelques sortes.

Ensuite tu **ouvres ta class MainController** que <u>tu vas toujours Extends avec Controller</u>, a l'intérrieur c'est tout simple tu as une fonction **HomeAction** <u>qui a pour le moment pour seul utilisé de te retourner ta view ('home')</u>, mais celle si n'existe pas encore car **il faut maintenant créer la route** ! ça c'est l'étape d'apres ! 



*ps: le use App\Models\Quizzes; tu verra plus bas a quoi il correspond*



## Créer la route 

Maintenant que ton <u>MainController te redirige vers une route</u> il faut bien la créer la route! Pour ça on va dans **routes/web.php**

La c'est vraiment simple :

`<?php`

` $router->get('/', [`

​    `'as' => 'home',`

​    `'uses' => 'MainController@homeAction'`

`]);`



<u>Voila l'exemple pour ta page home</u> que tu as appelé a l'aide de **return view('home')** dans ton **MainController**.

Ici ton **$router->get('/'**, te permet de <u>designer l'adresse de ta route</u>, ici vu que c'est la racine on met juste un slash, si ça avait été account on aurait mis */account*

Ton **'as' = >** <u>c'est le nom que tu donnes a ta route</u>, celle que tu demandes a afficher dans ton **MainController** 

Ton **'uses' =>** bah c'est simplement **le Controller associé avec en @ la fonction correspondante** ici homeAction





A ce stade il faut que tu comprennes que c'est relativement simple et qu'**une fois que tu as compris le cycle c'est toujours pareil**. Tu nas plus que créer les autres functions en faisant <u>attention de les mettres dans les bons Controlers</u> ( toujours le fichier route.md ) 



## Afficher la liste des articles sur ta page daccueil via la BDD



Une fois que tu as créés toutes tes routes et t'es Controllers, il va te valoir créer des **Models** histoire de pouvoir commencer a faire des trucs cool avec php ( allez je te taquine :D )



Dans ton dossier **app** tu créer un dossier **Models** (attention a la maj). Dans tu créer ton premier model ici ça sera quizzes.php. Ton Model il va te servir a quoi ? D'après ce que j'ai compris c'est ton model qui va permettre de déclarer ta table 



`<?php`

`namespace App\Models;`

`use Illuminate\Database\Eloquent\Model;`

`class Quizzes extends Model`

`{`

​      `protected $table = 'quizzes';`

`}`



Voila rien de plus Un ptit **namespace et un use** des familles en début de code  que tu pourra recopier a ton prochain model ensuite <u>ta class que tu nommes</u> (ici Quizzes) et <u>on oublie pas le extends Model</u> ( si tu demande a quoi ça corresponds te pose pas la question ça fait parti du code du framework que des gens se sont prit la tete de faire a ta place )

ensuite le plus important **protected** **$table=** *"nom de ta table dans la base de donné"



Voila pour le model c'est callé! en gros ton model (si j'ai bien tout compris) il sert a déclarer ta table sous forme d'objet ! 



maintenant c'est bien de déclarer une table dans un model  mais il faut en faire quelque chose. Déjà pour le moment premier question a ce poser c'est qu'est ce qui me permet d'afficher ma page ? mon Controller. Dans quel controller je vais avoir besoin de cette table ? Ici on veut afficher la liste des quizz sur la page daccueil donc on a vu que notre fonction homeAction qui nous permet d'aller en page daccueil ce situe dans le MainController! Donc  il nous suffit juste de rajouter quelques infos a notre fonction HomeAction.



Pour commencer on va lui declarer une variable qui aura pour role de selectionner toute la base de donné déclaré dans notre model 



$quizzList = Quizzes::all();      // ps : Quizzes écrit comme ça corresponds au nom de la class du Model.



donc ta homeAction deviens : 



 `public function homeAction()`

​    `{`

​        `$quizzList = Quizzes::all();`

​        `//dump($quizzList);`

​        `return view('home', [`

​            `'quizzes' => $quizzList`

​          `]);`

​    `}`

Tu peux dumper le $quizzList pour mieux le comprendre 



Maintenant pour que ça donne quelque chose de concret on va venir s'en servir, il faut que tu modifie ton fichier html ou les quizz sont ecrit en dur dans le code et boucler en php en te servant de lobjet qu'on vient de creer ( désolé j'ai encore un peu de mal avec les notions objets et tableau mais lidée est la )



 `<?php foreach ($quizzes as $quizz) : ?>`

`<?= $quizz->title ?>`

`<?= $quizz->description ?>`

  `<?php endforeach; ?>`



Voilà j'espère que ça peu t'aider a comprendre un peu, même si on devait aller un peu plus loin c'est déjà bien si tu arrives a comprendre la base! 