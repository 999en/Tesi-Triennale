# Tesi Triennale — struttura dell'elaborato

Questo documento contiene la tesi, costruita sul template `uninathesis` dell'Università degli
Studi di Napoli Federico II. Compila senza errori con `latexmk -pdf thesis.tex` (98 pagine: 76 di
corpo — introduzione, dieci capitoli, conclusioni — e 22 fra appendici e bibliografia; zero
errori, zero riferimenti irrisolti, zero riquadri fuori margine, 7 figure, 3 algoritmi,
35 tabelle).

## Cosa c'è

| file | contenuto |
|---|---|
| `thesis.tex` | documento principale: metadati del frontespizio, pacchetti, inclusione dei capitoli |
| `_chapters/0-abstract.tex` | sommario in italiano (unico riassunto; l'abstract in inglese non è richiesto) |
| `_chapters/0-introduction.tex` | introduzione: contesto, problema, **domanda di ricerca**, obiettivi, contributi con puntatori, struttura |
| `_chapters/1-background.tex` | background **e stato dell'arte**: dimensioni della qualità, valori mancanti, famiglie di imputazione, RFD, riduzione di insiemi di regole, similarità, definizione operativa di semantic pruning |
| `_chapters/2-renuver.tex` | analisi del sistema per disassemblaggio: interfaccia, architettura, dataset, doppio ruolo, due procedure di riempimento, costo |
| `_chapters/3-methodology.tex` | metodologia: motivazioni, spazio delle strategie, strategie implementate, condizioni esatte `[V1]`–`[V3]`, limite della generazione, contratto di integrazione |
| `_chapters/4-theory.tex` | **capitolo di teoria**: impostazione formale, teorema negativo, dimostrazione, corollario sul target, strettezza del limite, portata |
| `_chapters/5-design.tex` | ipotesi: nascita delle ipotesi, sette ipotesi primarie senza esiti (con la sezione in cui ciascuna è risolta), domande esplorative |
| `_chapters/6-experiments.tex` | valutazione sperimentale: protocollo, metodologia statistica, infrastruttura, dataset, metriche, termine di paragone banale, esperimenti `E1`–`E7`, ablazione |
| `_chapters/7-results.tex` | risultati: potatura garantita, riduzione classica, guadagno, confine, meccanismo, criterio diagnostico, prova su Physician, risultati negativi, costo |
| `_chapters/8-discussion.tex` | discussione: esiti delle ipotesi con il quadro complessivo, limiti dell'evidenza, collocazione rispetto alla letteratura |
| `_chapters/9-defects.tex` | difetti: tabella d'impatto, `D1`–`D4` del sistema studiato, `D5`–`D6` del codice sviluppato, che cosa insegnano |
| `_chapters/10-conclusions.tex` | conclusioni: sintesi e sviluppi |
| `_chapters/A-appendix-reproducibility.tex` | riproducibilità: materiale, verifica automatica, equivalenza della ricompilazione, comandi, ambiente |
| `_chapters/B-appendix-results.tex` | tabelle complete delle campagne (include il file generato) |
| `_chapters/B-tables-generated.tex` | **file generato** da `pruning/make_appendix_tables.py`: non modificare a mano |
| `_bib/bibliography.bib` | bibliografia |
| `_utils/macro.tex` | macro di notazione (RFD, LHS/RHS, condizioni `[V1]`–`[V3]`) |

## Da completare a cura del candidato

Tutti i campi sono contrassegnati nel sorgente con `<<<`.

1. **Frontespizio** (`thesis.tex`): nome del relatore, nome del candidato, numero di matricola,
   anno accademico, eventuale correlatore. Se non c'è un correlatore, non dichiararlo: il
   frontespizio omette automaticamente il blocco.
2. **Metadati del PDF** (`thesis.tex`, comando `\hypersetup`): autore.
3. **Bibliografia**: le voci contrassegnate `% <<< VERIFICARE` vanno ricontrollate su fonte
   primaria prima della consegna. Le due voci fornite dal relatore richiedono la verifica dei
   campi ricavati dai PDF; le altre, inserite senza accesso alla fonte, richiedono la verifica
   dell'intero record (volume, numero, pagine, sede di pubblicazione).
4. **Riferimenti bibliografici del capitolo 1**: le sezioni sono scritte, ma i riferimenti vanno
   letti in originale dal candidato, perché è su quelli che il relatore valuterà la padronanza
   della letteratura.
5. **Figure e listati**: presenti (6 figure, 3 algoritmi). La cartella `_figures/` resta
   disponibile se se ne volessero aggiungere; i candidati naturali sono lo schema del doppio ruolo
   delle RFD e il grafico dei quattro regimi di costo.

## Correzioni apportate al template

I file originali sono conservati come `.bak`.

| file | problema | correzione |
|---|---|---|
| `uninathesis.cls` | definiva un ambiente chiamato `expanded`, nome che dalla versione 2020-10-01 del kernel LaTeX collide con la primitiva `\expanded` e **impedisce la compilazione** | ambiente rinominato in `wideblock` (non era impiegato altrove) |
| `uninafrontespizio.sty` | il blocco del correlatore era emesso incondizionatamente, mostrando l'etichetta con valore vuoto | blocco reso condizionale alla presenza di almeno un correlatore |
| `uninathesis.cls` | le intestazioni dei capitoli non numerati erano in inglese (*Introduction*, *Conclusions*, *Appendix*), e le pagine pari restavano senza numero di pagina | nomi in italiano, ridefinibili con `\introductionheadname` e `\conclusionsheadname`; numero di pagina sul margine esterno di ogni pagina |
| `uninathesis.cls` | `\chapter*` e `\section*` non producono voce d'indice né segnalibro: i capitoli non numerati restavano fuori dall'indice e le loro sezioni finivano annidate sotto il capitolo precedente nella barra laterale | `\unnumberedchapter` e `\unnumberedsection` ora usano `\phantomsection` e `\addcontentsline`; l'introduzione e le conclusioni sono incluse con il primo |
| `thesis.tex` | hyperref nomina le unità in inglese: nel testo comparivano «la Table 5.2», «nel chapter 6», «la paragraph 3.4», «l'Appendix A» | `\addto\captionsitalian` ridefinisce `\chapterautorefname`, `\sectionautorefname`, `\subsectionautorefname`, `\paragraphautorefname`, `\tableautorefname`, `\figureautorefname`, `\appendixautorefname` |
| `thesis.tex` | una voce di bibliografia con DOI lungo sbordava dal margine destro | caricati `xurl` e le penalità di spezzamento di biblatex (`biburlnumpenalty`, `biburlucpenalty`, `biburllcpenalty`) |

### Trappola: `\pagestyle` dentro un ambiente

`\pagestyle` è un'assegnazione **locale al gruppo corrente**. Il pagestyle `appendices` va
impostato in `thesis.tex` **prima** di `\begin{appendices}`: se lo si imposta nel file incluso
dentro l'ambiente, l'ultima pagina dell'appendice — che viene mandata in stampa dopo
`\end{appendices}`, quando il gruppo è già chiuso — riprende l'intestazione del capitolo
precedente. È il motivo per cui una pagina dell'appendice compariva intestata «Conclusioni».

## Compilazione

```bash
latexmk -pdf thesis.tex        # compila (biber compreso)
latexmk -C                     # pulisce i file generati
```

Se la compilazione fallisce con errori su file `.aux` o `.out`, significa che un file ausiliario
è rimasto corrotto da un'esecuzione interrotta: eseguire `latexmk -C` e ricompilare.

## Struttura del front matter

Il documento è in modalità `twoside`: i capitoli non numerati iniziano obbligatoriamente su pagina
dispari, quindi tra un blocco e il successivo resta una pagina bianca. La sequenza è:

| pagina | contenuto |
|---|---|
| 1 | frontespizio (uno solo: `\makefrontespizio`) |
| 3 | Sommario |
| 5 | Indice |
| 7 | Introduzione |

Le pagine bianche sono dovute alla regola tipografica suddetta, non a un errore di compilazione.
Il comando `\makefrontespizioalt` non è invocato: generava un secondo frontespizio identico.

## Da dove vengono i numeri

Tutti i valori presenti nelle tabelle sono ricalcolabili dagli artefatti del lavoro, senza
rieseguire il sistema:

| procedura | che cosa verifica |
|---|---|
| `pruning/audit_report.py` | **67 asserzioni numeriche** contro i file di risultato delle campagne (oggi: 67 OK, 0 divergenze) |
| `pruning/stats_effects.py` | test della famiglia primaria con dimensione dell'effetto, intervallo bootstrap e correzione di Holm; include quattro controlli di regressione sui valori di `p` già pubblicati (4/4 PASS) |
| `pruning/make_appendix_tables.py` | genera le tabelle dell'appendice B; i totali coincidono con quelli discussi nel capitolo 7 |
| `pruning/prepare_physician_tuples.py` | costruisce i file di ground truth della famiglia Physician escludendo le celle non localizzabili (difetto D3) |
| `pruning/probe_physician_full.py` | sonda limitata nel tempo sulla configurazione piena di Physician; l'esito è in `results/probe_physician_full.json` |
| `pruning/repair_physician_datasets.py` | ripara le righe malformate dei dataset Physician 0.05 e 0.1 (procedura **non** applicata alla campagna) |
| `pruning/trivial_baseline.py` | calcola il termine di paragone banale (moda di colonna) sulle stesse celle valutate: `results/trivial_baseline.json` |
| `pruning/pruning.py` (strategia `classical-cover`) | riduzione di ridondanza classica, usata come termine di paragone: `results/analisi/results_classical_cover.json` |

La campagna sulla quarta famiglia (Physician) è documentata nella §18 di `report.md` e i suoi
dodici record sono in `results/analisi/results_physician_remodulated.json`: sta in una
sottocartella perché non appartiene alla griglia delle 1 618 esecuzioni e i conteggi di
`audit_report.py` devono restare invariati.

Prima di modificare un numero in questa tesi conviene verificarlo con queste procedure. Se si
aggiungono file di risultato in `results/`, i conteggi di `audit_report.py` cambiano: i dati che
non appartengono a una campagna (come l'esperimento di ordinamento) vanno archiviati in
`results/analisi/`.
