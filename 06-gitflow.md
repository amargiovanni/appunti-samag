# Git-flow

Il branching model Git-flow è un approccio alla gestione dei rami (branch) in un repository Git che offre una struttura ben definita per organizzare lo sviluppo di un progetto software. È stato creato da Vincent Driessen ed è diventato molto popolare nell'ambito dello sviluppo software per la sua chiara organizzazione dei rami e delle attività di sviluppo. Il modello Git-flow è basato su due rami principali: `master` e `develop`, e introduce rami ausiliari per gestire le feature, le hotfix e le release.

### Master Branch
Il ramo `main` rappresenta la linea di produzione stabile del software. Ogni commit in questo ramo dovrebbe rappresentare una versione stabile del software pronto per il rilascio.

### Develop Branch
Il ramo `develop` rappresenta la linea di sviluppo principale. È da questo ramo che vengono create tutte le feature, le hotfix e le release. È un ramo in cui si lavora costantemente per sviluppare nuove funzionalità.

### Feature Branches
Quando un nuovo lavoro o una nuova funzionalità deve essere sviluppato, viene creato un "feature branch" da `develop`. Questo ramo è specifico per la nuova funzionalità e permette ai membri del team di lavorare su di essa senza influire sulla stabilità del ramo `develop`. Una volta completata la funzionalità, il branch della feature viene riunito in `develop`.

### Release Branches
Quando è il momento di preparare una nuova release del software, viene creato un "release branch" da `develop`. In questo ramo vengono eseguite attività di controllo di qualità, correzione di bug e preparazione della documentazione per la release. Una volta pronta, la release branch viene riunita sia in `master` (per una nuova versione stabile) che in `develop` (per mantenere sincronizzata la linea di sviluppo).

### Hotfix Branches
Se si verificano problemi critici o bug nella versione in produzione, viene creato un "hotfix branch" da `master`. Questo ramo consente di correggere il problema in modo isolato e successivamente di riunirlo sia in `master` che in `develop`.

In generale, il modello Git-flow promuove una chiara separazione tra il lavoro in corso e il lavoro stabile. Ciò rende più facile per i team di sviluppo gestire le varie attività e garantire che le release siano stabili e di alta qualità. Il modello fornisce inoltre una guida chiara su come gestire i rami e le release, rendendo il processo di sviluppo più ordinato e prevedibile.

