# systemd?

## Che cos'è systemd?

Systemd è un gestore di sistema e di servizi per Linux, compatibile con gli initscript SysV e LSB. Esso fornisce una grande capacità di parallelizzazione, usa socket e D-Bus per l'avvio dei demoni, offre un avvio su richiesta dei demoni, tiene traccia dei processi con l'utilizzo del control groups di Linux, supporta lo snapshotting e il restore dello stato del sistema, ed inoltre implementa un elaborato servizio di controllo logico basato sulle relazioni delle dipendenze.

**Il principale comando per l’uso di systemd è systemctl.**

Sostanzialmente, systemd inizializza e supervisiona l'intero sistema ed è basato sul concetto di units composto da nome/tipo stessi del file di configurazione da maneggiare.

## 7 tipi differenti di UNITS

1. **service**: esistono tipologìe ovvie di unità: demoni che possono essere avviati, fermati, riavviati, ricaricati.

2. **socket**: questa unità contiene un socket nel file-system o su Internet. systemd attualmente supporta i socket AF_INET, AF_INET6, AF_UNIX di tipo stream, datagram esequential packet. Può inoltre supportare i classici FIFO come transport. Ogni unità socket ha un servizio corrispondente, che viene inizializzato se la prima connessione avviene sul socket o FIFO (ad esempio nscd.socket inizializza nscd.service su una connessione in ingresso).

3. **device**: questa unità incapsula un dispositivo nell'albero dei dispositivi Linux. Se uno di loro viene contrassegnato attraverso una regola udev (udev rules), sarà esposto come unità dispositivo in systemd. Le proprietà impostate con udev possono essere usate come configurazione per impostare le dipendenze per le device units.

4. **mount**: questa unità incapsula un punto di montaggio (mount point) nel filesystem.

5. **automount**: questo tipo di unità incapsula un punto di automontaggio nel filesystem. Ogni unità automount corrisponde ad una unità mount, che viene avviata (montata) non appena la directory automount è accessibile.

6. **target**: questo tipo di unità per il logical grouping: invece di fare qualcosa autonomamente, semplicemente fa riferimento ad altre unità che possono essere controllate in gruppo. (ad esempio multi-user.target, che è un target che svolge praticamente il ruolo di run-level 5 nel classico sistema SysV; o bluetooth.target richiesto appena un dispositivo bluetooth è disponibile e che semplicemente avvìa di conseguenza i servizi relativi altrimenti non necessari: bluetoothd e obexd e simili).

7. **snapshot**: simile alle unità target, attualmente non fanno nulla se nonché essere un riferimento per altre unità.

## Funzionalità nel dettaglio

Systemd dispone di diverse caratteristiche estremamente utili e particolari, vediamone qualcuna in dettaglio.

- attivazione di D-Bus per avviare i servizi

- avvìo on-demand dei demoni

- aggressive capacità di parallelizzazione usando il socket

- supporta lo snapshotting e il ripristino dello stato del sistema

- monitora tutti i punti di montaggio e può essere usato per modificarli

- /etc/fstab può essere usato come configurazione addizionale

- Usando l'opzione comment= di fstab, è possibile persino marcare le voci in /etc/fstab per lasciare a systemd il controllo dei punti di montaggio

- implementa una logica di controllo del servizio basata sulle dipendenze

> Inoltre, per ogni processo generato, systemd controlla: L'ambiente, i limiti delle risorse, directory working e root, umask, OOM killer adjustment, nice level, IO class and priority, CPU policy and priority, CPU affinity, timer slack, user id, group id, group id supplementari, directory leggibili/scrivibili/inaccessibili, mount flags condivise/private/secondarie, capabilities/bounding set, secure bits, CPU scheduler reset of fork, /tmp name-space privati, controllo cgroup per vari sottosistemi.

## Compatibilità con SysV init scripts

Si avvantaggia di LSB e degli headers Red Hat chkconfig se disponibili, altrimenti usa le informazioni disponibili, come le priorità di avv'o in /etc/rc.d. Questi script init vengono semplicemente considerati una fonte di configurazione differente, facilitano il percorso d'aggiornamento ai servizi systemd

## Avvio

All'avvio systemd attiva (come predefinito), l'unità target default.target il cui compito è quello di attivare i servizi ed altre unità a seconda delle dipendenze richieste.

Per gestire l'unità da attivare, systemd analizza gli argomenti della propria linea di comando kernel attraverso l'opzione systemd.unit=. Quest'ultima può essere usata per avvii temporanei differenti.

**I classici run-level sono rimpiazzati come di seguito:**

- **systemd.unit=rescue.target** è uno speciale target per impostare un sistema base e la rescue shell

- **systemd.unit=emergency.target** è molto simile al init=/bin/sh ma con l'opzione d'avvìo per l'intero sistema

- **systemd.unit=multi-user.target** per impostare un sistema multi utente non grafico

- **systemd.unit=graphical.target** per impostare il login grafico

> systemctl è lo strumento principale da usare. Combina le fuzionalità dei servizi e di chkconfig per abilitarli/disabilitarli permanentemente o solo per la connessione corrente

