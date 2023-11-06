# Introduzione ad AWK

Awk è un linguaggio di scripting generico progettato per l'elaborazione avanzata del testo. Viene utilizzato principalmente come strumento di reporting e analisi.

A differenza della maggior parte degli altri linguaggi di programmazione che sono procedurali, awk è basato sui dati, il che significa che si definisce un insieme di azioni da eseguire rispetto al testo di input. Prende i dati di input, li trasforma e invia il risultato allo standard output.

## Come funziona Awk

Esistono diverse implementazioni di `awk`. Useremo l'implementazione GNU di awk, che si chiama `gawk`. Sulla maggior parte dei sistemi Linux, l'interprete awk è solo un collegamento simbolico a gawk.

## Record e campi

Awk elabora i dati testuali, da file o flussi. I dati di input sono divisi in record e campi. Awk opera su un record alla volta fino al raggiungimento della fine dell'input. I record sono separati da un carattere chiamato separatore dei record. Il separatore di record predefinito è il carattere di nuova riga, il che significa che ogni riga nei dati di testo è un record. È possibile impostare un nuovo separatore di record utilizzando la variabile `RS`.

I record sono costituiti da campi separati dal separatore di campo. Per impostazione predefinita, i campi sono separati da uno spazio, inclusi uno o più caratteri tab, spazio e newline.

I campi in ciascun record sono indicati dal simbolo del dollaro (`$`) seguito dal numero del campo, che inizia con 1. Il primo campo è rappresentato con `$1`, il secondo con `$2` e così via. L'ultimo campo può anche essere referenziato con la variabile speciale `$NF`. È possibile fare riferimento all'intero record `$0`.

Ecco una rappresentazione visiva che mostra come fare riferimento a record e campi:

```bash
tmpfs      788M  1.8M  786M   1% /run/lock
/dev/sda1  234G  191G   31G  87% /
|-------|  |--|  |--|   |--| |-| |--------|
   $1       $2    $3     $4   $5  $6 ($NF) --> fields
|-----------------------------------------|
                    $0                     --> record
```

## Programma Awk

Per elaborare un testo con awk scrivi un programma che dice al comando cosa fare. I programmi sono costituiti da una serie di regole e funzioni definite dall'utente. Ogni regola contiene un modello e una coppia di azioni. Le regole sono separate da newline o punti e virgola `;`. In genere, un programma awk è simile al seguente:

```bash
pattern { action }
pattern { action }
```

Quando awk elabora un dato, se il modello corrisponde al record, esegue l'azione specificata su quel record. Quando la regola non ha pattern, tutti i record (righe) vengono abbinati.

Un'azione awk è racchiusa tra parentesi graffe `{}` e consiste in istruzioni. Ogni istruzione specifica l'operazione da eseguire. Un'azione può avere più di un'istruzione separate da newline o punti e virgola `;`. Se la regola non ha azioni, per impostazione predefinita stampa l'intero record.

Awk supporta diversi tipi di istruzioni, tra cui espressioni, condizionali, input, istruzioni di output e altro. Le dichiarazioni awk più comuni sono:

- `exit` - Interrompe l'esecuzione dell'intero programma ed esce.
- `next` - Interrompe l'elaborazione del record corrente e passa al record successivo nei dati di input.
- `print` - Stampa record, campi, variabili e testo personalizzato.
- `printf` - Ti dà un maggiore controllo sul formato di output, simile a C e bash printf.

Quando si scrivono programmi awk, tutto ciò che segue il segno di hash `(#)` e fino alla fine della riga è considerato un commento. Le righe lunghe possono essere suddivise in più righe utilizzando il carattere di continuazione, barra rovesciata `\`.

## Esecuzione di programmi awk

Un programma awk può essere eseguito in diversi modi. Se il programma è breve e semplice, può essere passato direttamente all'interprete awk dalla riga di comando:

```bash
awk 'program' input-file...
```

Quando si esegue il programma dalla riga di comando, dovrebbe essere racchiuso tra virgolette singole '', quindi la shell non interpreta il programma.

Se il programma è grande e complesso, è meglio inserirlo in un file e utilizzare l'opzione `-f` per passare il file al comando awk:

```bash
awk -f program-file input-file...
```

Negli esempi seguenti useremo un file chiamato "teams.txt" che assomiglia a quello qui sotto:

```bash
Bucks Milwaukee    60 22 0.732
Raptors Toronto    58 24 0.707
76ers Philadelphia 51 31 0.622
Celtics Boston     49 33 0.598
Pacers Indiana     48 34 0.585
```

## Modelli Awk

I pattern in awk controllano se l'azione associata deve essere eseguita o meno.

Awk supporta diversi tipi di modelli, inclusi espressione regolare, espressione di relazione, intervallo e schemi di espressione speciali.

Quando la regola non ha alcun pattern, ogni record di input viene abbinato. Ecco un esempio di una regola che contiene solo un'azione:

```bash
awk '{ print $3 }' teams.txt
```

Il programma stamperà il terzo campo di ciascun record:

```bash
60
58
51
49
48
```

## Schemi di espressione regolare

Un'espressione regolare o regex è un modello che corrisponde a una serie di stringhe. I modelli di espressioni regolari Awk sono racchiusi tra barre `//`:

```bash 
/regex pattern/ { action }
```

L'esempio più semplice è un carattere letterale o una corrispondenza di stringhe. Ad esempio, per visualizzare il primo campo di ciascun record che contiene "0,5" è necessario eseguire il comando seguente:

```bash
awk '/0.5/ { print $1 }' teams.txt
```

Output:

```bash 
Celtics
Pacers
```

Il modello può essere qualsiasi tipo di espressione regolare estesa. Ecco un esempio che stampa il primo campo se il record inizia con due o più cifre:

