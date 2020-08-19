# Discorso

## Slide iniziale

Buongiorno a tutti, mi chiamo Marco Olivo e sono qui per presentare il
mio lavoro di tesi riguardante lo studio di nuove euristiche per il
miglioramento di algoritmi di ranking per il World-Wide Web.

## Slide 2 direttrici

Durante il corso di questa tesi mi sono concentrato su due differenti
linee di sviluppo: la prima ha riguardato lo studio e
l'implementazione di svariati algoritmi preposti all'assegnazione di
un punteggio alle pagine web in base alle interrogazioni poste al
motore di ricerca da parte degli utenti: in questa fase si è proceduto
anzitutto all'implementazione di alcuni noti algoritmi tra i quali
cito ad esempio PageRank e Proximity, fase seguita in seconda battuta
dallo studio e dall'implementazione di nuovi algoritmi che permettono
di fornire la sensazione di "correttezza" che viene percepita da un
utente nel momento in cui riceve i risultati da un motore di ricerca;
la seconda linea di sviluppo, invece, si è concentrata sullo studio di
una tecnica per aggregare i risultati ottenuti dai vari algoritmi e
sullo studio ed implementazione di una tecnica per velocizzare il
calcolo delle occorrenze tramite una valutazione lazy.

## Slide dei nuovi algoritmi di ranking

Come ho detto in precedenza, la prima fase della mia tesi è stata
incentrata sullo studio di algoritmi di ranking, ovvero di algoritmi
che forniscono un punteggio alle pagine web in funzione di vari
fattori, tra cui l'importanza della pagina - assoluta o relativa in
relazione all'interrogazione posta dall'utente al motore.

Anzitutto, si sono implementati due noti algoritmi di ranking
ampiamente utilizzati nei motori di ricerca commerciali: PageRank e
Proximity.  Il primo di questi è un algoritmo che si basa sulla
struttura di grafo del web per fornire un punteggio alle pagine e il
punteggio fornito non dipende assolutamente dall'interrogazione, ma
solamente dalla rilevanza "assoluta" delle pagine in questione: in
questo senso si può parlare di PageRank come di un algoritmo esogeno,
in cui cioè il punteggio proviene "da fuori". Il principio su cui si
basa PageRank è fondamentalmente quello che afferma che pagine puntate
sono importanti.  L'altro algoritmo, invece, è Proximity ed è un
algoritmo endogeno, in cui cioè il punteggio deriva direttamente dai
contenuti della pagina. Esso inoltre dipende dall'interrogazione, in
quanto misura la vicinanza delle parole ricercate all'interno delle
pagine, e fornisce un risultato calcolato di conseguenza. Tipicamente,
un motore di ricerca scarterà poi le pagine con Proximity nulla, così
da mostrare solamente le pagine che soddisfano realmente
l'interrogazione.

## Slide di PageRank + Proximity che non bastano

PageRank e Proximity da soli tuttavia non bastano. E per accorgersene
è sufficiente fare qualche prova e metterla a confronto con i
risultati attesi: ad esempio, sull'interrogazione...

=== INSERIRE ESEMPIO ===

Per realizzare pertanto un motore di ricerca competitivo occorre
affidarsi anche ad altri algoritmi. Tra questi, in fase di
sperimentazione, si è pensato di proporre svariate euristiche, basate
sui titoli delle pagine, sugli indirizzi HTTP, e sul testo con cui le
pagine vengono puntate all'interno del web.

## Slide TitleRank

Il primo tra gli algoritmi proposti da me nel corso della tesi e
battezzato "TitleRank" è un algoritmo che si basa sui titoli delle
pagine. Va infatti tenuto conto che il titolo delle pagine è forse la
parte più significativa di una pagina web, in quanto il suo creatore
tende fisiologicamente ad inserirvi all'interno le parole o la frase
che meglio caratterizza il contenuto della pagina. Pertanto avere una
qualche forma di punteggio - ovviamente endogeno - sui titoli pare
essere una buona idea, e infatti così è.  L'algoritmo che ho proposto
fornisce alle pagine un punteggio dato secondo una formula che
predilige la vicinanza delle parole nei titoli delle pagine.

## Slide URLRank

Un problema che molti motori di ricerca non risolvono è il fatto che
spesso gli utenti dei motori di ricerca tendono ad inserire il nome
del sito che stanno cercando (es. microsoft per www.microsoft.com) A
fronte di una richiesta simile un requisito che sarebbe bello
soddisfare è che siano in un qualche modo privilegiati i siti web che,
direttamente nella loro URL, soddisfano l'interrogazione dell'utente.
E' proprio questo l'obbiettivo che si pone URLRank, un altro degli
algoritmi che ho proposto e che, in maniera simile a TitleRank,
fornisce un punteggio alle pagine web.

Il funzionamento di URLRank tuttavia è interessante per quanto
riguarda le strutture dati utilizzate: si utilizza infatti un ternary
search tree per ricercare le parole di lunghezza massimale contenute
nelle URL dei documenti. In questo modo ad esempio ci si accorge che
all'indirizzo www.comunedimilano.it corrispondono tre parole: comune,
di, milano.

