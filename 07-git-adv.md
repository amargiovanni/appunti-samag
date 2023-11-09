# Git advanced

Ecco una serie di utilizzi avanzati di Git con esempi e spiegazioni in formato Markdown:

### 1. **Ripristino di un Commit Specifico**

**Spiegazione**: Puoi ripristinare il repository a uno stato specifico utilizzando `git reset`. Questo ti permette di annullare i commit fino a un commit di riferimento, mantenendo i file modificati nel working directory.

**Esempio**:

```sh
# Ripristina al commit precedente mantenendo i file modificati nel working directory
git reset HEAD~1

# Ripristina al commit con SHA-1 specifico e mantieni i file nel working directory
git reset <commit_sha1>
```

### 2. **Creazione di un Branch da un Commit Specifico**

**Spiegazione**: Puoi creare un nuovo branch basato su un commit specifico, utile per sviluppare funzionalità o correzioni su un punto specifico della cronologia.

**Esempio**:

```sh
# Crea un nuovo branch "mia-funzionalità" dal commit specifico
git checkout -b mia-funzionalità <commit_sha1>
```

### 3. **Rebase Interattivo**

**Spiegazione**: Il rebase interattivo ti consente di riorganizzare e modificare la cronologia dei commit, eliminare, modificare o combinare commit per rendere la cronologia più pulita.

**Esempio**:

```sh
# Esegui un rebase interattivo per modificare la cronologia
git rebase -i HEAD~5
```

### 4. **Cherry-Pick di Commit da Altri Branch**

**Spiegazione**: Puoi incorporare uno o più commit da un altro branch nella cronologia del branch corrente utilizzando `git cherry-pick`.

**Esempio**:


```sh
# Cherry-pick di un singolo commit da un altro branch
git cherry-pick <commit_sha1>

# Cherry-pick di una serie di commit da un altro branch
git cherry-pick <commit1_sha1> <commit2_sha1> <commit3_sha1>
```


### 5. **Utilizzo di Submodule**

**Spiegazione**: I submoduli permettono di includere un repository Git all'interno di un altro. Questo è utile per includere dipendenze esterne nel tuo progetto.

**Esempio**:


```sh
# Aggiungi un submodule al tuo progetto
git submodule add <repository_url> cartella_submodulo

# Inizializza e aggiorna i submoduli
git submodule init
git submodule update
```


### 6. **Fusioni Personalizzate (Merge vs. Rebase)**

**Spiegazione**: Puoi scegliere tra fusioni (merge) o riorganizzazioni (rebase) di commit in base alle esigenze del tuo progetto.

**Esempio**:


```sh
# Esegui una fusione tra il branch feature e il branch principale
git merge feature

# Esegui un rebase del branch feature sul branch principale
git rebase feature
```


### 7. **Filtrare la Cronologia dei Commit (Git Log)**

**Spiegazione**: `git log` può essere personalizzato per mostrare solo i commit rilevanti utilizzando opzioni come `--author`, `--since`, `--until`, `--grep`, ecc.

**Esempio**:


```sh
# Mostra solo i commit dell'autore specifico
git log --author="Nome Autore"

# Mostra i commit nell'intervallo di date specifico
git log --since="2022-01-01" --until="2022-12-31"
```


Questi sono alcuni esempi avanzati di come puoi utilizzare Git per gestire in modo più efficace il tuo repository e la cronologia dei commit. Git offre molte altre funzionalità avanzate che ti consentono di adattare il flusso di lavoro alle esigenze specifiche del tuo progetto.

## Esercizi

Ecco una serie di esercizi avanzati per esercitarsi con Git. Questi esercizi ti permetteranno di approfondire alcune delle funzionalità più potenti e meno note di Git.

### Esercizio 1: Rebase Interattivo
**Obiettivo**: Comprimi tre commit in uno e modifica il messaggio di commit.
1. Crea un nuovo branch dal `main` e aggiungi tre file separati con tre commit distinti.
2. Usa `git rebase -i HEAD~3` per avviare un rebase interattivo.
3. Comprimi (squash) i tre commit in uno.
4. Cambia il messaggio di commit in qualcosa di significativo che rifletta le modifiche di tutti e tre i commit.

