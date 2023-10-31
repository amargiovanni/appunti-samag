# Set Options in Bash

## errexit (-e)

Esce in caso di **non-zero exit status**.

```bash
#!/bin/bash
set -e
 
cp file_non_esistente.txt /tmp
echo "Questa riga non verrà eseguita se il comando precedente fallisce."
```

## nounset (-u)

Esce in caso di **variabili non inizializzate**.

```bash
#!/bin/bash
set -u
 
variabile_inizializzata="Hello, World!"
echo $variabile_zializzata
```

## pipefail (-o pipefail)

Esce in caso di **non-zero exit status nelle pipeline**.

```bash 
#!/bin/bash
set -o pipefail
 
grep "error" logfile.txt | sort | uniq
```

## xtrace (-x)

**Stampa i comandi prima della loro esecuzione** per aiutarci nel debug.

```bash
#!/bin/bash
set -x

echo "Hello, World!"
```

## Combinare Set Options

Possiamo combinare diversi Set Options insieme includendoli nello stesso comando:

```bash
set -e -u -o pipefail

# Qui mettiamo il nostro script fighissimo
```

## Conclusioni

Comprendere e utilizzare **Set Options** in Bash può aiutarti a scrivere codice **più robusto, efficiente e manutenibile**. 

Abilitando opzioni come “errexit”, “nounset” e “pipefail”, puoi individuare tempestivamente gli errori ed evitare che ci causino grattacapi. 

Inoltre, l'opzione "xtrace" può essere un potente strumento di debug per script complessi. 

