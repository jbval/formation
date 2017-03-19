Formation Angular (v1.4) - Exercices
------------------------------------

## AVANT DE DÉMARRER LES EXERCICES

### Logiciels

Les logiciels suivants doivent être installés sur votre machine :

- [Node.js](https://nodejs.org/) et npm (versions minimum : node v4.x.x et npm 3.x.x).
- IDE qui supporte TypeScript, par exemple [WebStorm](https://www.jetbrains.com/webstorm/download/) (ou équivalent).
- Navigateur web moderne, par exemple Google Chrome.

Si ce n'est pas le cas, installez-les.

### angular-cli et fichiers

Installer [angular-cli](https://github.com/angular/angular-cli) globalement sur votre machine :

```bash
$ npm install -g angular-cli
```

Téléchargez le fichier [EXO_STARTER.zip](EXO_STARTER.zip), qui contient :
- Le support de cours en PDF.
- Les fichiers pour faire les exercices.



## EXO 0 : Votre première appli Angular

### Créer une appli angular-cli standard

```bash
$ ng new superquiz
$ cd superquiz

# Supprimer le dépôt git
# Équivalent Windows: rmdir /s /q .git
$ rm -rf .git
```

Dans le répertoire `superquiz`, **remplacez** le répertoire `src/app` par le répertoire `src/app` fourni dans EXO_STARTER_PLB.zip.

Ensuite, lancez la compilation et le serveur de développement avec cette commande :

```bash
$ ng serve
```

Ouvrez votre navigateur à l'adresse http://localhost:4200/. Voyez-vous le texte "app works!" ?

Ouvrez le répertoire de l'application dans votre IDE. Si vous modifiez le texte `'app works!'` situé dans `src/app/app.component.ts` puis enregistrez, voyez-vous le texte modifié apparaître automatiquement dans votre navigateur ?

### Personnaliser l'appli angular-cli

Installer [Bootstrap CSS](http://getbootstrap.com/) :

```bash
$ npm install bootstrap --save-dev
```

Charger la CSS de BootStrap en ajoutant cette ligne dans le fichier `.angular-cli.json` :

```json
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.css",   // <!-- ajouter ça
  "styles.css"
],
```

Et ajouter ce code dans `src/styles.css` :

```css
body {
  padding-top: 60px;
  padding-bottom: 30px;
}
```

### Comprendre l'organisation du code

Nous avons vu que le fichier `main.ts` est le point d'entrée dans l'application Angular. `main.ts` charge un **module** qui charge un **composant**. Pouvez-vous dérouler le chemin exact suivi par le code à partir de `main.ts` ?

Observez le code, et essayez d'expliquer ce qu'est `AppModule` ?

Observez le code, et essayez d'expliquer ce qu'est `AppComponent` ?

Angular est donc parti de `main.ts` pour initialiser l'application. Mais comment cette application (constituée de code JavaScript) est-elle reliée à la page `index.html` ? Quel est le lien entre les deux ?



## EXO 1 : Comprendre TypeScript

**1) Identifier les syntaxes TypeScript**

Lister les endroits où les syntaxes TypeScript suivantes ont été utilisées dans l'application :

- Classes
- Décorateurs - Comment Angular utilise les décorateurs ?
- Modules
- Qu'est-ce qui rend un symbole importable ? Quels sont les 2 types d'import qu'on trouve dans l'appli ?
- Types TypeScript

**2) Utiliser d'autres fonctionnalités de TypeScript**

Modifiez le code de `AppComponent` pour utiliser les fonctionnalités suivantes du langage TypeScript :

- Ajouter une **méthode** `updateTitle()` dans `AppComponent`. Cette méthode devra être déclenchée quand on clique sur un bouton et changer la valeur de la propriété `title`. Voici le code HTML du bouton à mettre dans le template : `<button (click)="updateTitle()">Change le titre</button>`
- Au lieu de coder le titre en dur dans `AppComponent`, récupérez-le via une classe. Dans le fichier de `AppComponent`, créer une **simple classe** `DataService` avec une méthode `getTitle()` qui renvoie le titre.
- Au lieu d'utiliser un template externe pour le composant (propriété `templateUrl`), utilisez un template en ligne (propriété `template`) avec la syntaxe **template-chaîne**.
- Ajouter des **types TypeScript** partout où c'est possible.

CHALLENGE : Déplacez la classe `DataService` dans son propre fichier (en continuant à l'utiliser dans `AppComponent`).

**3) Créer un 2e composant**

Toujours dans le fichier `app.component.ts`, essayez de créer un 2e composant `NavigationComponent` qui affiche un menu de navigation (utilisez du HTML bidon). Vous afficherez ce composant dans le template de `AppComponent`.

BONNE PRATIQUE : À quoi faut-il faire attention **à chaque fois** qu'on crée un nouveau composant ?



## EXO 2 : Imbriquer des composants (navbar, footer)

### Créer la structure générale de page

Toutes les pages de notre application seront organisées en 3 blocs :
1. Barre de navigation
2. Contenu
3. Pied de page

La partie "contenu" changera dynamiquement en fonction des actions utilisateur, mais la navigation et le pied de page resteront fixes.

Pour afficher cette structure navigation/contenu/pied de page, commencez par copier le HTML suivant dans le template de `AppComponent`, et vérifiez que l'affichage est correct.

```html
<!-- Fixed navbar -->
<nav class="navbar navbar-inverse navbar-fixed-top">
  <div class="container">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand">SuperQuiz</a>
    </div>
    <div id="navbar" class="navbar-collapse collapse">
      <ul class="nav navbar-nav">
        <li><a href="#">Accueil</a></li>
        <li><a href="#">Quizzes</a></li>
        <li><a href="#">Admin</a></li>
        <li><a href="#">Login</a></li>
      </ul>
    </div><!--/.nav-collapse -->
  </div>
</nav>

<!-- Contenu de la page -->
<div class="container">
  <h1>Bienvenue sur SuperQuiz</h1>
</div>

<!-- Pied de page -->
<footer>
  <div class="container">
    <p class="text-center">&copy; Copyright 2017</p>
  </div>
</footer>
```

### Refactoriser l'affichage

On ne veut pas afficher tout le HTML dans UN SEUL composant.

Sortez le HTML de la **barre de navigation** dans un composant `NavbarComponent`, et le HTML du **pied de page** dans `FooterComponent`.

Vous créerez ces composants avec angular-cli :

```bash
# Le préfixe `common/` permet de créer les composants dans un dossier "common".
$ ng g component common/navbar
$ ng g component common/footer
```

Bien-sûr, il faudra ensuite ré-afficher ces composants dans `AppComponent` grâce à leur balise.

### Penchons-nous sur `ng g component`

Listez les tâches réalisées par la commande `ng g component`.

Quels sont les pour et les contre d'utiliser cette commande ?



## EXO 3 : Afficher la liste des quizzes (`QuizListComponent`)

Commençons à coder le SuperQuiz.

Notre premier écran va afficher la **liste de tous les quizzes** pour permettre à l'utilisateur de choisir celui auquel il veut répondre.

- Créez un composant `QuizListComponent` avec angular-cli, et affichez-le dans le template de `AppComponent`.
- Copiez le HTML ci-dessous dans le template de `QuizListComponent`.
- À la place des quizzes "en dur", affichez les quizzes que vous trouverez dans la variable `QUIZZES` du fichier `data/quizzes.ts` (il suffit d'importer cette variable dans votre composant).

```html
<h2>Liste des quizzes</h2>
<p>Sélectionner un quiz :</p>
<ul>
  <li><a href="#">Quiz #1</a></li>
  <li><a href="#">Quiz #2</a></li>
  <li><a href="#">Quiz #3</a></li>
</ul>
```

Dans un prochain exercice, nous permettrons à l'utilisateur de cliquer sur un quiz pour le démarrer.



## EXO 4 : Afficher le détail d'une question + réponses possibles (`QuizQuestionComponent`)

Imaginons qu'un utilisateur soit en train de répondre à l'une des questions d'un quiz.

Il voit **la question et les réponses possibles**.

Pour générer cet écran, nous allons utiliser un composant **déjà créé** `QuizQuestionComponent` (fichier `quiz-question.component.ts`). TOUT L'EXERCICE SE FERA DANS CE COMPOSANT.

**1) Commencez par afficher le composant**

Affichez `QuizQuestionComponent` dans le template de `AppComponent` afin de visualiser vos modifs.

Dans ce template, vous pouvez supprimer la balise `<quiz-list></quiz-list>` correspondant à l'exercice précédent.

**2) Déclarez les propriétés du composant**

- `question` - La question en cours.
- `answer` - La réponse en cours (c. à d. les choix sélectionnés).
- `isAnswered` - Un flag true/false indiquant si la question a déjà reçue une réponse.
- `isSubmitted` - Un flag true/false indiquant si la réponse a déjà été soumise.

Avez-vous pensé à indiquer le type de chaque propriété ?

**3) Initialisez les propriétés du composant**

