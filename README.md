# Retranscription Grafikart-CRUD

## Qu'est-ce que le CRUD?

Le CRUD est un framework. Lorsque nous créons un site web, nous voulons pouvoir avoir une gestion des contenus, les afficher, les modifier et les supprimer: c'est ce que l'on appelle le CRUD.

## Ruby on Rails

Ruby on rails a une convention sur la méthodologie à adopter pour les URL et les méthodes HTTP.

Pour la création d'un CRUD, nous donnons un nom à notre **'resources'**

  ```Exemple avec: resources :articles```

|HTTP Verb| Path |Controller#Action| Utilisé pour|
|--|--|--|--|
| GET | /articles | articles#index | obtenir la liste de tous les articles|
| GET | /articles/new | articles#new | obtenir un formulaire html pour créer un nouvel article|
| POST | /articles | articles#create | créer un nouvel article|
| GET | /articles/:id | articles#show | obtenir la page html de l'article en question|
| GET | /articles/:id/edit | articles#edit | obtenir un formulaire html pour éditer un article en particulier|
| PATCH/PUT | /articles/:id | articles#update | mettre à jour un article en particulier|
| DELETE | /articles/:id | articles#destroy | supprimer un article en particulier|


### Création des routes

La première étape va être de créer les différentes URL qui correspondent à ce CRUD.

Il existe un raccourcis sur Ruby on Rails, dans le fichier  ```router.rb``` vous pouvez utiliser la méthode ```resources```, suivi du ```nom de la resource```. 
```Exemple: resources :articles```. Cela a pour effet de créer tout un système de CRUD automatiquement.

Pour lister les routes qui se sont créées, il suffit de taper dans le terminal la commande: ```$rails routes```.

