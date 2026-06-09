# Analisi dell'Andamento degli Indici Azionari S&P 500 ed EURO STOXX 50

[![CI](https://github.com/profession-ai-data-engineering-master/profession_ai_data_engineering_progetto2/actions/workflows/ci.yml/badge.svg)](https://github.com/profession-ai-data-engineering-master/profession_ai_data_engineering_progetto2/actions/workflows/ci.yml)

## Descrizione del progetto

L'azienda Global Investment Insights è una società di consulenza finanziaria che si occupa di fornire analisi dettagliate sull'andamento dei mercati azionari. Il progetto si concentra su due indici fondamentali: - S&P 500: che rappresenta il mercato azionario statunitense. - EURO STOXX 50: che descrive l'andamento del mercato azionario europeo.

L'obiettivo è studiare l'andamento degli ultimi 10 anni di questi indici e trarne insights preziosi che possano aiutare gli investitori a prendere decisioni strategiche.

L'analisi fornita da Global Investment Insights offrirà agli investitori una panoramica chiara e dettagliata sui trend storici di due dei principali indici azionari globali. Grazie all'approfondimento sui rendimenti giornalieri e mensili, nonché all'identificazione dei giorni di maggiore volatilità, gli investitori potranno pianificare strategie di investimento più informate e ridurre il rischio associato ai loro portafogli. Il calcolo del volume medio giornaliero, inoltre, fornirà indicazioni utili sull’interesse degli investitori e sui periodi di maggiore attività di mercato, consentendo decisioni più oculate e mirate.

## Obiettivi del progetto
L'analisi prevede i seguenti step: - Calcolo del rendimento percentuale mensile e annuale. - Calcolo del rendimento medio giornaliero per ciascun indice e per ciascun giorno della settimana. - Individuazione dei giorni con il rendimento giornaliero più alto e più basso per entrambi gli indici. - Calcolo del volume medio giornaliero di scambi per ciascun indice.

## Dataset
Puoi scaricare i dataset da qui: https://drive.google.com/drive/folders/1j9tlNmoUyqlOQd8HInHoa950Gccji4mb?usp=sharing

I dataset forniti, sp500.csv per l'indice S&P 500 e euro50.csv per l'indice EURO STOXX 50, contengono le seguenti colonne:

- Date: la data della rilevazione.
- Open: il prezzo di apertura di quel giorno.
- High: il prezzo massimo raggiunto durante il giorno.
- Low: il prezzo minimo raggiunto durante il giorno.
- Close: il prezzo di chiusura del giorno.
- Volume: il numero di scambi avvenuti durante quel giorno.

## Steps del progetto
### 1. Calcolo del rendimento percentuale
Per ciascun indice, sarà calcolato il rendimento percentuale utilizzando la formula della variazione percentuale tra i prezzi di chiusura di un periodo rispetto al periodo precedente: - Rendimento mensile - Rendimento annuale

### 2. Rendimento medio giornaliero per giorno della settimana
Calcolare il rendimento medio giornaliero di ciascun indice per ogni giorno della settimana. Questa analisi permetterà di identificare eventuali trend particolari legati a giorni specifici (ad esempio, l'andamento delle borse il lunedì rispetto agli altri giorni della settimana).

### 3. Giorni con rendimento massimo e minimo
Individuare, per ciascun indice, i giorni con il rendimento giornaliero più alto e quelli con il rendimento giornaliero più basso. Questa informazione è utile per analizzare le giornate di estrema volatilità sui mercati e comprenderne le cause.

### 4. Calcolo del volume medio giornaliero
Analizzare il volume medio giornaliero di scambi per ciascun indice. Questo dato rappresenta il livello di attività di mercato e può aiutare a capire l'interesse degli investitori nei due indici.

## Metodologia
- I rendimenti percentuali verranno calcolati come variazione percentuale del prezzo di chiusura rispetto al periodo precedente (giornaliero, mensile o annuale).
- Per il rendimento medio giornaliero, i dati verranno aggregati in base al giorno della settimana per ciascun indice.
- Per i giorni con rendimento massimo e minimo, verranno individuati i valori estremi del rendimento giornaliero nei due dataset.

---

# Resoconto dello svolgimento

> Questa sezione documenta come è stato affrontato il problema e il lavoro svolto. **Non modifica la consegna originale** riportata sopra: la integra con un resoconto metodologico. L'analisi completa, con codice, grafici e commenti, è nel notebook `profession_ai_data_engineering_progetto2.ipynb`.

## Approccio: scelta del framework metodologico
Per non "reinventare la ruota", la prima attività è stata la ricerca di un framework standard e maturo su cui appoggiare il flusso di lavoro:

- **CRISP-DM** è stato valutato ma scartato: troppo orientato al data mining / ML per le esigenze, tutto sommato lineari, di questo progetto (resta un candidato per progetti futuri di data mining e AI).
- **[OSEMN](https://www.datascience-pm.com/osemn/)** è stato adottato come miglior compromesso tra rigore di un processo testato in ambito enterprise e flessibilità di adattamento. Le sue 5 fasi (iterative, non lineari) hanno fatto da struttura portante dell'intero lavoro.

| Fase | Significato | Scopo |
|---|---|---|
| **O** | Obtain | Ottenere i dati da fonti esterne in modo lecito |
| **S** | Scrub | Pulire errori, duplicati, valori mancanti; rendere i dati utilizzabili |
| **E** | Explore | Comprendere struttura, pattern e relazioni con statistiche e visualizzazioni |
| **M** | Model | Costruire modelli predittivi/descrittivi |
| **N** | iNterpret | Tradurre i risultati in decisioni e comunicarli agli stakeholder |

## O - Obtain
I due dataset sono **versionati nel repository** in `data/` (`data/sp500.csv`, `data/euro50.csv`) — originariamente forniti via Google Drive — e caricati con **pandas** da file locale: il notebook è così eseguibile end-to-end e riproducibile offline, senza download manuali né dipendenze di rete. Dall'ispezione dei dati grezzi sono emersi subito tre nodi da risolvere in fase di Scrub: fuso orario non uniforme sulle date, valori `Volume = 0` anomali sull'EURO STOXX 50, e numero di sedute differente tra i due indici (2517 vs 2512).

## S - Scrub
Pulizia e allineamento mirati alla natura dei dati di mercato:
- **Verifica** preventiva di valori nulli e duplicati (nessuno riscontrato nei dataset di partenza).
- **Uniformazione delle date in UTC** e successiva **normalizzazione alla mezzanotte** (`normalize`): i due indici registrano le sedute con fusi orari diversi, ma l'orario è irrilevante avendo una sola rilevazione giornaliera per indice.
- **Rimozione delle righe con `Volume = 0`** sull'EURO STOXX 50 (giornate senza scambi reali, non informative).
- **Allineamento sul calendario di trading comune**: tenuti solo i giorni presenti in entrambi gli indici (intersezione delle date), così da confrontare i due mercati a parità di contesto macro e geopolitico ed evitare distorsioni dovute a chiusure/festivi asimmetrici.

## E - Explore
- **Data Dictionary**: prima dell'analisi sono state formalizzate le feature di ciascun indice (tipo, descrizione, business rules es. `Low ≤ Open,Close ≤ High`; significato del `Volume` come somma stimata degli scambi sui titoli che compongono l'indice, non sull'indice stesso). Questa scelta - derivata dall'esperienza professionale - riduce gli errori concettuali nella manipolazione dei dati.
- **Analisi preliminare** (dimensioni, tipi, nulli, statistiche descrittive) sui dataset puliti.
- **Feature engineering**: rendimenti percentuali **giornaliero, mensile e annuale** (variazione % del prezzo di chiusura sul periodo precedente), giorno della settimana e chiavi di periodo, a supporto dei quattro quesiti.
- **Risposta ai quesiti della consegna (Q1–Q4)**, ciascuno con analisi, visualizzazioni e conclusioni:
  - **Q1 - Rendimento percentuale**: rendimenti giornaliero/mensile/annuale confrontati tra i due indici, affiancati alla **performance cumulata** (`(1+r).cumprod()`). Evidenza: S&P 500 più performante e resiliente (es. recupero post-COVID), EURO STOXX 50 mediamente più volatile.
  - **Q2 - Rendimento medio per giorno della settimana**: aggregazione per giorno e confronto a barre. I risultati sono stati interpretati alla luce di fenomeni documentati (*reverse weekend effect* sull'S&P 500, reazione ritardata dei listini euro, diversa composizione settoriale USA/Europa).
  - **Q3 - Giorni di rendimento estremo**: il quesito è stato impostato come **outlier detection** anziché semplice max/min. È stato scelto il **metodo IQR** (e non lo Z-score, che assume distribuzione normale) con **fattore 5** per isolare solo gli eventi realmente eccezionali. Le giornate individuate sono state ricondotte ad eventi reali di mercato (Black Monday 2015/2020, Volmageddon, Brexit, ecc.).
  - **Q4 - Volume medio giornaliero**: confronto delle medie; l'S&P 500 scambia oltre il doppio dei volumi dell'EURO STOXX 50.

## M - Model
Fase **non sviluppata consapevolmente**: esula dalle competenze di AI/Data Science maturate finora. Viene mantenuta esplicitamente nel flusso per coerenza e rigore nell'applicazione di OSEMN, come spazio per estensioni future (es. modelli predittivi sui rendimenti).

## N - iNterpret
Gli insight sono stati tradotti in indicazioni operative per gli investitori:
- **Q1**: allocazione **core-satellite** ispirata alla *Modern Portfolio Theory* (≈65–70% core USA, 20–25% satellite Europa, 10–15% cuscinetto difensivo) per bilanciare rendimento e rischio geografico.
- **Q2**: timing degli acquisti coerente con gli effetti di calendario osservati (preferenza per il lunedì sull'S&P 500, prudenza a metà settimana sull'Europa).
- **Q3**: la diversificazione e la quota di beni rifugio difendono rispettivamente da shock locali e globali; suggeriti ribilanciamenti preventivi nel medio termine.
- **Q4**: sui mercati USA, più liquidi, ordini anche corposi in un'unica operazione; in Europa, esecuzione dilazionata (es. algoritmi tipo **VWAP**) per contenere slippage e impatto sul prezzo.

## Sintesi delle scelte di progettazione
- **Framework riconosciuto** (OSEMN) al posto di un processo ad-hoc, con motivazione esplicita della scelta rispetto a CRISP-DM.
- **Confronto equo tra indici** garantito da normalizzazione delle date e allineamento sul calendario comune.
- **Metodi statistici motivati** (IQR vs Z-score) e **Data Dictionary** come presidio di qualità sui dati.
- **Riproducibilità**: dataset versionati nel repo (`data/`) e caricati da file locale; nessuno stato esterno né dipendenza di rete per rieseguire l'analisi.

## Come eseguire

```bash
pip install -r requirements.txt
jupyter notebook profession_ai_data_engineering_progetto2.ipynb
```

I dataset sono già inclusi in `data/`: il notebook gira end-to-end senza ulteriore configurazione.

## Qualità e CI

Una pipeline **GitHub Actions** (`.github/workflows/ci.yml`) viene eseguita a ogni push e Pull Request su `main`:

- **Lint** del codice del notebook con `ruff` (via `nbqa`);
- **Esecuzione end-to-end** del notebook con `nbmake`, a garanzia che l'analisi resti sempre riproducibile.