Utilisez la méthode `initState()` (appelée depuis le constructeur) pour affecter une valeur initiale aux 4 propriétés déclarées à l'étape précédente : `question`, `answer`, `isAnswered`, et `isSubmitted`.

Pour `question` et `answer`, vous utiliserez des données en dur pour le moment :

```ts
this.question = new Question({
  "id": 12,
  "questionType": "multiple_choice",
  "title": "En quelle année AngularJS (première version) est-il sorti ?",
  "choices": [
    { "text": "2010", "isCorrect": true },
    { "text": "2011"},
    { "text": "2012"},
    { "text": "2013"}
  ]
});
this.answer = new Answer({questionId: 12});  // réponse "vierge"
```

**4) Affichez la question et récoltez le ou les choix sélectionné(s)**

- Aux endroits appropriés du template, affichez la question en cours et ses différents choix possibles.
- Affichez le message "Plusieurs réponses possibles" uniquement si la question accepte plusieurs réponses (`question.multipleChoicesAllowed == true`).
- Quand on clique sur un choix, il doit s'afficher en surbrillance (en ajoutant la classe `active` sur la balise `<a>` du choix). Si on le reclique, il s'éteint. Si la question accepte plusieurs réponses, il faut pouvoir sélectionner plusieurs choix. Pour ces comportements, vous utiliserez les méthodes `clickChoice()` et `isChoiceSelected()` du composant.

**5) Gérez la soumission de la réponse**

Désactiver le bouton "Soumettre" si l'utilisateur n'a pas fait au moins un choix (`<button [disabled]="true">`).

Au clic sur "Soumettre" :
- Loggez la réponse en cours dans la console.
- Changez la valeur de `isSubmitted`.
- Changer le libellé et la couleur du bouton "Soumettre" en fonction de la réponse :
  - Libellé "CORRECT" + couleur verte (classe `btn-success`) si la réponse est correcte,
  - Libellé "INCORRECT" + couleur rouge (class `btn-danger`) si la réponse est incorrecte.

INDICE: Les objets de type `Answer` possèdent une propriété `isCorrect` qui passe automatiquement à true ou false en fonction des choix sélectionnés. Il vous suffit de tester la valeur de cette propriété pour savoir si la réponse en cours est correcte.

**6) Gérez le scénario "question déjà répondue"**

En effet, ce composant sera utilisé aussi bien pour afficher une question PAS ENCORE répondue qu'une question DÉJÀ répondue (par ex, si l'utilisateur revient en arrière dans le quiz).

Dans la méthode `initState()`, ajoutez un bloc `if... else` qui couvre les 2 scénarios :
1. La question n'a pas encore de réponse. C'est tout bon, c'est le code que vous venez de faire.
2. La question a déjà une réponse. C'est le code à ajouter maintenant : dans ce scénario, initialisez l'état du composant avec les valeurs adéquates, et adaptez le reste du code à ce cas de figure.

**IMPORTANT. Que signifie "avoir déjà une réponse" ?** Notre code sera plus simple à structurer si la propriété `this.answer` contient TOUJOURS une instance de `Answer`. Pour savoir si une question est déjà répondue, il suffit de tester que `this.answer.choices` n'est pas vide, c. à d. qu'au moins un choix a été ajouté à la réponse.

----

POUR TESTER : Changez les valeurs de la question et de la réponse initiales pour vérifier que votre composant les affiche correctement.



## EXO 5 : Utiliser le pattern smart/dumb pour afficher les questions

### Créer le `QuizPlayerComponent` (smart)

Créez un composant `QuizPlayerComponent` avec angular-cli.

Ce composant est "smart", c'est lui qui va déterminer le **quiz en cours**, la **question en cours**... et qui transmettra ces données à ses enfants.

