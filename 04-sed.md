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