![enter image description here](https://zupimages.net/up/20/23/7os1.png)

Pour tester la route de la page index de nos articles, il suffit de rentrer dans le navigateur l'adresse suivante: ```localhost:3000/articles```.

La partie ```/articles``` nous envoie sur le ```controller Articles``` et sur l'action ```index``` de celui-ci.

Exemple d'action ```index``` qui récupère tous les articles.
![enter image description here](https://zupimages.net/up/20/23/m83v.png)

### Visualiser un article
Si l'on veut voir un article en particulier, nous pouvons commencer par créer un bouton qui nous emmènera vers celui-ci, en lui indiquant le bon chemin (path) et son id (article.id).

    <a href="<%= article_path(article.id)%>" class="btn">Voir l'article</a>

Lorsque l'on clique sur ce bouton, nous sommes re-dirigés vers la page show de l'article avec pour URL ```localhost:3000/articles/:id``` avec l'id correspondant à l'id de l'article.

Pour l'instant cela nous indique que la page n'existe pas. Il faut en effet aller la créer dans le ```controller Articles``` en ajoutant l'action ```show```.

![enter image description here](https://zupimages.net/up/20/23/le3a.png)

Puis créer la page HTML de la vue show avec un fichier ```show.html.erb```.

Exemple:
![enter image description here](https://zupimages.net/up/20/23/v1ub.png)

### Editer un article
Maintenant si on veut éditer un article, il faut créer l'action ```edit``` dans le ```controller Articles```.

![enter image description here](https://zupimages.net/up/20/23/2k0q.png)

Puis créer la page HTML de la vue edit avec un fichier ```edit.html.erb``` où on y ajoutera un formulaire. Pour cela, nous allons nous aider du **FormHelper** de Rails. 
Pour générer un formulaire qui correspond à un model en particulier, il y a une méthode qui s'appelle form_for. Cette méthode prend en paramètre l'enregistrement que vous souhaitez modifier (ici @article) et un block qui aura comme paramètre un **FormBuilder**, qui est un objet qui va nous permettre de générer les différents champs.

Exemple:
![enter image description here](https://zupimages.net/up/20/23/uav9.png)

Lorsque l'on soumet le formulaire, il nous renvoie une erreur en nous disant qu'il ne trouve pas l'action ```update```.
Il faut donc aller créer cette méthode au niveau du ```controller Articles```.

![enter image description here](https://zupimages.net/up/20/23/84cn.png)

Pour pouvoir modifier un article à partir de la page index, nous pouvons rajouter un bouton qui nous emmènera vers le formulaire d'édition. Ce bouton aura comme attribut ```href``` le chemin correspondant à la route d'édition d'un article:

    <a href="<%= edit_article_path(article.id)%>" class="btn">Editer l'article</a>


### Créer un article
Si on veut pouvoir créer un nouvel article, il faut aller créer l'action dans le ```controller Articles```:

![enter image description here](https://zupimages.net/up/20/23/yebu.png)

Ensuite, pour créer la vue de création d'un article, nous pouvons commencer par ajouter un bouton sur la page index:

    <a href="<%= new_article_path %>" class="btn">Créer un article</a>
    
Avec comme chemin, la route vers la création d'un article.

Et enfin, on termine avec la création de  la vue qui lui correspond: ```new.html.erb``` en récupérant le formulaire de l'édition.

Lorsque l'on soumet le formulaire de création, il nous renvoie de nouveau une erreur car il ne trouve pas l'action ```create``` que l'on va tout de suite créer dans le ```controller Articles```.

![enter image description here](https://zupimages.net/up/20/23/i5u5.png)
On peut voir qu'on se répète entre la ```def update``` et de la ```def create``` avec le  ``` post_params = params.require(:article).permit(:title, :content)```.
Le mieux est de créer une méthode qui permet de faire l'opération ci-dessus.

On va donc créer une méthode privée, ce qui donnera ceci:

![enter image description here](https://zupimages.net/up/20/23/zttn.png)

### Supprimer un article

Pour finir, nous allons créer la méthode pour supprimer un article. Pour cela nous devons créer un formulaire qui appelle la méthode DELETE.

Il existe un raccourcis qui permet de créer des liens qui vont avoir une méthode.

Grâce aux librairies injectées par Rails, notamment le javascript ``` jquery_ujs```, qui nous permet de rajouter des fonctionnalités à l'HTML à travers l'attribut ```data```. Par exemple,  ```data-confirm="êtes-vous sur?"```, nous affiche un message d'alerte.
Dans notre cas, nous voulons que notre lien soit posté avec une méthode particulière ```DELETE```, nous pouvons donc utiliser l'attribut: ```data-method="DELETE"```.

Au niveau du ```controller Articles```, il faut créer la méthode ```destroy```:

![enter image description here](https://zupimages.net/up/20/23/w22p.png)

Après toutes ces étapes, nous avons mis en place un CRUD tout simplement.

## Second exemple
Nous allons revoir les étapes avec une nouvelle migration.

 1. Création d'une table:
 
 ```$rails g migration CreateCategories name:string slug:string```
 
 2. Faire la migration:
 
 ```$rails db:migrate```
 
 3. Création d'un model:
 
 ```$rails g model Category --skip```  (```--skip``` permet de sauter la migration qui est faite juste avant).
 
 4. Création du controller et de ses méthodes:
 
 ```$rails g controller Categories index show new create edit update destroy```
 
 5. Ajouter les routes dans le fichier routes.rb:
 
```resources :categories```

 6. Remplir toutes les méthodes dans le controller Categories:
 
![enter image description here](https://zupimages.net/up/20/23/9f52.png)

 7. Création des pages HTML dans le dossier ```app/views/categories```:
 
  ``` index.html.erb```
  
  ```show.html.erb```
  
  ```new.html.erb ``` qui est un formulaire.
  
  ```edit.html.erb``` qui est un formulaire.

 8. Création des boutons qui renvoient vers les différentes pages ou actions:
  
```<a href="<%= new_category_path %>" class="btn">Créer une catégorie</a>```
    
```<a href="<%= category_path(category.id) %>" class="btn">Voir la catégorie</a>```
  
```<a href="<%= edit_category_path(category.id) %>" class="btn">Modifier la catégorie</a>```
  
``` <a href="<%= category_path(category.id) %>" class="btn" data-confirm="êtes-vous sur?" data-method="DELETE">Supprimer la catégorie</a>```



## Conlusion

Comme nous pouvons le constater, la mise en place d'un CRUD est plutôt générique.

Il existe donc une commande qui nous génère tout cela automatiquement: ```scaffold```

Exemple avec la création d'un User:

  ```$rails g scaffold User username:string bio:text```

Cela va créer:
- une migration
- les tests
- le controller
- les différentes vues qui correspondent

Puis faire:
  ```$rails db:migrate```

Nous avons maintenant accès à une nouvelle URL: ```localhost:3000/users```
qui nous permet de gérer les utilisateurs. Toutes les fonctionnalités sont présentes. Il suffit juste de rajouter un peu de style à nos pages.

## Mission THP Delmas/Dupuy
