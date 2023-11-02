# sed

## Per cosa è utilizzato sed?

E' **presente in ogni installazione di Linux**, perché fa parte di `coreutils`.

E' un editor di testo non interattivo che **non apporta modifiche dirette al file** che si sta modificando ma che permette di creare prima un file temporaneo il cui contenuto viene poi trasferito al file di origine.

SED **procede riga per riga**: ogni riga del file viene letta individualmente, elaborata e poi riprodotta.

La funzione più importante di SED è quella di **cercare determinate stringhe di caratteri nel file e poi sostituirle con altri caratteri**.

## Sintassi e funzione del comando SED

### Sintassi

```bash
sed [option] [file]
```

## Esempi

### Sostituire testo in input

```bash
echo 'Hello world!' | sed 's/world/universe/'
```

### Sostituire testo in input da un file

```bash
echo 'world strawberry fields forever...' >> file.txt
sed 's/world/universe/' file.txt
```

### Salvare le modifiche su file

```bash
sed 's/world/universe/' file.txt > new_file.txt
```

### Salvare i cambiamenti nel file originale

```bash
# usare --in-place
sed -i 's/world/universe/' file.txt

# oppure
sed 's/world/universe/' file.txt > file.txt
```

### Escape dei delimitatori

Molti esempi di utilizzo di sed "/" come delimitatore. Cosa succede se voglio sostituire una stringa che contiene questo carattere?

Semplice: **usiamo underscore come delimitatore**.

Potremmo usare "_" invece di "/" come delimitatore. È perfettamente valido poiché sed non impone alcun delimitatore specifico. **Il "/" è utilizzato per convenzione, non come requisito**.

```bash
sed 's_/usr/local/bin/dummy_/usr/bin/dummy/_' input-file
```

### Sostituzione di ogni istanza di una stringa

Una caratteristica interessante del comando di sostituzione è che, per impostazione predefinita, sostituirà solo una singola istanza di una stringa su ogni riga.

Se ci trovassimo in questa situazione:
```
$ cat << EOF >> input-file 
one two one three
two four two
three one four
EOF
```

Ci basterà aggiungere il delimitatore `g`:

```bash
sed 's/one/ONE/g' input-file
```

### Usare le Matched String

A volte potremmo voler aggiungere alcune cose come parentesi o virgolette attorno a una stringa specifica. Questo è facile da fare se sappiamo esattamente cosa stiamo cercando. 

Ma cosa succede se non sappiamo esattamente cosa troveremo? SED fornisce una piccola funzionalità carina per la corrispondenza di stringhe.

```bash
# così aggiungiamo le parentesi a "123"
echo 'one two three 123' | sed 's/123/(123)/'

# se volessimo invece aggiungere le parentesi a tutte le parole in minuscolo
echo 'one two three 123' | sed 's/[a-z][a-z]*/(&)/g'
```

### Usare Extended Regular Expressions con SED

Prima abbiamo utilizzato `[a-z][a-z]*`.

Sarebbe ancora più comodo usare `[a-z]+`, ma SED non supporta le Extended Regular Expressions di default.

È necessario utilizzare le opzioni `-E` o `-r` per abilitare le espressioni regolari estese in sed:

```bash
echo 'one two three 123' | sed -E 's/[a-z]+/(&)/g'
echo 'one two three 123' | sed -r 's/[a-z]+/(&)/g'
```

### Fare sostituzioni multiple

Possiamo usare più di un comando sed contemporaneamente separandoli con ";"

```bash
echo 'one two three' | sed 's/one/1/; s/two/2/; s/three/3/'
```

### Sostituzione "case insensitive"

Diciamo che vogliamo sostituire tutte le occorrenze di "one", indipendentemente dal loro essere maiuscole o minuscole. Possiamo farlo utilizzando il flag "i":

```bash
echo 'one ONE OnE' | sed 's/one/1/gi'
```

### Print di linee specifiche

È possibile visualizzare una riga specifica dell'input utilizzando il comando 'p'. Aggiungiamo altro testo al nostro file di input e dimostriamo questo esempio.