```bash
awk '/^[0-9][0-9]/ { print $1 }' teams.txt
```

Output:

```bash 
76ers
```

## Schemi di espressioni relazionali

I modelli di espressioni relazionali sono generalmente utilizzati per abbinare il contenuto di uno specifico campo o variabile.

Per impostazione predefinita, i modelli di espressioni regolari vengono confrontati con i record. Per abbinare una regex a un campo, specificare il campo e utilizzare l'operatore di confronto "contain" `~`.

Ad esempio, per stampare il primo campo di ciascun record il cui secondo campo contiene "ia", digitare:

```bash
awk '$2 ~ /ia/ { print $1 }' teams.txt
```

Output:

```bash 
76ers
Pacers
```

Per abbinare i campi che non contengono un determinato modello, utilizzare l'operatore `!~`:

```bash
awk '$2 !~ /ia/ { print $1 }' teams.txt
```

Output: 

```bash 
Bucks
Raptors
Celtics
```

È possibile confrontare stringhe o numeri per relazioni come maggiore di, minore di, uguale e così via. Il comando seguente stampa il primo campo di tutti i record il cui terzo campo è maggiore di 50:

```bash
awk '$3 > 50 { print $1 }' teams.txt
```

Output:

```bash 
Bucks
Raptors
76ers
```

## Modelli di range

I modelli di intervallo sono composti da due modelli separati da una virgola:

```bash
pattern1, pattern2 { action }
```

Tutti i record che iniziano con un record che corrisponde al primo modello fino a quando un record che corrisponde al secondo modello viene abbinato.

Ecco un esempio che stamperà il primo campo di tutti i record a partire dal record incluso "Raptors" fino al record che include "Celtics":

```bash
awk '/Raptors/,/Celtics/ { print $1 }' teams.txt
```

Output:

```bash 
Raptors
76ers
Celtics
```

Gli schemi possono anche essere espressioni di relazione. Il comando seguente stamperà tutti i record a partire da quello il cui quarto campo è uguale a 32 fino a quello il cui quarto campo è uguale a 33:

```bash
awk '$4 == 32, $4 == 33 { print $0 }' teams.txt
```

Output:

```bash 
76ers Philadelphia 51 31 0.622
Celtics Boston     49 33 0.598
```

I pattern di intervallo non possono essere combinati con altre espressioni di pattern.

## Modelli di espressione speciali

Awk include i seguenti patten speciali:

- `BEGIN` - Utilizzato per eseguire azioni prima dell'elaborazione dei record.
- `END` - Utilizzato per eseguire azioni dopo l'elaborazione dei record.

Il modello BEGIN viene generalmente utilizzato per impostare le variabili e il modello END per elaborare i dati dai record come il calcolo.

L'esempio seguente stamperà "Start Processing.", quindi stampa il terzo campo di ciascun record e infine "End Processing.":

```bash
awk 'BEGIN { print "Start Processing." }; { print $3 }; END { print "End Processing." }' teams.txt
```

Output:

```bash 
Start Processing.
60
58
51
49
48
End Processing.
```

Se un programma ha solo un modello BEGIN, le azioni vengono eseguite e l'input non viene elaborato. Se un programma ha solo un modello END, l'input viene elaborato prima di eseguire le azioni della regola.

La versione Gnu di awk include anche altri due schemi speciali `BEGINFILE` e `ENDFILE` che consente di eseguire azioni durante l'elaborazione dei file.

## Combinazione di modelli

Awk consente di combinare due o più pattern utilizzando l'operatore logico AND `&&` e l'operatore logico OR `||`.

Ecco un esempio che utilizza l'operatore `&&` per stampare il primo campo di quei record il cui terzo campo è maggiore di 50 e il quarto campo è inferiore a 30:

```bash
awk '$3 > 50 && $4 < 30 { print $1 }' teams.txt
```

Output:

```bash 
76ers Philadelphia 51 31 0.622
Celtics Boston     49 33 0.598
```

## Variabili integrate

Awk ha una serie di variabili integrate che contengono informazioni utili e ti permettono di controllare come viene elaborato il programma. Di seguito sono riportate alcune delle variabili incorporate più comuni:

- `NF` - Il numero di campi nel record.
- `NR` - Il numero del record corrente.
- `FILENAME` - Il nome del file di input attualmente elaborato.
- `FS` - Separatore di campo.
- `RS` - Separatore di record.
- `OFS` - Separatore del campo di uscita.
- `ORS` - Separatore record di uscita.

Ecco un esempio che mostra come stampare il nome del file e il numero di righe (record):

```bash
awk 'END { print "File", FILENAME, "contains", NR, "lines." }' teams.txt
```

Output: 

```bash 
File teams.txt contains 5 lines.
```

Le variabili in AWK possono essere impostate su qualsiasi riga del programma. Per definire una variabile per l'intero programma, dovrebbe essere impostata in un modello `BEGIN`.

## Modificare il campo e il record separatore

Il valore predefinito del separatore di campo è un numero qualsiasi di spazi o caratteri di tabulazione. Può essere modificato impostando nella variabile `FS`.

Ad esempio, per impostare il separatore di campo su `.` si deve usare:

```bash
awk 'BEGIN { FS = "." } { print $1 }' teams.txt
```

Output:

```bash 
Bucks Milwaukee    60 22 0
Raptors Toronto    58 24 0
76ers Philadelphia 51 31 0
Celtics Boston     49 33 0
Pacers Indiana     48 34 0
```

Il separatore di campo può anche essere impostato su più di un carattere:

```bash
awk 'BEGIN { FS = ".." } { print $1 }' teams.txt
```

Quando si esegue awk one-liners sulla riga di comando, è inoltre possibile utilizzare l'opzione -F per modificare il separatore di campo: