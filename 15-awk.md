`awk` è un potente linguaggio di programmazione e strumento da linea di comando utilizzato per la manipolazione e l'analisi di dati in file di testo, specialmente quando questi dati sono organizzati in colonne. Ecco una panoramica delle sue funzioni base:

### 1. **Pattern Matching**
   `awk` è molto efficiente nel trovare e lavorare con pattern specifici in un file di testo. Può essere usato per eseguire azioni su righe che corrispondono a un certo pattern.

   ```bash
   awk '/pattern/ { azioni }' file.txt
   ```

### 2. **Lavorare con Campi e Record**
   In `awk`, un file è suddiviso in record (tipicamente righe) e campi (tipicamente separati da spazi o da un altro delimitatore). `awk` rende molto semplice estrarre e manipolare questi campi.

   ```bash
   awk '{print $1, $3}' file.txt
   ```
   Questo comando stampa il primo e terzo campo di ogni riga.

### 3. **Variabili Predefinite**
   `awk` ha diverse variabili predefinite, come `NR` per il numero di record corrente, `NF` per il numero di campi nel record corrente, `$0` per l'intera riga corrente, e così via.

   ```bash
   awk '{print NR, $0}' file.txt
   ```
   Stampa il numero di riga seguito dall'intera riga.

### 4. **Operazioni Matematiche e Stringhe**
   `awk` supporta operazioni matematiche e funzioni di manipolazione delle stringhe.

   ```bash
   awk '{print $1, $2, $3 * $4}' file.txt
   ```
   Qui si moltiplicano il terzo e il quarto campo.

### 5. **Condizioni e Cicli**
   Si possono usare istruzioni condizionali (`if`, `else`) e cicli (`for`, `while`) per eseguire complesse operazioni logiche.

   ```bash
   awk '{if ($3 > 100) print $1, $2}' file.txt
   ```
   Stampa nome e cognome dove le ore lavorative superano 100.

### 6. **Begin e End Blocks**
   I blocchi `BEGIN` ed `END` in `awk` sono utilizzati per eseguire azioni prima della lettura del primo record e dopo la lettura dell'ultimo record.

   ```bash
   awk 'BEGIN {print "Inizio"} {print $0} END {print "Fine"}' file.txt
   ```
   Stampa "Inizio", tutte le righe del file e poi "Fine".

### 7. **Delimitatori di Campo Personalizzati**
   `awk` permette di specificare un delimitatore di campo diverso dallo spazio predefinito usando l'opzione `-F`.

   ```bash
   awk -F: '{print $1}' file.txt
   ```
   Qui si usa `:` come delimitatore per i campi.

### 8. **Passaggio di Variabili Esterno**
   Puoi passare variabili dall'esterno allo script `awk`.

   ```bash
   awk -v var="valore" '{print var, $1}' file.txt
   ```

`awk` è estremamente potente per il suo semplice modello di programmazione basato su pattern e azioni, e per la sua capacità di manipolare facilmente dati strutturati. È uno strumento indispensabile per lo scripting in ambiente Unix/Linux, specialmente per la manipolazione di dati in colonne, report di testo e processamento di file di log.