```bash
echo 'Adding some more
text to input file
for better demonstration' >> input-file

sed '3p; 6p' input-file
```

L'output dovrebbe contenere due volte il numero di riga tre e sei. Questo non è quello che ci aspettavamo, giusto? Questo accade perché, per impostazione predefinita, sed produce l'output di tutte le righe del flusso di input, oltre alle righe richieste in modo specifico. Per stampare solo le righe specifiche, dobbiamo sopprimere tutti gli altri output.

```bash
sed -n '3p; 6p' input-file
sed --quiet '3p; 6p' input-file
sed --silent '3p; 6p' input-file
```

Tutti questi comandi sed sono equivalenti e stampano solo la terza e la sesta riga del nostro file di input. È quindi possibile eliminare l'output indesiderato utilizzando una delle opzioni -n, -quiet o -silent.

### Stampare un intervallo di righe

Il comando seguente stampa un intervallo di righe dal nostro file di input. Il simbolo ',' può essere usato per specificare un intervallo di input per sed.

```bash
sed -n '2,4p' input-file
sed --quiet '2,4p' input-file
sed --silent '2,4p' input-file
```

Tutti e tre i comandi sono equivalenti. Essi stampano le righe da due a quattro del nostro file di input.

### Stampa di righe non consecutive

Supponiamo di voler stampare righe specifiche dall'input di testo con un unico comando. È possibile gestire questo tipo di operazioni in due modi. Il primo consiste nell'unire più operazioni di stampa utilizzando il separatore ';'.

```bash
sed -n '1,2p; 5,6p' input-file
```

Questo comando stampa le prime due righe del file di input seguite dalle ultime due righe. È possibile eseguire questa operazione anche utilizzando l'opzione -e di sed. Notate le differenze nella sintassi.

```bash
sed -e '1,2p' -e '5,6p' input-file
```

### Stampa ogni N-esima riga

Supponiamo di voler visualizzare ogni due righe del nostro file di input. L'utilità sed rende questo molto semplice, fornendo l'operatore tilde '~'. Date una rapida occhiata al comando seguente per vedere come funziona.

```bash
sed -n '1~2p' input-file
```

Questo comando funziona stampando la prima riga seguita da ogni seconda riga dell'input. Il comando seguente stampa la seconda riga seguita da ogni terza riga dell'output di un semplice comando ip.

```bash
ip -4 a | sed -n '2~3p'
```

### Sostituzione di testo all'interno di un intervallo di righe

Possiamo anche sostituire del testo solo all'interno di un intervallo specificato, nello stesso modo in cui lo abbiamo stampato. Il comando seguente mostra come sostituire gli "uno" con gli "1" nelle prime tre righe del nostro file di input usando sed.

```bash
sed '1,3 s/uno/1/gi' input-file
```

### Eliminazione di righe dall'input

The ed command ‘d’ allows us to delete specific lines or range of lines from text stream or from input files. The following command demonstrates how to delete the first line from the output of sed.

```bash
sed '1d' input-file
```

Poiché sed scrive solo sullo standard output, questa cancellazione non si riflette sul file originale. Lo stesso comando può essere usato per eliminare la prima riga da un flusso di testo multilinea.

```bash
ps | sed '1d'
```

Quindi, usando semplicemente il comando 'd' dopo l'indirizzo della riga, possiamo sopprimere l'input per sed.

### Eliminazione di un intervallo di righe dall'input

È anche molto facile cancellare un intervallo di righe usando l'operatore ',' insieme all'opzione 'd'. Il prossimo comando sed sopprimerà le prime tre righe del nostro file di input.

```bash
sed '1,3d' input-file
```

È possibile eliminare anche righe non consecutive utilizzando il seguente comando.

```bash
sed '1d; 3d; 5d' input-file
```

### Eliminazione dell'ultima riga

L'utilità sed dispone di un semplice meccanismo che ci permette di cancellare l'ultima riga da un flusso di testo o da un file di input. È il simbolo '$' e può essere utilizzato anche per altri tipi di operazioni oltre alla cancellazione. Il comando seguente cancella l'ultima riga dal file di input.

```bash
sed '$d' input-file
```

### Eliminazione di tutte le righe tranne alcune

Un altro pratico esempio di cancellazione con sed è quello di cancellare tutte le righe tranne quelle specificate nel comando. Questo è utile per filtrare le informazioni essenziali dai flussi di testo o dall'output di altri comandi del terminale Linux.

```bash
free | sed '2!d'
```

È possibile fare lo stesso con i file di input, come dimostrato di seguito.

```bash
sed '1,3!d' input-file
```

Questo comando cancella dal file di input tutte le righe tranne le prime tre.

### Aggiunta di righe vuote

A volte il flusso di input può essere troppo fitto. In questi casi si può usare l'utilità sed per aggiungere righe vuote tra l'input. Il prossimo esempio aggiunge una riga vuota tra ogni riga dell'output del comando ps.

```bash
ps aux | sed 'G'
```

Il comando 'G' aggiunge questa riga vuota. È possibile aggiungere più righe vuote utilizzando più di un comando 'G' per sed.

``` bash
sed 'G; G' input-file
```

Il comando seguente mostra come aggiungere una riga vuota dopo un numero di riga specifico. Aggiungerà una riga vuota dopo la terza riga del nostro file di input.

```bash
sed '3G' input-file
```

### Sostituzione di testo su righe specifiche

L'utilità sed consente agli utenti di sostituire il testo su una determinata riga. Ciò è utile in diversi scenari. Supponiamo di voler sostituire la parola 'uno' sulla terza riga del nostro file di input. Per farlo possiamo usare il seguente comando.

```bash
sed '3 s/one/1/' input-file
```

Il '3' prima dell'inizio del comando 's' specifica che si vuole sostituire solo la parola che si trova sulla terza riga.

### Sostituzione della parola N-esima di una stringa

Si può anche usare il comando sed per sostituire l'n-esima occorrenza di uno schema per una determinata stringa. L'esempio seguente lo illustra utilizzando una singola riga in bash.

```bash
echo 'one one one one one one' | sed 's/one/1/3'
```

Questo comando sostituisce il terzo 'uno' con il numero 1. Questo funziona allo stesso modo per i file di input. Il comando seguente sostituisce l'ultimo "two" della seconda riga del file di input.

```bash
cat input-file | sed '2 s/two/2/2'
```

Selezioniamo prima la seconda riga e poi specifichiamo quale occorrenza modificare.

### Aggiunta di nuove linee

È possibile aggiungere facilmente nuove righe al flusso di input utilizzando il comando 'a'. Guardate il semplice esempio qui sotto per vedere come funziona.

```bash
sed 'nuova riga in input' input-file
```

Il comando precedente aggiunge la stringa 'nuova riga in input' dopo ogni riga del file di input originale. Tuttavia, questo potrebbe non essere il risultato desiderato. È possibile aggiungere nuove righe dopo una riga specifica utilizzando la seguente sintassi.

```bash
sed '3 una nuova riga in input' input-file
```

### Inserimento di nuove righe

Si possono anche inserire righe invece di aggiungerle (append). Il comando seguente inserisce una nuova riga prima di ogni riga di input.

```bash
seq 5 | sed 'i 888'
```

Il comando 'i' fa sì che la stringa 888 venga inserita prima di ogni riga dell'output di seq.

### Modifica delle righe in input

È anche possibile modificare direttamente le righe di un flusso di input utilizzando il comando 'c' dell'utilità sed. Questo è utile quando si conosce esattamente la riga da sostituire e non si vuole eseguire una corrispondenza con le espressioni regolari. L'esempio seguente modifica la terza riga dell'output del comando seq.

```bash
seq 5 | sed '3 c 123'
```

Sostituisce il contenuto della terza riga, che è 3, con il numero 123. L'esempio successivo mostra come modificare l'ultima riga del nostro file di input utilizzando 'c'.

