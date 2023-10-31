# Linux filesystems

## Ext2

Creato nel lontano 1993 da Rémy Card, fu sviluppato per porre rimedio ai limiti del filesystem originale (sviluppato dallo stesso Card appositamente per Linux, come evoluzione a sua volta di Minix).

A seconda della dimensione del blocco di scrittura, su un filesystem ext2 è possibile scrivere **file grandi fino a 2 TB**; lo stesso filesystem può avere dimensione massima di 32 TB ed ospitare sino a 31998 directory – in totale sono 32000 se si calcolano anche la directory corrente (dot) e la directory radice (dot dot).

Ext2 **non dispone di un meccanismo per la prevenzione degli errori di scrittura** – che tra poco scopriremo avere il nome di *journaling*.

## Ext3

Ext3 nasce circa 8 anni dopo ext2 dall’ingegno di Stephen Tweedie e viene introdotto nei sistemi operativi GNU/Linux-based a partire dal kernel 2.4.15. Un filesystem in Ext2 può essere convertito in Ext3 senza perdita di dati (a meno di errori non previsti).

Ext3 migliora il suo predecessore in termini di allocazione dei blocchi, evitando così il rischio di problemi dovuti a movimenti multipli dei dischi meccanici “alla ricerca del blocco perduto”. 

La grande novità rispetto al suo predecessore è rappresentata dal nuovo meccanismo di prevenzione degli errori di scrittura dei dati detto journaling: in parole povere, una parte del disco viene riservata alla scrittura continua delle modifiche su ciascun file, così da mantenere la consistenza del filesystem anche in caso di errori di scrittura accidentali (ad esempio un’interruzione di corrente).

Per la precisione, Ext3 prevede tre tipi di journaling:

1. Ordered (modalità predefinita): salva nell’area di journaling soltanto i metadati, che vengono aggiornati dopo la scrittura del contenuto su disco. Rappresenta un buon compromesso tra sicurezza e prestazioni. 

2. Journal: salva nell’area di journaling sia i metadati dei file modificati che l’effettivo contenuto; è l’impostazione migliore per la sicurezza.

3. Writeback: salva nell’area di journaling soltanto i metadati, aggiornati (a seconda del sistema) prima o dopo la scrittura effettiva del contenuto su disco. E’ il migliore in termini di prestazioni.

## Ext4

Ext4 viene alla luce nell’ormai lontano 2008, sviluppato da Andrew Morton, Alex Tomas e diversi altri esperti del settore, e viene introdotto nei sistemi GNU/Linux-based a partire dal kernel 2.6.19.

Un filesystem in Ext3 può essere convertito in Ext4 senza perdita di dati (a meno di errori non previsti).

Esso elimina per la maggiore i limiti di spazio di Ext3, portando la dimensione massima di un singolo file a 16 TB, la dimensione massima di un filesystem a ben 1 EB (= 1 exabyte, ovvero 1024 petabyte, cioè poco più di 1 milione di terabyte) ed eliminando totalmente il limite al numero di directory presenti sul filesystem.

Anche Ext4 supporta il journaling in tutte le modalità sovracitate, journaling che può tuttavia essere disattivato su richiesta (in tal caso il filesystem sarà riazzerato).

La grossa novità rispetto ad Ext3, questa volta, è in termini prestazionali: vengono infatti introdotte funzionalità come la pre-allocazione dei file (su richiesta, lo spazio viene allocato “prima ancora” della creazione di un file per assicurare la contiguità fisica dei blocchi occupati), l’allocazione multiblocco (ovvero l’allocazione di più gruppi di blocchi contemporaneamente) e l’allocazione sfalsata (delayed allocation, i blocchi vengono allocati soltanto quando vengono scritti su disco, per una riduzione della frammentazione).

Inoltre sono stati introdotti i checksum sui dati nell’area di journaling (per un controllo migliore sull’integrità), la possibilità di “saltare” l’analisi dei blocchi allocati durante il controllo del disco (velocizzando l’intera procedura), il miglioramento dei timestamp che vengono ora calcolati nell’ordine dei nanosecondi e tanto altro.