### Esercizio 2: Risoluzione dei Conflitti in Rebase
**Obiettivo**: Risolvere i conflitti che emergono durante un rebase.
1. In un branch, fai delle modifiche a una parte di un file e crea un commit.
2. Cambia al branch principale e fai delle modifiche conflittuali alla stessa parte del file e committa le modifiche.
3. Torna al primo branch e avvia un rebase sul branch principale.
4. Risolvi i conflitti che si presentano.
5. Completa il rebase.

### Esercizio 3: Cherry-Picking
**Obiettivo**: Selezionare e applicare commit da un branch all'altro.
1. Crea un branch `feature` dal `main` e aggiungi alcuni commit.
2. Cambia al branch `main`.
3. Usa `git cherry-pick` per selezionare uno dei commit dal branch `feature` e applicalo al `main`.

### Esercizio 4: Uso Avanzato di Git Log
**Obiettivo**: Trovare commit specifici usando filtri avanzati.
1. Utilizza `git log` per trovare tutti i commit che contengono la parola "bugfix" nei loro messaggi.
2. Usa `git log` con un filtro di data per trovare i commit fatti nell'ultima settimana.
3. Visualizza il log in una sola riga per commit con un grafico della cronologia dei branch.

### Esercizio 5: Gestione dei Submodule
**Obiettivo**: Aggiungere e aggiornare un submodule.

Un submodule Git è un repository Git incorporato all'interno di un altro repository Git più grande. È uno strumento utile per gestire dipendenze esterne e consentire di includere un repository come parte di un altro progetto. In altre parole, un submodule consente di avere un repository Git all'interno di un altro repository Git.

Ecco alcune caratteristiche e concetti chiave relativi ai submodule Git:

1. **Repository Principale (Supermodulo)**: Il repository principale è quello che contiene il submodule. Questo è il progetto principale che utilizza il repository del submodule come dipendenza.

2. **Submodule (Sottomodulo)**: Il submodule è il repository Git che viene incorporato all'interno del repository principale come una directory. Può essere un progetto Git completamente separato, con la sua cronologia, i suoi branch e i suoi commit.

3. **Riferimento al Commit**: Un submodule è collegato al repository principale attraverso un riferimento a un commit specifico del repository del submodule. Questo significa che il repository principale fa riferimento a un commit del submodule piuttosto che alla sua branch principale.

4. **Gestione Separata**: Un submodule ha la sua gestione dei file e dei commit, il che significa che le modifiche al submodule devono essere gestite all'interno del suo repository e commesse separatamente. Questo permette una gestione indipendente dei progetti.

5. **Aggiornamento dei Submodule**: Quando si lavora in un repository principale, è possibile aggiornare i submodule in modo che riflettano le modifiche più recenti nel repository del submodule. Questo può essere fatto con il comando `git submodule update` o in combinazione con il comando `git pull` nel repository principale.

6. **Inizializzazione dei Submodule**: Quando si clona un repository principale che contiene submoduli, è necessario inizializzarli usando il comando `git submodule init` e quindi eseguire `git submodule update` per ottenere il contenuto effettivo del submodule.

L'uso di submoduli Git è comune in scenari in cui è necessario includere librerie o progetti esterni nei tuoi progetti, mantenendo comunque la separazione tra il codice del progetto principale e quello del submodule. Ciò permette di gestire in modo efficace le dipendenze esterne e semplifica la collaborazione con altri sviluppatori.

1. Crea un nuovo repository e aggiungilo come submodule al tuo progetto principale.
2. Clona il progetto principale in una nuova directory e inizializza i suoi submodules.
3. Apporta una modifica al repository del submodule, committala e aggiorna il riferimento nel progetto principale.