```bash
sed '$ c STRINGA CAMBIATA' file di input
```

Si può anche usare una regex per selezionare il numero di riga da modificare. Il prossimo esempio lo illustra.

``` bash
sed '/demo*/ c TESTO MODIFICATO' file di input
```

### Creazione di file di backup per l'input

Se si desidera trasformare del testo e salvare le modifiche nel file originale, si consiglia vivamente di creare dei file di backup prima di procedere. Il comando seguente esegue alcune operazioni sed sul nostro file di input e lo salva come originale. Inoltre, per precauzione, crea un file di backup chiamato input-file.old.

```bash
sed -i.old 's/one/1/g; s/two/2/g; s/three/3/g' input-file
```

Il -i scrive le modifiche apportate da sed al file originale. Il suffisso .old è responsabile della creazione del documento input-file.old.

### Stampa di linee basate su pattern

Supponiamo di voler stampare tutte le righe di un input in base a un certo schema. Questo è abbastanza facile se si combinano i comandi di sed 'p' con l'opzione -n. L'esempio seguente lo illustra utilizzando il file di input.

```bash
sed -n '/^for/ p' input-file
```

Questo comando cerca lo schema 'for' all'inizio di ogni riga e stampa solo le righe che iniziano con questo carattere. Il carattere '^' è un carattere speciale di espressione regolare noto come ancora. Specifica che il modello deve trovarsi all'inizio della riga.

### Utilizzo del SED come alternativa al GREP

Il comando grep di Linux cerca un particolare schema in un file e, se lo trova, visualizza la riga. Possiamo emulare questo comportamento utilizzando l'utilità sed. Il comando seguente lo illustra con un semplice esempio.

```bash
sed -n 's/strawberry/&/p' /usr/share/dict/american-english
```

Questo comando individua la parola fragola nel file del dizionario americano-inglese. Cerca lo schema strawberry e utilizza la stringa corrispondente insieme al comando 'p' per stamparla. Il flag -n sopprime tutte le altre righe dell'output. È possibile rendere questo comando più semplice utilizzando la seguente sintassi.

```bash
sed -n '/strawberry/p' /usr/share/dict/american-english
```

### Aggiunta di testo da file

Il comando 'r' dell'utilità sed permette di aggiungere al flusso di input il testo letto da un file. Il comando seguente genera un flusso di input per sed utilizzando il comando seq e aggiunge a questo flusso i testi contenuti in input-file.

```bash
seq 5 | sed 'r input-file'
```

Questo comando aggiunge il contenuto del file di input dopo ogni sequenza di input consecutiva prodotta da seq. Usate il comando successivo per aggiungere il contenuto dopo i numeri generati da seq.

```bash
seq 5 | sed '$ r input-file'
```

Si può usare il seguente comando per aggiungere il contenuto dopo la n-esima riga di input.

```bash
seq 5 | sed '3 r input-file'
```

### Scrivere le modifiche ai file

Supponiamo di avere un file di testo che contiene un elenco di indirizzi web. Alcuni di essi iniziano con www, altri con https e altri ancora con http. Possiamo cambiare tutti gli indirizzi che iniziano con www in https e salvare solo quelli modificati in un file completamente nuovo.

```bash
sed 's/www/https/ w modified-websites' websites
```

Ora, se si ispeziona il contenuto del file modified-websites, si troveranno solo gli indirizzi che sono stati modificati da sed. L'opzione 'w filename' fa sì che sed scriva le modifiche nel nome del file specificato. È utile quando si ha a che fare con file di grandi dimensioni e si desidera memorizzare separatamente i dati modificati.

### Utilizzo dei SED Program Files

A volte può essere necessario eseguire un certo numero di operazioni sed su un dato set di input. In questi casi, è meglio scrivere un program file contenente tutti i diversi script di sed. È quindi possibile invocare semplicemente questo file di programma utilizzando il comando
-f dell'utilità sed.