Pour l'instant, contentez-vous d'initialiser deux propriétés avec des données en dur :
- `question` (question en cours) 
- `answer` (réponse en cours)

### Refactoriser le `QuizQuestionComponent` (dumb)

Le `QuizQuestionComponent` est devenu "dumb". Il communique avec l'extérieur uniquement via des inputs/outputs.

- Transformez en **inputs** les propriétés `question` (question en cours) et `answer` (réponse en cours). ATTENTION, cela signifie que ces propriétés ne doivent plus être initialisées dans la méthode `initState()` du composant, mais en recevant des données depuis le composant parent.
- Les propriétés `isAnswered` et `isSubmitted` sont toujours initialisées dans le composant, mais la méthode `initState()` doit maintenant être appelée depuis `ngOnInit()` car elle utilise les valeurs des inputs.
- Créez un **output** `onSubmitAnswer` qui transmettra au composant parent la réponse en cours lors du clic sur le bouton "Soumettre".

### Relier les 2 composants, smart et dumb

Dans le template du composant parent `QuizPlayerComponent` :
- Passez les variables `question` et `answer` aux inputs du `QuizQuestionComponent`.
- Réagissez à l'événement `onSubmitAnswer` de l'enfant en appelant une méthode qui logge dans la console la réponse reçue.



## EXO 6 : Afficher un quiz entier, c. à d. toutes les questions (`QuizStateManager`)


Dans cet exercice, on va permettre à l'utilisateur de passer d'une question à l'autre, et on va récolter les réponses soumises au fur et à mesure.

On suppose que l'utilisateur **a déjà choisi un quiz précis**.

Fonctionnalités souhaitées :
- Le premier écran affiché contient le titre et la description du quiz, et un bouton "Démarrer".
- Au clic sur "Démarrer", la première question du quiz est affichée.
- L'utilisateur peut choisir une réponse et la soumettre, puis cliquer "Suivant" pour passer à la question suivante.
- Le score et la position de l'utilisateur dans le quiz son calculés automatiquement.
- Il peut toujours revenir en arrière en cliquant sur "Précédent".
- À la fin du quiz, l'utilisateur peut soumettre l'ensemble de ses réponses.


### Relier le quiz player au `QuizStateManager`

Le quiz player doit maintenant utiliser le service `QuizStateManager` (déjà codé pour vous) pour **gérer l'état de l'application**.

Voici les tâches à effectuer dans `QuizPlayerComponent` :

1) Injectez le service `QuizStateManager` dans le quiz player grâce à l'injection de dépendance.

2) À l'initialisation du quiz player, définissez le quiz en cours. Pour cela, appelez `QuizStateManager.setQuiz()` et passez-lui le quiz `QUIZ_WITH_QUESTIONS` issu du fichier `data/quiz-with_questions.ts`.

*À terme, on récupérera l'id du quiz à afficher dans l'URL et on le chargera depuis la base de données.*

3) Déclarez les propriétés permettant de gérer l'état de l'application :

```ts
quizPlaying: boolean = false;              // true if the "Start" button has been clicked
currentQuiz: Observable<Quiz>;             // Quiz being played
currentQuestion: Observable<Question>;     // Question being displayed
currentAnswer: Observable<Answer>;         // Answer for the question being displayed
currentAnswers: Observable<AnswersState>;  // All answers submitted so far, keyed by questionId
```

Vous instancierez les observables ci-dessus avec les différentes valeurs renvoyées par le `QuizStateManager`, par exemple :

```ts
this.currentQuiz = quizStateManager.getCurrentQuiz();
```

### Afficher l'écran de bienvenue

Dans le `QuizPlayerComponent` :

En haut du template, affichez le titre et la description du quiz en cours :

```html
<!-- NB. Remplacez le texte statique par des variables -->
<h2>QUIZ_TITLE</h2>
<p>QUIZ_DESCRIPTION</p>
```

Pour la suite du template, utilisez la propriété `quizPlaying` permettant de savoir si le quiz est démarré ou non.

Si `quizPlaying` vaut `false`, affichez le bouton "Démarrer" qui suit :

```html
<p>
  <button class="btn btn-info btn-lg">
    Démarrer le quiz
  </button>
</p>
```

Si `quizPlaying` vaut `true`, affichez le composant `<quiz-question>` créé précédemment.

Au clic sur le bouton "Démarrer", vous déclencherez une méthode (à créer) qui démarre le quiz.

----

POUR TESTER : Vous devez pouvoir passer de l'écran de bienvenue à la première question, répondre à la question et soumettre. Mais il n'est pas encore possible de passer à la question suivante.


### Répondre à une question, pour de vrai

Jusqu'à maintenant, on se contentait de logger dans la console quand l'utilisateur soumettait une réponse à une question.

À présent, on veut utiliser le `QuizStateManager` pour enregistrer les réponses aux questions.

Vérifiez les deux points suivants  :

1) Quand le composant "question" émet une réponse au quiz player, il faut mettre à jour l'état avec `QuizStateManager.addAnswer()`.

2) L'état du composant "question" doit être ré-initialisé À CHAQUE FOIS fois qu'il reçoit une nouvelle question (c. à d. à chaque fois que l'utilisateur clique sur Précédent ou Suivant).

Il vous faut donc résoudre le problème suivant :
- CODE ACTUEL : L'état du composant est initialisé une seule fois, à l'affichage de la première question, grâce à `ngOnInit()`. Lorsque les questions changent par la suite, `ngOnInit()` n'est plus rappelé...
- CE QU'ON VOUDRAIT : Comment faire pour que l'état du composant soit ré-initialisé **à chaque fois que ses inputs changent**.


### Passer d'une question à l'autre (boutons Précédent/Suivant)

Un composant `QuizNavComponent` (`quiz-nav.component.ts`) a **déjà été créé** pour vous.

Il contient les boutons Précédent/Suivant, le score en cours, et le n° de la question en cours.

**1) Configurer les inputs/outputs**

Le `QuizNavComponent` possède 3 inputs (déjà créés) pour recevoir les données nécessaires à son affichage :

- `quiz` - quiz en cours
- `question` - question en cours
- `answers` - réponses déjà soumises

Il vous reste à créer les 3 outputs suivants :

