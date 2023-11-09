# AWK

Ecco 10 esempi di come usare `awk` in un ambiente Unix-like, con spiegazioni dettagliate. `awk` è un potente linguaggio di scripting utilizzato per manipolare dati e generare report. È particolarmente utile per processare testo basato su pattern e supporta la programmazione procedurale.

### Esempio 1: Stampare Colonne Specifiche

```bash
awk '{print $1, $3}' file.txt
```

Questo comando stampa la prima e la terza colonna del file `file.txt`. `$1` e `$3` rappresentano rispettivamente la prima e la terza colonna.

### Esempio 2: Filtrare Righe Basate su Condizioni

```bash
awk '$3 > 100' file.txt
```

Questo stampa le righe di `file.txt` dove il valore della terza colonna è maggiore di 100.

### Esempio 3: Calcolo della Somma di una Colonna

```bash
awk '{sum += $4} END {print sum}' file.txt
```

Calcola la somma dei valori nella quarta colonna di `file.txt`.

### Esempio 4: Modificare il Separatore di Campo

```bash
awk -F, '{print $2}' file.csv
```

Stampa la seconda colonna di un file CSV, dove i campi sono separati da virgole.

### Esempio 5: Formattare l'Output

```bash
awk '{printf "Nome: %s, Salario: %d\n", $1, $5}' file.txt
```

Formatta e stampa specifiche colonne con una stringa di formattazione. Qui, stampa la prima colonna come nome e la quinta come salario.

### Esempio 6: Contare le Righe

```bash
awk 'END {print NR}' file.txt
```

Conta il numero di righe in `file.txt`.

### Esempio 7: Esecuzione di Script su più File

```bash
awk '{sum += $2} END {print sum}' file1.txt file2.txt
```

Calcola la somma della seconda colonna su più file.

### Esempio 8: Filtrare e Stampa con Condizioni Complesse

```bash
awk '$1 == "Mario" && $4 > 2000' file.txt
```

Stampa le righe dove il primo campo è "Mario" e il quarto campo è maggiore di 2000.

### Esempio 9: Esecuzione di un Comando Esterno

```bash
awk '{print $1 | "sort"}' file.txt
```

Invia la prima colonna di `file.txt` al comando `sort`.

### Esempio 10: Usare Variabili e Cicli

```bash
awk -v limit=100 '{if ($3 > limit) print $0}' file.txt
```

Stampa le righe di `file.txt` dove il terzo campo supera un limite specificato (100 in questo caso).

Questi esempi mostrano come `awk` può essere utilizzato per diverse operazioni di manipolazione di testo e dati. È importante notare che `awk` è estremamente potente e versatile, e questi esempi ne coprono solo una piccola parte delle sue capacità.