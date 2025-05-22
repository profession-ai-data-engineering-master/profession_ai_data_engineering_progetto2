# Analisi dell'Andamento degli Indici Azionari S&P 500 ed EURO STOXX 50
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