- `onPrevQuestion` - l'utilisateur a cliqué sur le bouton "précédent"
- `onNextQuestion` - l'utilisateur a cliqué sur  le bouton "suivant"
- `onSubmitQuiz` - l'utilisateur a répondu à toutes les questions et a cliqué sur un bouton "Soumettre le quiz" pour transmettre l'ensemble de ses réponses

Émettez les événements correspondant à ces outputs aux moments opportuns.

NB. Le bouton "Soumettre le quiz" ne doit être affiché que si on est à la dernière question ET qu'on a une réponse pour cette dernière question.

**2) Affichez la navigation dans le quiz player**

Retournez maintenant dans le quiz player.

Affichez le composant `<quiz-nav>` dans le template, au-dessus de `<quiz-question>`.

Passez à `<quiz-nav>` tous les **inputs** qu'il attend.

Écoutez tous les **outputs** émis par `<quiz-nav>`, et réagissez en appelant des méthodes (à créer) qui mettent à jour l'état de l'application.



## EXO 7 : Créer un module dédié à l'affichage des quizzes

Pour l'instant, nous avons créé tous nos fichiers à la racine du projet et nous les avons associés au `AppModule`. Pas idéal...

Il est temps de créer un module dédié à l'affichage des quizzes : le `QuizModule` !

**1) Créez le `QuizModule` avec angular-cli**

```bash
ng g module quiz
```

**2) Déplacez tous les fichiers liés aux quizzes dans le `QuizModule`**

Cela veut dire :
- Déplacer tous les fichiers situés sous `app/` et dont le nom commence par `quiz-xxx` dans le répertoire `quiz/`.
- Déplacer tous les symboles dont le nom commence par `QuizXXX` de `AppModule` vers `QuizModule` (vous les trouverez dans les propriétés `declarations` et `providers` de `AppModule`).
- Repasser sur tous les fichiers du QuizModule pour vérifier les chemins d'import. En effet, le fait de déplacer les fichiers va casser certains chemins d'import. Si votre IDE possède une fonctionnalité "refactoriser", utilisez-la et les chemins seront ajustés automatiquement.

**3)  Utiliser le `QuizModule` depuis `AppModule`**

À ce stade, les fonctionnalités du `QuizModule` sont privées, et inaccessibles au reste de l'application.

Pour résoudre ce problème, il faut :

- Importer `QuizModule` dans `AppModule`.
- OPTIONNEL. Mettre dans la propriété `exports` de `QuizModule` les composants qu'on veut pouvoir utiliser directement dans `AppModule`. Par exemple, pour que notre code actuel continue à fonctionner, il faut exporter `QuizPlayerComponent`, car son sélecteur `<quiz-player>` est utilisé dans le template du `AppComponent`.

Remarque. Dans la version définitive de l'appli, tous les composants du QuizModule seront affichés via les routes de ce module. Il ne sera donc pas nécessaire de les exporter.



## EXO 8 : Créer les routes de l'application


### Routes du module principal

**1) Déclarez les routes suivantes dans `app.module.ts`**

- Chemin `home` qui pointe sur `HomeComponent`.
- Chemin `login` qui pointe sur `LoginComponent`.
- Chemin vide qui redirige vers chemin `home`.

**2) Indiquez où les routes doivent s'afficher**

Placez la directive `<router-outlet>` dans le template de `AppComponent`, à l'endroit où les routes doivent s'afficher.

**3) Créez les liens dans la barre de nav**

Dans la barre de navigation en haut de page, faites pointer les liens "Accueil" et "Login" sur leurs routes respectives.

Pour l'instant, les liens "Quizzes" et "Admin" restent inactifs.


### Routes du module Quiz

**1) Déclarez les routes suivantes dans `quiz.module.ts`**

- Chemin `list` qui pointe sur `QuizListComponent`.
- Chemin `:quizId` qui pointe sur `QuizPlayerComponent`.
- Chemin vide qui redirige vers chemin `list`.

**2) Chargez le `QuizModule` en *lazy-loading***

Pour cela, dans `AppModule` :
- Supprimez `QuizModule` de la propriété `@NgModule.imports`, et supprimez l'import TypeScript correspondant.
- Déclarez une route qui associe le chemin `quiz` au module `QuizModule` (grâce à la propriété `loadChildren`).

**3) Câblez les liens associés aux quizzes**

- Lien "Quizzes" dans la barre de nav.
- Liens depuis la liste des quizzes vers chaque quiz.



## EXO 9 : Créer un service d'accès aux données (`QuizService`)


Dans cet exercice, nous allons créer un service chargé de toutes les interactions avec la base de données : charger les quizzes, créer ou modifier un quiz, etc.

Nous ferons une première implémentation **synchrone** qui utilise des données en dur, puis nous refactoriserons le code pour utiliser une API REST, **asynchrone**.


### Créer le service

Créez un service `QuizService`.

Ce service est transversal et sera réutilisé dans le module d'admin qu'on codera ensuite, vous le placerez donc dans le répertoire `services` dédié aux services transversaux de notre app.

```bash
ng g service services/quiz
```
Implémentez les méthodes suivantes dans le service :

- `loadQuizzes(): Quiz[] { ... }` - Retourne la liste de tous les quizzes
- `loadQuiz(quizId: number): Quiz { ... }` - Retourne un quiz complet, avec ses questions

Pour le moment, ces méthodes renverront les données en dur qu'on trouve dans les fichiers `data/quizzes.ts` (liste des quizzes) et `data/questions.ts` (liste des questions).

NB. La méthode `loadQuiz()` doit assembler deux sources de données pour renvoyer un quiz complet : le quiz lui-même (fichier `quizzes.ts`) + les questions du quiz (fichier `questions.ts`).


### Utiliser le service

Utilisez le service aux endroits suivants :

- Pour récupérer la liste des quizzes, dans `QuizListComponent`.
- Pour récupérer un quiz précis, dans `QuizPlayerComponent`. Adaptez votre code pour récupérer dans l'URL l'id du quiz à afficher et pour charger le quiz correspondant.

Soyez bien vigilant aux petits pièges (indice : la méthode `loadQuiz()` attend un nombre). 😣


### Refactorisez le service pour utiliser une API REST


Pour la formation, nous allons utiliser JSON Server (https://github.com/typicode/json-server) qui permet d'exposer un simple fichier JSON à travers une API REST. De l'extérieur, il se comporte comme une véritable API REST.


**1) Installez JSON Server**