## BTRFS

BTRFS nasce nel 2008, poco dopo ext4 (con un primo rilascio pubblico nell’anno successivo) per sopperire alle mancanze del precedente filesystem in configurazioni server fortemente incentrate sullo storaging, in particolare nella gestione di array (RAID) e per sistemi con scalabilità indispensabile in termini di archiviazione (ad esempio un server cloud).

Ad oggi BTRFS è ancora in fase di sviluppo e viene dichiarato non stabile, sebbene sia un filesystem dalle grosse potenzialità già così com’è.

E’ un filesystem a 64 bit con grandezza limite di 16 EB (su sistemi operativi a 32 bit il limite scende ad 8 EB), in grado di ospitare file grandi fino a 16 EB – salvo limitazioni del sistema.

Esattamente come ci si aspetta, BTRFS è un filesystem copy-on-write: per una sorta di risparmio in termini di operazioni di I/O, i file aperti da più processi contemporaneamente vengono copiati in punti diversi del disco (per assicurare la consistenza) soltanto se effettivamente modificati e non se aperti in lettura.

> **Proprio grazie al copy-on-write BTRFS può usufruire di alcune funzionalità avanzate irrinunciabili per una buona gestione di array multi-disco configurati in RAID: load balancing (carico delle risorse) integrato, funzionalità di auto-riparazione in molteplici scenari, aggiunta e rimozione di device di caching a filesystem montato, funzionalità di aumento e diminuzione delle dimensioni a filesystem montato, controllo del filesystem (da non montato), oltre che conversione in-place da ext3/ext4 con la possibilità di mantenere la formattazione originale.**

BTRFS è pienamente compatibile le operazioni di clonazione dei file e la creazione di snapshot in sola lettura; permette il disaster-recovery dei file tramite un tool integrato (btrfs-restore), in caso di filesystem illegibile.

Per quanto riguarda il journaling, anche il suo funzionamento è stato pensato per rispondere egregiamente a configurazioni server: l’area di journaling può essere sia estesa su più dischi che clonata in caso di array.

E’ inoltre grazie al journaling intelligente che le funzionalità di riparazione automatica sono efficienti: per impostazione predefinita, infatti, i dati “journaled” vengono scritti su disco ogni 30 secondi scongiurando inconsistenze e perdite di dati accidentali.

BTRFS è estremamente stabile (sebbene incompleto) ed adatto a chi pretende consistenza dei dati; fa davvero la differenza in caso di gestione di interi array e/o rack di dischi altrimenti, in termini di prestazioni, si avvicina molto ad ext4.

## F2FS

F2FS è il filesystem più giovane della nostra analisi: creato da Samsung e rilasciato per la prima volta nel 2013, è pensato appositamente per la gestione delle memorie NAND presenti sui dispositivi di nuova generazione.

Implementato soltanto parzialmente, F2FS supporta funzionalità minimali ma estremamente utile: la deframmentazione al volo (integrata nei meccanismi di allocazione), modalità di recupero dati sia rollback che rollforward (grazie al journaling) ed operazioni di verifica del filesystem, quando esso è smontato, senza possibilità di correzione.

Nella lista delle cose da fare stilate da Samsung compare innanzitutto la possibilità di correggere la consistenza del filesystem, le operazioni atomiche (ovvero più operazioni che devono essere svolte senza interruzione), i driver per Windows ed il ridimensionamento.

Come ci si aspetta F2FS supporta il comando TRIM  ed agisce sul filesystem con due algoritmi differenti: uno greedy – ovvero con una strategia di eliminazione decisa in base alle necessità del momento – per la gestione delle “emergenze”, ed uno invece ottimizzato in termini di prestazioni per gestire le pulizie programmate.

F2FS supporta il journaling tramite checkpoint, ovvero previa analisi dei metadati presenti nell’area dedicata a intervalli di tempo ben definiti: in fase di montaggio, F2FS tenta di ripristinare l’ultimo checkpoint valido, il che potenzialmente lo rende più “esposto” alle inconsistenze generate da errori accidentali.