```bash
cat << EOF >> sed-script
s/a/A/g
s/e/E/g
s/i/I/g
s/o/O/g
s/u/U/g
EOF
```

Questo programma sed cambia tutte le vocali minuscole in maiuscole. È possibile eseguirlo utilizzando la sintassi seguente.

```bash
sed -f sed-script input-file
```

### Utilizzo dei comandi SED multilinea

Se si sta scrivendo un programma sed di grandi dimensioni che si estende su più righe, è necessario quotarle correttamente. La sintassi differisce leggermente tra le diverse shell di Linux. Fortunatamente, è molto semplice per la shell bourne e i suoi derivati (bash).

```bash
sed '
s/a/A/g 
s/e/E/g 
s/i/I/g 
s/o/O/g 
s/u/U/g' < input-file
```

In alcune shell, come la shell C (csh), è necessario proteggere le virgolette utilizzando il carattere backslash (\\).

```bash
sed 's/a/A/g \
s/e/E/g \
s/i/I/g \
s/o/O/g \
s/u/U/g' < input-file
```

### Stampa dei numeri di riga

Se si desidera stampare il numero di riga contenente una stringa specifica, è possibile cercarla utilizzando un pattern e stamparla in modo molto semplice. A tale scopo, è necessario utilizzare il comando '=' dell'utilità sed.

```bash
sed -n '/ion*/ =' < input-file
```

Questo comando cercherà lo schema dato nel file di input e stamperà il suo numero di riga nello standard output. È anche possibile utilizzare una combinazione di grep e awk.

```bash
cat -n input-file | grep 'ion*' | awk '{print $1}'
```

È possibile utilizzare il seguente comando per stampare il numero totale di righe dell'input.

```bash
sed -n '$=' input-file
```

### Prevenire la sovrascrittura dei symlink

Il comando sed 'i' o '-in-place' spesso sovrascrive i collegamenti di sistema con i file regolari. In molti casi si tratta di una situazione indesiderata e quindi gli utenti potrebbero voler evitare che ciò accada. Fortunatamente, sed offre una semplice opzione da riga di comando per disabilitare la sovrascrittura dei collegamenti simbolici.

```bash
echo 'apple' > fruit
ln --symbolic fruit fruit-link
sed --in-place --follow-symlinks 's/apple/banana/' fruit-link
cat fruit
```

È quindi possibile evitare la sovrascrittura dei collegamenti simbolici utilizzando l'opzione -follow-symlinks dell'utilità sed. In questo modo, è possibile preservare i collegamenti simbolici durante l'elaborazione del testo.

### Eliminazione di righe commentate dall'input

Molti strumenti di sistema, così come le applicazioni di terze parti, sono dotati di file di configurazione. Questi file contengono solitamente molti commenti che descrivono dettagliatamente i parametri. Tuttavia, a volte si desidera visualizzare solo le opzioni di configurazione, mantenendo i commenti originali.

```bash
cat ~/.bashrc | sed -e 's/#.*//;/^$/d'
```

Questo comando cancella le righe commentate dal file di configurazione di bash. I commenti sono contrassegnati dal segno "#" che li precede. Abbiamo quindi rimosso tutte queste righe utilizzando un semplice schema regex. Se i commenti sono contrassegnati da un simbolo diverso, sostituire il '#' nello schema precedente con quel simbolo specifico.

```bash
cat ~/.vimrc | sed -e 's/".*//;/^$/d'
```