Installez le paquet npm :

```bash
npm install json-server --save
```

Ajoutez/Modifiez les scripts suivants dans le fichier `package.json` :

```json
{
  "scripts": {
    "api:start": "json-server --watch src/app/data/db.json --port 3004",
    "start": "npm run api:start & ng serve",
    ...
  }
}
```

Testez que vous arriver à lancer le JSON server avec cette commande

```bash
npm run api:start
```

En principe, le fichier contenant notre "base de données" (`src/app/data/db.json`) est maintenant exposé à travers une API REST à l'URL http://localhost:3004/. Familiarisez-vous avec ses URLs.

Notre API contient 3 collections :
- `quizzes` : les quizzes
- `questions` : les jeux de questions rattachées à chaque quiz (un jeu de question = TOUTES les questions d'un quiz)
- `submissions` : les réponses soumises par les utilisateurs (une soumission = TOUTES les réponses à un quiz donné par un utilisateur donné)

Nous avons aussi redéfini la commande `npm start` pour démarrer le JSON Server ET de lancer le serveur de développement (`ng serve`) en même temps.

**IMPORTANT.** Interrompez tous les processes en cours dans vos consoles (`ng serve`, `api:start`) et lancez uniquement :

```bash
npm start
```


**2) Utilisez l'API REST dans le `QuizService`**

Modifiez le service pour qu'il fasse des requêtes HTTP sur l'API REST, et ajoutez les méthodes manquantes :

- `loadQuizzes()`
  - Requête GET sur http://localhost:3004/quizzes
- `loadQuiz(quizId)`
  - Récupérer le quiz : Requête GET sur http://localhost:3004/quizzes/QUIZ_ID
  - **ET** récupérer ses questions :  Requête GET sur http://localhost:3004/questions?quizId=QUIZ_ID (ATTENTION. Cette requête utilise un paramètre pour filtrer les données, et renverra TOUJOURS un tableau contenant les résultats obtenus.)
- `saveQuiz(quiz)`
  - Si création d'un quiz : Requête POST sur http://localhost:3004/quizzes
  - Si modification d'un quiz existant : Requête PUT sur http://localhost:3004/quizzes/QUIZ_ID
- `deleteQuiz(quizId)`
  - Requête DELETE sur http://localhost:3004/quizzes/QUIZ_ID

Assurez-vous que le **type** retourné par chaque méthode est TOUJOURS un `Observable`.

La requête `loadQuiz()` est complexe car elle doit *combiner les données renvoyées par deux observables*. Utilisez l'opérateur RxJS `combineLatest()` pour résoudre le problème :

```ts
// 1er observable - Récupère le quiz
const obs1 = this.http.get()...;

// 2e observable - Récupère les questions du quiz
const obs2 = this.http.get()...;

// Combine les 2 observables
return Observable.combineLatest(obs1, obs2)
  // On reçoit un tableau contenant les données renvoyées par les 2 observables
  .map(combinedArr => {
    let quiz = combinedArr[0];         // Données du 1er observable (quiz)
    let questionSet = combinedArr[1];  // Données du 2e observable (questions)
    quiz.questions = questionSet.questions;
    return quiz;
});
```


**3) Modifiez le code qui utilise le `QuizService`**

Les données sont maintenant retournées via des observables asynchrones par le `QuizService`.

Le code utilisateur du service doit donc s'abonner à ces observables (`subscribe`) pour récupérer les données :

- Adaptez le code qui récupère la liste des quizzes, dans `QuizListComponent`.
- Adaptez le code qui récupère un quiz précis, dans `QuizPlayerComponent`.
- Adaptez le code des templates correspondants. En effet, les templates pourraient planter s'ils essaient d'afficher des données pas encore chargées (car asynchrones).



## EXO 10 : Créer le formulaire de création/édition d’un quiz


### Activer le module d'admin

Un module `AdminModule` a **déjà été créé**. Il est destiné à recevoir toutes les fonctionnalités administratives de l'application (gestion des formulaires et de leurs questions).

Dans les routes du module principal, ajoutez une route qui charge le module d'admin en *lazy loading* :

```ts
{ path: 'admin', loadChildren: 'app/admin/admin.module#AdminModule' },
```

Câblez le lien "Admin" dans la barre de navigation du site.


### Créer un composant pour le formulaire

Créez un composant `AdminQuizFormComponent` à l'intérieur du module d'admin (avec angular-cli).

Utilisez le HTML suivant pour le template :

```
<form novalidate>

  <div class="form-group">
    <label class="control-label">Titre*</label>
    <input type="text" class="form-control" placeholder="Titre">
  </div>

  <div class="form-group">
    <label class="control-label">Description</label>
    <textarea class="form-control" placeholder="Description" rows="3"></textarea>
  </div>

  <div class="form-group">
    <label class="control-label">Peut ré-essayer les questions</label>
    <input type="checkbox">
  </div>

  <button type="submit" class="btn btn-primary">Enregistrer</button>
  <a>Annuler</a>

</form>
```


### Relier le formulaire d'édition au module d'admin

Dans le module d'admin, réorganisez les routes de la manière suivante :
- Déplacez les routes déjà déclarées de sorte qu'elles deviennent des **enfants** de la route par défaut (`AdminShellComponent`).
- Ajoutez une route pour *créer un nouveau quiz*, avec le chemin `"quiz/new"` qui pointe vers le formulaire qu'on vient de créer.
- Ajoutez une route pour *modifier un quiz existant*, avec le chemin `"quiz/:quizId"` qui pointe vers le formulaire qu'on vient de créer.

**ATTENTION.** Pensez à déclarer un `<router-outlet>` dans le template du composant parent pour indiquer où vont s'afficher les routes enfant.

Rendez le composant `AdminQuizListComponent` dynamique :
- Dans `loadQuizzes()`, chargez la liste des quizzes grâce au `QuizService` créé précédemment.
- Dans le template, remplacez les données statiques par les vraies données.
- Dans `deleteQuiz()`, supprimez le quiz passé en argument en utilisant le `QuizService`.

Câblez les différents liens dans les écrans d'admin :
- Sur la liste des quizzes (`AdminQuizListComponent`) :
  - Les boutons "Nouveau Quiz" et "Éditer" doivent mener vers le formulaire créé à l'instant.
  - Le bouton "Voir" doit mener sur le quiz correspondant dans le front-office.
  - Le bouton "Supprimer" doit supprimer le quiz sélectionné **après confirmation** et rechargez la liste des quizzes.
  - Le bouton "Gérer les questions" doit mener sur le composant `QuestionsListComponent`.