Raggiunge le prestazioni massime su dispositivi a stato solido, con i quali si comporta egregiamente. Risulta quasi del tutto inutile, invece, su dispositivi meccanici.

## XFS

XFS nasce originariamente nel 1993 da Silicon Graphics, tuttavia viene integrato nel kernel Linux soltanto nel 2001 ed è ad oggi utilizzato come filesystem di default in diverse distribuzioni. XFS è un filesystem a 64 bit ed ha dimensione massima di 8 EB. Tuttavia, per i sistemi operativi a 64 bit, il limite scende drasticamente a 16 TB.

E’ praticamente il filesystem più complesso della nostra analisi.

Anche XFS è un filesystem pensato prevalentemente per l’uso server, ma questa volta la scalabilità – differentemente da quanto succede con BTRFS – è intesa in termini di risorse di elaborazione: grazie alla suddivisione in gruppi di allocazione, ciascuno dei quali gestisce una certa quantità di inode occupati e di spazio libero, i processi a thread multipli sono in grado di effettuare operazioni simultanee, una per ogni gruppo di allocazione.

Ciò lo rende ideale su configurazioni che debbono sostenere applicazioni server onerose o intere “colonie” di macchine virtuali, oltre che su computer pensati per eseguire programmi esosi in termini di letture e scritture su disco.

La gestione delle inconsistenze sul filesystem grazie al journaling ed alle write barriers è qualitativamente alla pari (sebbene strutturalmente differente) rispetto a quella di BTRFS, anche se pecca leggermente in termini di performance quando si parla di RAID o della presenza di file di grandi dimensioni (ecco perché XFS è inadatto all’utilizzo su server di storaging).

Le write barriers sono delle precise direttive di scrittura della cache su disco a lassi di tempo predefiniti considerati ottimali per una buona prevenzione delle inconsistenze, tuttavia ciò può essere fastidioso per alcuni dischi collegati in RAID. La funzionalità di write barriers è attiva di default (ma disattivabile in fase di creazione del filesystem).

Altre peculiarità di XFS incentrate sulle prestazioni sono l’allocazione sfalsata per la riduzione della frammentazione, gli spazi di indirizzi a 64 bit per la localizzazione rapida di file “con buchi”, l’I/O diretto ed il pre-calcolo della banda richiesta per determinati scopi, funzionalità ottima per le applicazioni real-time ma che richiede hardware specifico.

XFS non dispone di funzionalità di snapshot non-bloccanti, è possibile ridimensionare il filesystem soltanto in aumento ma non diminuirne le dimensioni senza perdita di dati, il che lo rende estremamente inadatto per l’utilizzo su dischi virtuali.

## In sintesi

1. ext4 è un filesystem dalle medie pretese, con funzionalità implementate al 100% e general purpose, ideale per PC casalinghi con utilizzo medio;

2. BTRFS è il filesystem ideale per server di storaging (casalinghi o aziendali) grazie alle funzionalità avanzate di pooling, snapping, spanning e journaling, che assicurano prima di tutto la consistenza dei dati.

3. F2FS è il filesystem che potrebbe trovare, in un futuro abbastanza vicino, campo abbastanza fertile su smartphone, tablet e smartwatch. E’ particolarmente indicato per l’utilizzo su memorie NAND anche su microboard, tenendo però presente che si tratta di un filesystem incompleto e non in grado di garantire consistenza in caso di eventi non previsti (come l’interruzione di corrente);

4. XFS è un filesystem indicato per sistemi multipurpose, scalabili in termini hardware e che hanno bisogno sia di rapidità di accesso ai dati che di effettuare operazioni di I/O con programmi a thread multipli. In parole povere, XFS è il filesystem ideale per grossi server operativi (e non di archiviazione) o per configurazioni Linux da gaming. Impeccabile la gestione delle inconsistenze, anche se in alcuni casi può rivelarsi aberrante in termini di prestazioni.