### Esercizio 6: Git Reflog e Recupero di Commit
**Obiettivo**: Recuperare un commit dopo un reset duro.
1. Fai qualche commit sul tuo branch corrente.
2. Esegui un `git reset --hard` a un commit precedente, perdi i commit più recenti.
3. Usa `git reflog` per trovare gli SHA dei commit persi.
4. Recupera i commit utilizzando `git cherry-pick` o `git reset`.

## Soluzioni

Certamente, ecco le soluzioni agli esercizi avanzati di Git proposti:

### Soluzione Esercizio 1: Rebase Interattivo

```sh
# Crea un nuovo branch e aggiungi i file
git checkout -b esercizio-rebase
echo "Primo file" > file1.txt
git add file1.txt
git commit -m "Aggiunge file1"
echo "Secondo file" > file2.txt
git add file2.txt
git commit -m "Aggiunge file2"
echo "Terzo file" > file3.txt
git add file3.txt
git commit -m "Aggiunge file3"

# Avvia un rebase interattivo
git rebase -i HEAD~3

# Nel testo dell'editor che si apre, sostituisci "pick" con "squash" nei due commit più recenti
# Modifica il messaggio di commit come desiderato, poi salva e chiudi l'editor
```

### Soluzione Esercizio 2: Risoluzione dei Conflitti in Rebase

```sh
# Crea un branch e modifica il file
git checkout -b esercizio-conflitti
echo "Modifica 1" > conflitto.txt
git add conflitto.txt
git commit -m "Modifica 1 su esercizio-conflitti"

# Cambia al branch principale e apporta modifiche conflittuali
git checkout main
echo "Modifica 2" > conflitto.txt
git add conflitto.txt
git commit -m "Modifica 2 su main"

# Torna al branch di esercizio e inizia il rebase
git checkout esercizio-conflitti
git rebase main

# Risolvi i conflitti che emergono, quindi continua il rebase
git add conflitto.txt
git rebase --continue
```

### Soluzione Esercizio 3: Cherry-Picking

```sh
# Crea un branch feature e aggiungi commit
git checkout -b feature
echo "Nuova funzionalità" > feature.txt
git add feature.txt
git commit -m "Aggiunge nuova funzionalità"

# Cambia al branch main
git checkout main

# Seleziona e applica il commit dal branch feature
git cherry-pick feature
```

### Soluzione Esercizio 4: Uso Avanzato di Git Log

```sh
# Trova tutti i commit con "bugfix" nei messaggi
git log --grep="bugfix"

# Trova i commit fatti nell'ultima settimana
git log --since="1 week ago"

# Visualizza il log in una riga per commit con un grafico
git log --graph --oneline --all
```

### Soluzione Esercizio 5: Gestione dei Submodule

```sh
# Aggiungi un submodule
git submodule add <repository_url> path/to/submodule
git commit -m "Aggiunge submodule"

# Clona il progetto principale e inizializza i submodule
git clone <main_project_url> main_project_clone
cd main_project_clone
git submodule update --init

# Aggiorna il riferimento nel progetto principale dopo una modifica nel submodule
cd path/to/submodule
echo "Modifica nel submodule" > file.txt
git commit -am "Modifica nel submodule"
cd ../..
git add path/to/submodule
git commit -m "Aggiorna riferimento al submodule"
```

### Soluzione Esercizio 6: Git Reflog e Recupero di Commit

```sh
# Fai qualche commit
git commit -am "Un commit importante"
git commit -am "Un altro commit importante"

# Esegui un reset --hard a un commit precedente
git reset --hard HEAD~2

# Usa git reflog per trovare lo SHA dei commit persi
git reflog

# Recupera i commit (assumendo che l'SHA del commit perso sia 123abc)
git cherry-pick 123abc
# Oppure, se vuoi spostare il branch attuale all'SHA del commit:
git reset --hard 123abc
```

Per ogni esercizio, gli SHA dei commit, i nomi dei branch e gli URL dei repository dovranno essere sostituiti con i valori corrispondenti al tuo ambiente e alla tua cronologia dei commit. Ricorda di eseguire questi esercizi in un repository di test per evitare di modificare la cronologia di commit in un progetto reale. 