- Sur le formulaire d'édition (`AdminQuizFormComponent`) : le lien "Annuler" doit ramener à la liste des quizzes.


### Optimiser l'affichage des champs

Dans le template du formulaire, on répète beaucoup de markup identique pour chaque champ.

Pour afficher un champ, au lieu d'écrire ça :

```html
<div class="form-group">
  <label class="control-label">Titre</label>
  <input type="text" class="form-control" placeholder="Titre">
</div>
```

On voudrait pouvoir écrire ça :

```html
<field label="Titre">
  <input type="text" class="form-control" placeholder="Titre">
</field>
```

Pour cela, créez un composant custom `FormFieldComponent` situé dans le répertoire `common` qui :
- Possède le selector `field`.
- Se charge d'afficher la `<div>` et le `<label>` qui encadrent chaque champ.
- Utilise la **projection** pour conserver la balise `<input>` correspondant au champ lui-même.

Ensuite, modifiez le template du formulaire pour afficher chaque champ avec la nouvelle balise `<field>`.


### Angulariser le formulaire

Il est temps de transformer le formulaire classique de `AdminQuizFormComponent` en formulaire Angular.

**IMPORTANT.** Avant de commencer, n'oubliez pas d'importer `ReactiveFormsModule` dans le(s) module(s) où vous utilisez des formulaires.

**Dans le template :**

- Ajoutez les directives de formulaires conformes à la syntaxe "piloté par le modèle".

**Dans la classe :**

- Avant tout, chargez le quiz à afficher dans le formulaire
  - Si l'URL contient un `quizId`, chargez le quiz correspondant grâce au `QuizService`.
  - Sinon, instanciez un nouveau quiz vide : `new Quiz({title: ''})`
- Utilisez le `FormBuilder` pour créer le modèle du formulaire :
  - Ce modèle doit correspondre aux champs du template.
  - Utilisez les propriétés du quiz créé à l'étape précédente comme valeurs par défaut pour les champs. NB. N'oubliez pas qu'il faut gérer l'aspect asynchrone du chargement des données.
- Appelez une méthode `saveQuiz()` à la soumission du formulaire. Elle doit récupérer les valeurs du formulaire et les enregistrer grâce au `QuizService`. En cas de succès, redirigez l'utilisateur vers la liste des quizzes.


### Afficher les erreurs

Grâce à notre composant custom `FormFieldComponent` utilisé pour afficher chaque champ, on va gagner un temps fou !

En implémentant l'affichage des erreurs dans ce composant, tous nos champs en bénéficieront automatiquement.

Dans le modèle du formulaire, rendez le champ "Titre" obligatoire + minimum 5 caractères.

Dans le composant `FormFieldComponent`, implémentez l'affichage des erreurs du champ en cours :
- Créez une propriété input qui reçoit l'instance de `FormControl` qui représente le champ en cours.
- Grâce au `FormControl`, testez les propriétés d'erreur et de validité/invalidité du champ et affichez le(s) message(s) adéquats :
  - "Ce champ est obligatoire" si erreur "required".
  - "Ce champ doit faire au minimum *n* caractères" si erreur "minlength".



## EXO 11 : Comprendre les Observables

Nous allons nous entraîner à manipuler un Observable dans `ObservablesComponent` (`common/observables.component.ts`).

**TÂCHE PRÉALABLE :** Affichez `ObservablesComponent` à l'écran pour visualiser vos modifs (donnez-lui sa propre route, ou insérez-le dans un template existant).

Le composant contient déjà un observable renvoyé par `getObs()`. Dans une vraie appli, l'observable pourrait venir d'une requête HTTP (`http.get()`), des changements d'un champ de formulaire (`form.valueChanges`)...

Abonnez-vous à l'observable pour déclencher son exécution.

Observez les valeurs émises qui sont affichées à l'écran.

Puis, transformez les valeurs émises grâce aux opérateurs RxJS :
- Multiplier chaque valeur par 10.
- Ne garder que les valeurs comprise *strictement* entre 0 et 40.
- Préfixer chaque valeur avec la chaîne contenue dans `this.ROOT_URL`. Vous obtenez une URL valide pour du style `http://jsonplaceholder.typicode.com/posts/4`.
- Transformer chaque URL en requête HTTP (on dit "projeter un observable dans un autre").
- Extraire le corps JSON de la réponse HTTP.
- Extraire les propriétés .id et  .title du JSON. Concaténer ces deux propriétés.
- Réduire l'ensemble des valeurs de l'observable à une valeur unique : un tableau de valeurs.



## EXO 12 : Protéger les routes de l’application et précharger les données (routeur avancé)

Pour ne pas perdre de temps pour l'authentification, nous allons utiliser l'**API d'authentification de Google** : tout utilisateur possédant un compte Google qui s'identifiera auprès de notre application pourra accéder au back-office.


### Tâches préalables Auth - Dans la console Google Developers

Tâches à réaliser dans la console Google Developers (pour créer un client id) :

- Allez sur la [console Google Developers](https://console.developers.google.com/).
- Créez un projet dédié à la formation Angular.
- Dans ce projet, activez l'API "Google+ API".
- Dans la rubrique “Credentials”, choisir **Create Credentials > OAuth Client ID** :
  - Application type: Web
  - Name: Angular 2 Client
  - Authorized JavaScript origins: `http://localhost:4200` (= URL de notre serveur de développement)

Copiez le **Client ID** généré par la console et reportez-le à l’endroit approprié du fichier `app/settings.ts`.


### Tâches préalables Auth - Dans le projet Angular

**1) Charger le client JavaScript Google**

Dans le fichier `index.html`, ajoutez la balise suivante dans le HEAD :

```html
<script src="https://apis.google.com/js/client.js"></script>
```

**2) Déclarer les settings dans les providers de l'application**

Pourquoi ? Pour pouvoir utiliser les settings dans l'injection de dépendance.

Jusqu'à maintenant, tous les providers utilisés étaient des **services**, mais on peut aussi utiliser de **simples valeurs**, avec la syntaxe suivante :

```ts
// app.module.ts
providers: [
  { provide: 'APP_SETTINGS', useValue: APP_SETTINGS }
],
```

