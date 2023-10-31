# Utilizzare le variabili in Bash

Come quasi tutti i linguaggi di programmazione esistenti, anche Bash possiede il concetto di variabili.

## Assegnare un valore a una variabile

**Non c'è bisogno di dichiarare prima la variabile**, come invece accade in altri linguaggi di programmazione. Basta assegnarli un valore.

```bash
variabile="valore"
echo $variabile
```

## Assegnare più di un valore a una variabile

È possibile assegnare più di un valore a una variabile usando il comando **+=**.
L'echo ci tornerà fondamentalmente un array.

```bash
variabile="valore1"
variabile+=" valore2"
echo $variabile
```

## Esempio pratico

Chiedo all'utente di digitare il suo nome e lo registro nella variabile NOME.

```bash
echo "Inserisci il tuo nome:"
read NOME
echo "Il tuo nome è $NOME"
```

## Cancellare una variabile

```bash
unset NOME
```

## I tipi di variabili

Ci sono diversi tipi di variabili in Bash:

### Le variabili locali

Le variabili locali sono le variabili che si assegnano durante l'esecuzione del comando. Le variabili locali non vengono modificate durante l'esecuzione del comando.

```bash
function funzione {
    local variabile="valore"
    echo $variabile
}
```

### Le variabili globali

Le variabili globali sono le variabili che vengono assegnate durante l'esecuzione del comando. Le variabili globali vengono modificate durante l'esecuzione del comando.

```bash
function funzione {
    variabile="valore"
    echo $variabile
}
```

### Le variabili shell

Le variabili shell sono le variabili che vengono assegnate durante l'esecuzione del comando. Le variabili shell vengono modificate durante l'esecuzione del comando.

```bash
# script prova.sh
echo "Il tuo nome è $1"
echo "Il tuo cognome è $2"

# comando
./prova.sh andrea margiovanni

# output
Il tuo nome è andrea
Il tuo cognome è margiovanni
```

### Altre variabili di shell

Le principali variabili shell sono le seguenti:

1. `$0`: il nome del file di esecuzione
2. `$1`: la prima variabile di linea di comando
3. `$2`: la seconda variabile di linea di comando
4. `$#`: il numero di variabili di linea di comando
5. `$@`: tutte le variabili di linea di comando
6. `$$`: il PID del processo corrente
7. `$!`: il PID del processo in background
8. `$?`: il valore di exit status del comando precedente

### Variabili di ambiente

Le variabili di ambiente sono variabili di sistema. Sono inizializzate all'avvio del sistema o della shell e conservano il loro valore nel tempo. Possono essere lette e in alcuni casi anche modificate dagli script.

**A differenza delle variabili locali, le variabili di ambiente mantengono il dato anche al termine dell'esecuzione dello script.**

Alcune variabili di sistema:

1. `$HOSTNAME`: il nome del host corrente
2. `$PWD`: la directory corrente
3. `$USER`: l'utente corrente
4. `$UID`: l'ID dell'utente corrente
5. `$EUID`: l'ID dell'utente effettivo
6. `$GROUPS`: i gruppi dell'utente
7. `$PATH`: la variabile di ambiente PATH
8. `$SHELL`: la variabile di ambiente SHELL
9. `$OLDPWD`: la directory precedente
10. `$RANDOM`: un numero casuale

**I nomi delle variabili di ambiente sono dedicati**: non possono essere usati per creare variabili locali.