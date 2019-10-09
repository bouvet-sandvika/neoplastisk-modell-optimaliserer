# Oppgavesett 1

### Informasjon
Formålet med dette oppgavesettet er å konfigurere og trene et kunstig nevralt nettverk 
med formål om å avgjøre en svulst er godartet eller ondartet og dette med svært god nøyaktighet.

### Datasettet
Den amerikanske delstaten Wisconsin har publisert vevsprøver(med godkjenning av pasientene) fra svulster funnet i brystene hos kvinner.
Dataene beskriver visse karakteristikker ved cellene tatt ved disse vevsprøvene. Disse karakteristikkene er listet under:

* radius (gjennomsnittlig avstand fra cellekjernen til punkter på cellemembranen)
* texture (standard avvik av gråskala verdier)
* perimeter (mpling på hvor "rund" cellen er)
* area (cellens tverrsnittareal)
* smoothness (Måling av variasjoner i radius lengde)
* compactness (perimeter^2 / area - 1.0)
* concavity (alvorlighet av konkave områder)
* concave points (konkave områder ved cellemembranen)
* symmetry (Målinger av cellens symmetri. Kreftceller er kjent for å være asymmetriske)
* fractal dimension ("kystlinje approksimasjon" - 1)

#### Nyttig link
https://deeplearning4j.org/docs/latest/deeplearning4j-quickstart

### Oppgaver

#### Oppgave 1.1 Opprett prosjektet
Finn git repository og klon prosjektet.

#### Oppgave 1.1 - Trenings -og testsett
Naviger til `resources`-katalogen, åpen filen `fine-needle-aspirate-celleprover.csv` og velg et utvalg av dataene som skal brukes til trening og test. 
Opprett en trening -og testfil. 

Tips: En tommelfingerregel er å fordele 70% av dataene til trening og 30% til testing.

#### Oppgave 1.2 - Les trenings -og testsett.
Konfigurasjonen, treningen og evalueringen kan godt gjøres i `main()`-metoden.
Bruk kontrakten `RecordReader` med implementasjonen `CSVRecordReader` for å lese inn dataene.

#### Oppgave 1.3 Opprett DataSetIterator for trenings -og testsett 
Vi må kunne å iterere gjennom datasettet. Bruk implementasjonen `RecordReaderDataSetIterator` og opprett objekt av denne klassen.

#### Oppgave 1.4 Konfigurasjon av nevralt nettverk
Vi er nå klare til å konfigurere vårt nevrale nettverk.

Opprett et objekt av den indre klassen `NeuralNetConfiguration.Builder()` og bygg opp det nevrale nettverket med god gammeldags DSL-syntax.

Følg strukturen:
```$xslt
MultiLayerConfiguration conf = new NeuralNetConfiguration.Builder()
            .seed(SEED)
            .iterations(ITERASJONER)
            .optimizationAlgo(VELG_OPTIMALISERINGSALGORITME)
            .updater(VELG_OPPDATERINGS_ALGORITME)
            .learningRate(VELG_LÆRINGSRATE)
            .list()                                      // Her opprettes lagene. NB: Spesifiser Layer implementasjonen
            .layer(0, new Layer() {...})
            .layer(1, new Layer() {...})
            ...
            .layer(x-1, new OutputLayer{...})              // Siste laget MÅ være av klassen OutputLayer
            .backprop(true)                              // Vi ønsker at algoritmen som oppdaterer nettverket skal være aktivert
            .backpropType(VELG_BACKPROPAGATION_ALGORITME)
            .build();
```

NB: Det MÅ være minst 2 lag i et multilags nevralt nettverk. Bestem antall "Hidden layers", riktig implementasjon av `Layer`  og konfigurering av lagene.

#### Oppgave 1.5 Initialisering av modellen
Opprett objekt av klassen `MultiLayerNetwork` og kall `init()`- metoden på objektet.

#### Oppgave 1.6 Status ved trening
For å monitorere treningen ønsker vi å sette `ScoreIterationListener` til `MultiLayerNetwork`-objektet.
Hvor mange ganger scoren skal logges til konsollet under trening er opp til deg selv.

#### Oppgave 1.7 Treningen starter
Start trening en ved å angi antall gjennomkjøringer av hele datasettet. 1 gjennomkjøring = 1 epoch.
For hver epoch, kall metoden `fit(DataSetIterator)` på `MultiLayerNetwork`- objektet.

#### Oppgave 1.8 Evaluering
Når treningen er ferdig ønsker vi en evaluering av hvor trent modellen faktisk er.
Opprett et objekt av klassen `Evaluation`.
Det skal gjøres en prediksjon mot testdataene som ble opprettet i oppgave 1.3.

Iterer gjennom testdataene og kall metodene `output(List<INDArray> features)` på `MultiLayerNetwork`-objektet og `eval(INDArray, INDArray)` på `Evaluation`-objektet.

PS: `INDArray` er et en klasse som kan håndtere høyrere dimensjonale vektorer. Et `INDArray` objekt av rang 
* 0 er en skalar
* rang 1 en vektor
* rang 2 en matrise
* rang >=3 en tensor (Spesielt brukt i avansert bilde -og lydanalyse)

#### Oppgave 1.9 Statistisk oppsumering
Print ut statistikken ved å kalle metoden `stats()` på objektet klassen `Evaluation`.

#### Oppgave 1.10 Optimalisering
Kaliberer nødvendige parametere slik at score'n konvergerer og at modellen oppfyller følgende krav:
```$xslt
==========================Scores========================================
 # of classes:    X
 Accuracy:        > 95%
 Precision:       > 90%
 Recall:          > 90%
 F1 Score:        > 95%
========================================================================
```


#### Oppgave 1.11 Lagre modellen
Når modellen er godt trent (ikke overtrent), så skal den kunne brukes i et produksjonsmiljø.
Lagre modellen som en `zip`-fil til disk vha den statiske metoden:
```$xslt
ModelSerializer.writeModel(Model, File, boolean);
```

#### Oppgave 1.12 Klone prosjektet "kunstig-intelligens-web-tjeneste"
Følg videre instrukser i `Oppgaver.md` filen i det prosjektet.