**BONNE PRATIQUE.** Grâce à la DI, on pourra facilement changer les settings lors du déploiement ou des tests unitaires.


### Utiliser le service d'authentification `AuthService`

Un service d'authentification `AuthService` (`services/auth.service.ts`) qui utilise l'API Google a **déjà été créé**.

Toutes les méthodes de `AuthService` renvoient des promesses qui s'utilisent comme suit :

```ts
promise
  .then(result => console.log('Successful promise with value:', result))
  .catch(error => console.error('Rejected promise with error:', error));
```

Ajoutez `AuthService` aux providers globaux de l'application.

Vérifiez que la page de Login fonctionne à présent (elle utilise `AuthService`).


### Protéger les pages de l'interface d'admin (`CanActivate`)

OBJECTIF : Empêcher les utilisateurs non authentifiés d'accéder aux pages du back-office.

Créez un service `AuthGuard` qui implémente l'interface `CanActivate`  (fichier `services/auth-guard.service.ts` déjà créé).

Dans la méthode `canActivate()` du service :
- Renvoyez une promesse qui se résout à `true` si l'utilisateur est authentifié.
- Redirigez l'utilisateur vers la page de login s'il n'est pas authentifié.

Vous utiliserez `AuthService.isLoggedIn()`, qui renvoie une promesse dénouée à `true` si l'utilisateur est identifié, ou qui échoue sinon.

Appliquez ce garde à toutes les routes commençant par le chemin `/admin`.


### Protéger les réponses de quiz non sauvegardées (`CanDeactivate`)

OBJECTIF : Empêcher l'utilisateur de quitter un quiz tant qu'il n'a pas enregistré ses réponses.

Créez un service `CanDeactivateQuiz` qui implémente l'interface `CanDeactivate` (fichier `services/can-deactivate-quiz.service.ts` déjà créé).

Si l'utilisateur tente de quitter la route `/quiz/:quizId` alors qu'il n'a pas encore enregistré ses réponses (`QuizStateManager.hasPendingChanges == true`), demandez-lui confirmation : "Vous avez des réponses en cours. Voulez-vous poursuivre et perdre vos réponses ?"

S'il accepte, votre garde doit renvoyer `true` (ce qui autorise la navigation), sinon votre garde doit renvoyer `false`.

**ATTENTION.** Pour que cela fonctionne, le `QuizStateManager` doit être un provider GLOBAL, et donc PAS un provider d'un module *lazy-loaded* comme c'est le cas actuellement avec `QuizModule`. En effet, les providers d'un module *lazy-loaded* sont scopés à ce module.


### Précharger les quizzes (`Resolve`)

OBJECTIF : Charger le quiz en cours depuis le routeur, plutôt que dans le code du composant qui utilise le quiz.

BÉNÉFICE ? Éviter de répéter le code "chargement d'un quiz" dans tous les composants qui ont besoin de charger un quiz (le quiz player, le formulaire d'édition d'un quiz...).

Créez un service `QuizResolver` qui implémente l'interface `Resolve` (fichier `services/quiz-resolver.service.ts` déjà créé).

Dans la méthode `resolve` du service, renvoyez un observable qui wrappe le quiz dont l'id est trouvé dans l'URL.

Appliquez ce resolve à la route `":quizId"` du `QuizModule`, de manière à précharger le quiz en cours sous la clé `quiz`.

Dans le composant associé à cette route, récupérez le quiz préchargé grâce au resolve.



## EXO 13 : Formulaires avancés


### Répéter les choix du formulaire de question

Dans le formulaire permettant de créer/éditer une question (`QuestionFormComponent`), faites en sorte qu'il soit possible d'ajouter et de retirer des choix dynamiquement, grâce à la technique du "champ répété".


### Valider plusieurs champs à la fois avec un validateur custom

Dans le formulaire permettant de créer/éditer un quiz (`AdminQuizFormComponent`), ajoutez un validateur custom qui permet de cocher "sauter les questions" uniquement si l'accès du quiz est "public".

Vous afficherez l'erreur correspondante dans le template du formulaire.



## EXO 14 : HTTP avancé

Dans cet exercice, nous allons changer les paramètres de TOUTES les requêtes HTTP de l'application.

Redéfinissez le fonctionnement des requêtes dans `app/app-request-options.ts` : faites en sorte que toutes les requêtes POST et PUT aient automatiquement le Content-Type `'application/json'`.

Ensuite, faites en sorte que les options customisées remplacent le `RequestOptions` par défaut en déclarant le provider suivant dans `AppModule` :

```ts
{ provide: RequestOptions, useClass: AppRequestOptions }
```



## EXO 15 : Tests unitaires

**IMPORTANT.** Avant de vous lancer dans les tests, assurez-vous que vous n'avez pas de tests en échec dans les fichiers `.spec.ts` créé par Angular CLI. Si oui, supprimez ces fichiers.


### Votre premier test

Pour valider votre configuration de test, créez votre premier test dans le fichier `app/1st.spec.ts` avec le code suivant :

```ts
describe('1st tests', () => {
  it('true is true', () => expect(true).toBe(true));
});
```

Exécutez ensuite la commande `ng test` dans le terminal, et assurez-vous que le test est exécuté avec succès (les résultats des tests sont affichés dans la console).


### Tester un composant simple

Nous allons tester le `HomeComponent`.

Vous placerez votre code dans le fichier `common/home.component.spec.ts` (déjà créé).

Dans le setup (`beforeEach`), récupérez une référence à l'élément DOM qui représente la balise `<h1>...</h1>` affichée dans le template du composant et stockez-la dans la variable `el`.

Créez les tests suivants :
- La balise `<h1>` doit contenir la propriété `appName` du composant.
- Si on change la valeur de la propriété `appName` (depuis le test), alors le contenu de la balise `<h1>` doit contenir la nouvelle valeur.

**ATTENTION.** N'oubliez pas de déclencher la détection de changement quand c'est approprié.


### Tester un composant qui utilise un service

Créez les tests suivants pour tester le `QuizListComponentSync` (liste des quizzes). Vous placerez votre code dans le fichier `quiz/quiz-list.component.sync.spec.ts` (déjà créé).