## Slide AnchorRank

Già con gli algoritmi che ho appena presentato la qualità delle
risposte alle ricerche più comuni aumenta in maniera visibile ed
oggettiva.  Tuttavia una cosa che ancora non si può fare con gli
algoritmi presentati è recuperare una pagina che non contiene i
termini ricercati, ma che è nota essere rilevante per l'argomento
richiesto. Di esempi ve ne sono molti: dal sito della Fiat, che in
tutta la homepage non contiene la parola "automobili", al sito
dell'Ansa, in cui non vi è traccia delle parole "agenzia di stampa".

L'idea è di utilizzare il testo delle pagine che puntano ad esempio a
www.ansa.it per costruire un "meta-testo" che permetta di estendere il
testo vero e proprio e che permetta pertanto di fare ricerche più
mirate e precise, trovando anche le pagine che non contengono
esattamente i termini ricercati ma che, comunque, sono note per
trattare l'argomento ricercato.  E' proprio questo che l'algoritmo di
AnchorRank da me proposto si pone di fare.

## Slide demo risultati

Ho preparato una piccola dimostrazione nella quale - incrociamo le
dita - farò vedere il motore di ricerca da me sviluppato in azione su
un paio di interrogazioni.  I dati su cui ci baseremo saranno un
insieme di poco più di 20 milioni di pagine web estratte durante una
crawl di 7 giorni da .it - ovvero saranno pressoché tutte pagine
italiane.  Il tempo di indicizzazione di questa quantità di dati -
ridotta per un motore di ricerca commerciale, piuttosto grossa per le
nostre possibilità - è stato di circa 3 giorni di lavoro macchina.

## Slide aggregazione

Una volta creati tutti gli algoritmi che ho menzionato nel corso di
questa presentazione, è sorto il problema di come fondere i risultati
prodotti per avere una lista di pagine da ordinare in base al
punteggio. Il problema è noto come problema del rank-merging.
Anzitutto si è scelto per ovvie ragioni di eliminare tutte le pagine
che non soddisfano l'interrogazione, al fine di scremare il numero di
pagine restituite.  Dopodichè, a questo insieme di pagine si sono
aggiunte le pagine che soddisfano l'interrogazione nei documenti che
puntano a loro: in questo modo se www.fiat.it è puntata con la parola
"automobili" pur non contenendo questa parola al suo interno potremo
comunque recuperarla. Successivamente si è scelto di combinare
linearmente i risultati dei vari algoritmi in maniera tale che sia
possibile con facilità testare nuove soluzioni basate su pesi
differenti e sia inoltre possibile dare un chiaro pulito modo di
procedere.

## Slide valutazione lazy

L'ultima direttrice di sviluppo di questa tesi ha riguardato lo studio
di una tecnica per diminuire sensibilmente i tempi di risposta, in
particolar modo a fronte di richieste "pesanti" come ad esempio
interrogazioni contenenti delle congiunzioni.  Va infatti tenuto conto
che, poiché le congiunzioni compaiono in pressoché tutti i documenti
redatti in lingua italiana, il numero di documenti da esaminare per
determinare se una interrogazione è soddisfatta o meno aumenta in
maniera vertiginosa, così come i tempi di risposta. Risulta quindi
necessario tagliare questa valutazione dopo una certa soglia di
documenti valutati.

L'idea è stata quella di ordinare anzitutto i documenti secondo
l'ordinamento statico fornito da PageRank, unico algoritmo tra quelli
implementati ad essere precomputato. In questo modo i primi documenti
ad essere valutati sono anche i documenti che hanno il peso maggiore
secondo questo algoritmo. Dopodiché, tramite simulazione, si sono
provate alcune interrogazioni multi-parola (poco più di 200) e si sono
registrati i seguenti dati:

* quante pagine soddisfacevano l'interrogazione
* quante pagine bisognava "chiedere" affinché, sommando i risultati dei vari algoritmi, si trovassero le prime X con una certa precisione fissata

Tutto questo ha permesso di trovare una funzione che restituisce
il numero di pagine da valutare dato un numero di documenti che si presume
soddisfino l'interrogazione.
Il numero di pagine che si pensa soddisfino l'interrogazione viene calcolato
tramite una proiezione basata sull'ipotesi che le pagine
siano equidistribuite nell'intero insieme di pagine recuperato.

## Slide conclusioni

Per concludere, nel corso di questa tesi è stato sviluppato un motore
di ricerca basato su tecniche in parte note ed in parte innovative in
grado di rispondere in tempi ragionevolmente brevi alle interrogazioni
degli utenti.

Si è inoltre presentato un metodo per aggregare i risultati (problema
del rank-merging) e si è anche presentato un sistema per tagliare la
valutazione dei documenti che soddisfano una data interrogazione pur
non perdendo nulla, dal punto di vista della percezione dell'utente,
tra i primi N risultati.

E con questo ho concluso, vi ringrazio per l'attenzione. Grazie.