Questo rimuoverà i commenti dal file di configurazione di vim, che inizia con un simbolo di doppia virgoletta (").

### Eliminazione degli spazi bianchi dall'input

Molti documenti di testo sono pieni di spazi bianchi inutili. Spesso sono il risultato di una cattiva formattazione e possono rovinare l'intero documento. Fortunatamente, sed permette agli utenti di rimuovere queste spaziature indesiderate in modo piuttosto semplice. È possibile utilizzare il comando successivo per rimuovere gli spazi bianchi iniziali da un flusso di input.

```bash
sed 's/^[ \t]*//' whitespace.txt
```

Questo comando rimuove tutti gli spazi bianchi iniziali dal file whitespace.txt. Se si desidera rimuovere gli spazi bianchi finali, utilizzare il comando seguente.

```bash
sed 's/[ \t]*$//' whitespace.txt
```

Si può anche usare il comando sed per rimuovere contemporaneamente gli spazi bianchi iniziali e finali. Il comando seguente può essere utilizzato per questo scopo.

```bash
sed 's/^[ \t]*//;s/[ \t]*$//' whitespace.txt
```

### Inversione delle linee di input

Il comando seguente mostra come utilizzare sed per invertire l'ordine delle righe in un file di input. Emula il comportamento del comando Linux
tac.

```bash
sed '1!G;h;$!d' input-file
```

Questo comando inverte le righe del documento di input.

### Inversione dei caratteri in input

Si può anche usare l'utilità sed per invertire i caratteri delle righe di input. Questo invertirà l'ordine di ogni carattere consecutivo nel flusso di input.

```bash
sed '/\n/!G;s/\(.\)\(.*\n\)/&\2\1/;//D;s/.//' input-file
```

Questo comando emula il comportamento del comando rev di Linux. È possibile verificarlo eseguendo il comando seguente dopo quello precedente.

```bash
rev input-file
```

### Stampa di righe contenenti un numero specifico di caratteri

È molto semplice stampare le righe in base al numero di caratteri. Il semplice comando seguente stampa le righe che contengono 15 o più caratteri.

```bash
sed -n '/^.\{15\}/p' input-file
```

Utilizzare il comando seguente per stampare le righe con meno di 20 caratteri.

```bash
sed -n '/^.\{20\}/!p' input-file
```

È possibile farlo anche in modo più semplice, utilizzando il metodo seguente.

```bash
sed '/^.\{20\}/d' input-file
```

### Eliminazione di righe doppie

Il seguente esempio di sed ci mostra come emulare il comportamento del comando uniq di Linux. Esso elimina due righe consecutive duplicate dall'input.

```bash
sed '$!N; /^\(.*\)\n\1$/!P; D' input-file
```

Tuttavia, sed non può eliminare tutte le righe duplicate se l'input non è ordinato. Anche se è possibile ordinare il testo usando il comando sort e poi collegare l'output a sed usando una pipe, questo cambierà l'orientamento delle righe.

### Eliminazione di tutte le righe vuote

Se il vostro file di testo contiene molte righe vuote non necessarie, potete eliminarle usando l'utilità sed. Il comando seguente ne è una dimostrazione.

```bash
sed '/^$/d' input-file
sed '/./!d' input-file
```

### Eliminazione delle ultime righe dei paragrafi

È possibile eliminare l'ultima riga di tutti i paragrafi utilizzando il seguente comando sed. Per questo esempio utilizzeremo un nome di file fittizio. Sostituitelo con il nome di un file reale che contiene alcuni paragrafi.

```bash
sed -n '/^$/{p;h;};/./{x;/./p;}' paragraphs.txt
```

### Visualizzazione Help Page

La pagina di aiuto contiene informazioni sintetiche su tutte le opzioni disponibili e sull'uso del programma sed. È possibile richiamarla utilizzando la seguente sintassi.

```bash
sed --help
```

### Visualizzazione man page

```bash
man sed
```

### Visualizzazione delle informazioni sulla versione

```bash
sed --version
```

### Pensieri conclusivi

> Il comando sed è uno degli strumenti di manipolazione del testo più utilizzati dalle distribuzioni Linux. È uno dei tre strumenti di filtraggio principali di Unix, insieme a grep e awk. Abbiamo descritto un po' di esempi semplici ma utili per aiutavi a iniziare a utilizzare questo straordinario strumento. Vi raccomando di provare personalmente questi comandi per ottenere informazioni pratiche. Inoltre, provate a modificare gli esempi forniti qui ed esaminatene gli effetti. Questo vi aiuterà a padroneggiare rapidamente Sed.

Qui un playground: [https://sed.js.org](https://sed.js.org)