Dans cet exercice, on va tester la version **synchrone** du composant, `QuizListComponentSync`. Les tests asynchrones sont un peu plus complexes à écrire, mais très bien expliqués sur la [doc officielle](https://angular.io/docs/ts/latest/guide/testing.html).

Pour rappel, ce composant injecte le service `QuizServiceSync` et appelle sa méthode `loadQuizzes()` pour obtenir la liste des quizzes, qui sont alors affichés dans le template sous forme de liste à puces. L'objectif du test est de remplacer `QuizServiceSync` par un stub qui renvoie des quizzes en dur et de tester que l'affichage du composant est correct.

**Dans le setup (`beforeEach`) :**
- Le stub `QuizServiceStub` a déjà été créé pour vous. C'est un simple objet JavaScript avec une méthode `loadQuizzes()` qui renvoie deux quizzes en dur.
- Dans la configuration du module de test, faites pointer `QuizServiceSync` (le "vrai" service) sur le stub.
- Déclenchez la détection de changement.
- Récupérez l'instance de `QuizServiceSync` injectée dans le composant.
- Récupérez la liste des `<li>...</li>` dans le template.
- Récupérez la liste des routerLinks dans le template.

**Tests :**
- La méthode `loadQuizzes()` du service doit renvoyer 2 valeurs.
- La première valeur renvoyée par la méthode `loadQuizzes()` du service doit avoir une propriété `title` qui vaut "Quiz bidon #1".
- Le template du composant doit contenir deux `<li>...</li>`.
- Le template du composant doit contenir deux routerLinks.



## EXO 16 : Internationalisation

**IMPORTANT.** En préparant la formation, je n'ai pas réussi à faire fonctionner l'outil `ng-xi18n`. Vous utiliserez donc le fichier `locale/messages.fr.xlf` déjà généré pour stocker vos traductions.

1) Passez le texte du `HomeComponent` en anglais et marquez les chaînes à traduire avec `i18n` :

```html
<div class="jumbotron">
  <h1 i18n>{{ appName }}</h1>
  <p i18n>Welcome to SuperQuiz, a quiz app created with Angular 2.</p>
</div>
```

2) Ajoutez le code suivant dans `index.html`, **juste avant** `System.import('app')...` :

```html
<script>
  // Get the locale id somehow
  document.locale = 'fr';

  // Map to the text plugin
  System.config({
    map: {
      text: 'systemjs-text-plugin.js'
    }
  });
</script>
```

3) Modifiez le code du bootstrap dans `main.ts` :

```typescript
import { getTranslationProviders } from './i18n-providers';

getTranslationProviders().then(providers => {
  const options = { providers };
  platformBrowserDynamic().bootstrapModule(AppModule, options);
});
```

4) Adaptez les id des chaînes à traduire

Le problème d'avoir utilisé un fichier `messages.fr.xlf` déjà généré est que les id ne sont pas corrects (`<trans-unit id="???">`).

Il faut donc regarder dans la console du navigateur les messages d'erreur du type :

```bash
Translation unavailable for message id="14d28f660b993e4b1f4ce4c4847be5b1c2d9799b"
```

et reporter les ids cités dans votre fichier `messages.fr.xlf`.

5) Vérifiez que le texte de la page d'accueil de l'application s'affiche bien en français.



## EXO 17 : Déploiement

Suivre les instructions données par le formateur.



## EXO 18 : Gérer les questions d'un quiz dans le back-office


### Présentation

Nous allons créer un écran permettant de gérer les questions associées à un quiz. Il devra offrir les fonctionnalités suivantes :
- Ajouter une nouvelle question, avec un nombre illimité de réponses.
- Modifier une question existante.
- Changer l'ordre des questions.

Pour minimiser la quantité de code, nous avons fait les choix d'implémentation suivants :
- Tous les changements apportés aux questions sont faits **côté client**. Ils ne sont persistés sur le serveur que lorsque l'utilisateur clique sur un gros bouton "Enregistrer".
- Le formulaire d'ajout/modification de question est affiché directement dans l'écran de gestion des questions. Il n'y a pas de navigation entre plusieurs pages.
- Sur le serveur, les questions associées à un quiz sont enregistrées sous forme d'un unique objet `QuestionsSet` contenant une liste de questions et l'id du quiz auquel elles se rattachent.

Notre code sera implémenté dans deux composants :
- `QuestionsListComponent` qui affiche la liste des questions d'un quiz, et permet de les ré-ordonner, ou de déclencher l'ajout/suppression/modification d'une question.
- `QuestionFormComponent` qui contient le formulaire permettant d'ajouter/modifier une question.


### Liste des questions

Commençons par la liste des questions dans `QuestionsListComponent` (`app/admin/questions-list.component.ts`).

Tâches préalables :
- Examinons ensemble les **propriétés de classe** qui vont nous permettre d'implémenter les comportement ci-dessus : `questionSet`, `questionInEdition`, `hasPendingChanges`, `isEditMode`.
- Voyons aussi les **méthodes helper** qui vont nous aider à coder : `getQuestionIndex()`, `generateNextQuestionId()`, `moveQuestionUp()`, `moveQuestionDown()`.

Fonctionnalités à coder :
- Codez tous les emplacements signalés par des @TODO.
- Changement d'ordre des questions. Vous désactiverez le bouton "Haut" pour la première question, et le bouton "Bas" pour la dernière question.
- Bouton global "Enregistrer" : changer la classe ; désactiver le bouton si pas de "pending changes" ; enregistrer les questions.


### Formulaire ajout/modification de question

Dans `QuestionFormComponent` (`app/admin/question-form.component.ts`), nous avons déjà codé la possibilité de répéter un "choix".

À présent, il faut utiliser ce formulaire pour créer/éditer une question.

Vous implémenterez les fonctionnalités suivantes :

- Le formulaire est inséré en haut du template de la liste des questions. On active son affichage grâce au flag `isEditMode` dans le composant parent. Le flag passe à `true` quand on clique sur "Nouvelle question" ou "Editer une question" dans le composant parent, et à `false` quand on clique sur "Annuler" ou "Enregistrer" depuis le formulaire (utiliser un **Output** pour transmettre cet événement au composant parent).
- La question à éditer dans le formulaire (nouvelle ou existante) est transmise du composant parent au formulaire via un **Input**.
- La question à enregistrer (après modification) est transmise du formulaire au composant parent via un **Output**.
- Annuler l'édition (bouton "Annuler" dans le formulaire) est transmis du formulaire au composant parent via un **Output**.
